<span data-ttu-id="7e15c-101">Si è scelto di usare una coda del bus di servizio per lo scambio di messaggi relativi alle singole vendite tra l'app per dispositivi mobili usata dal personale di vendita e il servizio Web, ospitato in Azure, che archivierà i dettagli di ogni vendita in un'istanza di database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="7e15c-101">You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.</span></span>

<span data-ttu-id="7e15c-102">Sono già stati implementati gli oggetti necessari nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="7e15c-102">You've already implemented the necessary objects in your Azure subscription.</span></span> <span data-ttu-id="7e15c-103">Ora si vuole scrivere il codice che invia i messaggi alla coda e recupera i messaggi.</span><span class="sxs-lookup"><span data-stu-id="7e15c-103">Now, you want to write code that sends messages to that queue and retrieves messages.</span></span>

## <a name="clone-and-open-the-starter-application"></a><span data-ttu-id="7e15c-104">Clonare e aprire l'applicazione iniziale</span><span class="sxs-lookup"><span data-stu-id="7e15c-104">Clone and open the starter application</span></span>

<span data-ttu-id="7e15c-105">In questa unità verranno create due applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="7e15c-105">In this unit, you'll build two console applications.</span></span> <span data-ttu-id="7e15c-106">La prima applicazione inserisce i messaggi in una coda del bus di servizio e la seconda li recupera.</span><span class="sxs-lookup"><span data-stu-id="7e15c-106">The first application places messages into a Service Bus queue and the second retrieves them.</span></span> <span data-ttu-id="7e15c-107">Le applicazioni fanno parte di un'unica soluzione .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e15c-107">The applications are part of a single .NET Core solution.</span></span>

1. <span data-ttu-id="7e15c-108">Per iniziare, clonare la soluzione eseguendo i comandi seguenti in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="7e15c-108">Start by cloning the solution: run the following commands in the Cloud Shell:</span></span>

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. <span data-ttu-id="7e15c-109">Passare quindi alla cartella iniziale e aprire l'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="7e15c-109">Next, change directories into the starter folder and open the Cloud Shell editor.</span></span>

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="7e15c-110">Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="7e15c-110">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="7e15c-111">Per accedere a uno spazio dei nomi del bus di servizio e usare una coda, è necessario configurare due informazioni nelle app console:</span><span class="sxs-lookup"><span data-stu-id="7e15c-111">In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:</span></span>

* <span data-ttu-id="7e15c-112">Endpoint per lo spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="7e15c-112">The endpoint for your namespace</span></span>
* <span data-ttu-id="7e15c-113">Chiave di accesso condiviso per l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="7e15c-113">The shared access key for authentication</span></span>

<span data-ttu-id="7e15c-114">È possibile ottenere entrambi questi valori dal portale di Azure sotto forma di stringa di connessione completa.</span><span class="sxs-lookup"><span data-stu-id="7e15c-114">Both of these values can be obtained from the Azure portal in the form of a complete connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="7e15c-115">Per semplicità, la stringa di connessione sarà hardcoded nel file **Program.cs** di entrambe le applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="7e15c-115">For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="7e15c-116">In un'applicazione di produzione si potrebbe usare un file di configurazione o anche Azure Key Vault per archiviare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="7e15c-116">In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.</span></span>

