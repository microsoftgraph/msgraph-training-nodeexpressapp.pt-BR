<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

## <a name="create-a-new-event-form"></a>Criar um novo formulário de eventos

1. Crie um novo arquivo no diretório **./views** chamado **NewEvent. HBS** e adicione o código a seguir.

    :::code language="html" source="../demo/graph-tutorial/views/newevent.hbs" id="NewEventFormSnippet":::

1. Adicione o código a seguir ao arquivo **./routes/calendar.js** antes da `module.exports = router;` linha.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="GetEventFormSnippet":::

Isso implementa um formulário para entrada do usuário e o renderiza.

## <a name="create-the-event"></a>Criar o evento

1. Abra **./graph.js** e adicione a seguinte função dentro do `module.exports` .

    :::code language="javascript" source="../demo/graph-tutorial/graph.js" id="CreateEventSnippet":::

    Este código usa os campos de formulário para criar um objeto de evento de gráfico e, em seguida, envia uma solicitação POST ao `/me/events` ponto de extremidade para criar o evento no calendário padrão do usuário.

1. Adicione o código a seguir ao arquivo **./routes/calendar.js** antes da `module.exports = router;` linha.

    :::code language="javascript" source="../demo/graph-tutorial/routes/calendar.js" id="PostEventFormSnippet":::

    Este código valida e limpou a entrada do formulário e, em seguida, chama `graph.createEvent` para criar o evento. Ele redireciona de volta para o modo de exibição calendário após a chamada ser concluída.

1. Salve suas alterações e reinicie o aplicativo. Clique no item de navegação **calendário** e, em seguida, clique no botão **criar evento** . Preencha os valores e clique em **criar**. O aplicativo retorna para o modo de exibição calendário depois que o novo evento é criado.

    ![Uma captura de tela do novo formulário de evento](images/create-event-01.png)
