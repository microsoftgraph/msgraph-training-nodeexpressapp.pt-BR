<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="25960-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="25960-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="25960-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="25960-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="25960-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="25960-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="25960-104">Abra **./graph.js** e adicione a seguinte função dentro do `module.exports` .</span><span class="sxs-lookup"><span data-stu-id="25960-104">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="GetCalendarViewSnippet":::

    <span data-ttu-id="25960-105">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="25960-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="25960-106">A URL que será chamada é `/me/events` .</span><span class="sxs-lookup"><span data-stu-id="25960-106">The URL that will be called is `/me/events`.</span></span>
    - <span data-ttu-id="25960-107">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="25960-107">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="25960-108">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="25960-108">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="25960-109">Crie um novo arquivo no diretório **./routes** chamado **calendar.js** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="25960-109">Create a new file in the **./routes** directory named **calendar.js** , and add the following code.</span></span>

    ```javascript
    const router = require('express-promise-router')();
    const graph = require('../graph.js');
    const moment = require('moment-timezone');
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
          // Moment needs IANA format
          const timeZoneId = iana.findOneIana(user.timeZone);
          console.log(`Time zone: ${timeZoneId.valueOf()}`);

          // Calculate the start and end of the current week
          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(timeZoneId.valueOf()).startOf('week').utc();
          var endOfWeek = moment(startOfWeek).add(7, 'day');
          console.log(`Start: ${startOfWeek.format()}`);

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
                startOfWeek.format(),
                endOfWeek.format(),
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

1. <span data-ttu-id="25960-110">Update **./app.js** para usar essa nova rota.</span><span class="sxs-lookup"><span data-stu-id="25960-110">Update **./app.js** to use this new route.</span></span> <span data-ttu-id="25960-111">Adicione a seguinte linha **antes** da `var app = express();` linha.</span><span class="sxs-lookup"><span data-stu-id="25960-111">Add the following line **before** the `var app = express();` line.</span></span>

    ```javascript
    var calendarRouter = require('./routes/calendar');
    ```

1. <span data-ttu-id="25960-112">Adicione a seguinte linha **após** a `app.use('/auth', authRouter);` linha.</span><span class="sxs-lookup"><span data-stu-id="25960-112">Add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

    ```javascript
    app.use('/calendar', calendarRouter);
    ```

1. <span data-ttu-id="25960-113">Reiniciar o servidor.</span><span class="sxs-lookup"><span data-stu-id="25960-113">Restart the server.</span></span> <span data-ttu-id="25960-114">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="25960-114">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="25960-115">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="25960-115">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="25960-116">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="25960-116">Display the results</span></span>

<span data-ttu-id="25960-117">Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="25960-117">Now you can add a view to display the results in a more user-friendly manner.</span></span>

1. <span data-ttu-id="25960-118">Adicione o código a seguir em **./app.js após** a `app.set('view engine', 'hbs');` linha.</span><span class="sxs-lookup"><span data-stu-id="25960-118">Add the following code in **./app.js after** the `app.set('view engine', 'hbs');` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/app.js" id="FormatDateSnippet":::

    <span data-ttu-id="25960-119">Isso implementa um [auxiliar guias](http://handlebarsjs.com/#helpers) para formatar a data ISO 8601 retornada pelo Microsoft Graph em algo mais amigável.</span><span class="sxs-lookup"><span data-stu-id="25960-119">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

1. <span data-ttu-id="25960-120">Crie um novo arquivo no diretório **./views** chamado **Calendar. HBS** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="25960-120">Create a new file in the **./views** directory named **calendar.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/calendar.hbs" id="LayoutSnippet":::

    <span data-ttu-id="25960-121">Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="25960-121">That will loop through a collection of events and add a table row for each one.</span></span>

1. <span data-ttu-id="25960-122">Agora, atualize a rota em **./routes/calendar.js** para usar este modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="25960-122">Now update the route in **./routes/calendar.js** to use this view.</span></span> <span data-ttu-id="25960-123">Substitua a rota existente pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="25960-123">Replace the existing route with the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetRouteSnippet" highlight="33-36,49,51-54,61":::

1. <span data-ttu-id="25960-124">Salve suas alterações, reinicie o servidor e entre no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="25960-124">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="25960-125">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="25960-125">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
