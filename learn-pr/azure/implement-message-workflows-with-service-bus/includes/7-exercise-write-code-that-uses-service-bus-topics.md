<span data-ttu-id="4f8f2-101">Si è scelto di usare un argomento del bus di servizio di Azure per distribuire i messaggi relativi all'andamento delle vendite nell'applicazione distribuita degli addetti alle vendite.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-101">You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your Sales Force distributed application.</span></span> <span data-ttu-id="4f8f2-102">L'app usata dal personale di vendita sui propri dispositivi mobili invierà messaggi che riassumono i dati di vendita per ogni area e periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-102">The app used by sales personnel on their mobile devices will send messages that summarized sales figures for each area and time period.</span></span> <span data-ttu-id="4f8f2-103">Tali messaggi saranno distribuiti ai servizi Web situati nelle aree operative dell'azienda, comprese le Americhe e l'Europa.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-103">Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.</span></span>

<span data-ttu-id="4f8f2-104">È già stata implementata l'infrastruttura necessaria nella sottoscrizione di Azure, incluso l'argomento e le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-104">You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions.</span></span> <span data-ttu-id="4f8f2-105">Ora si vuole scrivere il codice che invia messaggi all'argomento e recupera i messaggi da ogni sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-105">Now you want to write the code that sends messages to the topic and retrieves messages from each subscription.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="4f8f2-106">Configurare una stringa di connessione a uno spazio dei nomi del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="4f8f2-106">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="4f8f2-107">Iniziare configurando le stringhe di connessione nei componenti di invio e di ricezione:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-107">Start by configuring connection strings in both the sending and receiving components:</span></span>

1. <span data-ttu-id="4f8f2-108">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-108">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="4f8f2-109">Nella home page fare clic su **Tutte le risorse**, quindi sullo spazio dei nomi del bus di Servizio creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-109">In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="4f8f2-110">In **IMPOSTAZIONI** fare clic su **Criteri di accesso condiviso**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-110">Under **SETTINGS**, click **Shared Access Policies**.</span></span>

1. <span data-ttu-id="4f8f2-111">Nell'elenco dei criteri fare clic su **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-111">In the list of policies, click **RootManageSharedAccessKey**.</span></span>

1. <span data-ttu-id="4f8f2-112">A destra della casella di testo **Stringa di connessione primaria** scegliere il pulsante **Fare clic per copiare**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-112">To the right of the **Primary Connection string** textbox, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="4f8f2-113">Passare a **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-113">Switch to **Visual Studio Code**.</span></span>

1. <span data-ttu-id="4f8f2-114">Nel riquadro **Esplora risorse** fare clic sul file **Program.cs** nella cartella **performancemessagesender**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-114">In the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="4f8f2-115">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-115">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="4f8f2-116">Posizionare il cursore tra le virgolette e quindi premere **Ctrl + V**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-116">Place the cursor between the quotation marks and then press **Ctrl-V**.</span></span>

1. <span data-ttu-id="4f8f2-117">Nel riquadro **Esplora risorse** fare clic sul file **Program.cs** nella cartella **performancemessagereceiver**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-117">In the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="4f8f2-118">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-118">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="4f8f2-119">Posizionare il cursore tra le virgolette e quindi premere **Ctrl + V**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-119">Place the cursor between the quotation marks and then press **Ctrl-V**.</span></span>

1. <span data-ttu-id="4f8f2-120">Fare clic su **File** e quindi su **Salva tutto**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-120">Click **File** and then click **Save All**.</span></span>

1. <span data-ttu-id="4f8f2-121">Chiudere tutte le finestre aperte dell'editor.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-121">Close all open editor windows.</span></span>

## <a name="write-code-that-sends-a-message-to-the-topic"></a><span data-ttu-id="4f8f2-122">Scrivere il codice che invia un messaggio all'argomento</span><span class="sxs-lookup"><span data-stu-id="4f8f2-122">Write code that sends a message to the topic</span></span>

<span data-ttu-id="4f8f2-123">Per completare il componente che invia messaggi sull'andamento delle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-123">To complete the component that sends messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="4f8f2-124">Nel riquadro **Esplora risorse** di Visual Studio Code fare clic sul file **Program.cs** nella cartella **performancemessagesender**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-124">In Visual Studio Code, in the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="4f8f2-125">Individuare il metodo `SendPerformanceMessageAsync()`.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-125">Locate the `SendPerformanceMessageAsync()` method.</span></span>

1. <span data-ttu-id="4f8f2-126">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-126">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Topic Client here
    ```

1. <span data-ttu-id="4f8f2-127">Per creare un client di argomento, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-127">To create a topic client, replace that line of code with the following code:</span></span>

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. <span data-ttu-id="4f8f2-128">All'interno del blocco `try...catch` individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-128">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="4f8f2-129">Per creare e formattare un messaggio per la coda, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-129">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="4f8f2-130">Per visualizzare il messaggio nella console, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-130">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="4f8f2-131">Per inviare il messaggio alla coda, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-131">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await topicClient.SendAsync(message);
    ```

1. <span data-ttu-id="4f8f2-132">Individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-132">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the topic here
    ```

1. <span data-ttu-id="4f8f2-133">Per chiudere la connessione del bus di servizio, sostituire la riga di codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-133">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await topicClient.CloseAsync();
    ```

1. <span data-ttu-id="4f8f2-134">In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-134">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="send-a-message-to-the-topic"></a><span data-ttu-id="4f8f2-135">Inviare un messaggio all'argomento</span><span class="sxs-lookup"><span data-stu-id="4f8f2-135">Send a message to the topic</span></span>

