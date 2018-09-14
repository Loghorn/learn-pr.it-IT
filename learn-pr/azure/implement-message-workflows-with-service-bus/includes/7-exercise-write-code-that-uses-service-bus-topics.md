Si è scelto di usare un argomento del Bus di servizio di Azure per distribuire i messaggi relativi alle prestazioni di vendita nell'applicazione forza vendita distribuito. L'app usata dal personale addetto alle vendite nei dispositivi mobili verrà inviati i messaggi che riepilogano i dati relativi alle vendite per ogni area e periodo di tempo. Tali messaggi saranno distribuiti ai servizi Web situati nelle aree operative dell'azienda, comprese le Americhe e l'Europa.

È già stata implementata l'infrastruttura necessaria nella sottoscrizione di Azure, incluso l'argomento e le sottoscrizioni. A questo punto, si desidera scrivere il codice che invia messaggi all'argomento e recupera i messaggi di ogni sottoscrizione.

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio

È possibile iniziare configurando le stringhe di connessione sia l'invio e la ricezione di componenti:

1. Passare al portale di Azure.

1. Nella home page fare clic su **Tutte le risorse** e quindi fare clic sullo spazio dei nomi del bus di servizio creato in precedenza.

1. In **IMPOSTAZIONI** fare clic su **Criteri di accesso condiviso**.

1. Nell'elenco dei criteri fare clic su **RootManageSharedAccessKey**.

1. A destra del **stringa di connessione primaria** casella di testo, fare clic sui **fare clic per copiare** pulsante.

1. Passare a **Visual Studio Code**.

1. Nel riquadro **Esplora risorse** fare clic sul file **Program.cs** nella cartella **performancemessagesender**.

1. Individuare la riga di codice seguente:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Posizionare il cursore tra le virgolette e quindi premere **Ctrl + V**.

1. Nel riquadro **Esplora risorse** fare clic sul file **Program.cs** nella cartella **performancemessagereceiver**.

1. Individuare la riga di codice seguente:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Posizionare il cursore tra le virgolette e quindi premere **Ctrl + V**.

1. Fare clic su **File**, quindi fare clic su **Salva tutto**.

1. Chiudere tutte le finestre aperte dell'editor.

## <a name="write-code-that-sends-a-message-to-the-topic"></a>Scrivere il codice che invia un messaggio all'argomento

Per completare il componente che invia messaggi sull'andamento delle vendite, seguire questa procedura:

1. Nel riquadro **Esplora risorse** di Visual Studio Code fare clic sul file **Program.cs** nella cartella **performancemessagesender**.

1. Individuare il metodo `SendPerformanceMessageAsync()`.

1. All'interno di questo metodo individuare la riga di codice seguente:

    ```C#
    // Create a Topic Client here
    ```

1. Per creare un client di argomento, sostituire la riga di codice con il codice seguente:

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. All'interno del blocco `try...catch` individuare la riga di codice seguente:

    ```C#
    // Create and send a message here
    ```

1. Per creare e formattare un messaggio per la coda, sostituire la riga di codice con il codice seguente:

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. Per visualizzare il messaggio nella console, nella riga successiva aggiungere il codice seguente:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. Per inviare il messaggio alla coda, nella riga successiva aggiungere il codice seguente:

    ```C#
    await topicClient.SendAsync(message);
    ```

1. Individuare la riga di codice seguente:

    ```C#
    // Close the connection to the topic here
    ```

1. Per chiudere la connessione al Bus di servizio, sostituire la riga di codice con il codice seguente:

    ```C#
    await topicClient.CloseAsync();
    ```

1. In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.

## <a name="send-a-message-to-the-topic"></a>Inviare un messaggio all'argomento

Per eseguire il componente che invia un messaggio su una vendita, seguire questa procedura:

1. In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.

1. Nel **Debug** riquadro, nell'elenco a discesa elenco, selezionare **mittente del messaggio prestazioni avviare**e quindi premere **F5**. Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.

1. Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.

1. Passare al portale di Azure.

1. Se lo spazio dei nomi del Bus di servizio non viene visualizzato, nella home page, fare clic su **tutte le risorse**, quindi fare clic sullo spazio dei nomi del Bus di servizio creato in precedenza.

1. Nel pannello **Spazio dei nomi del bus di servizio** fare clic su **Argomenti** in **ENTITÀ** e quindi sull'argomento **salesperformancemessages**. Nell'elenco delle sottoscrizioni verrà visualizzato un messaggio nelle sottoscrizioni **Americhe** ed **Europa**.

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>Scrivere il codice che consente di ricevere un messaggio da una sottoscrizione dell'argomento

Per completare il componente che recupera messaggi sull'andamento delle vendite, seguire questa procedura:

1. Nel riquadro **Esplora risorse** di Visual Studio Code fare clic sul file **Program.cs** nella cartella **performancemessagereceiver**.

1. Individuare il metodo `MainAsync()`.

1. All'interno di questo metodo individuare la riga di codice seguente:

    ```C#
    // Create a subscription client here
    ```

1. Per creare un client di sottoscrizione, sostituire la riga con il codice seguente:

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. Individuare il metodo `RegisterMessageHandler()`.

1. Per configurare le opzioni di gestione dei messaggi, sostituire tutto il codice all'interno del metodo con il codice seguente:

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. Per registrare il gestore di messaggi, nella riga successiva aggiungere il codice seguente:

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Individuare il metodo `ProcessMessagesAsync()`. Il metodo è stato registrato come metodo di gestione dei messaggi in arrivo.

1. Per visualizzare i messaggi in arrivo nella console, sostituire tutto il codice all'interno del metodo con il codice seguente:

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. Per rimuovere il messaggio ricevuto dalla sottoscrizione, nella riga successiva aggiungere il codice seguente:

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Tornare al metodo `MainAsync()` e individuare la riga di codice seguente:

    ```C#
    // Close the subscription here
    ```

1. Per chiudere la connessione al bus di servizio, sostituire il codice con il codice seguente:

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.

## <a name="retrieve-a-message-from-a-topic-subscription"></a>Recuperare un messaggio da una sottoscrizione dell'argomento

Per eseguire il componente che recupera un messaggio sull'andamento delle vendite, seguire questa procedura:

1. In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.

1. Nel **Debug** riquadro, nell'elenco a discesa elenco, selezionare **ricevitore del messaggio prestazioni avviare**e quindi premere **F5**. Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.

1. Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.

1. Quando si nota che il messaggio è stato ricevuto e viene visualizzato nella console, scegliere **Arresta debug**dal menu **Debug**.

1. Passare al portale di Azure.

1. Se lo spazio dei nomi del Bus di servizio non viene visualizzato, nella home page, fare clic su **tutte le risorse**, quindi fare clic sullo spazio dei nomi del Bus di servizio creato in precedenza.

1. Nel pannello **Spazio dei nomi del bus di servizio** fare clic su **Argomenti** in **ENTITÀ** e quindi sull'argomento **salesperformancemessages**. Nell'elenco delle sottoscrizioni non verranno visualizzati messaggi nella sottoscrizione **Americhe** perché l'applicazione ha elaborato e rimosso l'unico messaggio presente. Si noti che il messaggio è ancora presente nella sottoscrizione **Europa**.
