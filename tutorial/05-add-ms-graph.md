<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Abra `./graph.js` e adicione a seguinte função dentro `module.exports`do.

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetEventsSnippet":::

    Considere o que esse código está fazendo.

    - A URL que será chamada é `/me/events`.
    - O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
    - O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

1. Crie um novo arquivo no `./routes` diretório chamado `calendar.js`e adicione o código a seguir.

    ```javascript
    var express = require('express');
    var router = express.Router();
    var tokens = require('../tokens.js');
    var graph = require('../graph.js');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.isAuthenticated()) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          let params = {
            active: { calendar: true }
          };

          // Get the access token
          var accessToken;
          try {
            accessToken = await tokens.getAccessToken(req);
          } catch (err) {
            res.json(err);
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              var events = await graph.getEvents(accessToken);

              res.json(events.value);
            } catch (err) {
              res.json(err);
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    module.exports = router;
    ```

1. Atualize `./app.js` para usar essa nova rota. Adicione a seguinte linha **antes** da `var app = express();` linha.

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. Adicione a seguinte linha **após** a `app.use('/auth', authRouter);` linha.

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. Reiniciar o servidor. Entre e clique no link **calendário** na barra de navegação. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.

1. Adicione o código a seguir `./app.js` em **após** a `app.set('view engine', 'hbs');` linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    Isso implementa um [auxiliar guias](http://handlebarsjs.com/#helpers) para formatar a data ISO 8601 retornada pelo Microsoft Graph em algo mais amigável.

1. Crie um novo arquivo no `./views` diretório chamado `calendar.hbs` e adicione o código a seguir.

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.

1. Agora atualize a rota `./routes/calendar.js` para usar este modo de exibição. Substitua a rota existente pelo código a seguir.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="16-19,26,28-31,37":::

1. Salve suas alterações, reinicie o servidor e entre no aplicativo. Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
