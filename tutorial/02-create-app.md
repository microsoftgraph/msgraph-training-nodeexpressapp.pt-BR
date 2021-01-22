<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você usará o [Express para](http://expressjs.com/) criar um aplicativo Web.

1. Abra sua CLI, navegue até um diretório no qual você tenha direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo Express que use [Handlebars](http://handlebarsjs.com/) como mecanismo de renderização.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    O gerador Express cria um novo diretório chamado `graph-tutorial` e estrutura um aplicativo Express.

1. Navegue até `graph-tutorial` o diretório e insira o seguinte comando para instalar dependências.

    ```Shell
    npm install
    ```

1. Execute o seguinte comando para atualizar pacotes node com vulnerabilidades relatadas.

    ```Shell
    npm audit fix
    ```

1. Execute o seguinte comando para atualizar a versão do Express e outras dependências.

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.1.1
    ```

1. Use o comando a seguir para iniciar um servidor Web local.

    ```Shell
    npm start
    ```

1. Abra o navegador e vá até `http://localhost:3000`. Se tudo estiver funcionando, você verá uma mensagem "Bem-vindo ao Express". Se você não vir essa mensagem, verifique o guia [de iniciação do Express.](http://expressjs.com/starter/generator.html)

## <a name="install-node-packages"></a>Instalar pacotes de nó

Antes de continuar, instale alguns pacotes adicionais que você usará mais tarde:

- [dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo .env.
- [momento](https://github.com/moment/moment/) para formatar valores de data/hora.
- [windows-iana para](https://github.com/rubenillodo/windows-iana) traduzir nomes de fuso horário do Windows para IDs de fuso horário IANA.
- [connect-flash](https://github.com/jaredhanson/connect-flash) para flash de mensagens de erro no aplicativo.
- [express-session](https://github.com/expressjs/session) to store values in an in-memory server-side session.
- [express-promise-router para permitir](https://github.com/express-promise-router/express-promise-router) que manipuladores de rota retornem uma Promessa.
- [express-validator](https://github.com/express-validator/express-validator) para análise e validação de dados de formulário.
- [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar e obter tokens de acesso.
- [uuid](https://github.com/uuidjs/uuid) usado por msal-node para gerar GUIDs.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) to polyfill the fetch for Node. Um polyfill de busca é necessário para a `microsoft-graph-client` biblioteca. Consulte a wiki da biblioteca de cliente [javaScript do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) para obter mais informações.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    npm install dotenv@8.2.0 moment@2.29.1 moment-timezone@0.5.31 connect-flash@0.1.1 express-session@1.17.1 express-promise-router@4.0.1 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.0-beta.0 @microsoft/microsoft-graph-client@2.1.1 windows-iana@4.2.1 express-validator@6.6.1 uuid@8.3.1
    ```

    > [!TIP]
    > Os usuários do Windows podem receber a seguinte mensagem de erro ao tentar instalar esses pacotes no Windows.
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > Para resolver o erro, execute o seguinte comando para instalar as Ferramentas de Com build do Windows usando uma janela de terminal com privilégios elevados (Administrador) que instala as Ferramentas de Com build do VS e o Python.
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. Atualize o aplicativo para usar `connect-flash` o `express-session` middleware e o middleware. Abra **./app.js** e adicione a `require` instrução a seguir na parte superior do arquivo.

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. Adicione o código a seguir imediatamente após a `var app = express();` linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você implementará a interface do usuário do aplicativo.

1. Abra **./views/layout.hbs** e substitua todo o conteúdo pelo código a seguir.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    Este código adiciona [Bootstrap](http://getbootstrap.com/) para estilo simples e [Font Awesome](https://fontawesome.com/) para alguns ícones simples. Ele também define um layout global com uma barra de inv.

1. Abra **./public/stylesheets/style.css** e substitua seu conteúdo inteiro pelo seguinte.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Abra **./views/index.hbs** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Abra **./routes/index.js** e substitua o código existente pelo seguinte.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Salve todas as suas alterações e reinicie o servidor. Agora, o aplicativo deve parecer muito diferente.

    ![Uma captura de tela da home page reprojetada](./images/create-app-01.png)
