<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você usará o [Express](http://expressjs.com/) para compilar um aplicativo Web.

1. Abra sua CLI, navegue até um diretório onde você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo expresso que usa o [guias](http://handlebarsjs.com/) como o mecanismo de renderização.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    O gerador Express cria um novo diretório chamado `graph-tutorial` e estruturará um aplicativo expresso.

1. Navegue até o `graph-tutorial` diretório e digite o seguinte comando para instalar as dependências.

    ```Shell
    npm install
    ```

1. Execute o seguinte comando para atualizar pacotes de nós com vulnerabilidades relatadas.

    ```Shell
    npm audit fix
    ```

1. Use o seguinte comando para iniciar um servidor Web local.

    ```Shell
    npm start
    ```

1. Abra o navegador e vá até `http://localhost:3000`. Se tudo estiver funcionando, você verá uma mensagem de "bem-vindo ao Express". Se você não vir essa mensagem, confira o [Guia de introdução ao Express](http://expressjs.com/starter/generator.html).

## <a name="install-node-packages"></a>Instalar pacotes de nós

Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:

- [dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo. env.
- [tempo para formatar](https://github.com/moment/moment/) valores de data/hora.
- [Connect –](https://github.com/jaredhanson/connect-flash) mensagens de erro do flash para Flash no aplicativo.
- [Express-Session](https://github.com/expressjs/session) para armazenar valores em uma sessão do lado do servidor na memória.
- [Passport-Azure-ad](https://github.com/AzureAD/passport-azure-ad) para autenticar e obter tokens de acesso.
- [oauth2 simples](https://github.com/lelylan/simple-oauth2) para gerenciamento de token.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.
- [isomorphic-FETCH](https://github.com/matthew-andrews/isomorphic-fetch) to metafill o nó de busca. Um polipreenchimento de busca é necessário para `microsoft-graph-client` a biblioteca. Consulte o [Microsoft Graph JavaScript Client library wiki](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obter mais informações.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    npm install dotenv@8.2.0 moment@2.24.0 connect-flash@0.1.1 express-session@1.17.0 isomorphic-fetch@2.2.1
    npm install passport-azure-ad@4.2.1 simple-oauth2@3.3.0 @microsoft/microsoft-graph-client@2.0.0
    ```

    > [!TIP]
    > Os usuários do Windows podem receber a seguinte mensagem de erro ao tentar instalar esses pacotes no Windows.
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > Para resolver o erro, execute o seguinte comando para instalar as ferramentas de compilação do Windows usando uma janela de terminal elevada (administrador) que instala as ferramentas de desenvolvimento do VS e o Python.
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. Atualize o aplicativo para usar o `connect-flash` e `express-session` middleware. Abra o `./app.js` arquivo e adicione a seguinte `require` instrução à parte superior do arquivo.

    ```javascript
    var session = require('express-session');
    var flash = require('connect-flash');
    ```

1. Adicione o seguinte código imediatamente após a `var app = express();` linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você implementará a interface do usuário para o aplicativo.

1. Abra o `./views/layout.hbs` arquivo e substitua todo o conteúdo pelo código a seguir.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    Este código adiciona a [inicialização](http://getbootstrap.com/) para estilos simples e a [fonte incrível](https://fontawesome.com/) para alguns ícones simples. Também define um layout global com uma barra de navegação.

1. Abra `./public/stylesheets/style.css` e substitua todo o conteúdo pelo seguinte.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Abra o `./views/index.hbs` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Abra o `./routes/index.js` arquivo e substitua o código existente pelo seguinte.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Salve todas as suas alterações e reinicie o servidor. Agora, o aplicativo deve ser muito diferente.

    ![Uma captura de tela da página inicial reprojetada](./images/create-app-01.png)
