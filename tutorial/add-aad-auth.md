<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a biblioteca do [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) ao aplicativo.

Crie um novo arquivo chamado `.env` arquivo na raiz do seu aplicativo e adicione o código a seguir.

```text
OAUTH_APP_ID=YOUR_APP_ID_HERE
OAUTH_APP_PASSWORD=YOUR_APP_PASSWORD_HERE
OAUTH_REDIRECT_URI=http://localhost:3000/auth/callback
OAUTH_SCOPES='profile offline_access user.read calendars.read'
OAUTH_AUTHORITY=https://login.microsoftonline.com/common
OAUTH_ID_METADATA=/v2.0/.well-known/openid-configuration
OAUTH_AUTHORIZE_ENDPOINT=/oauth2/v2.0/authorize
OAUTH_TOKEN_ENDPOINT=/oauth2/v2.0/token
```

Substitua `YOUR APP ID HERE` pela ID do aplicativo do portal de registro do aplicativo e substitua `YOUR APP SECRET HERE` pela senha gerada.

> [!IMPORTANT]
> Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `.env` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo e sua senha.

Abra `./app.js` e adicione a seguinte linha na parte superior do arquivo para carregar o `.env` arquivo.

```js
require('dotenv').config();
```

## <a name="implement-sign-in"></a>Implementar logon

Localize a linha `var indexRouter = require('./routes/index');` no `./app.js`. Insira o código a seguir **antes** dessa linha.

