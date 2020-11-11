<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a biblioteca de [nós MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) ao aplicativo.

1. Crie um novo arquivo chamado **. env** na raiz do seu aplicativo e adicione o código a seguir.

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    Substitua `YOUR_CLIENT_SECRET_HERE` pela ID do aplicativo do portal de registro do aplicativo e substitua `YOUR_APP_SECRET_HERE` pelo segredo do cliente gerado.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o arquivo **. env** do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo e sua senha.

1. Abra **./app.js** e adicione a seguinte linha à parte superior do arquivo para carregar o arquivo **. env** .

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>Implementar logon

1. Localize a linha `var indexRouter = require('./routes/index');` em **./app.js**. Insira o código a seguir **antes** dessa linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    Este código inicializa a biblioteca MSAL com a ID do aplicativo e a senha do aplicativo.

1. Crie um novo arquivo no diretório **./routes** chamado **auth.js** e adicione o código a seguir.

    ```javascript
    var router = require('express-promise-router')();

    /* GET auth callback. */
    router.get('/signin',
      async function (req, res) {
        const urlParameters = {
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const authUrl = await req.app.locals
            .msalClient.getAuthCodeUrl(urlParameters);
          res.redirect(authUrl);
        }
        catch (error) {
          console.log(`Error: ${error}`);
          req.flash('error_msg', {
            message: 'Error getting auth URL',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
          res.redirect('/');
        }
      }
    );

    router.get('/callback',
      async function(req, res) {
        const tokenRequest = {
          code: req.query.code,
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI
        };

        try {
          const response = await req.app.locals
            .msalClient.acquireTokenByCode(tokenRequest);

          // TEMPORARY!
          // Flash the access token for testing purposes
          req.flash('error_msg', {
            message: 'Access token',
            debug: response.accessToken
          });
        } catch (error) {
          req.flash('error_msg', {
            message: 'Error completing authentication',
            debug: JSON.stringify(error, Object.getOwnPropertyNames(error))
          });
        }

        res.redirect('/');
      }
    );

    router.get('/signout',
      async function(req, res) {
        // Sign out
        if (req.session.userId) {
          // Look up the user's account in the cache
          const accounts = await req.app.locals.msalClient
            .getTokenCache()
            .getAllAccounts();

          const userAccount = accounts.find(a => a.homeAccountId === req.session.userId);

          // Remove the account
          if (userAccount) {
            req.app.locals.msalClient
              .getTokenCache()
              .removeAccount(userAccount);
          }
        }

        // Destroy the user's session
        req.session.destroy(function (err) {
          res.redirect('/');
        });
      }
    );

    module.exports = router;
    ```

    Isso define um roteador com três rotas: `signin` , `callback` e `signout` .

    A `signin` rota chama a `getAuthCodeUrl` função para gerar a URL de logon e redireciona o navegador para essa URL.

    A `callback` rota é onde o Azure é redirecionado após a conclusão da entrada. O código chama a `acquireTokenByCode` função para trocar o código de autorização para um token de acesso. Após o token ser obtido, ele redireciona de volta para a Home Page com o token de acesso no valor de erro temporário. Usaremos isso para verificar se a entrada está funcionando antes de prosseguir. Antes de testar, precisamos configurar o aplicativo expresso para usar o novo roteador de **./routes/auth.js**.

    O `signout` método registra o usuário e destrói a sessão.

1. Abra **./app.js** e insira o código a seguir **antes** da `var app = express();` linha.

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. Insira o código a seguir **após** a `app.use('/', indexRouter);` linha.

    ```javascript
    app.use('/auth', authRouter);
    ```

Inicie o servidor e navegue até `https://localhost:3000` . Clique no botão entrar e você deverá ser redirecionado para o `https://login.microsoftonline.com` . Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas. O navegador redireciona para o aplicativo, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

1. Crie um novo arquivo na raiz do projeto chamado **graph.js** e adicione o código a seguir.

    ```javascript
    var graph = require('@microsoft/microsoft-graph-client');
    require('isomorphic-fetch');

    module.exports = {
      getUserDetails: async function(accessToken) {
        const client = getAuthenticatedClient(accessToken);

        const user = await client
          .api('/me')
          .select('displayName,mail,mailboxSettings,userPrincipalName')
          .get();
        return user;
      },
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

    Isso exporta a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar o `/me` ponto de extremidade e retornar o resultado.

1. Abra **./routes/auth.js** e adicione as seguintes `require` instruções à parte superior do arquivo.

    ```javascript
    var graph = require('../graph');
    ```

1. Substitua a rota de retorno de chamada existente pelo código a seguir.

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    O novo código salva a ID da conta do usuário na sessão, obtém os detalhes do usuário do Microsoft Graph e o salva no armazenamento do usuário do aplicativo.

1. Reinicie o servidor e vá pelo processo de entrada. Você deve terminar de volta na Home Page, mas a interface do usuário deve ser alterada para indicar que você está conectado.

    ![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

1. Clique no avatar do usuário no canto superior direito para **acessar o link sair.** Clicar **em sair** redefine a sessão e retorna à Home Page.

    ![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Armazenar e atualizar tokens

Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API. Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token é de vida curta. O token expira uma hora após sua emissão. É onde o token de atualização se torna útil. A especificação OAuth introduz um token de atualização, que permite ao aplicativo solicitar um novo token de acesso sem exigir que o usuário entre novamente.

Como o aplicativo está usando o pacote MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização. O aplicativo usa o cache de token de MSAL no nó padrão, que é suficiente para um aplicativo de exemplo. Os aplicativos de produção devem fornecer seus próprios [plug-ins de cache](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) para serializar o cache de token em uma mídia de armazenamento segura e confiável.
