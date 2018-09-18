<span data-ttu-id="ce63c-101">Si è scelto di usare una coda del bus di servizio per lo scambio di messaggi relativi alle singole vendite tra l'app per dispositivi mobili usata dal personale di vendita e il servizio Web, ospitato in Azure, che archivierà i dettagli di ogni vendita in un'istanza di database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="ce63c-101">You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.</span></span>

<span data-ttu-id="ce63c-102">Sono già stati implementati gli oggetti necessari nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ce63c-102">You've already implemented the necessary objects in your Azure subscription.</span></span> <span data-ttu-id="ce63c-103">Ora si vuole scrivere il codice che invia i messaggi alla coda e recupera i messaggi.</span><span class="sxs-lookup"><span data-stu-id="ce63c-103">Now, you want to write code that sends messages to that queue and retrieves messages.</span></span>

## <a name="clone-and-open-the-starter-application"></a><span data-ttu-id="ce63c-104">Clonare e aprire l'applicazione iniziale</span><span class="sxs-lookup"><span data-stu-id="ce63c-104">Clone and open the starter application</span></span>

<span data-ttu-id="ce63c-105">In questa unità verranno create due applicazioni console in **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-105">In this unit, you'll build two console applications in **Visual Studio Code**.</span></span> <span data-ttu-id="ce63c-106">La prima applicazione inserisce i messaggi in una coda del bus di servizio e la seconda li recupera.</span><span class="sxs-lookup"><span data-stu-id="ce63c-106">The first application places messages into a Service Bus queue and the second retrieves them.</span></span> <span data-ttu-id="ce63c-107">Le applicazioni fanno parte di un'unica soluzione .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce63c-107">The applications are part of a single .NET Core solution.</span></span> 

<span data-ttu-id="ce63c-108">Iniziare clonando la soluzione:</span><span class="sxs-lookup"><span data-stu-id="ce63c-108">Start by cloning the solution:</span></span>

1. <span data-ttu-id="ce63c-109">Avviare un prompt dei comandi e passare alla directory in cui si vuole ospitare il codice sorgente per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ce63c-109">Start a command prompt and change to the directory where you want to host the source code for the application.</span></span>

1. <span data-ttu-id="ce63c-110">Digitare il comando seguente e quindi premere **INVIO**:</span><span class="sxs-lookup"><span data-stu-id="ce63c-110">Type the following command, and then press **Enter**:</span></span>

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. <span data-ttu-id="ce63c-111">Una volta completata l'operazione di clonazione, passare alla cartella iniziale.</span><span class="sxs-lookup"><span data-stu-id="ce63c-111">When the clone operation is complete, change to the starter folder.</span></span>

1. <span data-ttu-id="ce63c-112">Digitare il comando seguente e quindi premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-112">Type the following command, and then press **Enter**.</span></span>

    ```powershell
    code .
    ```

1. <span data-ttu-id="ce63c-113">Se viene visualizzato un messaggio in cui viene chiesto se si vuole ripristinare le dipendenze, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-113">If a message appears asking if you want to restore dependencies, click **Yes**.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="ce63c-114">Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="ce63c-114">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="ce63c-115">Per accedere a uno spazio dei nomi del bus di servizio e usare una coda, è necessario configurare due informazioni nelle app console:</span><span class="sxs-lookup"><span data-stu-id="ce63c-115">In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:</span></span>

* <span data-ttu-id="ce63c-116">Endpoint per lo spazio dei nomi</span><span class="sxs-lookup"><span data-stu-id="ce63c-116">The endpoint for your namespace</span></span>
* <span data-ttu-id="ce63c-117">Chiave di accesso condiviso per l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="ce63c-117">The shared access key for authentication</span></span>

<span data-ttu-id="ce63c-118">È possibile ottenere entrambi questi valori dal portale di Azure sotto forma di stringa di connessione completa.</span><span class="sxs-lookup"><span data-stu-id="ce63c-118">Both of these values can be obtained from the Azure portal in the form of a complete connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="ce63c-119">Per semplicità, la stringa di connessione sarà hardcoded nel file **Program.cs** di entrambe le applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="ce63c-119">For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="ce63c-120">In un'applicazione di produzione si potrebbe usare un file di configurazione o anche Azure Key Vault per archiviare la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="ce63c-120">In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.</span></span>

1. <span data-ttu-id="ce63c-121">Passare al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ce63c-121">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="ce63c-122">Nella home page fare clic su **Tutte le risorse**, quindi sullo spazio dei nomi del bus di Servizio creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ce63c-122">In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="ce63c-123">In **IMPOSTAZIONI** fare clic su **Criteri di accesso condiviso**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-123">Under **SETTINGS**, click **Shared Access Policies**.</span></span>

