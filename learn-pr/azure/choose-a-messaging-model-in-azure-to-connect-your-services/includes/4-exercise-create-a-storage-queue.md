<span data-ttu-id="b02af-101">In questa unità verrà creato un nuovo account di archiviazione nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="b02af-101">In this unit, you will create a new storage account in your Azure subscription.</span></span> <span data-ttu-id="b02af-102">Si userà quindi Azure Cloud Shell per creare una nuova coda, aggiungere un messaggio e quindi leggerlo e rimuoverlo dalla coda.</span><span class="sxs-lookup"><span data-stu-id="b02af-102">You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="b02af-103">Queste sono le stesse azioni eseguite dai componenti in un'applicazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="b02af-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="b02af-104">Ad esempio, un'app per dispositivi mobili può aggiungere un messaggio a una coda, in cui attende che un servizio Web lo recuperi e lo elabori.</span><span class="sxs-lookup"><span data-stu-id="b02af-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="b02af-105">Creare un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="b02af-105">Create a storage account</span></span>
<!---TODO: Update for sandbox.--->

<span data-ttu-id="b02af-106">Poiché le code di archiviazione di Azure fanno parte degli account di archiviazione per utilizzo generico di Azure, è necessario iniziare creando un account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="b02af-106">Since Azure Storage queues are part of Azure general-purpose storage accounts, you must start by creating a storage account:</span></span>

