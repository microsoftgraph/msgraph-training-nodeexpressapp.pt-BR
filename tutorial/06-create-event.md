<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="414ee-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="414ee-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="create-a-new-event-form"></a><span data-ttu-id="414ee-102">Criar um novo formulário de eventos</span><span class="sxs-lookup"><span data-stu-id="414ee-102">Create a new event form</span></span>

1. <span data-ttu-id="414ee-103">Crie um novo arquivo no diretório **./views** chamado **NewEvent. HBS** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="414ee-103">Create a new file in the **./views** directory named **newevent.hbs** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/views/newevent.hbs" id="NewEventFormSnippet":::

1. <span data-ttu-id="414ee-104">Adicione o código a seguir ao arquivo **./routes/calendar.js** antes da `module.exports = router;` linha.</span><span class="sxs-lookup"><span data-stu-id="414ee-104">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetEventFormSnippet":::

<span data-ttu-id="414ee-105">Isso implementa um formulário para entrada do usuário e o renderiza.</span><span class="sxs-lookup"><span data-stu-id="414ee-105">This implements a form for user input and renders it.</span></span>

## <a name="create-the-event"></a><span data-ttu-id="414ee-106">Criar o evento</span><span class="sxs-lookup"><span data-stu-id="414ee-106">Create the event</span></span>

1. <span data-ttu-id="414ee-107">Abra **./graph.js** e adicione a seguinte função dentro do `module.exports` .</span><span class="sxs-lookup"><span data-stu-id="414ee-107">Open **./graph.js** and add the following function inside `module.exports`.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="CreateEventSnippet":::

    <span data-ttu-id="414ee-108">Este código usa os campos de formulário para criar um objeto de evento de gráfico e, em seguida, envia uma solicitação POST ao `/me/events` ponto de extremidade para criar o evento no calendário padrão do usuário.</span><span class="sxs-lookup"><span data-stu-id="414ee-108">This code uses the form fields to create a Graph event object, then sends a POST request to the `/me/events` endpoint to create the event on the user's default calendar.</span></span>

1. <span data-ttu-id="414ee-109">Adicione o código a seguir ao arquivo **./routes/calendar.js** antes da `module.exports = router;` linha.</span><span class="sxs-lookup"><span data-stu-id="414ee-109">Add the following code to the **./routes/calendar.js** file before the `module.exports = router;` line.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="PostEventFormSnippet":::

    <span data-ttu-id="414ee-110">Este código valida e limpou a entrada do formulário e, em seguida, chama `graph.createEvent` para criar o evento.</span><span class="sxs-lookup"><span data-stu-id="414ee-110">This code validates and sanitized the form input, then calls `graph.createEvent` to create the event.</span></span> <span data-ttu-id="414ee-111">Ele redireciona de volta para o modo de exibição calendário após a chamada ser concluída.</span><span class="sxs-lookup"><span data-stu-id="414ee-111">It redirects back to the calendar view after the call completes.</span></span>

1. <span data-ttu-id="414ee-112">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="414ee-112">Save your changes and restart the app.</span></span> <span data-ttu-id="414ee-113">Clique no item de navegação **calendário** e, em seguida, clique no botão **criar evento** .</span><span class="sxs-lookup"><span data-stu-id="414ee-113">Click the **Calendar** nav item, then click the **Create event** button.</span></span> <span data-ttu-id="414ee-114">Preencha os valores e clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="414ee-114">Fill in the values and click **Create**.</span></span> <span data-ttu-id="414ee-115">O aplicativo retorna para o modo de exibição calendário depois que o novo evento é criado.</span><span class="sxs-lookup"><span data-stu-id="414ee-115">The app returns to the calendar view once the new event is created.</span></span>

    ![Uma captura de tela do novo formulário de evento](images/create-event-01.png)