1. <span data-ttu-id="ce63c-124">Nell'elenco dei criteri fare clic su **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-124">In the list of policies, click **RootManageSharedAccessKey**.</span></span>

1. <span data-ttu-id="ce63c-125">A destra della casella di testo **Stringa di connessione primaria** scegliere il pulsante **Fare clic per copiare**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-125">To the right of the **Primary Connection string** text box, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="ce63c-126">Passare a **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-126">Switch to **Visual Studio Code**.</span></span>

1. <span data-ttu-id="ce63c-127">Nel riquadro **Esplora risorse**, nella cartella **privatemessagesender** fare clic sul file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-127">In the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="ce63c-128">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-128">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="ce63c-129">Posizionare il cursore tra le virgolette e quindi premere **CTRL+V**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-129">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="ce63c-130">Nel riquadro **Esplora risorse**, nella cartella **privatemessagereceiver** fare clic sul file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-130">In the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="ce63c-131">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-131">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="ce63c-132">Posizionare il cursore tra le virgolette e quindi premere **CTRL+V**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-132">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="ce63c-133">Fare clic su **File** e quindi su **Salva tutto**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-133">Click **File**, and then click **Save All**.</span></span>

1. <span data-ttu-id="ce63c-134">Chiudere tutte le finestre aperte dell'editor.</span><span class="sxs-lookup"><span data-stu-id="ce63c-134">Close all open editor windows.</span></span>

## <a name="write-code-that-sends-a-message-to-the-queue"></a><span data-ttu-id="ce63c-135">Scrivere il codice che invia un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="ce63c-135">Write code that sends a message to the queue</span></span>

<span data-ttu-id="ce63c-136">Per completare il componente che invia i messaggi sulle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="ce63c-136">To complete the component that sends messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="ce63c-137">In Visual Studio Code, nel riquadro **Esplora risorse**, nella cartella **privatemessagesender** fare clic sul file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-137">In Visual Studio Code, in the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="ce63c-138">Individuare il metodo `SendSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="ce63c-138">Locate the `SendSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="ce63c-139">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-139">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="ce63c-140">Per creare un client di accodamento, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-140">To create a queue client, replace that line of code with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="ce63c-141">All'interno del blocco `try...catch` individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-141">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="ce63c-142">Per creare e formattare un messaggio per la coda, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-142">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="ce63c-143">Per visualizzare il messaggio nella console, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-143">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="ce63c-144">Per inviare il messaggio alla coda, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-144">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.SendAsync(message);
    ```

1. <span data-ttu-id="ce63c-145">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-145">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the queue here
    ```

1. <span data-ttu-id="ce63c-146">Per chiudere la connessione del bus di servizio, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-146">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="ce63c-147">In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.</span><span class="sxs-lookup"><span data-stu-id="ce63c-147">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="send-a-message-to-the-queue"></a><span data-ttu-id="ce63c-148">Inviare un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="ce63c-148">Send a message to the queue</span></span>

<span data-ttu-id="ce63c-149">Per eseguire il componente che invia un messaggio su una vendita, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="ce63c-149">To run the component that sends a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="ce63c-150">In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-150">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="ce63c-151">Nel riquadro **Debug** selezionare **Launch Private Message Sender** (Avvia mittente messaggio privato) nell'elenco a discesa e quindi premere **F5**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-151">In the **Debug** pane, in the drop-down list, select **Launch Private Message Sender**, and then press **F5**.</span></span> <span data-ttu-id="ce63c-152">Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="ce63c-152">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="ce63c-153">Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-153">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="ce63c-154">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ce63c-154">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="ce63c-155">Se lo spazio dei nomi del bus di servizio non viene visualizzato, nella home page fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi del bus di servizio creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ce63c-155">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="ce63c-156">Nel pannello **Spazio dei nomi del bus di servizio**, in **ENTITÀ** fare clic su **Code** e quindi sulla coda **salesmessages**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-156">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue.</span></span> <span data-ttu-id="ce63c-157">In **NUMERO DI MESSAGGI ATTIVI** dovrebbe venire indicato che è stato aggiunto un messaggio alla coda.</span><span class="sxs-lookup"><span data-stu-id="ce63c-157">The **ACTIVE MESSAGE COUNT** should indicate that one message has been added to the queue.</span></span>

