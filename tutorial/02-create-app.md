<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="411d8-101">Neste exercício, você usará o [Express](http://expressjs.com/) para compilar um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="411d8-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="411d8-102">Abra sua CLI, navegue até um diretório onde você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo expresso que usa o [guias](http://handlebarsjs.com/) como o mecanismo de renderização.</span><span class="sxs-lookup"><span data-stu-id="411d8-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="411d8-103">O gerador Express cria um novo diretório chamado `graph-tutorial` e estruturará um aplicativo expresso.</span><span class="sxs-lookup"><span data-stu-id="411d8-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="411d8-104">Navegue até o `graph-tutorial` diretório e digite o seguinte comando para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="411d8-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="411d8-105">Execute o seguinte comando para atualizar pacotes de nós com vulnerabilidades relatadas.</span><span class="sxs-lookup"><span data-stu-id="411d8-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="411d8-106">Use o seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="411d8-106">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="411d8-107">Abra o navegador e vá até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="411d8-107">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="411d8-108">Se tudo estiver funcionando, você verá uma mensagem de "bem-vindo ao Express".</span><span class="sxs-lookup"><span data-stu-id="411d8-108">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="411d8-109">Se você não vir essa mensagem, confira o [Guia de introdução ao Express](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="411d8-109">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="411d8-110">Instalar pacotes de nós</span><span class="sxs-lookup"><span data-stu-id="411d8-110">Install Node packages</span></span>

<span data-ttu-id="411d8-111">Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:</span><span class="sxs-lookup"><span data-stu-id="411d8-111">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="411d8-112">[dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo. env.</span><span class="sxs-lookup"><span data-stu-id="411d8-112">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="411d8-113">[tempo para formatar](https://github.com/moment/moment/) valores de data/hora.</span><span class="sxs-lookup"><span data-stu-id="411d8-113">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="411d8-114">[Connect –](https://github.com/jaredhanson/connect-flash) mensagens de erro do flash para Flash no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="411d8-114">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="411d8-115">[Express-Session](https://github.com/expressjs/session) para armazenar valores em uma sessão do lado do servidor na memória.</span><span class="sxs-lookup"><span data-stu-id="411d8-115">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="411d8-116">[Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) para autenticar e obter tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="411d8-116">[passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="411d8-117">[oauth2 simples](https://github.com/lelylan/simple-oauth2) para gerenciamento de token.</span><span class="sxs-lookup"><span data-stu-id="411d8-117">[simple-oauth2](https://github.com/lelylan/simple-oauth2) for token management.</span></span>
- <span data-ttu-id="411d8-118">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="411d8-118">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="411d8-119">[isomorphic-FETCH](https://github.com/matthew-andrews/isomorphic-fetch) to metafill o nó de busca.</span><span class="sxs-lookup"><span data-stu-id="411d8-119">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="411d8-120">Um polipreenchimento de busca é necessário para `microsoft-graph-client` a biblioteca.</span><span class="sxs-lookup"><span data-stu-id="411d8-120">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="411d8-121">Consulte o [Microsoft Graph JavaScript Client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="411d8-121">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="411d8-122">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="411d8-122">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
    npm install passport-azure-ad@4.2.1 simple-oauth2@3.3.0 @microsoft/microsoft-graph-client@2.0.0
    ```

    > [!TIP]
    > <span data-ttu-id="411d8-123">Os usuários do Windows podem receber a seguinte mensagem de erro ao tentar instalar esses pacotes no Windows.</span><span class="sxs-lookup"><span data-stu-id="411d8-123">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="411d8-124">Para resolver o erro, execute o seguinte comando para instalar as ferramentas de compilação do Windows usando uma janela de terminal elevada (administrador) que instala as ferramentas de desenvolvimento do VS e o Python.</span><span class="sxs-lookup"><span data-stu-id="411d8-124">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="411d8-125">Atualize o aplicativo para usar o `connect-flash` e `express-session` middleware.</span><span class="sxs-lookup"><span data-stu-id="411d8-125">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="411d8-126">Abra o `./app.js` arquivo e adicione a seguinte `require` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="411d8-126">Open the `./app.js` file and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    var session = require('express-session');
    var flash = require('connect-flash');
    ```

1. <span data-ttu-id="411d8-127">Adicione o seguinte código imediatamente após a `var app = express();` linha.</span><span class="sxs-lookup"><span data-stu-id="411d8-127">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="411d8-128">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="411d8-128">Design the app</span></span>

<span data-ttu-id="411d8-129">Nesta seção, você implementará a interface do usuário para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="411d8-129">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="411d8-130">Abra o `./views/layout.hbs` arquivo e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="411d8-130">Open the `./views/layout.hbs` file and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="411d8-131">Este código adiciona a [inicialização](http://getbootstrap.com/) para estilos simples e a [fonte incrível](https://fontawesome.com/) para alguns ícones simples.</span><span class="sxs-lookup"><span data-stu-id="411d8-131">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="411d8-132">Também define um layout global com uma barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="411d8-132">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="411d8-133">Abra `./public/stylesheets/style.css` e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="411d8-133">Open `./public/stylesheets/style.css` and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="411d8-134">Abra o `./views/index.hbs` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="411d8-134">Open the `./views/index.hbs` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="411d8-135">Abra o `./routes/index.js` arquivo e substitua o código existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="411d8-135">Open the `./routes/index.js` file and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="411d8-136">Salve todas as suas alterações e reinicie o servidor.</span><span class="sxs-lookup"><span data-stu-id="411d8-136">Save all of your changes and restart the server.</span></span> <span data-ttu-id="411d8-137">Agora, o aplicativo deve ser muito diferente.</span><span class="sxs-lookup"><span data-stu-id="411d8-137">Now, the app should look very different.</span></span>

    ![Uma captura de tela da página inicial reprojetada](./images/create-app-01.png)