1. <span data-ttu-id="b02af-107">In un browser passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e accedere con le credenziali normali.</span><span class="sxs-lookup"><span data-stu-id="b02af-107">In a browser, navigate to the [Azure portal](https://portal.azure.com?azure-portal=true), and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="b02af-108">In alto a sinistra fare clic su **Tutti i servizi**.</span><span class="sxs-lookup"><span data-stu-id="b02af-108">In the top left, click **All services**.</span></span>

1. <span data-ttu-id="b02af-109">Scorrere verso il basso fino alla sezione **Archiviazione** e quindi fare clic su **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="b02af-109">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>

1. <span data-ttu-id="b02af-110">In alto a sinistra nel pannello **Account di archiviazione** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="b02af-110">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

  ![Screenshot del pannello Account di archiviazione con Aggiungi evidenziato](../media-draft/4-create-a-storage-account-1.png)

1. <span data-ttu-id="b02af-112">Nella finestra di dialogo risultante immettere le informazioni seguenti. Ognuna di queste opzioni ha un'icona `(i)` nel portale, che è possibile usare per ottenere altre informazioni sull'opzione.</span><span class="sxs-lookup"><span data-stu-id="b02af-112">In the resulting dialog, enter the following information, each of these options has a `(i)` icon in the portal which you can use to get more information about what the option does.</span></span>

    - <span data-ttu-id="b02af-113">Nella casella di testo **Nome** digitare un nome univoco per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b02af-113">In the **Name** text box, type a unique name for the storage account.</span></span>
    - <span data-ttu-id="b02af-114">In **Modello di distribuzione** assicurarsi che l'opzione **Resource Manager** sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="b02af-114">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
    - <span data-ttu-id="b02af-115">Nell'elenco a discesa **Tipologia account** selezionare **Archiviazione (utilizzo generico v2)**.</span><span class="sxs-lookup"><span data-stu-id="b02af-115">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
    - <span data-ttu-id="b02af-116">Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.</span><span class="sxs-lookup"><span data-stu-id="b02af-116">In the **Location** drop-down list, select a region near you.</span></span>
    - <span data-ttu-id="b02af-117">Nell'elenco a discesa **Replica** selezionare **Archiviazione con ridondanza locale**.</span><span class="sxs-lookup"><span data-stu-id="b02af-117">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
    - <span data-ttu-id="b02af-118">In **Prestazioni** selezionare **Standard**.</span><span class="sxs-lookup"><span data-stu-id="b02af-118">Under **Performance**, select **Standard**.</span></span>
    - <span data-ttu-id="b02af-119">In **Livello di accesso** selezionare **Accesso sporadico**.</span><span class="sxs-lookup"><span data-stu-id="b02af-119">Under **Access tier**, select **Cool**.</span></span>
    - <span data-ttu-id="b02af-120">In **Trasferimento sicuro obbligatorio** selezionare **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="b02af-120">Under **Secure transfer required**, select **Disabled**.</span></span>
    - <span data-ttu-id="b02af-121">In **Sottoscrizione** selezionare la propria sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="b02af-121">Under **Subscription**, select your subscription.</span></span>
    - <span data-ttu-id="b02af-122">In **Gruppo di risorse** selezionare **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="b02af-122">Under **Resource group**, select **Create new**.</span></span> <span data-ttu-id="b02af-123">Nella casella di testo digitare **MusicSharingResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b02af-123">In the text box, type **MusicSharingResourceGroup**.</span></span>
    - <span data-ttu-id="b02af-124">In **Reti virtuali** selezionare **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="b02af-124">Under **Virtual networks**, select **Disabled**.</span></span> 

    ![Screenshot della finestra di dialogo Crea account di archiviazione](../media-draft/4-create-a-storage-account-2.png)

1. <span data-ttu-id="b02af-126">Fare clic su **Crea**: Azure creerà un nuovo gruppo di risorse con un nuovo account di archiviazione associato.</span><span class="sxs-lookup"><span data-stu-id="b02af-126">Click **Create** - Azure will create a new resource group and a new storage account associated with it.</span></span>

    ![Screenshot della finestra di dialogo Crea account di archiviazione, con Crea evidenziato](../media-draft/4-create-a-storage-account-3.png)

## <a name="create-a-queue"></a><span data-ttu-id="b02af-128">Creare una coda</span><span class="sxs-lookup"><span data-stu-id="b02af-128">Create a queue</span></span>

<span data-ttu-id="b02af-129">Ora che è stato creato l'account di archiviazione, è possibile aggiungere una nuova coda.</span><span class="sxs-lookup"><span data-stu-id="b02af-129">Now that the storage account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="b02af-130">È necessario creare la coda usando i comandi di PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b02af-130">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="b02af-131">In alto a destra nel portale fare clic sul collegamento **Cloud Shell** `(>_)`.</span><span class="sxs-lookup"><span data-stu-id="b02af-131">In the top right of the portal, click the **Cloud Shell** link `(>_)`.</span></span>

    ![Screenshot del portale di Azure con l'icona Cloud Shell evidenziata](../media-draft/4-create-a-storage-queue-1.png)

1. <span data-ttu-id="b02af-133">Nella schermata **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="b02af-133">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>

1. <span data-ttu-id="b02af-134">Se viene visualizzata la schermata **Non sono state montate risorse di archiviazione** fare clic su **Crea risorsa di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="b02af-134">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>

1. <span data-ttu-id="b02af-135">Quando viene visualizzato il prompt `PS Azure`, per ottenere l'account di archiviazione, digitare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="b02af-135">When the `PS Azure` prompt appears, to obtain the storage account, type the following command.</span></span> <span data-ttu-id="b02af-136">Sostituire `<storageaccountname>` con il nome univoco dell'account di archiviazione creato prima e quindi premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="b02af-136">Substitute `<storageaccountname>` with the unique name of your storage account you created above, and then press **Enter**.</span></span> <span data-ttu-id="b02af-137">Si vuole assegnare l'oggetto risultante a una variabile denominata `$storageaccount`.</span><span class="sxs-lookup"><span data-stu-id="b02af-137">We want to assign the resulting object to a variable named `$storageaccount`.</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="b02af-138">È quindi necessario ottenere il _contesto_ dell'account di archiviazione, che è una proprietà per l'oggetto restituito.</span><span class="sxs-lookup"><span data-stu-id="b02af-138">Next, we need to get the storage account _context_ - this is a property on the returned object.</span></span> <span data-ttu-id="b02af-139">Verrà ora assegnato a un'altra variabile denominata `$context`.</span><span class="sxs-lookup"><span data-stu-id="b02af-139">Let's assign it to another variable named `$context`.</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="b02af-140">A questo punto è possibile creare la coda.</span><span class="sxs-lookup"><span data-stu-id="b02af-140">Now we are ready to create the queue.</span></span> <span data-ttu-id="b02af-141">Usare il comando `New-AzureStorageQueue` e assegnarla a una variabile `$messageQueue`.</span><span class="sxs-lookup"><span data-stu-id="b02af-141">Use the `New-AzureStorageQueue` command and assign it to a `$messageQueue` variable.</span></span>
    - <span data-ttu-id="b02af-142">Passare un parametro `-Name` con il valore `musicsharingmessages`</span><span class="sxs-lookup"><span data-stu-id="b02af-142">Pass a `-Name` parameter with the value `musicsharingmessages`</span></span>
    - <span data-ttu-id="b02af-143">Passare un parametro `-Context` con il valore recuperato nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="b02af-143">Pass a `-Context` parameter with the value you retrieved in the previous step.</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="b02af-144">Aggiungere un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="b02af-144">Add a message to the queue</span></span>

<span data-ttu-id="b02af-145">Dopo avere creato una coda nell'account di archiviazione, è possibile aggiungervi un messaggio.</span><span class="sxs-lookup"><span data-stu-id="b02af-145">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="b02af-146">Per creare un nuovo messaggio, usare il metodo `New-Object` per creare un elemento `CloudQueueMessage` .NET con un argomento basato su stringa:</span><span class="sxs-lookup"><span data-stu-id="b02af-146">To create a new message, use the `New-Object` method to create a .NET `CloudQueueMessage` with a string-based argument:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="b02af-147">Per aggiungere il nuovo messaggio alla nuova coda, passare l'elemento `CloudQueueMessage` creato al metodo `AddMessageAsync` nella coda `$messageQueue`.</span><span class="sxs-lookup"><span data-stu-id="b02af-147">To add the new message to the new queue, pass the created `CloudQueueMessage` to the `AddMessageAsync` method on your `$messageQueue` queue.</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

## <a name="verify-the-message-was-queued"></a><span data-ttu-id="b02af-148">Verificare che messaggio sia stato accodato</span><span class="sxs-lookup"><span data-stu-id="b02af-148">Verify the message was queued</span></span>

<span data-ttu-id="b02af-149">Quando si usano le code, è possibile usare **Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b02af-149">We can use the **Storage Explorer** to work with our queue.</span></span> <span data-ttu-id="b02af-150">Sono disponibili due varianti:</span><span class="sxs-lookup"><span data-stu-id="b02af-150">There are two variations available:</span></span>

- <span data-ttu-id="b02af-151">Un'app desktop multipiattaforma per Linux, macOS e Windows che è possibile scaricare.</span><span class="sxs-lookup"><span data-stu-id="b02af-151">A cross-platform desktop app for Linux, macOS, and Windows that you can download.</span></span>
- <span data-ttu-id="b02af-152">Una versione Web di anteprima nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b02af-152">A preview web version in the Azure portal.</span></span> <span data-ttu-id="b02af-153">Quest'ultima sarà quella usata qui, ma è possibile installare la versione desktop se si preferisce. Le istruzioni sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="b02af-153">This is the one we will use here, but you can install the desktop version if you prefer - the instructions are very similar.</span></span>

1. <span data-ttu-id="b02af-154">Fare clic su **Tutte le risorse** nel menu sul lato sinistro del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b02af-154">In the Azure portal, in the navigation on the left, click **All resources**.</span></span>

1. <span data-ttu-id="b02af-155">Nell'elenco delle risorse fare clic sull'account di archiviazione creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b02af-155">In the list of resources, click the storage account you created earlier.</span></span>

1. <span data-ttu-id="b02af-156">Nel pannello dell'account di archiviazione fare clic su **Storage Explorer (anteprima)**.</span><span class="sxs-lookup"><span data-stu-id="b02af-156">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>

1. <span data-ttu-id="b02af-157">In Storage Explorer, sotto **Code**, fare clic su **musicsharingmessages**.</span><span class="sxs-lookup"><span data-stu-id="b02af-157">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="b02af-158">Storage Explorer visualizzerà il messaggio appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="b02af-158">The Storage Explorer should display the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="b02af-159">Recuperare e rimuovere il messaggio</span><span class="sxs-lookup"><span data-stu-id="b02af-159">Retrieve and remove the message</span></span>

<span data-ttu-id="b02af-160">Un componente di destinazione per un messaggio in una coda di archiviazione deve recuperare il messaggio all'inizio della coda.</span><span class="sxs-lookup"><span data-stu-id="b02af-160">A destination component for a message in a Storage queue must retrieve the message at the front of the queue.</span></span> <span data-ttu-id="b02af-161">Il componente di destinazione deve quindi elaborare il messaggio ed eliminarlo dalla coda in modo che gli altri componenti non lo recuperino.</span><span class="sxs-lookup"><span data-stu-id="b02af-161">Then the destination component must process the message and delete it from the queue so that other components do not retrieve it.</span></span>

1. <span data-ttu-id="b02af-162">È possibile recuperare il primo messaggio disponibile in PowerShell usando il metodo `GetMessageAsync` sulla coda.</span><span class="sxs-lookup"><span data-stu-id="b02af-162">We can retrieve the first available message in PowerShell using the `GetMessageAsync` method on our queue.</span></span> <span data-ttu-id="b02af-163">Si tratta di un metodo .NET asincrono. Poiché lo si vuole attendere, è sufficiente usare la proprietà `Result` per ottenere il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="b02af-163">This is an asynchronous .NET method, since we want to wait for it we can just use the `Result` property to get the return value.</span></span> <span data-ttu-id="b02af-164">Viene restituito un oggetto che rappresenta il messaggio che è possibile assegnare a un parametro.</span><span class="sxs-lookup"><span data-stu-id="b02af-164">This returns an object representing the message which we can assign to a parameter.</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="b02af-165">È possibile ottenere una versione testuale del messaggio chiamando `AsString`. In questo modo il valore verrà visualizzato nella console.</span><span class="sxs-lookup"><span data-stu-id="b02af-165">We can get a textual version of the message by calling `AsString` - this will output the value on the console.</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="b02af-166">In alternativa, è possibile visualizzare tutte le proprietà del messaggio digitando il nome della variabile e premendo **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="b02af-166">Or, we can display all the properties of the message by just typing the variable name and pressing **Enter**.</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="b02af-167">`GetMessageAsync` *non* rimuove il messaggio, semplicemente lo restituisce ed è quindi possibile elaborarlo nuovamente.</span><span class="sxs-lookup"><span data-stu-id="b02af-167">`GetMessageAsync` does *not* remove the message - it simply returns it, which means we could process it again.</span></span> <span data-ttu-id="b02af-168">Per rimuovere il messaggio dalla coda, è possibile usare il metodo `DeleteMessageAsync` sulla coda. A questo scopo, è necessario passare il messaggio che si vuole rimuovere.</span><span class="sxs-lookup"><span data-stu-id="b02af-168">To remove the message from the queue, we can use the `DeleteMessageAsync` method on the queue - this requires that we pass in the message we want to remove.</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="b02af-169">Per verificare che il messaggio sia stato rimosso, aggiornare la visualizzazione della coda nel portale di Azure passando al pannello Account di archiviazione e selezionando **Panoramica> Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b02af-169">To verify that the message is gone, refresh the queue display in the Azure portal by navigating to the Storage Account blade and selecting **Overview > Storage Explorer**.</span></span> <span data-ttu-id="b02af-170">In **Code** fare clic su **musicsharingmessages**.</span><span class="sxs-lookup"><span data-stu-id="b02af-170">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="b02af-171">Storage Explorer mostrerà ora che la coda è vuota perché è stato rimosso l'unico messaggio.</span><span class="sxs-lookup"><span data-stu-id="b02af-171">The Storage Explorer should now show that the queue is empty because you removed the only message.</span></span>


## <a name="summary"></a><span data-ttu-id="b02af-172">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b02af-172">Summary</span></span>
<span data-ttu-id="b02af-173">Le code dell'account di archiviazione sono un'ottima soluzione quando si vogliono passare _messaggi_ tra i componenti di un'applicazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="b02af-173">Storage account queues are a good solution when you want to pass _messages_ between the components of a distributed application.</span></span> <span data-ttu-id="b02af-174">Si tratta di un'ottima scelta se si vuole garantire il recapito o assicurarsi che i messaggi vengano recapitati nello stesso ordine in cui sono stato inviati.</span><span class="sxs-lookup"><span data-stu-id="b02af-174">This is a great choice if you want to guarantee delivery or to ensure that messages are delivered in the same order you sent them.</span></span> <span data-ttu-id="b02af-175">Le code implicano tuttavia che il mittente e il destinatario conoscano il formato dei dati passati. Esiste infatti tra di essi un contratto di dati implicito che aggiunge una sorta di "accoppiamento" tra i due servizi comunicanti.</span><span class="sxs-lookup"><span data-stu-id="b02af-175">However, queues imply that the sender and receiver understand the format of the data being passed - there's an implied data contract between them which adds a bit of "coupling" between the two communicating services.</span></span>

<span data-ttu-id="b02af-176">Non tutte le architetture devono passare blocchi formattati di dati. Per alcune sono sufficienti messaggi semplici da gestire in base alla modalità fire-and-forget senza dover necessariamente conoscere come verrà gestito il messaggio.</span><span class="sxs-lookup"><span data-stu-id="b02af-176">Not all architectures need to pass formatted blocks of data, some really just need simple messages which we want to fire-and-forget without any knowledge of what will handle the message.</span></span> <span data-ttu-id="b02af-177">In questi scenari una coda non è la scelta ideale.</span><span class="sxs-lookup"><span data-stu-id="b02af-177">In these scenarios, a queue isn't a great choice.</span></span> <span data-ttu-id="b02af-178">Verrà ora esaminata un'altra strategia di messaggistica più adatta a questo stile di comunicazione.</span><span class="sxs-lookup"><span data-stu-id="b02af-178">Let's look at another messaging strategy which is more suited to this style of communication.</span></span>