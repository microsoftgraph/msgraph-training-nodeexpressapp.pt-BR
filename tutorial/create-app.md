<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="78b51-101">Neste exercício, você usará o [Express](http://expressjs.com/) para compilar um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="78b51-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span> <span data-ttu-id="78b51-102">Se você ainda não tiver o gerador Express instalado, você pode instalá-lo a partir da interface de linha de comando (CLI) com o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="78b51-102">If you don't already have the Express generator installed, you can install it from your command-line interface (CLI) with the following command.</span></span>

```Shell
npm install express-generator -g
```

<span data-ttu-id="78b51-103">Abra sua CLI, navegue até um diretório onde você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo expresso que usa o [guias](http://handlebarsjs.com/) como o mecanismo de renderização.</span><span class="sxs-lookup"><span data-stu-id="78b51-103">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

```Shell
express --hbs graph-tutorial
```

<span data-ttu-id="78b51-104">O gerador Express cria um novo diretório chamado `graph-tutorial` e estruturará um aplicativo expresso.</span><span class="sxs-lookup"><span data-stu-id="78b51-104">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span> <span data-ttu-id="78b51-105">Navegue até este novo diretório e insira o comando a seguir para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="78b51-105">Navigate to this new directory and enter the following command to install dependencies.</span></span>

```Shell
npm install
```

<span data-ttu-id="78b51-106">Quando o comando for concluído, use o comando a seguir para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="78b51-106">Once that command completes, use the following command to start a local web server.</span></span>

```Shell
npm start
```

<span data-ttu-id="78b51-107">Abra o navegador e vá até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="78b51-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="78b51-108">Se tudo estiver funcionando, você verá uma mensagem de "bem-vindo ao Express".</span><span class="sxs-lookup"><span data-stu-id="78b51-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="78b51-109">Se você não vir essa mensagem, confira o [Guia de introdução ao Express](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="78b51-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

<span data-ttu-id="78b51-110">Antes de prosseguir, instale algumas Gems adicionais que serão usadas posteriormente:</span><span class="sxs-lookup"><span data-stu-id="78b51-110">Before moving on, install some additional gems that you will use later:</span></span>

- <span data-ttu-id="78b51-111">[dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo. env.</span><span class="sxs-lookup"><span data-stu-id="78b51-111">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="78b51-112">[](https://github.com/moment/moment/) tempo para formatar valores de data/hora.</span><span class="sxs-lookup"><span data-stu-id="78b51-112">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="78b51-113">[Connect –](https://github.com/jaredhanson/connect-flash) mensagens de erro do flash para Flash no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78b51-113">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="78b51-114">[Express-Session](https://github.com/expressjs/session) para armazenar valores em uma sessão do lado do servidor na memória.</span><span class="sxs-lookup"><span data-stu-id="78b51-114">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="78b51-115">[Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) para autenticar e obter tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="78b51-115">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="78b51-116">[oauth2 simples](https://github.com/lelylan/simple-oauth2) para gerenciamento de token.</span><span class="sxs-lookup"><span data-stu-id="78b51-116">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="78b51-117">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="78b51-117">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="78b51-118">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="78b51-118">Run the following command in your CLI.</span></span>

```Shell
npm install dotenv@6.1.0 moment@2.22.2 connect-flash@0.1.1 express-session@1.15.6
npm install passport-azure-ad@4.0.0 simple-oauth2@2.2.1 @microsoft/microsoft-graph-client@1.3.0
```

<span data-ttu-id="78b51-119">Agora atualize o aplicativo para usar o `connect-flash` e `express-session` middleware.</span><span class="sxs-lookup"><span data-stu-id="78b51-119">Now update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="78b51-120">Abra o `./app.js` arquivo e adicione a seguinte `require` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="78b51-120">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

```js
var session = require('express-session');
var flash = require('connect-flash');
```

<span data-ttu-id="78b51-121">Adicione o seguinte código imediatamente após a `var app = express();` linha.</span><span class="sxs-lookup"><span data-stu-id="78b51-121">Add the following code immediately after the `var app = express();` line.</span></span>

```js
// Session middleware
// NOTE: Uses default in-memory session store, which is not
// suitable for production
app.use(session({
  secret: 'your_secret_value_here',
  resave: false,
  saveUninitialized: false,
  unset: 'destroy'
}));

// Flash middleware
app.use(flash());

// Set up local vars for template layout
app.use(function(req, res, next) {
  // Read any flashed errors and save
  // in the response locals
  res.locals.error = req.flash('error_msg');

  // Check for simple error string and
  // convert to layout's expected format
  var errs = req.flash('error');
  for (var i in errs){
    res.locals.error.push({message: 'An error occurred', debug: errs[i]});
  }

  next();
});
```

## <a name="design-the-app"></a><span data-ttu-id="78b51-122">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="78b51-122">Design the app</span></span>

<span data-ttu-id="78b51-123">Comece criando o layout global para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78b51-123">Start by creating the global layout for the app.</span></span> <span data-ttu-id="78b51-124">Abra o `./views/layout.hbs` arquivo e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="78b51-124">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Node.js Graph Tutorial</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css"
      integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.0/css/all.css"
      integrity="sha384-lKuwvrZot6UHsBSfcMvOkWwlCMgc0TaWr+30HWe3a4ltaBwTZhyTEggF5tJv8tbt" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
      integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"
      integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
  </head>

  <body>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <div class="container">
        <a href="/" class="navbar-brand">Node.js Graph Tutorial</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarCollapse"
          aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <a href="/" class="nav-link{{#if active.home}} active{{/if}}">Home</a>
            </li>
            {{#if user}}
              <li class="nav-item" data-turbolinks="false">
                <a href="/calendar" class="nav-link{{#if active.calendar}} active{{/if}}">Calendar</a>
              </li>
            {{/if}}
          </ul>
          <ul class="navbar-nav justify-content-end">
            <li class="nav-item">
              <a class="nav-link" href="https://developer.microsoft.com/graph/docs/concepts/overview" target="_blank">
                <i class="fas fa-external-link-alt mr-1"></i>Docs
              </a>
            </li>
            {{#if user}}
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true"
                  aria-expanded="false">
                  {{#if user.avatar}}
                    <img src="{{ user.avatar }}" class="rounded-circle align-self-center mr-2" style="width: 32px;">
                  {{else}}
                    <i class="far fa-user-circle fa-lg rounded-circle align-self-center mr-2" style="width: 32px;"></i>
                  {{/if}}
                </a>
                <div class="dropdown-menu dropdown-menu-right">
                  <h5 class="dropdown-item-text mb-0">{{ user.displayName }}</h5>
                  <p class="dropdown-item-text text-muted mb-0">{{ user.email }}</p>
                  <div class="dropdown-divider"></div>
                  <a href="/auth/signout" class="dropdown-item">Sign Out</a>
                </div>
              </li>
            {{else}}
              <li class="nav-item">
                <a href="/auth/signin" class="nav-link">Sign In</a>
              </li>
            {{/if}}
          </ul>
        </div>
      </div>
    </nav>
    <main role="main" class="container">
      {{#each error}}
        <div class="alert alert-danger" role="alert">
          <p class="mb-3">{{ this.message }}</p>
          {{#if this.debug }}
            <pre class="alert-pre border bg-light p-2"><code>{{ this.debug }}</code></pre>
          {{/if}}
        </div>
      {{/each}}

      {{{body}}}
    </main>
  </body>
</html>
```

<span data-ttu-id="78b51-125">Este código adiciona a [inicialização](http://getbootstrap.com/) para estilos simples e a [fonte incrível](https://fontawesome.com/) para alguns ícones simples.</span><span class="sxs-lookup"><span data-stu-id="78b51-125">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="78b51-126">Também define um layout global com uma barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="78b51-126">It also defines a global layout with a nav bar.</span></span>

<span data-ttu-id="78b51-127">Agora, `./public/stylesheets/style.css` abra e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="78b51-127">Now open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

```css
body {
  padding-top: 4.5rem;
}

.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

<span data-ttu-id="78b51-128">Agora, atualize a página padrão.</span><span class="sxs-lookup"><span data-stu-id="78b51-128">Now update the default page.</span></span> <span data-ttu-id="78b51-129">Abra o `./views/index.hbs` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="78b51-129">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

```html
<div class="jumbotron">
  <h1>Node.js Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API to access Outlook and OneDrive data from Node.js</p>
  {{#if user}}
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  {{else}}
    <a href="/auth/signin" class="btn btn-primary btn-large">Click here to sign in</a>
  {{/if}}
</div>
```

<span data-ttu-id="78b51-130">Abra o `./routes/index.js` arquivo e substitua o código existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="78b51-130">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  let params = {
    active: { home: true }
  };

  res.render('index', params);
});

module.exports = router;
```

<span data-ttu-id="78b51-131">Salve todas as suas alterações e reinicie o servidor.</span><span class="sxs-lookup"><span data-stu-id="78b51-131">Save all of your changes and restart the server.</span></span> <span data-ttu-id="78b51-132">Agora, o aplicativo deve ser muito diferente.</span><span class="sxs-lookup"><span data-stu-id="78b51-132">Now, the app should look very different.</span></span>

![Uma captura de tela da página inicial reprojetada](./images/create-app-01.png)