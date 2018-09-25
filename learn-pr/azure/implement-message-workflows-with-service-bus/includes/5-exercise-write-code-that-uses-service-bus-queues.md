Si è scelto di usare una coda del bus di servizio per lo scambio di messaggi relativi alle singole vendite tra l'app per dispositivi mobili usata dal personale di vendita e il servizio Web, ospitato in Azure, che archivierà i dettagli di ogni vendita in un'istanza di database SQL di Azure.

Sono già stati implementati gli oggetti necessari nella sottoscrizione di Azure. Ora si vuole scrivere il codice che invia i messaggi alla coda e recupera i messaggi.

## <a name="clone-and-open-the-starter-application"></a>Clonare e aprire l'applicazione iniziale

In questa unità verranno create due applicazioni console. La prima applicazione inserisce i messaggi in una coda del bus di servizio e la seconda li recupera. Le applicazioni fanno parte di un'unica soluzione .NET Core.

1. Per iniziare, clonare la soluzione eseguendo i comandi seguenti in Cloud Shell:

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. Passare quindi alla cartella iniziale e aprire l'editor di Cloud Shell.

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio

Per accedere a uno spazio dei nomi del bus di servizio e usare una coda, è necessario configurare due informazioni nelle app console:

* Endpoint per lo spazio dei nomi
* Chiave di accesso condiviso per l'autenticazione

È possibile ottenere entrambi questi valori dal portale di Azure sotto forma di stringa di connessione completa.

> [!NOTE]
> Per semplicità, la stringa di connessione sarà hardcoded nel file **Program.cs** di entrambe le applicazioni console. In un'applicazione di produzione si potrebbe usare un file di configurazione o anche Azure Key Vault per archiviare la stringa di connessione.

1. In Cloud Shell eseguire il comando seguente per visualizzare la stringa di connessione primaria per lo spazio dei nomi del bus di servizio. Sostituire `<namespace-name>` con il nome dello spazio dei nomi del bus di servizio.

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --namespace-name <namespace-name> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv
    ```

    Questa stringa di connessione dovrà essere usata più volte in questo modulo, pertanto è consigliabile incollarla in una posizione accessibile.

1. Copiare la chiave da Cloud Shell. Nell'editor aprire **privatemessagesender/Program.cs** e individuare la riga di codice seguente:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    Incollare la stringa di connessione tra virgolette. Salvare il file con <kbd>CTRL+S</kbd>.

1. Ripetere il passaggio precedente in **privatemessagereceiver/Program.cs**, incollando lo stesso valore di stringa di connessione. Salvare il file tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).

## <a name="write-code-that-sends-a-message-to-the-queue"></a>Scrivere il codice che invia un messaggio alla coda

Per completare il componente che invia i messaggi sulle vendite, seguire questa procedura:

1. Aprire **privatemessagesender/Program.cs** nell'editor

1. Individuare il metodo `SendSalesMessageAsync()`.

1. All'interno di questo metodo individuare la riga di codice seguente:

    ```C#
    // Create a queue client here
    ```

1. Per creare un client di accodamento, sostituire la riga di codice con il codice seguente:

    ```C#
    var queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
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

1. Salvare il file.

## <a name="send-a-message-to-the-queue"></a>Inviare un messaggio alla coda

Per eseguire il componente che invia un messaggio su una vendita, eseguire questo comando in Cloud Shell:

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> L'avvio delle app eseguite durante questo esercizio potrebbe richiedere qualche istante, dal momento che `dotnet` deve ripristinare i pacchetti da origini remote e compilare le app la prima volta che vengono eseguite.

Durante l'esecuzione del programma, vengono visualizzati messaggi che indicano che è in corso l'invio di un messaggio. Ogni volta che si esegue l'app, viene aggiunto un ulteriore messaggio alla coda.

Al termine, eseguire il comando seguente per vedere quanti messaggi si trovano nella coda:

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a>Scrivere il codice che riceve un messaggio dalla coda

1. Aprire **privatemessagereceiver/Program.cs** nell'editor

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

1. Salvare il file.

## <a name="retrieve-a-message-from-the-queue"></a>Recuperare un messaggio dalla coda

Per eseguire il componente che invia un messaggio su una vendita, eseguire questo comando in Cloud Shell:

```bash
dotnet run -p privatemessagereceiver
```

Quando si nota che il messaggio è stato ricevuto e viene visualizzato nella console, premere `Enter` per arrestare l'app. Eseguire quindi lo stesso comando di prima per confermare che tutti i messaggi sono stati rimossi dalla coda:

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

Verrà visualizzato `0` se tutti i messaggi sono stati rimossi.

È stato scritto codice che invia un messaggio sulle singole vendite a una coda del bus di servizio. Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nell'app per dispositivi mobili usata dal personale di vendita nei dispositivi.

È stato scritto anche il codice che riceve un messaggio dalla coda del bus di servizio. Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nel servizio Web in esecuzione in Azure che elabora i messaggi ricevuti.
