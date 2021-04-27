<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para esse aplicativo, você usará a biblioteca [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtenha eventos de calendário do Outlook

1. Abra **./graph.js** e adicione a seguinte função dentro `module.exports` .

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    Considere o que este código está fazendo.

    - O URL que será chamado é `/me/calendarview`.
    - O método adiciona o header à solicitação, fazendo com que os horários de início e término sejam retornados no `header` `Prefer: outlook.timezone` fuso horário do usuário.
    - O `query` método define os `startDateTime` `endDateTime` parâmetros e para o modo de exibição de calendário.
    - O método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição `select` realmente usará.
    - O `orderby` método classifica os resultados pela hora de início.
    - O `top` método limita os resultados a 50 eventos.

1. Crie um novo arquivo no **diretório ./routes** chamado **calendar.js**, e adicione o código a seguir.

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const addDays = require('date-fns/addDays');
    const formatISO = require('date-fns/formatISO');
    const startOfWeek = require('date-fns/startOfWeek');
    const zonedTimeToUtc = require('date-fns-tz/zonedTimeToUtc');
    const iana = require('windows-iana');
    const { body, validationResult } = require('express-validator');
    const validator = require('validator');

    /* GET /calendar */
    router.get('/',
      async function(req, res) {
        if (!req.session.userId) {
          // Redirect unauthenticated requests to home page
          res.redirect('/')
        } else {
          const params = {
            active: { calendar: true }
          };

          // Get the user
          const user = req.app.locals.users[req.session.userId];
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          const timeZoneId = iana.findIana(user.timeZone)[0];
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var weekStart = zonedTimeToUtc(startOfWeek(new Date()), timeZoneId.valueOf());
          var weekEnd = addDays(weekStart, 7);
          console.log(`Start: ${formatISO(weekStart)}`);

          // Get the access token
          var accessToken;
          try {
            accessToken = await getAccessToken(req.session.userId, req.app.locals.msalClient);
          } catch (err) {
            res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            return;
          }

          if (accessToken && accessToken.length > 0) {
            try {
              // Get the events
              const events = await graph.getCalendarView(
                accessToken,
                formatISO(weekStart),
                formatISO(weekEnd),
                user.timeZone);

              res.json(events.value);
            } catch (err) {
              res.send(JSON.stringify(err, Object.getOwnPropertyNames(err)));
            }
          }
          else {
            req.flash('error_msg', 'Could not get an access token');
          }
        }
      }
    );

    async function getAccessToken(userId, msalClient) {
      // Look up the user's account in the cache
      try {
        const accounts = await msalClient
          .getTokenCache()
          .getAllAccounts();

        const userAccount = accounts.find(a => a.homeAccountId === userId);

        // Get the token silently
        const response = await msalClient.acquireTokenSilent({
          scopes: process.env.OAUTH_SCOPES.split(','),
          redirectUri: process.env.OAUTH_REDIRECT_URI,
          account: userAccount
        });

        return response.accessToken;
      } catch (err) {
        console.log(JSON.stringify(err, Object.getOwnPropertyNames(err)));
      }
    }

    module.exports = router;
    ```

1. Atualize **./app.js** para usar essa nova rota. Adicione a seguinte linha **antes** da `var app = express();` linha.

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. Adicione a seguinte linha **após** a `app.use('/auth', authRouter);` linha.

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. Reiniciar o servidor. Entre e clique no link **Calendário** na barra de nav. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode adicionar um modo de exibição para exibir os resultados de uma maneira mais amigável.

1. Adicione o seguinte código **em ./app.js após** a `app.set('view engine', 'hbs');` linha.

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    Isso implementa um auxiliar [de Guidões](http://handlebarsjs.com/#helpers) para formatar a data ISO 8601 retornada pela Microsoft Graph em algo mais amigável para humanos.

1. Crie um novo arquivo no **diretório ./views** chamado **calendar.hbs** e adicione o código a seguir.

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    Isso percorrerá uma coleção de eventos e adicionará uma linha de tabela para cada um.

1. Agora atualize a rota **em ./routes/calendar.js** para usar esse exibição. Substitua a rota existente pelo código a seguir.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. Salve suas alterações, reinicie o servidor e entre no aplicativo. Clique no link **Calendário** e o aplicativo deve renderizar uma tabela de eventos.

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
