<span data-ttu-id="ba849-101">In questo esercizio si creerà un nuovo account di archiviazione nella sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba849-101">In this exercise, you will create a new Storage Account in your Azure subscription.</span></span> <span data-ttu-id="ba849-102">Si userà quindi Azure Cloud Shell per creare una nuova coda, aggiungere un messaggio e quindi leggere il messaggio e rimuoverlo dalla coda.</span><span class="sxs-lookup"><span data-stu-id="ba849-102">You will then use the Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="ba849-103">Queste sono le stesse azioni eseguite dai componenti in un'applicazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="ba849-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="ba849-104">Ad esempio, un'app per dispositivi mobili può aggiungere un messaggio a una coda, in cui attende che un servizio Web lo recuperi e lo elabori.</span><span class="sxs-lookup"><span data-stu-id="ba849-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="ba849-105">Creare un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="ba849-105">Create a Storage Account</span></span>

<span data-ttu-id="ba849-106">Le code di archiviazione fanno parte degli account di archiviazione per utilizzo generico di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba849-106">Since Storage queues are part of Azure general purpose Storage accounts.</span></span> <span data-ttu-id="ba849-107">È necessario iniziare creando un account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="ba849-107">You must start by creating a Storage account:</span></span>

1. <span data-ttu-id="ba849-108">In un browser passare al [portale di Azure](http://portal.azure.com) e accedere con le credenziali normali.</span><span class="sxs-lookup"><span data-stu-id="ba849-108">In a browser, navigate to the [Azure Portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>
1. <span data-ttu-id="ba849-109">In alto a sinistra fare clic su **Tutti i servizi**.</span><span class="sxs-lookup"><span data-stu-id="ba849-109">In the top left, click **All services**.</span></span>
1. <span data-ttu-id="ba849-110">Scorrere verso il basso fino alla sezione **Archiviazione** e quindi fare clic su **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="ba849-110">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>
1. <span data-ttu-id="ba849-111">In alto a sinistra nel pannello **Account di archiviazione** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ba849-111">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

    ![Creare un account di archiviazione](../images/5-create-a-storage-account-1.png)

1. <span data-ttu-id="ba849-113">Nella casella di testo **Nome** digitare un nome univoco per l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ba849-113">In the **Name** text box, type a unique name for the storage account.</span></span>
1. <span data-ttu-id="ba849-114">In **Modello di distribuzione** assicurarsi che l'opzione **Resource Manager** sia selezionata.</span><span class="sxs-lookup"><span data-stu-id="ba849-114">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
1. <span data-ttu-id="ba849-115">Nell'elenco a discesa **Tipologia account** selezionare **Archiviazione (utilizzo generico v2)**.</span><span class="sxs-lookup"><span data-stu-id="ba849-115">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
1. <span data-ttu-id="ba849-116">Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.</span><span class="sxs-lookup"><span data-stu-id="ba849-116">In the **Location** drop-down list, select a region near you.</span></span>
1. <span data-ttu-id="ba849-117">Nell'elenco a discesa **Replica** selezionare **Archiviazione con ridondanza locale**.</span><span class="sxs-lookup"><span data-stu-id="ba849-117">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
1. <span data-ttu-id="ba849-118">In **Prestazioni** selezionare **Standard**.</span><span class="sxs-lookup"><span data-stu-id="ba849-118">Under **Performance**, select **Standard**.</span></span>
1. <span data-ttu-id="ba849-119">In **Livello di accesso** selezionare **Accesso sporadico**.</span><span class="sxs-lookup"><span data-stu-id="ba849-119">Under **Access tier**, select **Cool**.</span></span>
1. <span data-ttu-id="ba849-120">In **Trasferimento sicuro obbligatorio** selezionare **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="ba849-120">Under **Secure transfer required** select, **Disabled**.</span></span>
1. <span data-ttu-id="ba849-121">In **Sottoscrizione** selezionare la propria sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="ba849-121">Under **Subscription**, select your subscription.</span></span>

    ![Creare un account di archiviazione](../images/5-create-a-storage-account-2.png)

1. <span data-ttu-id="ba849-123">In **Gruppo di risorse** selezionare **Crea nuovo** e quindi nella casella di testo digitare **MusicSharingResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="ba849-123">Under **Resource group** select **Create new**, and then in the textbox type **MusicSharingResourceGroup**.</span></span>
1. <span data-ttu-id="ba849-124">In **Reti virtuali** selezionare **Disabilitato** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="ba849-124">Under **Virtual networks** select **Disabled** and then click **Create**.</span></span>

    ![Creare un account di archiviazione](../images/5-create-a-storage-account-3.png)

<span data-ttu-id="ba849-126">Azure crea il nuovo account di archiviazione e il nuovo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="ba849-126">Azure creates the new storage account and the new resource group.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="ba849-127">Creare una coda</span><span class="sxs-lookup"><span data-stu-id="ba849-127">Create a Queue</span></span>

<span data-ttu-id="ba849-128">Ora che è stato creato l'account di archiviazione, è possibile aggiungere una nuova coda.</span><span class="sxs-lookup"><span data-stu-id="ba849-128">Now that the Storage Account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="ba849-129">È necessario creare la coda usando i comandi di PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ba849-129">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="ba849-130">In alto a destra nel portale fare clic sul collegamento **Cloud Shell**.</span><span class="sxs-lookup"><span data-stu-id="ba849-130">In the top right of the portal, click the **Cloud Shell** link.</span></span>

    ![Avviare Cloud Shell](../images/5-create-a-storage-queue-1.png)

1. <span data-ttu-id="ba849-132">Nella schermata **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="ba849-132">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>
1. <span data-ttu-id="ba849-133">Se viene visualizzata la schermata **You have no storage mounted** (Nessuna risorsa di archiviazione montata) fare clic su **Create storage** (Crea archiviazione).</span><span class="sxs-lookup"><span data-stu-id="ba849-133">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>
1. <span data-ttu-id="ba849-134">Quando viene visualizzato il prompt `PS Azure`, per ottenere l'account di archiviazione digitare il comando seguente, sostituendo `<storageaccountname>` con il nome univoco dell'account di archiviazione e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-134">When the `PS Azure` prompt appears, to obtain the storage account, type the following command, substituting `<storageaccountname>` with the unique name of your storage account, and then press Enter:</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="ba849-135">Per ottenere il contesto dell'account di archiviazione, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-135">To obtain the context of the storage account, type the following command and then press Enter:</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="ba849-136">Per creare una nuova coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-136">To create a new queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="ba849-137">Aggiungere un messaggio alla coda</span><span class="sxs-lookup"><span data-stu-id="ba849-137">Add a Message to the Queue</span></span>

<span data-ttu-id="ba849-138">Dopo avere creato una coda nell'account di archiviazione, è possibile aggiungervi un messaggio.</span><span class="sxs-lookup"><span data-stu-id="ba849-138">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="ba849-139">Per creare un nuovo messaggio, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-139">To create a new message, type the following command and then press Enter:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="ba849-140">Per aggiungere il nuovo messaggio alla nuova coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-140">To add the new message to the new queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. <span data-ttu-id="ba849-141">Fare clic su **Tutte le risorse** nel menu sul lato sinistro del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba849-141">In the Azure Portal, in the navigation on the left, click **All resources**.</span></span>
1. <span data-ttu-id="ba849-142">Nell'elenco delle risorse fare clic sull'account di archiviazione creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ba849-142">In the list of resources, click the storage account you created earlier.</span></span>
1. <span data-ttu-id="ba849-143">Nel pannello dell'account di archiviazione fare clic su **Storage Explorer (anteprima)**.</span><span class="sxs-lookup"><span data-stu-id="ba849-143">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>
1. <span data-ttu-id="ba849-144">In Storage Explorer, sotto **Code**, fare clic su **musicsharingmessages**.</span><span class="sxs-lookup"><span data-stu-id="ba849-144">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="ba849-145">Storage Explorer visualizza il messaggio appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="ba849-145">The Storage Explorer displays the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="ba849-146">Recuperare e rimuovere il messaggio</span><span class="sxs-lookup"><span data-stu-id="ba849-146">Retrieve and Remove the Message</span></span>

<span data-ttu-id="ba849-147">Un componente di destinazione per un messaggio in una coda di archiviazione deve recuperare il messaggio all'inizio della coda, elaborarlo e quindi eliminarlo dalla coda in modo che non venga recuperato da altri componenti:</span><span class="sxs-lookup"><span data-stu-id="ba849-147">A destination component for a message in a Storage queue, must retrieve the message at the front of the queue, process it, and then delete it from the queue so that other components do not retrieve it:</span></span>

1. <span data-ttu-id="ba849-148">In Azure Cloud Shell, per ottenere il messaggio all'inizio della coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-148">In the Azure Cloud Shell, to get the message at the front of the queue, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="ba849-149">Per visualizzare il messaggio, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-149">To display the message, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="ba849-150">Per visualizzare tutte le proprietà del messaggio, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-150">To display all the properties of the message, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="ba849-151">Per rimuovere il messaggio dalla coda, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="ba849-151">To remove the message from the queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="ba849-152">Nel portale di Azure, per aggiornare la visualizzazione della coda, nel pannello Account di archiviazione fare clic su **Panoramica** e quindi fare clic su **Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ba849-152">In the Azure Portal, to refresh the queue display, in the Storage Account blade, click **Overview** and then click **Storage Explorer**.</span></span>
1. <span data-ttu-id="ba849-153">In **Code** fare clic su **musicsharingmessages**.</span><span class="sxs-lookup"><span data-stu-id="ba849-153">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="ba849-154">Storage Explorer mostra che la coda è vuota perché è stato rimosso l'unico messaggio.</span><span class="sxs-lookup"><span data-stu-id="ba849-154">The Storage Explorer shows that the queue is empty because you removed the only message.</span></span>

## <a name="cleanup"></a><span data-ttu-id="ba849-155">Pulizia</span><span class="sxs-lookup"><span data-stu-id="ba849-155">Cleanup</span></span>

<span data-ttu-id="ba849-156">Per rimuovere tutte le risorse create nel corso dell'esercizio, immettere il comando seguente in Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="ba849-156">To remove all resources created during this exercise enter the following command in the Azure Cloud Shell</span></span> 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a><span data-ttu-id="ba849-157">Summary</span><span class="sxs-lookup"><span data-stu-id="ba849-157">Summary</span></span>

<span data-ttu-id="ba849-158">In questo esercizio è stato creato un account di archiviazione nella sottoscrizione di Azure ed è stata creata una nuova coda in esso.</span><span class="sxs-lookup"><span data-stu-id="ba849-158">Here, you created a Storage Account in your Azure subscription and created a new queue in it.</span></span> <span data-ttu-id="ba849-159">Si è anche usato PowerShell per simulare le azioni dei componenti dell'applicazione distribuita aggiungendo un messaggio alla coda per poi recuperarlo e rimuoverlo.</span><span class="sxs-lookup"><span data-stu-id="ba849-159">You also used PowerShell to simulate the actions of distributed application components by adding a message to the queue and then retrieving and removing it.</span></span>

<span data-ttu-id="ba849-160">Le code dell'account di archiviazione di Azure sono un'ottima soluzione quando si vogliono passare messaggi tra i componenti di un'applicazione distribuita.</span><span class="sxs-lookup"><span data-stu-id="ba849-160">Azure Storage Account queues are a good solution when you want to pass messages between the components of a distributed application.</span></span> <span data-ttu-id="ba849-161">Non scegliere le code di archiviazione quando si vogliono pubblicare eventi.</span><span class="sxs-lookup"><span data-stu-id="ba849-161">Do not choose Storage queues when you want to publish events.</span></span>