1. <span data-ttu-id="7e15c-117">In Cloud Shell eseguire il comando seguente per visualizzare la stringa di connessione primaria per lo spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7e15c-117">In the Cloud Shell, run the following command to display the primary connection string for your Service Bus namespace.</span></span> <span data-ttu-id="7e15c-118">Sostituire `<namespace-name>` con il nome dello spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7e15c-118">Replace `<namespace-name>` with the name of your Service Bus namespace.</span></span>

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --namespace-name <namespace-name> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv
    ```

    <span data-ttu-id="7e15c-119">Questa stringa di connessione dovrà essere usata più volte in questo modulo, pertanto è consigliabile incollarla in una posizione accessibile.</span><span class="sxs-lookup"><span data-stu-id="7e15c-119">You'll be needing this connection string multiple times throughout this module, so you might want to paste it somewhere handy.</span></span>

1. <span data-ttu-id="7e15c-120">Copiare la chiave da Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="7e15c-120">Copy the key from Cloud Shell.</span></span> <span data-ttu-id="7e15c-121">Nell'editor aprire **privatemessagesender/Program.cs** e individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-121">In the editor, open **privatemessagesender/Program.cs** and locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    <span data-ttu-id="7e15c-122">Incollare la stringa di connessione tra virgolette.</span><span class="sxs-lookup"><span data-stu-id="7e15c-122">Paste the connection string between the quotation marks.</span></span> <span data-ttu-id="7e15c-123">Salvare il file con <kbd>CTRL+S</kbd>.</span><span class="sxs-lookup"><span data-stu-id="7e15c-123">Save the file with <kbd>Ctrl+S</kbd>.</span></span>

1. <span data-ttu-id="7e15c-124">Ripetere il passaggio precedente in **privatemessagereceiver/Program.cs**, incollando lo stesso valore di stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="7e15c-124">Repeat the previous step in **privatemessagereceiver/Program.cs**, pasting in the same connection string value.</span></span> <span data-ttu-id="7e15c-125">Salvare il file tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux, <kbd>Comando+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="7e15c-125">Save the file either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

## <a name="write-code-that-sends-a-message-to-the-queue"></a><span data-ttu-id="7e15c-126">Scrivere il codice che invia un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="7e15c-126">Write code that sends a message to the queue</span></span>

<span data-ttu-id="7e15c-127">Per completare il componente che invia i messaggi sulle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="7e15c-127">To complete the component that sends messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="7e15c-128">Aprire **privatemessagesender/Program.cs** nell'editor</span><span class="sxs-lookup"><span data-stu-id="7e15c-128">Open **privatemessagesender/Program.cs** in the editor</span></span>

1. <span data-ttu-id="7e15c-129">Individuare il metodo `SendSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="7e15c-129">Locate the `SendSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="7e15c-130">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-130">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="7e15c-131">Per creare un client di accodamento, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-131">To create a queue client, replace that line of code with the following code:</span></span>

    ```C#
    var queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="7e15c-132">All'interno del blocco `try...catch` individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-132">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="7e15c-133">Per creare e formattare un messaggio per la coda, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-133">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="7e15c-134">Per visualizzare il messaggio nella console, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-134">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="7e15c-135">Per inviare il messaggio alla coda, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-135">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.SendAsync(message);
    ```

1. <span data-ttu-id="7e15c-136">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-136">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the queue here
    ```

1. <span data-ttu-id="7e15c-137">Per chiudere la connessione del bus di servizio, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-137">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="7e15c-138">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7e15c-138">Save the file.</span></span>

## <a name="send-a-message-to-the-queue"></a><span data-ttu-id="7e15c-139">Inviare un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="7e15c-139">Send a message to the queue</span></span>

<span data-ttu-id="7e15c-140">Per eseguire il componente che invia un messaggio su una vendita, eseguire questo comando in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="7e15c-140">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> <span data-ttu-id="7e15c-141">L'avvio delle app eseguite durante questo esercizio potrebbe richiedere qualche istante, dal momento che `dotnet` deve ripristinare i pacchetti da origini remote e compilare le app la prima volta che vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="7e15c-141">The apps you run during this exercise may take a moment to start up, as `dotnet` has to restore packages from remote sources and build the apps the first time they are run.</span></span>

<span data-ttu-id="7e15c-142">Durante l'esecuzione del programma, vengono visualizzati messaggi che indicano che è in corso l'invio di un messaggio.</span><span class="sxs-lookup"><span data-stu-id="7e15c-142">As the program executes, you'll see messages printed indicating that it's sending a message.</span></span> <span data-ttu-id="7e15c-143">Ogni volta che si esegue l'app, viene aggiunto un ulteriore messaggio alla coda.</span><span class="sxs-lookup"><span data-stu-id="7e15c-143">Each time you run the app, one additional message will be added to the queue.</span></span>

