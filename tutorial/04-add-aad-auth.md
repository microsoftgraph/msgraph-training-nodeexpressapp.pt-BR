<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a [biblioteca msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) ao aplicativo.

1. Crie um novo arquivo chamado **.env** na raiz do aplicativo e adicione o código a seguir.

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    Substitua pela ID do aplicativo do Portal de Registro de Aplicativo e substitua pelo `YOUR_CLIENT_SECRET_HERE` `YOUR_APP_SECRET_HERE` segredo do cliente gerado.

    > [!IMPORTANT]
    > Se você estiver usando o controle de código-fonte, como git, agora seria um bom momento para excluir o arquivo **.env** do controle de código-fonte para evitar o vazamento inadvertida da ID e da senha do aplicativo.

1. Abra **./app.js** e adicione a seguinte linha na parte superior do arquivo para carregar o **arquivo .env.**

    ```javascript
    require('dotenv').config();
    ```

## <a name="implement-sign-in"></a>Implementar a login

1. Localize a `var app = express();` linha **em ./app.js**. Insira o código a **seguir após** essa linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="MsalInitSnippet":::

    Esse código inicializa a biblioteca msal-node com a ID do aplicativo e a senha do aplicativo.

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

    Isso define um roteador com três rotas: `signin` `callback` e `signout` .

    A `signin` rota chama a função para gerar a URL de `getAuthCodeUrl` logon e redireciona o navegador para essa URL.

    A `callback` rota é para onde o Azure redireciona após a conclusão da assinatura. O código chama a `acquireTokenByCode` função para trocar o código de autorização por um token de acesso. Depois que o token é obtido, ele redireciona de volta para a home page com o token de acesso no valor de erro temporário. Vamos usar isso para verificar se nossa assinatura está funcionando antes de continuar. Antes de testarmos, precisamos configurar o aplicativo Express para usar o novo roteador **de ./routes/auth.js**.

    O `signout` método faz o usuário sair e destruir a sessão.

1. Abra **./app.js** e insira o código a **seguir antes** da `var app = express();` linha.

    ```javascript
    var authRouter = require('./routes/auth');
    ```

1. Insira o código a **seguir após** a `app.use('/', indexRouter);` linha.

    ```javascript
    app.use('/auth', authRouter);
    ```

Inicie o servidor e navegue até `https://localhost:3000` . Clique no botão de login e você deve ser redirecionado `https://login.microsoftonline.com` para. Entre com sua conta da Microsoft e consenta com as permissões solicitadas. O navegador redireciona para o aplicativo, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

1. Crie um novo arquivo na raiz do projeto chamado **graph.js** adicione o código a seguir.

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

    Isso exporta a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar o ponto `/me` de extremidade e retornar o resultado.

1. Abra **./routes/auth.js** e adicione as instruções a `require` seguir na parte superior do arquivo.

    ```javascript
    var graph = require('../graph');
    ```

1. Substitua a rota de retorno de chamada existente pelo código a seguir.

    :::code language="javascript" source="../demo/graph-tutorial/routes/auth.js" id="CallbackSnippet" highlight="13-23":::

    O novo código salva a ID da conta do usuário na sessão, obtém os detalhes do usuário do Microsoft Graph e o salva no armazenamento do usuário do aplicativo.

1. Reinicie o servidor e vá pelo processo de login. Você deve voltar à home page, mas a interface do usuário deve mudar para indicar que você está entrar.

    ![Uma captura de tela da home page após entrar](./images/add-aad-auth-01.png)

1. Clique no avatar do usuário no canto superior direito para acessar o link **Sair.** Clicar em **Sair** redefine a sessão e o retorna para a home page.

    ![Uma captura de tela do menu suspenso com o link Sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Armazenar e atualizar tokens

Neste ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` header de chamadas de API. Esse é o token que permite ao aplicativo acessar o Microsoft Graph em nome do usuário.

No entanto, esse token tem curta duração. O token expira uma hora após sua emissão. É aqui que o token de atualização se torna útil. A especificação OAuth apresenta um token de atualização, que permite ao aplicativo solicitar um novo token de acesso sem exigir que o usuário entre novamente.

Como o aplicativo está usando o pacote msal-node, você não precisa implementar qualquer armazenamento de token ou lógica de atualização. O aplicativo usa o cache de token msal-node na memória padrão, o que é suficiente para um aplicativo de exemplo. Os aplicativos de produção devem fornecer seu próprio [plug-in de](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md) cache para serializar o cache de token em um meio de armazenamento seguro e confiável.
