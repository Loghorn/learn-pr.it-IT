<span data-ttu-id="c63d3-101">In questo esercizio si creerà un nuovo account di archiviazione nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="c63d3-101">In this exercise, you will create a new storage account in your Azure subscription.</span></span> <span data-ttu-id="c63d3-102">Si userà quindi Azure Cloud Shell per creare una nuova coda, aggiungere un messaggio e quindi leggere il messaggio e rimuoverlo dalla coda.</span><span class="sxs-lookup"><span data-stu-id="c63d3-102">You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="c63d3-103">Queste sono le stesse azioni eseguite dai componenti in un'applicazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="c63d3-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="c63d3-104">Ad esempio, un'app per dispositivi mobili può aggiungere un messaggio a una coda, in cui attende che un servizio Web lo recuperi e lo elabori.</span><span class="sxs-lookup"><span data-stu-id="c63d3-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="c63d3-105">Creare un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="c63d3-105">Create a storage account</span></span>

<span data-ttu-id="c63d3-106">Poiché le code di archiviazione di Azure fanno parte degli account di archiviazione per utilizzo generico, è necessario iniziare creando un account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="c63d3-106">Since Azure Storage queues are part of Azure general purpose storage accounts, you must start by creating a storage account:</span></span>

1. <span data-ttu-id="c63d3-107">In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.</span><span class="sxs-lookup"><span data-stu-id="c63d3-107">In a browser, navigate to the [Azure portal](http://portal.azure.com), and sign in with your normal credentials.</span></span>
1. <span data-ttu-id="c63d3-108">In alto a sinistra fare clic su **Tutti i servizi**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-108">In the top left, click **All services**.</span></span>
1. <span data-ttu-id="c63d3-109">Scorrere verso il basso fino alla sezione **Archiviazione** e quindi fare clic su **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-109">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>
1. <span data-ttu-id="c63d3-110">In alto a sinistra nel pannello **Account di archiviazione** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-110">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

    ![Screenshot del pannello Account di archiviazione con il comando Aggiungi evidenziato](../images/5-create-a-storage-account-1.png)

1. <span data-ttu-id="c63d3-112">Nella casella di testo **Nome** digitare un nome univoco per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c63d3-112">In the **Name** text box, type a unique name for the storage account.</span></span>
1. <span data-ttu-id="c63d3-113">In **Modello di distribuzione** assicurarsi che l'opzione **Resource Manager** sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="c63d3-113">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
1. <span data-ttu-id="c63d3-114">Nell'elenco a discesa **Tipologia account** selezionare **Archiviazione (utilizzo generico v2)**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-114">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
1. <span data-ttu-id="c63d3-115">Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-115">In the **Location** drop-down list, select a region near you.</span></span>
1. <span data-ttu-id="c63d3-116">Nell'elenco a discesa **Replica** selezionare **Archiviazione con ridondanza locale**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-116">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
1. <span data-ttu-id="c63d3-117">In **Prestazioni** selezionare **Standard**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-117">Under **Performance**, select **Standard**.</span></span>
1. <span data-ttu-id="c63d3-118">In **Livello di accesso** selezionare **Sporadico**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-118">Under **Access tier**, select **Cool**.</span></span>
1. <span data-ttu-id="c63d3-119">In **Trasferimento sicuro obbligatorio** selezionare **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-119">Under **Secure transfer required**, select **Disabled**.</span></span>
1. <span data-ttu-id="c63d3-120">In **Sottoscrizione** selezionare la propria sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="c63d3-120">Under **Subscription**, select your subscription.</span></span>

    ![Screenshot della finestra di dialogo Crea account di archiviazione](../images/5-create-a-storage-account-2.png)

1. <span data-ttu-id="c63d3-122">In **Gruppo di risorse** selezionare **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-122">Under **Resource group**, select **Create new**.</span></span> <span data-ttu-id="c63d3-123">Nella casella di testo digitare **MusicSharingResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-123">In the text box, type **MusicSharingResourceGroup**.</span></span>
1. <span data-ttu-id="c63d3-124">In **Reti virtuali** selezionare **Disabilitato** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-124">Under **Virtual networks**, select **Disabled**, and then click **Create**.</span></span>

    ![Screenshot della finestra di dialogo Crea account di archiviazione con il comando Crea evidenziato](../images/5-create-a-storage-account-3.png)

<span data-ttu-id="c63d3-126">Azure crea il nuovo account di archiviazione e il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="c63d3-126">Azure creates the new storage account and the new resource group.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="c63d3-127">Creare una coda</span><span class="sxs-lookup"><span data-stu-id="c63d3-127">Create a queue</span></span>

<span data-ttu-id="c63d3-128">Ora che è stato creato l'account di archiviazione, è possibile aggiungere una nuova coda.</span><span class="sxs-lookup"><span data-stu-id="c63d3-128">Now that the storage account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="c63d3-129">È necessario creare la coda usando i comandi di PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c63d3-129">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="c63d3-130">In alto a destra nel portale fare clic sul collegamento **Cloud Shell**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-130">In the top right of the portal, click the **Cloud Shell** link.</span></span>

    ![Screenshot del portale di Azure con l'icona Cloud Shell evidenziata](../images/5-create-a-storage-queue-1.png)

1. <span data-ttu-id="c63d3-132">Nella schermata **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-132">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>
1. <span data-ttu-id="c63d3-133">Se viene visualizzata la schermata **Non sono state montate risorse di archiviazione** fare clic su **Crea risorsa di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-133">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>
1. <span data-ttu-id="c63d3-134">Quando viene visualizzato il prompt `PS Azure`, per ottenere l'account di archiviazione, digitare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="c63d3-134">When the `PS Azure` prompt appears, to obtain the storage account, type the following command.</span></span> <span data-ttu-id="c63d3-135">Sostituire `<storageaccountname>` con il nome univoco dell'account di archiviazione e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-135">Substitute `<storageaccountname>` with the unique name of your storage account, and then press Enter:</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="c63d3-136">Per ottenere il contesto dell'account di archiviazione, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-136">To obtain the context of the storage account, type the following command, and then press Enter:</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="c63d3-137">Per creare una nuova coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-137">To create a new queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="c63d3-138">Aggiungere un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="c63d3-138">Add a message to the queue</span></span>

<span data-ttu-id="c63d3-139">Dopo avere creato una coda nell'account di archiviazione, è possibile aggiungervi un messaggio.</span><span class="sxs-lookup"><span data-stu-id="c63d3-139">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="c63d3-140">Per creare un nuovo messaggio, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-140">To create a new message, type the following command, and then press Enter:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="c63d3-141">Per aggiungere il nuovo messaggio alla nuova coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-141">To add the new message to the new queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. <span data-ttu-id="c63d3-142">Fare clic su **Tutte le risorse** nel menu sul lato sinistro del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="c63d3-142">In the Azure portal, in the navigation on the left, click **All resources**.</span></span>
1. <span data-ttu-id="c63d3-143">Nell'elenco delle risorse fare clic sull'account di archiviazione creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c63d3-143">In the list of resources, click the storage account you created earlier.</span></span>
1. <span data-ttu-id="c63d3-144">Nel pannello dell'account di archiviazione fare clic su **Storage Explorer (anteprima)**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-144">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>
1. <span data-ttu-id="c63d3-145">In Storage Explorer, sotto **Code**, fare clic su **musicsharingmessages**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-145">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="c63d3-146">Storage Explorer visualizza il messaggio appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="c63d3-146">The Storage Explorer displays the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="c63d3-147">Recuperare e rimuovere il messaggio</span><span class="sxs-lookup"><span data-stu-id="c63d3-147">Retrieve and remove the message</span></span>

<span data-ttu-id="c63d3-148">Un componente di destinazione per un messaggio in una coda di archiviazione deve recuperare il messaggio all'inizio della coda.</span><span class="sxs-lookup"><span data-stu-id="c63d3-148">A destination component for a message in a Storage queue must retrieve the message at the front of the queue.</span></span> <span data-ttu-id="c63d3-149">Il componente di destinazione deve quindi elaborare il messaggio ed eliminarlo dalla coda in modo che gli altri componenti non lo recuperino.</span><span class="sxs-lookup"><span data-stu-id="c63d3-149">Then the destination component must process the message, and delete it from the queue so that other components do not retrieve it.</span></span>

1. <span data-ttu-id="c63d3-150">In Cloud Shell, per ottenere il messaggio all'inizio della coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-150">In Cloud Shell, to get the message at the front of the queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="c63d3-151">Per visualizzare il messaggio, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-151">To display the message, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="c63d3-152">Per visualizzare tutte le proprietà del messaggio, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-152">To display all the properties of the message, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="c63d3-153">Per rimuovere il messaggio dalla coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="c63d3-153">To remove the message from the queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="c63d3-154">Nel portale di Azure, per aggiornare la visualizzazione della coda, passare al pannello Account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c63d3-154">In the Azure portal, to refresh the queue display, go to the Storage Account blade.</span></span> <span data-ttu-id="c63d3-155">Fare clic su **Panoramica** e quindi su **Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-155">Click **Overview**, and then click **Storage Explorer**.</span></span>
1. <span data-ttu-id="c63d3-156">In **CODE** fare clic su **musicsharingmessages**.</span><span class="sxs-lookup"><span data-stu-id="c63d3-156">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="c63d3-157">Storage Explorer mostra che la coda è vuota perché è stato rimosso l'unico messaggio.</span><span class="sxs-lookup"><span data-stu-id="c63d3-157">The Storage Explorer shows that the queue is empty because you removed the only message.</span></span>

## <a name="cleanup"></a><span data-ttu-id="c63d3-158">Pulizia</span><span class="sxs-lookup"><span data-stu-id="c63d3-158">Cleanup</span></span>

<span data-ttu-id="c63d3-159">Per rimuovere tutte le risorse create nel corso dell'esercizio, immettere il comando seguente in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="c63d3-159">To remove all resources created during this exercise, enter the following command in Cloud Shell:</span></span> 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a><span data-ttu-id="c63d3-160">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c63d3-160">Summary</span></span>

<span data-ttu-id="c63d3-161">In questo esercizio è stato creato un account di archiviazione nella sottoscrizione di Azure ed è stata creata una nuova coda in esso.</span><span class="sxs-lookup"><span data-stu-id="c63d3-161">Here, you created a storage account in your Azure subscription, and created a new queue in it.</span></span> <span data-ttu-id="c63d3-162">Si è anche usato PowerShell per simulare le azioni dei componenti dell'applicazione distribuita aggiungendo un messaggio alla coda per poi recuperarlo e rimuoverlo.</span><span class="sxs-lookup"><span data-stu-id="c63d3-162">You also used PowerShell to simulate the actions of distributed application components by adding a message to the queue, and then retrieving and removing it.</span></span>

<span data-ttu-id="c63d3-163">Le code dell'account di archiviazione sono un'ottima soluzione quando si vogliono passare messaggi tra i componenti di un'applicazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="c63d3-163">Storage account queues are a good solution when you want to pass messages between the components of a distributed application.</span></span> <span data-ttu-id="c63d3-164">Non scegliere le code di archiviazione quando si vogliono pubblicare eventi.</span><span class="sxs-lookup"><span data-stu-id="c63d3-164">Do not choose Storage queues when you want to publish events.</span></span>