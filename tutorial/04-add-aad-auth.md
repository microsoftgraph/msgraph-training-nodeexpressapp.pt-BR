<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="84c00-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="84c00-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="84c00-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84c00-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="84c00-103">Nesta etapa, você integrará a biblioteca do [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84c00-103">In this step you will integrate the [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) library into the application.</span></span>

<span data-ttu-id="84c00-104">Crie um novo arquivo chamado `.env` arquivo na raiz do seu aplicativo e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="84c00-104">Create a new file named `.env` file in the root of your application, and add the following code.</span></span>

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

<span data-ttu-id="84c00-105">Substitua `YOUR APP ID HERE` pela ID do aplicativo do portal de registro do aplicativo e substitua `YOUR APP SECRET HERE` pela senha gerada.</span><span class="sxs-lookup"><span data-stu-id="84c00-105">Replace `YOUR APP ID HERE` with the application ID from the Application Registration Portal, and replace `YOUR APP SECRET HERE` with the password you generated.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84c00-106">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `.env` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo e sua senha.</span><span class="sxs-lookup"><span data-stu-id="84c00-106">If you're using source control such as git, now would be a good time to exclude the `.env` file from source control to avoid inadvertently leaking your app ID and password.</span></span>

<span data-ttu-id="84c00-107">Abra `./app.js` e adicione a seguinte linha na parte superior do arquivo para carregar o `.env` arquivo.</span><span class="sxs-lookup"><span data-stu-id="84c00-107">Open `./app.js` and add the following line to the top of the file to load the `.env` file.</span></span>

```js
require('dotenv').config();
```

## <a name="implement-sign-in"></a><span data-ttu-id="84c00-108">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="84c00-108">Implement sign-in</span></span>

<span data-ttu-id="84c00-109">Localize a linha `var indexRouter = require('./routes/index');` no `./app.js`.</span><span class="sxs-lookup"><span data-stu-id="84c00-109">Locate the line `var indexRouter = require('./routes/index');` in `./app.js`.</span></span> <span data-ttu-id="84c00-110">Insira o código a seguir **antes** dessa linha.</span><span class="sxs-lookup"><span data-stu-id="84c00-110">Insert the following code **before** that line.</span></span>

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

