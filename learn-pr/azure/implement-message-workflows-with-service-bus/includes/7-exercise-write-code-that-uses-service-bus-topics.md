<span data-ttu-id="b611b-101">Si è scelto di usare un argomento del bus di servizio di Azure per distribuire i messaggi relativi all'andamento delle vendite nell'applicazione distribuita degli addetti alle vendite.</span><span class="sxs-lookup"><span data-stu-id="b611b-101">You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your sales force distributed application.</span></span> <span data-ttu-id="b611b-102">L'app usata dal personale di vendita sui propri dispositivi mobili invierà messaggi che riassumono i dati di vendita per ogni area e periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="b611b-102">The app used by sales personnel on their mobile devices will send messages that summarize sales figures for each area and time period.</span></span> <span data-ttu-id="b611b-103">Tali messaggi saranno distribuiti ai servizi Web situati nelle aree operative dell'azienda, comprese le Americhe e l'Europa.</span><span class="sxs-lookup"><span data-stu-id="b611b-103">Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.</span></span>

<span data-ttu-id="b611b-104">È già stata implementata l'infrastruttura necessaria nella sottoscrizione di Azure, incluso l'argomento e le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="b611b-104">You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions.</span></span> <span data-ttu-id="b611b-105">Ora si vuole scrivere il codice che invia messaggi all'argomento e recupera i messaggi da una sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="b611b-105">Now, you want to write the code that sends messages to the topic and retrieves messages from a subscription.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="b611b-106">Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="b611b-106">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="b611b-107">Iniziare configurando le stringhe di connessione nei componenti di invio e di ricezione:</span><span class="sxs-lookup"><span data-stu-id="b611b-107">Start by configuring connection strings both in the sending and receiving components:</span></span>

1. <span data-ttu-id="b611b-108">Nell'editor aprire **performancemessagesender/Program.cs** e individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-108">In the editor, open **performancemessagesender/Program.cs** and locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    <span data-ttu-id="b611b-109">Incollare la stringa di connessione tra virgolette e salvare il file tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="b611b-109">Paste the connection string between the quotation marks and save the file either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="b611b-110">Ripetere il passaggio precedente in **performancemessagereceiver/Program.cs**, incollare lo stesso valore di stringa di connessione e salvare il file.</span><span class="sxs-lookup"><span data-stu-id="b611b-110">Repeat the previous step in **performancemessagereceiver/Program.cs**, pasting in the same connection string value and save the file.</span></span>

## <a name="write-code-that-sends-a-message-to-the-topic"></a><span data-ttu-id="b611b-111">Scrivere il codice che invia un messaggio all'argomento</span><span class="sxs-lookup"><span data-stu-id="b611b-111">Write code that sends a message to the topic</span></span>

<span data-ttu-id="b611b-112">Per completare il componente che invia messaggi sull'andamento delle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="b611b-112">To complete the component that sends messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="b611b-113">Aprire **performancemessagesender/Program.cs** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="b611b-113">Open **performancemessagesender/Program.cs** in the editor.</span></span>

1. <span data-ttu-id="b611b-114">Individuare il metodo `SendPerformanceMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="b611b-114">Locate the `SendPerformanceMessageAsync()` method.</span></span>

1. <span data-ttu-id="b611b-115">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-115">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Topic Client here
    ```

1. <span data-ttu-id="b611b-116">Per creare un client di argomento, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-116">To create a topic client, replace that line of code with the following code:</span></span>

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. <span data-ttu-id="b611b-117">All'interno del blocco `try...catch` individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-117">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="b611b-118">Per creare e formattare un messaggio per la coda, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-118">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="b611b-119">Per visualizzare il messaggio nella console, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-119">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="b611b-120">Per inviare il messaggio alla coda, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-120">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await topicClient.SendAsync(message);
    ```

1. <span data-ttu-id="b611b-121">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-121">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the topic here
    ```

1. <span data-ttu-id="b611b-122">Per chiudere la connessione al bus di servizio, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-122">To close the connection to Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await topicClient.CloseAsync();
    ```

1. <span data-ttu-id="b611b-123">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="b611b-123">Save the file.</span></span>

## <a name="send-a-message-to-the-topic"></a><span data-ttu-id="b611b-124">Inviare un messaggio all'argomento</span><span class="sxs-lookup"><span data-stu-id="b611b-124">Send a message to the topic</span></span>

<span data-ttu-id="b611b-125">Per eseguire il componente che invia un messaggio su una vendita, eseguire questo comando in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="b611b-125">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p performancemessagesender
```

<span data-ttu-id="b611b-126">Durante l'esecuzione del programma, vengono visualizzati messaggi che indicano che è in corso l'invio di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="b611b-126">As the program executes, you'll see messages printed indicating that it's sending a message.</span></span> <span data-ttu-id="b611b-127">Ogni volta che si esegue l'app, viene aggiunto un ulteriore messaggio all'argomento e ogni abbonato riceve una copia.</span><span class="sxs-lookup"><span data-stu-id="b611b-127">Each time you run the app, one additional message will be added to the topic, and each subscriber will receive a copy.</span></span>

