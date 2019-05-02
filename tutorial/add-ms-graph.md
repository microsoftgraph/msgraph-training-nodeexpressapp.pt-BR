<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="783b1-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="783b1-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="783b1-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="783b1-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="783b1-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="783b1-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="783b1-104">Comece adicionando um novo método ao `./graph.js` arquivo para obter os eventos do calendário.</span><span class="sxs-lookup"><span data-stu-id="783b1-104">Start by adding a new method to the `./graph.js` file to get the events from the calendar.</span></span> <span data-ttu-id="783b1-105">Adicione a seguinte função dentro do `module.exports` no `./graph.js`.</span><span class="sxs-lookup"><span data-stu-id="783b1-105">Add the following function inside the `module.exports` in `./graph.js`.</span></span>

```js
getEvents: async function(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

<span data-ttu-id="783b1-106">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="783b1-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="783b1-107">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="783b1-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="783b1-108">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="783b1-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="783b1-109">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="783b1-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="783b1-110">Crie um novo arquivo no `./routes` diretório chamado `calendar.js`e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="783b1-110">Create a new file in the `./routes` directory named `calendar.js`, and add the following code.</span></span>

```js
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
    }
  }
);

module.exports = router;
```

<span data-ttu-id="783b1-111">Atualize `./app.js` para usar essa nova rota.</span><span class="sxs-lookup"><span data-stu-id="783b1-111">Update `./app.js` to use this new route.</span></span> <span data-ttu-id="783b1-112">Adicione a seguinte linha **antes** da `var app = express();` linha.</span><span class="sxs-lookup"><span data-stu-id="783b1-112">Add the following line **before** the `var app = express();` line.</span></span>

```js
var calendarRouter = require('./routes/calendar');
```

<span data-ttu-id="783b1-113">Em seguida, adicione a \*\*\*\* linha a `app.use('/auth', authRouter);` seguir após a linha.</span><span class="sxs-lookup"><span data-stu-id="783b1-113">Then add the following line **after** the `app.use('/auth', authRouter);` line.</span></span>

```js
app.use('/calendar', calendarRouter);
```

<span data-ttu-id="783b1-114">Agora você pode testar isso.</span><span class="sxs-lookup"><span data-stu-id="783b1-114">Now you can test this.</span></span> <span data-ttu-id="783b1-115">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="783b1-115">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="783b1-116">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="783b1-116">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="783b1-117">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="783b1-117">Display the results</span></span>

<span data-ttu-id="783b1-118">Agora você pode adicionar um modo de exibição para exibir os resultados de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="783b1-118">Now you can add a view to display the results in a more user-friendly manner.</span></span> <span data-ttu-id="783b1-119">Primeiro, adicione o código a seguir `./app.js` em **após** a `app.set('view engine', 'hbs');` linha.</span><span class="sxs-lookup"><span data-stu-id="783b1-119">First, add the following code in `./app.js` **after** the `app.set('view engine', 'hbs');` line.</span></span>

```js
var hbs = require('hbs');
var moment = require('moment');
// Helper to format date/time sent by Graph
hbs.registerHelper('eventDateTime', function(dateTime){
  return moment(dateTime).format('M/D/YY h:mm A');
});
```

<span data-ttu-id="783b1-120">Isso implementa um [auxiliar guias](http://handlebarsjs.com/#helpers) para formatar a data ISO 8601 retornada pelo Microsoft Graph em algo mais amigável.</span><span class="sxs-lookup"><span data-stu-id="783b1-120">This implements a [Handlebars helper](http://handlebarsjs.com/#helpers) to format the ISO 8601 date returned by Microsoft Graph into something more human-friendly.</span></span>

<span data-ttu-id="783b1-121">Crie um novo arquivo no `./views` diretório chamado `calendar.hbs` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="783b1-121">Create a new file in the `./views` directory named `calendar.hbs` and add the following code.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    {{#each events}}
      <tr>
        <td>{{this.organizer.emailAddress.name}}</td>
        <td>{{this.subject}}</td>
        <td>{{eventDateTime this.start.dateTime}}</td>
        <td>{{eventDateTime this.end.dateTime}}</td>
      </tr>
    {{/each}}
  </tbody>
</table>
```

<span data-ttu-id="783b1-122">Isso executará um loop através de uma coleção de eventos e adicionará uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="783b1-122">That will loop through a collection of events and add a table row for each one.</span></span> <span data-ttu-id="783b1-123">Agora atualize a rota `./routes/calendar.js` para usar este modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="783b1-123">Now update the route in `./routes/calendar.js` to use this view.</span></span> <span data-ttu-id="783b1-124">Substitua a rota existente pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="783b1-124">Replace the existing route with the following code.</span></span>

```js
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
        req.flash('error_msg', {
          message: 'Could not get access token. Try signing out and signing in again.',
          debug: JSON.stringify(err)
        });
      }

      if (accessToken && accessToken.length > 0) {
        try {
          // Get the events
          var events = await graph.getEvents(accessToken);
          params.events = events.value;
        } catch (err) {
          req.flash('error_msg', {
            message: 'Could not fetch events',
            debug: JSON.stringify(err)
          });
        }
      }

      res.render('calendar', params);
    }
  }
);
```

<span data-ttu-id="783b1-125">Salve suas alterações, reinicie o servidor e entre no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="783b1-125">Save your changes, restart the server, and sign in to the app.</span></span> <span data-ttu-id="783b1-126">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="783b1-126">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)