<span data-ttu-id="4f8f2-136">Per eseguire il componente che invia un messaggio su una vendita, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-136">To run the component that sends a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="4f8f2-137">In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-137">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="4f8f2-138">Nel riquadro **Debug** selezionare **Avvia Private Message Sender** nell'elenco a discesa e quindi premere **F5**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-138">In the **Debug** pane, in the drop-down list, select **Launch Performance Message Sender** and then press **F5**.</span></span> <span data-ttu-id="4f8f2-139">Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-139">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="4f8f2-140">Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-140">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="4f8f2-141">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-141">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="4f8f2-142">Se il bus di servizio non viene visualizzato, nella home page fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi del bus di servizio creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-142">If the Service Bus is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="4f8f2-143">Nel pannello **Spazio dei nomi del bus di servizio** fare clic su **Argomenti** in **ENTITÀ** e quindi sull'argomento **salesperformancemessages**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-143">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic.</span></span> <span data-ttu-id="4f8f2-144">Nell'elenco delle sottoscrizioni verrà visualizzato un messaggio nelle sottoscrizioni **Americhe** ed **Europa**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-144">In the list of subscriptions, there should be one message displayed in both the **Americas** and **Europe** subscriptions.</span></span>

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a><span data-ttu-id="4f8f2-145">Scrivere il codice che consente di ricevere un messaggio da una sottoscrizione dell'argomento</span><span class="sxs-lookup"><span data-stu-id="4f8f2-145">Write code that receives a message from a topic subscription</span></span>

<span data-ttu-id="4f8f2-146">Per completare il componente che recupera messaggi sull'andamento delle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-146">To complete the component that retrieves messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="4f8f2-147">Nel riquadro **Esplora risorse** di Visual Studio Code fare clic sul file **Program.cs** nella cartella **performancemessagereceiver**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-147">In Visual Studio Code, in the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="4f8f2-148">Individuare il metodo `MainAsync()`.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-148">Locate the `MainAsync()` method.</span></span>

1. <span data-ttu-id="4f8f2-149">All'interno di questo metodo individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-149">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Subscription Client here
    ```

1. <span data-ttu-id="4f8f2-150">Per creare un client di sottoscrizione, sostituire la riga con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-150">To create a subscription client, replace that line with the following code:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. <span data-ttu-id="4f8f2-151">Individuare il metodo `RegisterMessageHandler()`.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-151">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="4f8f2-152">Per configurare le opzioni di gestione dei messaggi, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-152">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="4f8f2-153">Per registrare il gestore di messaggi, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-153">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="4f8f2-154">Individuare il metodo `ProcessMessagesAsync()`.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-154">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="4f8f2-155">Il metodo è stato registrato come metodo di gestione dei messaggi in arrivo.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-155">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="4f8f2-156">Per visualizzare i messaggi in arrivo nella console, sostituire tutto il codice all'interno del metodo con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-156">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="4f8f2-157">Per rimuovere il messaggio ricevuto dalla sottoscrizione, nella riga successiva aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-157">To remove the received message from the subscription, on the next line, add the following code:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="4f8f2-158">Tornare al metodo `MainAsync()` e individuare la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-158">Return to the `MainAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the subscription here
    ```

1. <span data-ttu-id="4f8f2-159">Per chiudere la connessione al bus di servizio, sostituire il codice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-159">To close the connection to Service Bus, replace that code with the following code:</span></span>

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. <span data-ttu-id="4f8f2-160">In Visual Studio Code chiudere tutte le finestre dell'editor e salvare tutti i file modificati.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-160">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-a-topic-subscription"></a><span data-ttu-id="4f8f2-161">Recuperare un messaggio da una sottoscrizione dell'argomento</span><span class="sxs-lookup"><span data-stu-id="4f8f2-161">Retrieve a message from a topic subscription</span></span>

<span data-ttu-id="4f8f2-162">Per eseguire il componente che recupera un messaggio sull'andamento delle vendite, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="4f8f2-162">To run the component that retrieves a message about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="4f8f2-163">In Visual Studio Code scegliere **Debug** dal menu **Visualizza**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-163">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="4f8f2-164">Nel riquadro **Debug** selezionare **Avvia Performance Message Receiver** nell'elenco a discesa e quindi premere **F5**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-164">In the **Debug** pane, in the drop-down list, select **Launch Performance Message Receiver** and then press **F5**.</span></span> <span data-ttu-id="4f8f2-165">Visual Studio Code compila ed esegue l'applicazione console in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-165">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="4f8f2-166">Mentre il programma viene eseguito, esaminare i messaggi nella **Console di debug**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-166">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="4f8f2-167">Quando si nota che il messaggio è stato ricevuto e viene visualizzato nella console, scegliere **Arresta debug**dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-167">When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.</span></span>

1. <span data-ttu-id="4f8f2-168">Accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-168">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="4f8f2-169">Se il bus di servizio non viene visualizzato, nella home page fare clic su **Tutte le risorse** e quindi sullo spazio dei nomi del bus di servizio creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-169">If the Service Bus is not display, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="4f8f2-170">Nel pannello **Spazio dei nomi del bus di servizio** fare clic su **Argomenti** in **ENTITÀ** e quindi sull'argomento **salesperformancemessages**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-170">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic.</span></span> <span data-ttu-id="4f8f2-171">Nell'elenco delle sottoscrizioni non verranno visualizzati messaggi nella sottoscrizione **Americhe** perché l'applicazione ha elaborato e rimosso l'unico messaggio presente.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-171">In the list of subscriptions, there should be zero messages displayed in the **Americas** subscription because your application has processed and removed the only message.</span></span> <span data-ttu-id="4f8f2-172">Si noti che il messaggio è ancora presente nella sottoscrizione **Europa**.</span><span class="sxs-lookup"><span data-stu-id="4f8f2-172">Notice that the message is still present in the **Europe** subscription.</span></span>
