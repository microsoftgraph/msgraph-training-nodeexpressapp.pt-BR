<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cc5da-101">Neste exercício, você usará o [Express para](http://expressjs.com/) criar um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="cc5da-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="cc5da-102">Abra sua CLI, navegue até um diretório no qual você tenha direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo Express que use [Handlebars](http://handlebarsjs.com/) como mecanismo de renderização.</span><span class="sxs-lookup"><span data-stu-id="cc5da-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="cc5da-103">O gerador Express cria um novo diretório chamado `graph-tutorial` e estrutura um aplicativo Express.</span><span class="sxs-lookup"><span data-stu-id="cc5da-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="cc5da-104">Navegue até `graph-tutorial` o diretório e insira o seguinte comando para instalar dependências.</span><span class="sxs-lookup"><span data-stu-id="cc5da-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="cc5da-105">Execute o seguinte comando para atualizar pacotes node com vulnerabilidades relatadas.</span><span class="sxs-lookup"><span data-stu-id="cc5da-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="cc5da-106">Execute o seguinte comando para atualizar a versão do Express e outras dependências.</span><span class="sxs-lookup"><span data-stu-id="cc5da-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. <span data-ttu-id="cc5da-107">Use o comando a seguir para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="cc5da-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="cc5da-108">Abra o navegador e vá até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="cc5da-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="cc5da-109">Se tudo estiver funcionando, você verá uma mensagem "Bem-vindo ao Express".</span><span class="sxs-lookup"><span data-stu-id="cc5da-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="cc5da-110">Se você não vir essa mensagem, verifique o guia [de iniciação do Express.](http://expressjs.com/starter/generator.html)</span><span class="sxs-lookup"><span data-stu-id="cc5da-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="cc5da-111">Instalar pacotes de nó</span><span class="sxs-lookup"><span data-stu-id="cc5da-111">Install Node packages</span></span>

<span data-ttu-id="cc5da-112">Antes de continuar, instale alguns pacotes adicionais que você usará mais tarde:</span><span class="sxs-lookup"><span data-stu-id="cc5da-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="cc5da-113">[dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo .env.</span><span class="sxs-lookup"><span data-stu-id="cc5da-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="cc5da-114">[momento](https://github.com/moment/moment/) para formatar valores de data/hora.</span><span class="sxs-lookup"><span data-stu-id="cc5da-114">[moment](https://github.com/moment/moment/) for formatting date/time values.</span></span>
- <span data-ttu-id="cc5da-115">[windows-iana para](https://github.com/rubenillodo/windows-iana) traduzir nomes de fuso horário do Windows para IDs de fuso horário IANA.</span><span class="sxs-lookup"><span data-stu-id="cc5da-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="cc5da-116">[connect-flash](https://github.com/jaredhanson/connect-flash) para flash de mensagens de erro no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cc5da-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="cc5da-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span><span class="sxs-lookup"><span data-stu-id="cc5da-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="cc5da-118">[express-promise-router para permitir](https://github.com/express-promise-router/express-promise-router) que manipuladores de rota retornem uma Promessa.</span><span class="sxs-lookup"><span data-stu-id="cc5da-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="cc5da-119">[express-validator](https://github.com/express-validator/express-validator) para análise e validação de dados de formulário.</span><span class="sxs-lookup"><span data-stu-id="cc5da-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="cc5da-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar e obter tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="cc5da-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="cc5da-121">[uuid](https://github.com/uuidjs/uuid) usado por msal-node para gerar GUIDs.</span><span class="sxs-lookup"><span data-stu-id="cc5da-121">[uuid](https://github.com/uuidjs/uuid) used by msal-node to generate GUIDs.</span></span>
- <span data-ttu-id="cc5da-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="cc5da-122">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="cc5da-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span><span class="sxs-lookup"><span data-stu-id="cc5da-123">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="cc5da-124">Um polyfill de busca é necessário para a `microsoft-graph-client` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="cc5da-124">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="cc5da-125">Consulte a wiki da biblioteca de cliente [javaScript do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="cc5da-125">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="cc5da-126">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="cc5da-126">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > <span data-ttu-id="cc5da-127">Os usuários do Windows podem receber a seguinte mensagem de erro ao tentar instalar esses pacotes no Windows.</span><span class="sxs-lookup"><span data-stu-id="cc5da-127">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="cc5da-128">Para resolver o erro, execute o seguinte comando para instalar as Ferramentas de Com build do Windows usando uma janela de terminal com privilégios elevados (Administrador) que instala as Ferramentas de Com build do VS e o Python.</span><span class="sxs-lookup"><span data-stu-id="cc5da-128">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="cc5da-129">Atualize o aplicativo para usar `connect-flash` o `express-session` middleware e o middleware.</span><span class="sxs-lookup"><span data-stu-id="cc5da-129">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="cc5da-130">Abra **./app.js** e adicione a `require` instrução a seguir na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="cc5da-130">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="cc5da-131">Adicione o código a seguir imediatamente após a `var app = express();` linha.</span><span class="sxs-lookup"><span data-stu-id="cc5da-131">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="cc5da-132">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="cc5da-132">Design the app</span></span>

<span data-ttu-id="cc5da-133">Nesta seção, você implementará a interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cc5da-133">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="cc5da-134">Abra **./views/layout.hbs** e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="cc5da-134">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="cc5da-135">Este código adiciona [Bootstrap](http://getbootstrap.com/) para estilo simples e [Font Awesome](https://fontawesome.com/) para alguns ícones simples.</span><span class="sxs-lookup"><span data-stu-id="cc5da-135">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="cc5da-136">Ele também define um layout global com uma barra de inv.</span><span class="sxs-lookup"><span data-stu-id="cc5da-136">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="cc5da-137">Abra **./public/stylesheets/style.css** e substitua seu conteúdo inteiro pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="cc5da-137">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="cc5da-138">Abra **./views/index.hbs** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="cc5da-138">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="cc5da-139">Abra **./routes/index.js** e substitua o código existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="cc5da-139">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="cc5da-140">Salve todas as suas alterações e reinicie o servidor.</span><span class="sxs-lookup"><span data-stu-id="cc5da-140">Save all of your changes and restart the server.</span></span> <span data-ttu-id="cc5da-141">Agora, o aplicativo deve parecer muito diferente.</span><span class="sxs-lookup"><span data-stu-id="cc5da-141">Now, the app should look very different.</span></span>

    ![Uma captura de tela da home page reprojetada](./images/create-app-01.png)
