<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você usará o [Express para](http://expressjs.com/) criar um aplicativo Web.

1. Abra sua CLI, navegue até um diretório em que você tem direitos para criar arquivos e execute o seguinte comando para criar um novo aplicativo Express que usa [Guidões](http://handlebarsjs.com/) como o mecanismo de renderização.

    ```Shell
    npx express-generator@4.16.1 --hbs graph-tutorial
    ```

    O gerador Express cria um novo diretório chamado `graph-tutorial` e estrutura um aplicativo Express.

1. Navegue até `graph-tutorial` o diretório e insira o seguinte comando para instalar dependências.

    ```Shell
    npm install
    ```

1. Execute o seguinte comando para atualizar pacotes de nó com vulnerabilidades relatadas.

    ```Shell
    npm audit fix
    ```

1. Execute o seguinte comando para atualizar a versão do Express e outras dependências.

    ```Shell
    npm install express@4.17.1 http-errors@1.8.0 morgan@1.10.0 debug@4.3.1 hbs@4.1.2
    ```

1. Use o seguinte comando para iniciar um servidor Web local.

    ```Shell
    npm start
    ```

1. Abra o navegador e vá até `http://localhost:3000`. Se tudo estiver funcionando, você verá uma mensagem "Bem-vindo ao Express". Se você não vir essa mensagem, verifique o [guia Express getting started](http://expressjs.com/starter/generator.html).

## <a name="install-node-packages"></a>Instalar pacotes de nó

Antes de continuar, instale alguns pacotes adicionais que você usará posteriormente:

- [dotenv](https://github.com/motdotla/dotenv) para carregar valores de um arquivo .env.
- [date-fns](https://github.com/date-fns/date-fns) para formatação de valores de data/hora.
- [windows-iana para](https://github.com/rubenillodo/windows-iana) traduzir Windows nomes de fuso horário para IDs de fuso horário IANA.
- [connect-flash](https://github.com/jaredhanson/connect-flash) para mensagens de erro flash no aplicativo.
- [express-session](https://github.com/expressjs/session) para armazenar valores em uma sessão no lado do servidor na memória.
- [express-promise-router](https://github.com/express-promise-router/express-promise-router) para permitir que manipuladores de rota retornem um Promise.
- [express-validator](https://github.com/express-validator/express-validator) para análise e validação de dados de formulário.
- [msal-node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) para autenticar e obter tokens de acesso.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.
- [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) para polifilar a busca para Node. Um polifilo de busca é necessário para a `microsoft-graph-client` biblioteca. Consulte o [wiki da biblioteca de Graph javascript do Microsoft](https://github.com/microsoftgraph/msgraph-sdk-javascript/wiki/Migration-from-1.x.x-to-2.x.x#polyfill-only-when-required) Graph para obter mais informações.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    npm install dotenv@8.2.0 date-fns@2.21.1 date-fns-tz@1.1.4 connect-flash@0.1.1 express-validator@6.10.0
    npm install express-session@1.17.1 express-promise-router@4.1.0 isomorphic-fetch@3.0.0
    npm install @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1 windows-iana@5.0.1
    ```

    > [!TIP]
    > Windows os usuários podem receber a seguinte mensagem de erro ao tentar instalar esses pacotes Windows.
    >
    > ```Shell
    > gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
    > ```
    >
    > Para resolver o erro, execute o seguinte comando para instalar o Windows ferramentas de com build usando uma janela de terminal com privilégios elevados (Administrador) que instala as Ferramentas de Com build vs e Python.
    >
    > ```Shell
    > npm install --global --production windows-build-tools
    > ```

1. Atualize o aplicativo para usar `connect-flash` o `express-session` middleware e. Abra **./app.js** e adicione a instrução a `require` seguir à parte superior do arquivo.

    ```javascript
    const session = require('express-session');
    const flash = require('connect-flash');
    const msal = require('@azure/msal-node');
    ```

1. Adicione o seguinte código imediatamente após a `var app = express();` linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="SessionSnippet":::

## <a name="design-the-app"></a>Design do aplicativo

Nesta seção, você implementará a interface do usuário do aplicativo.

1. Abra **./views/layout.hbs** e substitua todo o conteúdo pelo código a seguir.

    :::code language="html" source="../demo/graph-tutorial/views/layout.hbs" id="LayoutSnippet":::

    Este código adiciona [Bootstrap](http://getbootstrap.com/) para um estilo simples e [Font Awesome](https://fontawesome.com/) para alguns ícones simples. Ele também define um layout global com uma barra de nav.

1. Abra **./public/stylesheets/style.css** e substitua todo o conteúdo pelo seguinte.

    :::code language="css" source="../demo/graph-tutorial/public/stylesheets/style.css":::

1. Abra **./views/index.hbs** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/views/index.hbs" id="IndexSnippet":::

1. Abra **./routes/index.js** e substitua o código existente pelo seguinte.

    :::code language="javascript" source="../demo/graph-tutorial/routes/index.js" id="IndexRouterSnippet" highlight="6-10":::

1. Salve todas as suas alterações e reinicie o aplicativo. Agora, o aplicativo deve ter uma aparência muito diferente.

    ![Captura de tela da home page](./images/create-app-01.png)