<span data-ttu-id="7e15c-144">Al termine, eseguire il comando seguente per vedere quanti messaggi si trovano nella coda:</span><span class="sxs-lookup"><span data-stu-id="7e15c-144">Once it's finished, run the following command to see how many messages are in the queue:</span></span>

```azurecli
az servicebus queue show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a><span data-ttu-id="7e15c-145">Scrivere il codice che riceve un messaggio dalla coda</span><span class="sxs-lookup"><span data-stu-id="7e15c-145">Write code that receives a message from the queue</span></span>

1. <span data-ttu-id="7e15c-146">Aprire **privatemessagereceiver/Program.cs** nell'editor</span><span class="sxs-lookup"><span data-stu-id="7e15c-146">Open **privatemessagereceiver/Program.cs** in the editor</span></span>

1. <span data-ttu-id="7e15c-147">Individuare il metodo `ReceiveSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="7e15c-147">Locate the `ReceiveSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="7e15c-148">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-148">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="7e15c-149">Per creare un client di accodamento, sostituire la riga con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-149">To create a queue client, replace that line with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="7e15c-150">Individuare il metodo `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="7e15c-150">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="7e15c-151">Per configurare le opzioni di gestione dei messaggi, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-151">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="7e15c-152">Per registrare il gestore di messaggi, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-152">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="7e15c-153">Individuare il metodo `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="7e15c-153">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="7e15c-154">Il metodo è stato registrato come metodo di gestione dei messaggi in arrivo.</span><span class="sxs-lookup"><span data-stu-id="7e15c-154">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="7e15c-155">Per visualizzare i messaggi in arrivo nella console, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-155">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="7e15c-156">Per rimuovere il messaggio ricevuto dalla coda, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-156">To remove the received message from the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="7e15c-157">Tornare al metodo `ReceiveSalesMessageAsync()` e individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-157">Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the queue here
    ```

1. <span data-ttu-id="7e15c-158">Per chiudere la connessione al bus di servizio, sostituire la riga con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e15c-158">To close the connection to Service Bus, replace that line with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="7e15c-159">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="7e15c-159">Save the file.</span></span>

## <a name="retrieve-a-message-from-the-queue"></a><span data-ttu-id="7e15c-160">Recuperare un messaggio dalla coda</span><span class="sxs-lookup"><span data-stu-id="7e15c-160">Retrieve a message from the queue</span></span>

<span data-ttu-id="7e15c-161">Per eseguire il componente che invia un messaggio su una vendita, eseguire questo comando in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="7e15c-161">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p privatemessagereceiver
```

<span data-ttu-id="7e15c-162">Quando si nota che il messaggio è stato ricevuto e viene visualizzato nella console, premere `Enter` per arrestare l'app.</span><span class="sxs-lookup"><span data-stu-id="7e15c-162">When you see that the message has been received and displayed in the console, press `Enter` to stop the app.</span></span> <span data-ttu-id="7e15c-163">Eseguire quindi lo stesso comando di prima per confermare che tutti i messaggi sono stati rimossi dalla coda:</span><span class="sxs-lookup"><span data-stu-id="7e15c-163">Then, run the same command as before to confirm that all of the messages have been removed from the queue:</span></span>

```azurecli
az servicebus queue show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

<span data-ttu-id="7e15c-164">Verrà visualizzato `0` se tutti i messaggi sono stati rimossi.</span><span class="sxs-lookup"><span data-stu-id="7e15c-164">This will show `0` if all the messages have been removed.</span></span>

<span data-ttu-id="7e15c-165">È stato scritto codice che invia un messaggio sulle singole vendite a una coda del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7e15c-165">You have written code that sends a message about individual sales to a Service Bus queue.</span></span> <span data-ttu-id="7e15c-166">Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nell'app per dispositivi mobili usata dal personale di vendita nei dispositivi.</span><span class="sxs-lookup"><span data-stu-id="7e15c-166">In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.</span></span>

<span data-ttu-id="7e15c-167">È stato scritto anche il codice che riceve un messaggio dalla coda del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="7e15c-167">You have also written code that receives a message from the Service Bus queue.</span></span> <span data-ttu-id="7e15c-168">Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nel servizio Web in esecuzione in Azure che elabora i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="7e15c-168">In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.</span></span>
