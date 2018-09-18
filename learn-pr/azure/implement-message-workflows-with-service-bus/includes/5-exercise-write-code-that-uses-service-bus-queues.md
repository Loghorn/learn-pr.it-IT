Si è scelto di usare una coda del bus di servizio per lo scambio di messaggi relativi alle singole vendite tra l'app per dispositivi mobili usata dal personale di vendita e il servizio Web, ospitato in Azure, che archivierà i dettagli di ogni vendita in un'istanza di database SQL di Azure.

Sono già stati implementati gli oggetti necessari nella sottoscrizione di Azure. Ora si vuole scrivere il codice che invia i messaggi alla coda e recupera i messaggi.

## <a name="clone-and-open-the-starter-application"></a>Clonare e aprire l'applicazione iniziale

In questa unità verranno create due applicazioni console in **Visual Studio Code**. La prima applicazione inserisce i messaggi in una coda del bus di servizio e la seconda li recupera. Le applicazioni fanno parte di un'unica soluzione .NET Core. 

Iniziare clonando la soluzione:

1. Avviare un prompt dei comandi e passare alla directory in cui si vuole ospitare il codice sorgente per l'applicazione.

1. Digitare il comando seguente e quindi premere **INVIO**:

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. Una volta completata l'operazione di clonazione, passare alla cartella iniziale.

1. Digitare il comando seguente e quindi premere **INVIO**.

    ```powershell
    code .
    ```

1. Se viene visualizzato un messaggio in cui viene chiesto se si vuole ripristinare le dipendenze, fare clic su **Sì**.

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio

Per accedere a uno spazio dei nomi del bus di servizio e usare una coda, è necessario configurare due informazioni nelle app console:

* Endpoint per lo spazio dei nomi
* Chiave di accesso condiviso per l'autenticazione

È possibile ottenere entrambi questi valori dal portale di Azure sotto forma di stringa di connessione completa.

> [!NOTE]
> Per semplicità, la stringa di connessione sarà hardcoded nel file **Program.cs** di entrambe le applicazioni console. In un'applicazione di produzione si potrebbe usare un file di configurazione o anche Azure Key Vault per archiviare la stringa di connessione.

1. Passare al portale di Azure.

1. Nella home page fare clic su **Tutte le risorse**, quindi sullo spazio dei nomi del bus di Servizio creato in precedenza.

1. In **IMPOSTAZIONI** fare clic su **Criteri di accesso condiviso**.

1. Nell'elenco dei criteri fare clic su **RootManageSharedAccessKey**.

1. A destra della casella di testo **Stringa di connessione primaria** scegliere il pulsante **Fare clic per copiare**.

1. Passare a **Visual Studio Code**.

1. Nel riquadro **Esplora risorse**, nella cartella **privatemessagesender** fare clic sul file **Program.cs**.

1. Individuare la riga di codice seguente:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Posizionare il cursore tra le virgolette e quindi premere **CTRL+V**.

1. Nel riquadro **Esplora risorse**, nella cartella **privatemessagereceiver** fare clic sul file **Program.cs**.

1. Individuare la riga di codice seguente:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. Posizionare il cursore tra le virgolette e quindi premere **CTRL+V**.

1. Fare clic su **File** e quindi su **Salva tutto**.

1. Chiudere tutte le finestre aperte dell'editor.

## <a name="write-code-that-sends-a-message-to-the-queue"></a>Scrivere il codice che invia un messaggio alla coda

Per completare il componente che invia i messaggi sulle vendite, seguire questa procedura:

1. In Visual Studio Code, nel riquadro **Esplora risorse**, nella cartella **privatemessagesender** fare clic sul file **Program.cs**.

1. Individuare il metodo `SendSalesMessageAsync()`.

1. All'interno di questo metodo individuare la riga di codice seguente:

    ```C#
    // Create a queue client here
    ```

1. Per creare un client di accodamento, sostituire la riga di codice con il codice seguente:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. All'interno del blocco `try...catch` individuare la riga di codice seguente:

    ```C#
    // Create and send a message here
    ```

