Si è scelto di usare un argomento del bus di servizio di Azure per distribuire i messaggi relativi all'andamento delle vendite nell'applicazione distribuita degli addetti alle vendite. L'app usata dal personale di vendita sui propri dispositivi mobili invierà messaggi che riassumono i dati di vendita per ogni area e periodo di tempo. Tali messaggi saranno distribuiti ai servizi Web situati nelle aree operative dell'azienda, comprese le Americhe e l'Europa.

È già stata implementata l'infrastruttura necessaria nella sottoscrizione di Azure, incluso l'argomento e le sottoscrizioni. Ora si vuole scrivere il codice che invia messaggi all'argomento e recupera i messaggi da una sottoscrizione.

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio

Iniziare configurando le stringhe di connessione nei componenti di invio e di ricezione:

1. Nell'editor aprire **performancemessagesender/Program.cs** e individuare la riga di codice seguente:

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    Incollare la stringa di connessione tra virgolette e salvare il file tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).

1. Ripetere il passaggio precedente in **performancemessagereceiver/Program.cs**, incollare lo stesso valore di stringa di connessione e salvare il file.

## <a name="write-code-that-sends-a-message-to-the-topic"></a>Scrivere il codice che invia un messaggio all'argomento

Per completare il componente che invia messaggi sull'andamento delle vendite, seguire questa procedura:

1. Aprire **performancemessagesender/Program.cs** nell'editor.

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

1. Per chiudere la connessione al bus di servizio, sostituire la riga di codice con il codice seguente:

    ```C#
    await topicClient.CloseAsync();
    ```

1. Salvare il file.

## <a name="send-a-message-to-the-topic"></a>Inviare un messaggio all'argomento

Per eseguire il componente che invia un messaggio su una vendita, eseguire questo comando in Cloud Shell:

```bash
dotnet run -p performancemessagesender
```

Durante l'esecuzione del programma, vengono visualizzati messaggi che indicano che è in corso l'invio di un messaggio. Ogni volta che si esegue l'app, viene aggiunto un ulteriore messaggio all'argomento e ogni abbonato riceve una copia.

Al termine, eseguire il comando seguente per vedere quanti messaggi si trovano nella sottoscrizione Americas:

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

Se si sostituisce `EuropeAndAfrica` con `Americas`, si può notare che entrambe le sottoscrizioni hanno lo stesso numero di messaggi.

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>Scrivere il codice che consente di ricevere un messaggio da una sottoscrizione dell'argomento

Per completare il componente che recupera messaggi sull'andamento delle vendite, seguire questa procedura:

1. Aprire **performancemessagereceiver/Program.cs** nell'editor.

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

```bash
dotnet run -p performancemessagereceiver
```

Quando il programma interrompe la stampa delle notifiche relative alla ricezione dei messaggi, premere `Enter` per arrestare l'app. Quindi, eseguire lo stesso comando come indicato in precedenza per verificare che non vi siano messaggi rimanenti nella sottoscrizione `Americas`.

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

Se si sostituisce `EuropeAndAfrica` con `Americas`, si noterà che il numero di messaggi non è cambiato. L'applicazione ha ricevuto i messaggi solo dalla sottoscrizione `Americas`.
