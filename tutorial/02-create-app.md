<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b8d30-101">Neste exercício, você usará o [Express para](http://expressjs.com/) criar um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="b8d30-101">In this exercise you will use [Express](http://expressjs.com/) to build a web app.</span></span>

1. <span data-ttu-id="b8d30-102">Abra sua CLI, navegue até um diretório em que você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo Express que usa [Guidões](http://handlebarsjs.com/) como o mecanismo de renderização.</span><span class="sxs-lookup"><span data-stu-id="b8d30-102">Open your CLI, navigate to a directory where you have rights to create files, and run the following command to create a new Express app that uses [Handlebars](http://handlebarsjs.com/) as the rendering engine.</span></span>

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    <span data-ttu-id="b8d30-103">O gerador Express cria um novo diretório chamado `graph-tutorial` e estrutura um aplicativo Express.</span><span class="sxs-lookup"><span data-stu-id="b8d30-103">The Express generator creates a new directory called `graph-tutorial` and scaffolds an Express app.</span></span>

1. <span data-ttu-id="b8d30-104">Navegue até `graph-tutorial` o diretório e insira o seguinte comando para instalar dependências.</span><span class="sxs-lookup"><span data-stu-id="b8d30-104">Navigate to the `graph-tutorial` directory and enter the following command to install dependencies.</span></span>

    ```Shell
    npm install
    ```

1. <span data-ttu-id="b8d30-105">Execute o seguinte comando para atualizar pacotes de nó com vulnerabilidades relatadas.</span><span class="sxs-lookup"><span data-stu-id="b8d30-105">Run the following command to update Node packages with reported vulnerabilities.</span></span>

    ```Shell
    npm audit fix
    ```

1. <span data-ttu-id="b8d30-106">Execute o seguinte comando para atualizar a versão do Express e outras dependências.</span><span class="sxs-lookup"><span data-stu-id="b8d30-106">Run the following command to update the version of Express and other dependencies.</span></span>

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. <span data-ttu-id="b8d30-107">Use o seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="b8d30-107">Use the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="b8d30-108">Abra o navegador e vá até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="b8d30-108">Open your browser and navigate to `http://localhost:3000`.</span></span> <span data-ttu-id="b8d30-109">Se tudo estiver funcionando, você verá uma mensagem "Bem-vindo ao Express".</span><span class="sxs-lookup"><span data-stu-id="b8d30-109">If everything is working, you will see a "Welcome to Express" message.</span></span> <span data-ttu-id="b8d30-110">Se você não vir essa mensagem, verifique o [guia Express getting started](http://expressjs.com/starter/generator.html).</span><span class="sxs-lookup"><span data-stu-id="b8d30-110">If you don't see that message, check the [Express getting started guide](http://expressjs.com/starter/generator.html).</span></span>

## <a name="install-node-packages"></a><span data-ttu-id="b8d30-111">Instalar pacotes de nó</span><span class="sxs-lookup"><span data-stu-id="b8d30-111">Install Node packages</span></span>

<span data-ttu-id="b8d30-112">Antes de continuar, instale alguns pacotes adicionais que você usará posteriormente:</span><span class="sxs-lookup"><span data-stu-id="b8d30-112">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="b8d30-113">[dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo .env.</span><span class="sxs-lookup"><span data-stu-id="b8d30-113">[dotenv](https://github.com/motdotla/dotenv) for loading values from a .env file.</span></span>
- <span data-ttu-id="b8d30-114">[date-fns](https://github.com/date-fns/date-fns) para formatação de valores de data/hora.</span><span class="sxs-lookup"><span data-stu-id="b8d30-114">[date-fns](https://github.com/date-fns/date-fns) for formatting date/time values.</span></span>
- <span data-ttu-id="b8d30-115">[windows-iana para](https://github.com/rubenillodo/windows-iana) traduzir Windows nomes de fuso horário para IDs de fuso horário IANA.</span><span class="sxs-lookup"><span data-stu-id="b8d30-115">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zone names to IANA time zone IDs.</span></span>
- <span data-ttu-id="b8d30-116">[connect-flash](https://github.com/jaredhanson/connect-flash) para mensagens de erro flash no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8d30-116">[connect-flash](https://github.com/jaredhanson/connect-flash) to flash error messages in the app.</span></span>
- <span data-ttu-id="b8d30-117">[express-session](https://github.com/expressjs/session) para armazenar valores em uma sessão no lado do servidor na memória.</span><span class="sxs-lookup"><span data-stu-id="b8d30-117">[express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.</span></span>
- <span data-ttu-id="b8d30-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) para permitir que manipuladores de rota retornem um Promise.</span><span class="sxs-lookup"><span data-stu-id="b8d30-118">[express-promise-router](https://github.com/express-promise-router/express-promise-router) to allow route handlers to return a Promise.</span></span>
- <span data-ttu-id="b8d30-119">[express-validator](https://github.com/express-validator/express-validator) para análise e validação de dados de formulário.</span><span class="sxs-lookup"><span data-stu-id="b8d30-119">[express-validator](https://github.com/express-validator/express-validator) for parsing and validating form data.</span></span>
- <span data-ttu-id="b8d30-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar e obter tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="b8d30-120">[msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for authenticating and getting access tokens.</span></span>
- <span data-ttu-id="b8d30-121">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b8d30-121">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="b8d30-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) para polifilar a busca para Node.</span><span class="sxs-lookup"><span data-stu-id="b8d30-122">[isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node.</span></span> <span data-ttu-id="b8d30-123">Um polifilo de busca é necessário para a `microsoft-graph-client` biblioteca.</span><span class="sxs-lookup"><span data-stu-id="b8d30-123">A fetch polyfill is required for the `microsoft-graph-client` library.</span></span> <span data-ttu-id="b8d30-124">Consulte o [wiki da biblioteca de Graph javascript do Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) Graph para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="b8d30-124">See the [Microsoft Graph JavaScript client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) for more information.</span></span>

1. <span data-ttu-id="b8d30-125">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="b8d30-125">Run the following command in your CLI.</span></span>

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > <span data-ttu-id="b8d30-126">Windows os usuários podem receber a seguinte mensagem de erro ao tentar instalar esses pacotes Windows.</span><span class="sxs-lookup"><span data-stu-id="b8d30-126">Windows users may get the following error message when trying to install these packages on Windows.</span></span>
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > <span data-ttu-id="b8d30-127">Para resolver o erro, execute o seguinte comando para instalar o Windows ferramentas de com build usando uma janela de terminal com privilégios elevados (Administrador) que instala as Ferramentas de Com build vs e Python.</span><span class="sxs-lookup"><span data-stu-id="b8d30-127">To resolve the error, run the following command to install the Windows Build Tools using an elevated (Administrator) terminal window which installs the VS Build Tools and Python.</span></span>
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. <span data-ttu-id="b8d30-128">Atualize o aplicativo para usar `connect-flash` o `express-session` middleware e.</span><span class="sxs-lookup"><span data-stu-id="b8d30-128">Update the application to use the `connect-flash` and `express-session` middleware.</span></span> <span data-ttu-id="b8d30-129">Abra **./app.js** e adicione a instrução a `require` seguir à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="b8d30-129">Open **./app.js** and add the following `require` statement to the top of the file.</span></span>

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. <span data-ttu-id="b8d30-130">Adicione o seguinte código imediatamente após a `var app = express();` linha.</span><span class="sxs-lookup"><span data-stu-id="b8d30-130">Add the following code immediately after the `var app = express();` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a><span data-ttu-id="b8d30-131">Design do aplicativo</span><span class="sxs-lookup"><span data-stu-id="b8d30-131">Design the app</span></span>

<span data-ttu-id="b8d30-132">Nesta seção, você implementará a interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8d30-132">In this section you will implement the UI for the app.</span></span>

1. <span data-ttu-id="b8d30-133">Abra **./views/layout.hbs** e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="b8d30-133">Open **./views/layout.hbs** and replace the entire contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    <span data-ttu-id="b8d30-134">Este código adiciona [Bootstrap](http://getbootstrap.com/) para um estilo simples e [Font Awesome](https://fontawesome.com/) para alguns ícones simples.</span><span class="sxs-lookup"><span data-stu-id="b8d30-134">This code adds [Bootstrap](http://getbootstrap.com/) for simple styling, and [Font Awesome](https://fontawesome.com/) for some simple icons.</span></span> <span data-ttu-id="b8d30-135">Ele também define um layout global com uma barra de nav.</span><span class="sxs-lookup"><span data-stu-id="b8d30-135">It also defines a global layout with a nav bar.</span></span>

1. <span data-ttu-id="b8d30-136">Abra **./public/stylesheets/style.css** e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="b8d30-136">Open **./public/stylesheets/style.css** and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. <span data-ttu-id="b8d30-137">Abra **./views/index.hbs** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="b8d30-137">Open **./views/index.hbs** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. <span data-ttu-id="b8d30-138">Abra **./routes/index.js** e substitua o código existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="b8d30-138">Open **./routes/index.js** and replace the existing code with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. <span data-ttu-id="b8d30-139">Salve todas as suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8d30-139">Save all of your changes and restart the server.</span></span> <span data-ttu-id="b8d30-140">Agora, o aplicativo deve ter uma aparência muito diferente.</span><span class="sxs-lookup"><span data-stu-id="b8d30-140">Now, the app should look very different.</span></span>

    ![Captura de tela da home page](./images/create-app-01.png)