## <a name="write-code-that-receives-a-message-from-the-queue"></a><span data-ttu-id="ce63c-158">Scrivere il codice che riceve un messaggio dalla coda</span><span class="sxs-lookup"><span data-stu-id="ce63c-158">Write code that receives a message from the queue</span></span>

<span data-ttu-id="ce63c-159">Per completare il componente che recupera i messaggi sulle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="ce63c-159">To complete the component that retrieves messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="ce63c-160">In Visual Studio Code, nel riquadro **Esplora risorse**, nella cartella **privatemessagereceiver** fare clic sul file **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-160">In Visual Studio Code, in the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="ce63c-161">Individuare il metodo `ReceiveSalesMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="ce63c-161">Locate the `ReceiveSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="ce63c-162">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-162">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="ce63c-163">Per creare un client di accodamento, sostituire la riga con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-163">To create a queue client, replace that line with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="ce63c-164">Individuare il metodo `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="ce63c-164">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="ce63c-165">Per configurare le opzioni di gestione dei messaggi, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-165">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="ce63c-166">Per registrare il gestore di messaggi, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-166">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="ce63c-167">Individuare il metodo `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="ce63c-167">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="ce63c-168">Il metodo è stato registrato come metodo di gestione dei messaggi in arrivo.</span><span class="sxs-lookup"><span data-stu-id="ce63c-168">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="ce63c-169">Per visualizzare i messaggi in arrivo nella console, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-169">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="ce63c-170">Per rimuovere il messaggio ricevuto dalla coda, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-170">To remove the received message from the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="ce63c-171">Tornare al metodo `ReceiveSalesMessageAsync()` e individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-171">Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the queue here
    ```

1. <span data-ttu-id="ce63c-172">Per chiudere la connessione al bus di servizio, sostituire la riga con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ce63c-172">To close the connection to Service Bus, replace that line with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="ce63c-173">In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.</span><span class="sxs-lookup"><span data-stu-id="ce63c-173">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-the-queue"></a><span data-ttu-id="ce63c-174">Recuperare un messaggio dalla coda</span><span class="sxs-lookup"><span data-stu-id="ce63c-174">Retrieve a message from the queue</span></span>

<span data-ttu-id="ce63c-175">Per eseguire il componente che recupera un messaggio su una vendita, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="ce63c-175">To run the component that retrieves a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="ce63c-176">In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-176">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="ce63c-177">Nel riquadro **Debug** selezionare **Launch Private Message Receiver** (Avvia destinatario messaggio privato) nell'elenco a discesa e quindi premere **F5**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-177">In the **Debug** pane, in the drop-down list, select **Launch Private Message Receiver**, and then press **F5**.</span></span> <span data-ttu-id="ce63c-178">Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="ce63c-178">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="ce63c-179">Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-179">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="ce63c-180">Quando si nota che il messaggio è stato ricevuto e viene visualizzato nella console, scegliere **Arresta debug**dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-180">When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.</span></span>

1. <span data-ttu-id="ce63c-181">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ce63c-181">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="ce63c-182">Se lo spazio dei nomi del bus di servizio non viene visualizzato, nella home page fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi del bus di servizio creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ce63c-182">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="ce63c-183">Nel pannello **Spazio dei nomi del bus di servizio**, in **ENTITÀ** fare clic su **Code** e quindi sulla coda **salesmessages**.</span><span class="sxs-lookup"><span data-stu-id="ce63c-183">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue.</span></span> <span data-ttu-id="ce63c-184">In **NUMERO DI MESSAGGI ATTIVI** dovrebbe venire indicato che il messaggio è stato rimosso dalla coda.</span><span class="sxs-lookup"><span data-stu-id="ce63c-184">The **ACTIVE MESSAGE COUNT** should indicate that the message has been removed from the queue.</span></span>

<span data-ttu-id="ce63c-185">È stato scritto codice che invia un messaggio sulle singole vendite a una coda del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="ce63c-185">You have written code that sends a message about individual sales to a Service Bus queue.</span></span> <span data-ttu-id="ce63c-186">Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nell'app per dispositivi mobili usata dal personale di vendita nei dispositivi.</span><span class="sxs-lookup"><span data-stu-id="ce63c-186">In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.</span></span>

<span data-ttu-id="ce63c-187">È stato scritto anche il codice che riceve un messaggio dalla coda del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="ce63c-187">You have also written code that receives a message from the Service Bus queue.</span></span> <span data-ttu-id="ce63c-188">Nell'applicazione distribuita per la forza vendita è necessario scrivere questo codice nel servizio Web in esecuzione in Azure che elabora i messaggi ricevuti.</span><span class="sxs-lookup"><span data-stu-id="ce63c-188">In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.</span></span>