<span data-ttu-id="84c00-111">Este código inicializa a biblioteca [Passport. js](http://www.passportjs.org/) para usar a `passport-azure-ad` biblioteca e a configura com a ID do aplicativo e a senha do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84c00-111">This code initializes the [Passport.js](http://www.passportjs.org/) library to use the `passport-azure-ad` library, and configures it with the app ID and password for the app.</span></span>

<span data-ttu-id="84c00-112">Agora passe o `passport` objeto para o aplicativo expresso.</span><span class="sxs-lookup"><span data-stu-id="84c00-112">Now pass the `passport` object to the Express app.</span></span> <span data-ttu-id="84c00-113">Localize a linha `app.use('/', indexRouter);` no `./app.js`.</span><span class="sxs-lookup"><span data-stu-id="84c00-113">Locate the line `app.use('/', indexRouter);` in `./app.js`.</span></span> <span data-ttu-id="84c00-114">Insira o código a seguir **antes** dessa linha.</span><span class="sxs-lookup"><span data-stu-id="84c00-114">Insert the following code **before** that line.</span></span>

```js
// Initialize passport
app.use(passport.initialize());
app.use(passport.session());
```

<span data-ttu-id="84c00-115">Crie um novo arquivo no `./routes` diretório chamado `auth.js` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="84c00-115">Create a new file in the `./routes` directory named `auth.js` and add the following code.</span></span>

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

<span data-ttu-id="84c00-116">Isso define um roteador com três rotas: `signin`, `callback`e `signout`.</span><span class="sxs-lookup"><span data-stu-id="84c00-116">This defines a router with three routes: `signin`, `callback`, and `signout`.</span></span>

<span data-ttu-id="84c00-117">A `signin` rota chama o `passport.authenticate` método, fazendo com que o aplicativo Redirecione para a página de logon do Azure.</span><span class="sxs-lookup"><span data-stu-id="84c00-117">The `signin` route calls the `passport.authenticate` method, causing the app to redirect to the Azure login page.</span></span>

<span data-ttu-id="84c00-118">A `callback` rota é onde o Azure é redirecionado após a conclusão da entrada.</span><span class="sxs-lookup"><span data-stu-id="84c00-118">The `callback` route is where Azure redirects after the signin is complete.</span></span> <span data-ttu-id="84c00-119">O código chama o `passport.authenticate` método novamente, fazendo com `passport-azure-ad` que a estratégia solicite um token de acesso.</span><span class="sxs-lookup"><span data-stu-id="84c00-119">The code calls the `passport.authenticate` method again, causing the `passport-azure-ad` strategy to request an access token.</span></span> <span data-ttu-id="84c00-120">Após o token ser obtido, o próximo manipulador é chamado, que redireciona de volta para a Home Page com o token de acesso no valor de erro temporário.</span><span class="sxs-lookup"><span data-stu-id="84c00-120">Once the token is obtained, the next handler is called, which redirects back to the home page with the access token in the temporary error value.</span></span> <span data-ttu-id="84c00-121">Usaremos isso para verificar se a entrada está funcionando antes de prosseguir.</span><span class="sxs-lookup"><span data-stu-id="84c00-121">We'll use this to verify that our sign-in is working before moving on.</span></span> <span data-ttu-id="84c00-122">Antes de `./routes/auth.js`testarmos, precisamos configurar o aplicativo expresso para usar o novo roteador.</span><span class="sxs-lookup"><span data-stu-id="84c00-122">Before we test, we need to configure the Express app to use the new router from `./routes/auth.js`.</span></span>

<span data-ttu-id="84c00-123">O `signout` método registra o usuário e destrói a sessão.</span><span class="sxs-lookup"><span data-stu-id="84c00-123">The `signout` method logs the user out and destroys the session.</span></span>

<span data-ttu-id="84c00-124">Insira o código a \*\*\*\* seguir antes `var app = express();` da linha.</span><span class="sxs-lookup"><span data-stu-id="84c00-124">Insert the following code **before** the `var app = express();` line.</span></span>

```js
var authRouter = require('./routes/auth');
```

<span data-ttu-id="84c00-125">Em seguida, insira o \*\*\*\* código a `app.use('/', indexRouter);` seguir após a linha.</span><span class="sxs-lookup"><span data-stu-id="84c00-125">Then insert the following code **after** the `app.use('/', indexRouter);` line.</span></span>

```js
app.use('/auth', authRouter);
```

<span data-ttu-id="84c00-126">Inicie o servidor e navegue até `https://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="84c00-126">Start the server and browse to `https://localhost:3000`.</span></span> <span data-ttu-id="84c00-127">Clique no botão entrar e você deverá ser redirecionado para `https://login.microsoftonline.com`o.</span><span class="sxs-lookup"><span data-stu-id="84c00-127">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="84c00-128">Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="84c00-128">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="84c00-129">O navegador redireciona para o aplicativo, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="84c00-129">The browser redirects to the app, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="84c00-130">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="84c00-130">Get user details</span></span>

<span data-ttu-id="84c00-131">Comece criando um novo arquivo para conter todas as chamadas do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84c00-131">Start by creating a new file to hold all of your Microsoft Graph calls.</span></span> <span data-ttu-id="84c00-132">Crie um novo arquivo na raiz do projeto chamado `graph.js` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="84c00-132">Create a new file in the root of the project named `graph.js` and add the following code.</span></span>

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

<span data-ttu-id="84c00-133">Isso exporta a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar `/me` o ponto de extremidade e retornar o resultado.</span><span class="sxs-lookup"><span data-stu-id="84c00-133">This exports the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

<span data-ttu-id="84c00-134">Atualize o `signInComplete` método no `/app.js` para chamar essa função.</span><span class="sxs-lookup"><span data-stu-id="84c00-134">Update the `signInComplete` method in `/app.js` to call this function.</span></span> <span data-ttu-id="84c00-135">Primeiro, adicione as seguintes `require` instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="84c00-135">First, add the following `require` statements to the top of the file.</span></span>

```js
var graph = require('./graph');
```

<span data-ttu-id="84c00-136">Substitua a função `signInComplete` existente pelo código seguinte.</span><span class="sxs-lookup"><span data-stu-id="84c00-136">Replace the existing `signInComplete` function with the following code.</span></span>

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

<span data-ttu-id="84c00-137">O novo código atualiza o `profile` fornecido pelo Passport para adicionar uma `email` Propriedade, usando os dados retornados pelo Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84c00-137">The new code updates the `profile` provided by Passport to add an `email` property, using the data returned by Microsoft Graph.</span></span>

<span data-ttu-id="84c00-138">Por fim, adicione o `./app.js` código para carregar o perfil de usuário `locals` na propriedade da resposta.</span><span class="sxs-lookup"><span data-stu-id="84c00-138">Finally, add code to `./app.js` to load the user profile into the `locals` property of the response.</span></span> <span data-ttu-id="84c00-139">Isso o tornará disponível para todos os modos de exibição no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84c00-139">This will make it available to all of the views in the app.</span></span>

<span data-ttu-id="84c00-140">Adicione o seguinte **após** a `app.use(passport.session());` linha.</span><span class="sxs-lookup"><span data-stu-id="84c00-140">Add the following **after** the `app.use(passport.session());` line.</span></span>

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

## <a name="storing-the-tokens"></a><span data-ttu-id="84c00-141">Armazenar tokens</span><span class="sxs-lookup"><span data-stu-id="84c00-141">Storing the tokens</span></span>

<span data-ttu-id="84c00-142">Agora que você pode obter tokens, é hora de implementar uma maneira de armazená-los no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84c00-142">Now that you can get tokens, it's time to implement a way to store them in the app.</span></span> <span data-ttu-id="84c00-143">No momento, o aplicativo está armazenando o token de acesso bruto no armazenamento do usuário na memória.</span><span class="sxs-lookup"><span data-stu-id="84c00-143">Currently, the app is storing the raw access token in the in-memory user storage.</span></span> <span data-ttu-id="84c00-144">Como este é um aplicativo de exemplo, por questões de simplicidade, você continuará a armazená-los.</span><span class="sxs-lookup"><span data-stu-id="84c00-144">Since this is a sample app, for simplicity's sake, you'll continue to store them there.</span></span> <span data-ttu-id="84c00-145">Um aplicativo real usaria uma solução de armazenamento segura mais confiável, como um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="84c00-145">A real-world app would use a more reliable secure storage solution, like a database.</span></span>

<span data-ttu-id="84c00-146">No entanto, armazenar apenas o token de acesso não permite que você verifique a validade ou atualize o token.</span><span class="sxs-lookup"><span data-stu-id="84c00-146">However, storing just the access token doesn't allow you to check expiration or refresh the token.</span></span> <span data-ttu-id="84c00-147">Para habilitar isso, atualize o exemplo para encapsular os tokens em um `AccessToken` objeto da `simple-oauth2` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="84c00-147">In order to enable that, update the sample to wrap the tokens in an `AccessToken` object from the `simple-oauth2` library.</span></span>

<span data-ttu-id="84c00-148">Primeiro, em `./app.js`, adicione o código a \*\*\*\* seguir antes `signInComplete` da função.</span><span class="sxs-lookup"><span data-stu-id="84c00-148">First, in `./app.js`, add the following code **before** the `signInComplete` function.</span></span>

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

<span data-ttu-id="84c00-149">Em seguida, atualize `signInComplete` a função para criar `AccessToken` um dos tokens brutos passados e armazene-os no armazenamento do usuário.</span><span class="sxs-lookup"><span data-stu-id="84c00-149">Then, update the `signInComplete` function to create an `AccessToken` from the raw tokens passed in and store that in the user storage.</span></span> <span data-ttu-id="84c00-150">Substitua a função `signInComplete` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="84c00-150">Replace the existing `signInComplete` function with the following.</span></span>

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

<span data-ttu-id="84c00-151">Atualize a `callback` rota em `./routes/auth.js` para remover a `req.flash` linha com o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="84c00-151">Update the `callback` route in `./routes/auth.js` to remove the `req.flash` line with the access token.</span></span> <span data-ttu-id="84c00-152">A `callback` rota deve ter a seguinte aparência.</span><span class="sxs-lookup"><span data-stu-id="84c00-152">The `callback` route should look like the following.</span></span>

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

<span data-ttu-id="84c00-153">Reinicie o servidor e vá pelo processo de entrada.</span><span class="sxs-lookup"><span data-stu-id="84c00-153">Restart the server and go through the sign-in process.</span></span> <span data-ttu-id="84c00-154">Você deve terminar de volta na Home Page, mas a interface do usuário deve ser alterada para indicar que você está conectado.</span><span class="sxs-lookup"><span data-stu-id="84c00-154">You should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

<span data-ttu-id="84c00-156">Clique no avatar do usuário no canto superior direito para **acessar o link sair.**</span><span class="sxs-lookup"><span data-stu-id="84c00-156">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="84c00-157">Clicar **em sair** redefine a sessão e retorna à Home Page.</span><span class="sxs-lookup"><span data-stu-id="84c00-157">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="refreshing-tokens"></a><span data-ttu-id="84c00-159">Atualizando tokens</span><span class="sxs-lookup"><span data-stu-id="84c00-159">Refreshing tokens</span></span>

<span data-ttu-id="84c00-160">Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="84c00-160">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="84c00-161">Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="84c00-161">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="84c00-162">No entanto, esse token é de vida curta.</span><span class="sxs-lookup"><span data-stu-id="84c00-162">However, this token is short-lived.</span></span> <span data-ttu-id="84c00-163">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="84c00-163">The token expires an hour after it is issued.</span></span> <span data-ttu-id="84c00-164">É onde o token de atualização se torna útil.</span><span class="sxs-lookup"><span data-stu-id="84c00-164">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="84c00-165">O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.</span><span class="sxs-lookup"><span data-stu-id="84c00-165">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="84c00-166">Para gerenciar isso, crie um novo arquivo na raiz do projeto chamado `tokens.js` para manter as funções de gerenciamento de token.</span><span class="sxs-lookup"><span data-stu-id="84c00-166">To manage this, create a new file in the root of the project named `tokens.js` to hold token management functions.</span></span> <span data-ttu-id="84c00-167">Adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="84c00-167">Add the following code.</span></span>

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

<span data-ttu-id="84c00-168">Este método primeiro verifica se o token de acesso expirou ou está prestes a expirar.</span><span class="sxs-lookup"><span data-stu-id="84c00-168">This method first checks if the access token is expired or close to expiring.</span></span> <span data-ttu-id="84c00-169">Se for, ele usará o token de atualização para obter novos tokens, atualizará o cache e retornará o novo token de acesso.</span><span class="sxs-lookup"><span data-stu-id="84c00-169">If it is, then it uses the refresh token to get new tokens, then updates the cache and returns the new access token.</span></span> <span data-ttu-id="84c00-170">Você usará esse método sempre que precisar obter o token de acesso fora de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="84c00-170">You'll use this method whenever you need to get the access token out of storage.</span></span>