```js
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
    return done(new Error("No OID found in user profile."), null);
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

Agora passe o `passport` objeto para o aplicativo expresso. Localize a linha `app.use('/', indexRouter);` no `./app.js`. Insira o código a seguir **antes** dessa linha.

```js
// Initialize passport
app.use(passport.initialize());
app.use(passport.session());
```

Crie um novo arquivo no `./routes` diretório chamado `auth.js` e adicione o código a seguir.

```js
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
        failureFlash: true
      }
    )(req,res,next);
  },
  function(req, res) {
    res.redirect('/');
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

Insira o código a **** seguir antes `var app = express();` da linha.

```js
var authRouter = require('./routes/auth');
```

Em seguida, insira o **** código a `app.use('/', indexRouter);` seguir após a linha.

```js
app.use('/auth', authRouter);
```

Inicie o servidor e navegue até `https://localhost:3000`. Clique no botão entrar e você deverá ser redirecionado para `https://login.microsoftonline.com`o. Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas. O navegador redireciona para o aplicativo, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

Comece criando um novo arquivo para conter todas as chamadas do Microsoft Graph. Crie um novo arquivo na raiz do projeto chamado `graph.js` e adicione o código a seguir.

```js
var graph = require('@microsoft/microsoft-graph-client');

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

Atualize o `signInComplete` método no `/app.s` para chamar essa função. Primeiro, adicione as seguintes `require` instruções à parte superior do arquivo.

```js
var graph = require('./graph');
```

Substitua a função `signInComplete` existente pelo código seguinte.

```js
async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
  if (!profile.oid) {
    return done(new Error("No OID found in user profile."), null);
  }

  try{
    const user = await graph.getUserDetails(accessToken);

    if (user) {
      // Add properties to profile
      profile['email'] = user.mail ? user.mail : user.userPrincipalName;
    }
  } catch (err) {
    done(err, null);
  }

  // Save the profile and tokens in user storage
  users[profile.oid] = { profile, accessToken };
  return done(null, users[profile.oid]);
}
```

O novo código atualiza o `profile` fornecido pelo Passport para adicionar uma `email` Propriedade, usando os dados retornados pelo Microsoft Graph.

Por fim, adicione o `./app.js` código para carregar o perfil de usuário `locals` na propriedade da resposta. Isso o tornará disponível para todos os modos de exibição no aplicativo.

Adicione o seguinte **após** a `app.use(passport.session());` linha.

```js
app.use(function(req, res, next) {
  // Set the authenticated user in the
  // template locals
  if (req.user) {
    res.locals.user = req.user.profile;
  }
  next();
});
```

## <a name="storing-the-tokens"></a>Armazenar tokens

Agora que você pode obter tokens, é hora de implementar uma maneira de armazená-los no aplicativo. No momento, o aplicativo está armazenando o token de acesso bruto no armazenamento do usuário na memória. Como este é um aplicativo de exemplo, por questões de simplicidade, você continuará a armazená-los. Um aplicativo real usaria uma solução de armazenamento segura mais confiável, como um banco de dados.

No entanto, armazenar apenas o token de acesso não permite que você verifique a validade ou atualize o token. Para habilitar isso, atualize o exemplo para encapsular os tokens em um `AccessToken` objeto da `simple-oauth2` biblioteca.

Primeiro, em `./app.js`, adicione o código a **** seguir antes `signInComplete` da função.

```js
// Configure simple-oauth2
const oauth2 = require('simple-oauth2').create({
  client: {
    id: process.env.OAUTH_APP_ID,
    secret: process.env.OAUTH_APP_PASSWORD
  },
  auth: {
    tokenHost: process.env.OAUTH_AUTHORITY,
    authorizePath: process.env.OAUTH_AUTHORIZE_ENDPOINT,
    tokenPath: process.env.OAUTH_TOKEN_ENDPOINT
  }
});
```

Em seguida, atualize `signInComplete` a função para criar `AccessToken` um dos tokens brutos passados e armazene-os no armazenamento do usuário. Substitua a função `signInComplete` existente pelo seguinte.

```js
async function signInComplete(iss, sub, profile, accessToken, refreshToken, params, done) {
  if (!profile.oid) {
    return done(new Error("No OID found in user profile."), null);
  }

  try{
    const user = await graph.getUserDetails(accessToken);

    if (user) {
      // Add properties to profile
      profile['email'] = user.mail ? user.mail : user.userPrincipalName;
    }
  } catch (err) {
    done(err, null);
  }

  // Create a simple-oauth2 token from raw tokens
  let oauthToken = oauth2.accessToken.create(params);

  // Save the profile and tokens in user storage
  users[profile.oid] = { profile, oauthToken };
  return done(null, users[profile.oid]);
}
```

Atualize a `callback` rota em `./routes/auth.js` para remover a `req.flash` linha com o token de acesso. A `callback` rota deve ter a seguinte aparência.

```js
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
    res.redirect('/');
  }
);
```

Reinicie o servidor e vá pelo processo de entrada. Você deve terminar de volta na Home Page, mas a interface do usuário deve ser alterada para indicar que você está conectado.

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

Clique no avatar do usuário no canto superior direito para acessar o **** link sair. Clicar **** em sair redefine a sessão e retorna à Home Page.

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a>Atualizando tokens

Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API. Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token é de vida curta. O token expira uma hora após sua emissão. É onde o token de atualização se torna útil. O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.

Para gerenciar isso, crie um novo arquivo na raiz do projeto chamado `tokens.js` para manter as funções de gerenciamento de token. Adicione o código a seguir.

```js
module.exports = {
  getAccessToken: async function(req) {
    if (req.user) {
      // Get the stored token
      var storedToken = req.user.oauthToken;

      if (storedToken) {
        if (storedToken.expired()) {
          // refresh token
          var newToken = await storedToken.refresh();

          // Update stored token
          req.user.oauthToken = newToken;
          return newToken.token.access_token;
        }

        // Token still valid, just return it
        return storedToken.token.access_token;
      }
    }
  }
};
```

Este método primeiro verifica se o token de acesso expirou ou está prestes a expirar. Se for, ele usará o token de atualização para obter novos tokens, atualizará o cache e retornará o novo token de acesso. Você usará esse método sempre que precisar obter o token de acesso fora de armazenamento.