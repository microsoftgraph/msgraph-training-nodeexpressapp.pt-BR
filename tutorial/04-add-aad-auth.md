<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a biblioteca do [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) ao aplicativo.

1. Crie um novo arquivo chamado `.env` arquivo na raiz do seu aplicativo e adicione o código a seguir.

    :::code language="ini" source="../demo/graph-tutorial/.env.example":::

    Substitua `YOUR APP ID HERE` pela ID do aplicativo do portal de registro do aplicativo e substitua `YOUR APP SECRET HERE` pela senha gerada.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `.env` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo e sua senha.

1. Abra `./app.js` e adicione a seguinte linha na parte superior do arquivo para carregar o `.env` arquivo.

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>Implementar logon

1. Localize a linha `var indexRouter = require('./routes/index');` no `./app.js`. Insira o código a seguir **antes** dessa linha.

    ```javascript
    var passport = require('passport');
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // Configure passport

    // In-memory storage of logged-in users
    // For demo purposes only, production apps should store
    // this in a reliable storage
    var users = {};

    // Passport calls serializeUser and deserializeUser to
    // manage users
    passport.serializeUser(function(user, done) {
      // Use the OID property of the user as a key
      users[user.profile.oid] = user;
      done (null, user.profile.oid);
    });

    passport.deserializeUser(function(id, done) {
      done(null, users[id]);
    });

    // Callback function called once the sign-in is complete
    // and an access token has been obtained
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }

    // Configure OIDC strategy
    passport.use(new OIDCStrategy(
      {
        identityMetadata: `${process.env.OAUTH_AUTHORITY}${process.env.OAUTH_ID_METADATA}`,
        clientID: process.env.OAUTH_APP_ID,
        responseType: 'code id_token',
        responseMode: 'form_post',
        redirectUrl: process.env.OAUTH_REDIRECT_URI,
        allowHttpForRedirectUrl: true,
        clientSecret: process.env.OAUTH_APP_PASSWORD,
        validateIssuer: false,
        passReqToCallback: false,
        scope: process.env.OAUTH_SCOPES.split(' ')
      },
      signInComplete
    ));
    ```

    Este código inicializa a biblioteca [Passport. js](http://www.passportjs.org/) para usar a `passport-azure-ad` biblioteca e a configura com a ID do aplicativo e a senha do aplicativo.

1. Localize a linha `app.use('/', indexRouter);` no `./app.js`. Insira o código a seguir **antes** dessa linha.

    ```javascript
    // Initialize passport
    app.use(passport.initialize());
    app.use(passport.session());
    ```

1. Crie um novo arquivo no `./routes` diretório chamado `auth.js` e adicione o código a seguir.

    ```javascript
    var express = require('express');
    var passport = require('passport');
    var router = express.Router();

    /* GET auth callback. */
    router.get('/signin',
      function  (req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            prompt: 'login',
            failureRedirect: '/',
            failureFlash: true,
            successRedirect: '/'
          }
        )(req,res,next);
      }
    );

    router.post('/callback',
      function(req, res, next) {
        passport.authenticate('azuread-openidconnect',
          {
            response: res,
            failureRedirect: '/',
            failureFlash: true
          }
        )(req,res,next);
      },
      function(req, res) {
        // TEMPORARY!
        // Flash the access token for testing purposes
        req.flash('error_msg', {message: 'Access token', debug: req.user.accessToken});
        res.redirect('/');
      }
    );

    router.get('/signout',
      function(req, res) {
        req.session.destroy(function(err) {
          req.logout();
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    Isso define um roteador com três rotas: `signin`, `callback`e `signout`.

    A `signin` rota chama o `passport.authenticate` método, fazendo com que o aplicativo Redirecione para a página de logon do Azure.

    A `callback` rota é onde o Azure é redirecionado após a conclusão da entrada. O código chama o `passport.authenticate` método novamente, fazendo com `passport-azure-ad` que a estratégia solicite um token de acesso. Após o token ser obtido, o próximo manipulador é chamado, que redireciona de volta para a Home Page com o token de acesso no valor de erro temporário. Usaremos isso para verificar se a entrada está funcionando antes de prosseguir. Antes de `./routes/auth.js`testarmos, precisamos configurar o aplicativo expresso para usar o novo roteador.

    O `signout` método registra o usuário e destrói a sessão.

1. Abra `./app.js` e insira o código a **before** seguir antes `var app = express();` da linha.

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. Insira o código a **after** seguir após `app.use('/', indexRouter);` a linha.

    ```javascript
    app.use('/auth', authRouter);
    ```

Inicie o servidor e navegue até `https://localhost:3000`. Clique no botão entrar e você deverá ser redirecionado para `https://login.microsoftonline.com`o. Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas. O navegador redireciona para o aplicativo, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

1. Crie um novo arquivo na raiz do projeto chamado `graph.js` e adicione o código a seguir.

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client.api('/me').get();
        return user;
      }
    };

    function getAuthenticatedClient(accessToken) {
      // Initialize Graph client
      const client = graph.Client.init({
        // Use the provided access token to authenticate
        // requests
        authProvider: (done) => {
          done(null, accessToken);
        }
      });

      return client;
    }
    ```

    Isso exporta a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar `/me` o ponto de extremidade e retornar o resultado.

1. Abra `/app.js` e adicione as seguintes `require` instruções à parte superior do arquivo.

    ```javascript
    var graph = require('./graph');
    ```

1. Substitua a função `signInComplete` existente pelo código seguinte.

    ```javascript
    async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
      if (!profile.oid) {
        return done(new Error("No OID found in user profile."));
      }

      try{
        const user = await graph.getUserDetails(accessToken);

        if (user) {
          // Add properties to profile
          profile['email'] = user.mail ? user.mail : user.userPrincipalName;
        }
      } catch (err) {
        return done(err);
      }

      // Save the profile and tokens in user storage
      users[profile.oid] = { profile, accessToken };
      return done(null, users[profile.oid]);
    }
    ```

    O novo código atualiza o `profile` fornecido pelo Passport para adicionar uma `email` Propriedade, usando os dados retornados pelo Microsoft Graph.

1. Adicione o seguinte **após** a `app.use(passport.session());` linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="AddProfileSnippet":::

    Este código carrega o perfil do usuário na `locals` propriedade da resposta. Isso o tornará disponível para todos os modos de exibição no aplicativo.

## <a name="storing-the-tokens"></a>Armazenar tokens

Agora que você pode obter tokens, é hora de implementar uma maneira de armazená-los no aplicativo. No momento, o aplicativo está armazenando o token de acesso bruto no armazenamento do usuário na memória. Como este é um aplicativo de exemplo, por questões de simplicidade, você continuará a armazená-los. Um aplicativo real usaria uma solução de armazenamento segura mais confiável, como um banco de dados.

No entanto, armazenar apenas o token de acesso não permite que você verifique a validade ou atualize o token. Para habilitar isso, atualize o exemplo para encapsular os tokens em um `AccessToken` objeto da `simple-oauth2` biblioteca.

1. Abra `./app.js` e adicione o código a **before** seguir antes `signInComplete` da função.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="ConfigureOAuth2Snippet":::

1. Substitua a função `signInComplete` existente pelo seguinte.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SignInCompleteSnippet" highlight="17-18, 21":::

1. Substitua a rota de retorno de `./routes/auth.js` chamada existente em com o seguinte.

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackRouteSnippet" highlight="17-18":::

1. Reinicie o servidor e vá pelo processo de entrada. Você deve terminar de volta na Home Page, mas a interface do usuário deve ser alterada para indicar que você está conectado.

    ![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

1. Clique no avatar do usuário no canto superior direito para **acessar o link sair.** Clicar **em sair** redefine a sessão e retorna à Home Page.

    ![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>Atualizando tokens

Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API. Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token é de vida curta. O token expira uma hora após sua emissão. É onde o token de atualização se torna útil. O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.

1. Crie um novo arquivo na raiz do projeto nomeado `tokens.js` para armazenar funções de gerenciamento de token. Adicione o código a seguir.

    :::code language="javascript" source="../demo/graph-tutorial/tokens.js" id="TokensSnippet":::

Este método primeiro verifica se o token de acesso expirou ou está prestes a expirar. Se for, ele usará o token de atualização para obter novos tokens, atualizará o cache e retornará o novo token de acesso. Você usará esse método sempre que precisar obter o token de acesso fora de armazenamento.