1. Per creare e formattare un messaggio per la coda, sostituire la riga di codice con il codice seguente:

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. Per visualizzare il messaggio nella console, nella riga successiva aggiungere il codice seguente:

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. Per inviare il messaggio alla coda, nella riga successiva aggiungere il codice seguente:

    ```C#
    await queueClient.SendAsync(message);
    ```

1. Individuare la riga di codice seguente:

    ```C#
    // Close the connection to the queue here
    ```

1. Per chiudere la connessione del bus di servizio, sostituire la riga di codice con il codice seguente:

    ```C#
    await queueClient.CloseAsync();
    ```

1. In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.

## <a name="send-a-message-to-the-queue"></a>Inviare un messaggio alla coda

Per eseguire il componente che invia un messaggio su una vendita, seguire questa procedura:

1. In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.

1. Nel riquadro **Debug** selezionare **Launch Private Message Sender** (Avvia mittente messaggio privato) nell'elenco a discesa e quindi premere **F5**. Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.

1. Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.

1. Accedere al portale di Azure.

1. Se lo spazio dei nomi del bus di servizio non viene visualizzato, nella home page fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi del bus di servizio creato in precedenza.

1. Nel pannello **Spazio dei nomi del bus di servizio**, in **ENTITÀ** fare clic su **Code** e quindi sulla coda **salesmessages**. In **NUMERO DI MESSAGGI ATTIVI** dovrebbe venire indicato che è stato aggiunto un messaggio alla coda.

## <a name="write-code-that-receives-a-message-from-the-queue"></a>Scrivere il codice che riceve un messaggio dalla coda

Per completare il componente che recupera i messaggi sulle vendite, seguire questa procedura:

1. In Visual Studio Code, nel riquadro **Esplora risorse**, nella cartella **privatemessagereceiver** fare clic sul file **Program.cs**.

1. Individuare il metodo `ReceiveSalesMessageAsync()`.

1. All'interno di questo metodo individuare la riga di codice seguente:

    ```C#
    // Create a queue client here
    ```

1. Per creare un client di accodamento, sostituire la riga con il codice seguente:

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
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
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. Individuare il metodo `ProcessMessagesAsync()`. Il metodo è stato registrato come metodo di gestione dei messaggi in arrivo.

1. Per visualizzare i messaggi in arrivo nella console, sostituire tutto il codice all'interno del metodo con il codice seguente:

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. Per rimuovere il messaggio ricevuto dalla coda, nella riga successiva aggiungere il codice seguente:

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. Tornare al metodo `ReceiveSalesMessageAsync()` e individuare la riga di codice seguente:

    ```C#
    // Close the queue here
    ```

1. Per chiudere la connessione al bus di servizio, sostituire la riga con il codice seguente:

    ```C#
    await queueClient.CloseAsync();
    ```

1. In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.

## <a name="retrieve-a-message-from-the-queue"></a>Recuperare un messaggio dalla coda

Per eseguire il componente che recupera un messaggio su una vendita, seguire questa procedura:

1. In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.

1. Nel riquadro **Debug** selezionare **Launch Private Message Receiver** (Avvia destinatario messaggio privato) nell'elenco a discesa e quindi premere **F5**. Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.

1. Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.

1. Quando si nota che il messaggio è stato ricevuto e viene visualizzato nella console, scegliere **Arresta debug**dal menu **Debug**.

1. Accedere al portale di Azure.

1. Se lo spazio dei nomi del bus di servizio non viene visualizzato, nella home page fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi del bus di servizio creato in precedenza.

1. Nel pannello **Spazio dei nomi del bus di servizio**, in **ENTITÀ** fare clic su **Code** e quindi sulla coda **salesmessages**. In **NUMERO DI MESSAGGI ATTIVI** dovrebbe venire indicato che il messaggio è stato rimosso dalla coda.

È stato scritto codice che invia un messaggio sulle singole vendite a una coda del bus di servizio. Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nell'app per dispositivi mobili usata dal personale di vendita nei dispositivi.

È stato scritto anche il codice che riceve un messaggio dalla coda del bus di servizio. Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nel servizio Web in esecuzione in Azure che elabora i messaggi ricevuti.