<span data-ttu-id="b611b-128">Al termine, eseguire il comando seguente per vedere quanti messaggi si trovano nella sottoscrizione Americas:</span><span class="sxs-lookup"><span data-stu-id="b611b-128">Once it's finished, run the following command to see how many messages are in the Americas subscription:</span></span>

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

<span data-ttu-id="b611b-129">Se si sostituisce `EuropeAndAfrica` con `Americas`, si può notare che entrambe le sottoscrizioni hanno lo stesso numero di messaggi.</span><span class="sxs-lookup"><span data-stu-id="b611b-129">If you substitute `EuropeAndAfrica` for `Americas`, you should see that both subscriptions have the same number of messages.</span></span>

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a><span data-ttu-id="b611b-130">Scrivere il codice che consente di ricevere un messaggio da una sottoscrizione dell'argomento</span><span class="sxs-lookup"><span data-stu-id="b611b-130">Write code that receives a message from a topic subscription</span></span>

<span data-ttu-id="b611b-131">Per completare il componente che recupera messaggi sull'andamento delle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="b611b-131">To complete the component that retrieves messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="b611b-132">Aprire **performancemessagereceiver/Program.cs** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="b611b-132">Open **performancemessagereceiver/Program.cs** in the editor.</span></span>

1. <span data-ttu-id="b611b-133">Individuare il metodo `MainAsync()`.</span><span class="sxs-lookup"><span data-stu-id="b611b-133">Locate the `MainAsync()` method.</span></span>

1. <span data-ttu-id="b611b-134">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-134">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a subscription client here
    ```

1. <span data-ttu-id="b611b-135">Per creare un client di sottoscrizione, sostituire la riga con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-135">To create a subscription client, replace that line with the following code:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. <span data-ttu-id="b611b-136">Individuare il metodo `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="b611b-136">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="b611b-137">Per configurare le opzioni di gestione dei messaggi, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-137">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="b611b-138">Per registrare il gestore di messaggi, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-138">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="b611b-139">Individuare il metodo `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="b611b-139">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="b611b-140">Il metodo è stato registrato come metodo di gestione dei messaggi in arrivo.</span><span class="sxs-lookup"><span data-stu-id="b611b-140">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="b611b-141">Per visualizzare i messaggi in arrivo nella console, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-141">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="b611b-142">Per rimuovere il messaggio ricevuto dalla sottoscrizione, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-142">To remove the received message from the subscription, on the next line, add the following code:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="b611b-143">Tornare al metodo `MainAsync()` e individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-143">Return to the `MainAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the subscription here
    ```

1. <span data-ttu-id="b611b-144">Per chiudere la connessione al bus di servizio, sostituire il codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b611b-144">To close the connection to Service Bus, replace that code with the following code:</span></span>

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. <span data-ttu-id="b611b-145">In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.</span><span class="sxs-lookup"><span data-stu-id="b611b-145">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-a-topic-subscription"></a><span data-ttu-id="b611b-146">Recuperare un messaggio da una sottoscrizione dell'argomento</span><span class="sxs-lookup"><span data-stu-id="b611b-146">Retrieve a message from a topic subscription</span></span>

<span data-ttu-id="b611b-147">Per eseguire il componente che recupera un messaggio sull'andamento delle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="b611b-147">To run the component that retrieves a message about sales performance, follow these steps:</span></span>

```bash
dotnet run -p performancemessagereceiver
```

<span data-ttu-id="b611b-148">Quando il programma interrompe la stampa delle notifiche relative alla ricezione dei messaggi,</span><span class="sxs-lookup"><span data-stu-id="b611b-148">When the program stops printing notifications that it is receiving messages.</span></span> <span data-ttu-id="b611b-149">premere `Enter` per arrestare l'app.</span><span class="sxs-lookup"><span data-stu-id="b611b-149">press `Enter` to stop the app.</span></span> <span data-ttu-id="b611b-150">Quindi, eseguire lo stesso comando come indicato in precedenza per verificare che non vi siano messaggi rimanenti nella sottoscrizione `Americas`.</span><span class="sxs-lookup"><span data-stu-id="b611b-150">Then, run the same command as before to confirm that there are zero remaining messages in the `Americas` subscription.</span></span>

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

<span data-ttu-id="b611b-151">Se si sostituisce `EuropeAndAfrica` con `Americas`, si noterà che il numero di messaggi non è cambiato.</span><span class="sxs-lookup"><span data-stu-id="b611b-151">If you substitute `EuropeAndAfrica` for `Americas`, you'll see that the message count has not changed.</span></span> <span data-ttu-id="b611b-152">L'applicazione ha ricevuto i messaggi solo dalla sottoscrizione `Americas`.</span><span class="sxs-lookup"><span data-stu-id="b611b-152">The application only received messages from the `Americas` subscription.</span></span>
