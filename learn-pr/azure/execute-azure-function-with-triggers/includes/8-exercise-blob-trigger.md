<span data-ttu-id="f169d-101">In questo esercizio, creeremo una funzione di Azure che consente di visualizzare il nome e le dimensioni di un blob quando viene creato o aggiornato.</span><span class="sxs-lookup"><span data-stu-id="f169d-101">In this exercise, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span> 

> [!NOTE]
> <span data-ttu-id="f169d-102">Per completare questo esercizio, assicurati di essere connesso al [portale di Azure](https://portal.azure.com/) con un account valido.</span><span class="sxs-lookup"><span data-stu-id="f169d-102">To complete this exercise, make sure you're signed in to the [Azure portal](https://portal.azure.com/) with a valid account.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="f169d-103">Creare un trigger del blob</span><span class="sxs-lookup"><span data-stu-id="f169d-103">Create a blob trigger</span></span>

<span data-ttu-id="f169d-104">Anche in questo caso, continuiamo a usare l'applicazione di Funzioni di Azure esistente e aggiungiamo un trigger del blob.</span><span class="sxs-lookup"><span data-stu-id="f169d-104">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="f169d-105">Selezionare **Funzioni**, quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="f169d-105">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Selezionare Funzioni, quindi il segno più (+)](../media/4-hover-function.png)

1. <span data-ttu-id="f169d-107">Selezionare **Trigger del blob**.</span><span class="sxs-lookup"><span data-stu-id="f169d-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="f169d-108">Selezionare il linguaggio di programmazione **C#**.</span><span class="sxs-lookup"><span data-stu-id="f169d-108">Select **C#** as the language.</span></span> 

1. <span data-ttu-id="f169d-109">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f169d-109">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="f169d-110">Lasciare **Percorso** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f169d-110">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="f169d-111">Selezionare un account di archiviazione di Azure esistente oppure selezionare **Crea** se si desidera che Azure crei un nuovo account per l'utente.</span><span class="sxs-lookup"><span data-stu-id="f169d-111">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="download-storage-explorer"></a><span data-ttu-id="f169d-112">Scaricare Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="f169d-112">Download Storage explorer</span></span>

<span data-ttu-id="f169d-113">Ora che abbiamo creato un trigger del blob, è possibile scaricare Storage Explorer grazie a cui creare facilmente un blob.</span><span class="sxs-lookup"><span data-stu-id="f169d-113">Now that we've created a blob trigger, let's download Storage explorer, which will allow us to easily create a blob.</span></span>

- <span data-ttu-id="f169d-114">Scaricare [Storage Explorer](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="f169d-114">Download [Storage explorer](http://storageexplorer.com).</span></span>

## <a name="connect-to-your-azure-storage-account"></a><span data-ttu-id="f169d-115">Collegare l'account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="f169d-115">Connect to your Azure Storage account</span></span>

<span data-ttu-id="f169d-116">Ora abbiamo scaricato Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="f169d-116">We now have Storage explorer downloaded.</span></span> <span data-ttu-id="f169d-117">È possibile accedere usando le credenziali che sono state specificate.</span><span class="sxs-lookup"><span data-stu-id="f169d-117">Let's sign in using the credentials that were supplied.</span></span>

1. <span data-ttu-id="f169d-118">In Storage Explorer selezionare l'icona del più (+) a sinistra.</span><span class="sxs-lookup"><span data-stu-id="f169d-118">In Storage explorer, select the plus (+) icon on the left.</span></span>

1. <span data-ttu-id="f169d-119">Selezionare **Usare un nome e una chiave dell'account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="f169d-119">Select **Use a storage account name and key**.</span></span>

1. <span data-ttu-id="f169d-120">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f169d-120">Select **Next**.</span></span>

1. <span data-ttu-id="f169d-121">In Azure, nel trigger di blob, selezionare **Integra**.</span><span class="sxs-lookup"><span data-stu-id="f169d-121">In Azure, under your blob trigger, select **Integrate**.</span></span>

1. <span data-ttu-id="f169d-122">Selezionare **Documentazione** per espandere la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f169d-122">Select **Documentation** to expand the view.</span></span>

1. <span data-ttu-id="f169d-123">Digitare il **Nome dell'account** e la **Chiave dell'account**.</span><span class="sxs-lookup"><span data-stu-id="f169d-123">Copy the **Account Name** and **Account Key**.</span></span>

1. <span data-ttu-id="f169d-124">Nuovamente in Storage Explorer, incollarli in **Nome dell'account** e in **Chiave dell'account**.</span><span class="sxs-lookup"><span data-stu-id="f169d-124">Back in Storage explorer, paste in the **Account Name** and **Account Key**.</span></span>

1. <span data-ttu-id="f169d-125">Inserire il **Nome visualizzato**.</span><span class="sxs-lookup"><span data-stu-id="f169d-125">Enter a **Display name**.</span></span> <span data-ttu-id="f169d-126">Questo valore è il nome della connessione in Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="f169d-126">This value is the name of the connection in Storage explorer.</span></span>

1. <span data-ttu-id="f169d-127">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f169d-127">Select **Next**.</span></span>

1. <span data-ttu-id="f169d-128">Selezionare **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="f169d-128">Select **Connect**.</span></span> 

## <a name="create-a-blob-container"></a><span data-ttu-id="f169d-129">Creare un contenitore BLOB</span><span class="sxs-lookup"><span data-stu-id="f169d-129">Create a blob container</span></span>

<span data-ttu-id="f169d-130">Non siamo connessi all'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="f169d-130">We aren't connected to our Azure Storage account.</span></span> <span data-ttu-id="f169d-131">Teniamo presente che il trigger del blob esegue solo il monitoraggio del percorso descritto nel campo **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="f169d-131">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="f169d-132">Per impostazione predefinita, il percorso deve essere:</span><span class="sxs-lookup"><span data-stu-id="f169d-132">By default, our path should be:</span></span>

> <span data-ttu-id="f169d-133">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="f169d-133">samples-workitems/{name}</span></span>

<span data-ttu-id="f169d-134">È necessario creare un contenitore denominato **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="f169d-134">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="f169d-135">In Storage Explorer, espandere l'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="f169d-135">In Storage explorer, expand your storage account.</span></span> <span data-ttu-id="f169d-136">Il nome deve essere il **Nome visualizzato** fornito durante il processo di connessione.</span><span class="sxs-lookup"><span data-stu-id="f169d-136">The name should be the **Display name** that you provided during the connection process.</span></span>

1. <span data-ttu-id="f169d-137">Fare clic con il pulsante destro del mouse su **Contenitori BLOB** e scegliere **Crea contenitore BLOB**.</span><span class="sxs-lookup"><span data-stu-id="f169d-137">Right-click **Blob Containers** and select **Create blob container**.</span></span>

1. <span data-ttu-id="f169d-138">Inserire **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="f169d-138">Enter **samples-workitems**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="f169d-139">Attivare il trigger del blob</span><span class="sxs-lookup"><span data-stu-id="f169d-139">Turn on your blob trigger</span></span>

<span data-ttu-id="f169d-140">Ora che abbiamo creato il contenitore per il monitoraggio, è possibile eseguire la funzione per vedere l'output quando il blob viene creato.</span><span class="sxs-lookup"><span data-stu-id="f169d-140">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="f169d-141">Selezionare il trigger del blob per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="f169d-141">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="f169d-142">Selezionare **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="f169d-142">Select **Run**.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="f169d-143">Creare un blob</span><span class="sxs-lookup"><span data-stu-id="f169d-143">Create a blob</span></span>

<span data-ttu-id="f169d-144">Il trigger del blob è ora attivo e in ascolto per l'attività.</span><span class="sxs-lookup"><span data-stu-id="f169d-144">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="f169d-145">È possibile creare un blob per vedere se viene visualizzato un messaggio di registro.</span><span class="sxs-lookup"><span data-stu-id="f169d-145">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="f169d-146">In Storage Explorer, selezionare il contenitore **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="f169d-146">In Storage explorer, select the **samples-workitems** container.</span></span>

1. <span data-ttu-id="f169d-147">Selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="f169d-147">Select **Upload**.</span></span> 

1. <span data-ttu-id="f169d-148">Selezionare **Carica file**.</span><span class="sxs-lookup"><span data-stu-id="f169d-148">Select **Upload Files**.</span></span>

1. <span data-ttu-id="f169d-149">Selezionare i file dal computer.</span><span class="sxs-lookup"><span data-stu-id="f169d-149">Select any file from your computer.</span></span>

1. <span data-ttu-id="f169d-150">Selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="f169d-150">Select **Upload**.</span></span>

1. <span data-ttu-id="f169d-151">Tornare ad Azure.</span><span class="sxs-lookup"><span data-stu-id="f169d-151">Go back to Azure.</span></span> <span data-ttu-id="f169d-152">Controllare i registri per trovare il messaggio che consente di visualizzare i file caricati.</span><span class="sxs-lookup"><span data-stu-id="f169d-152">Check your logs for a message that displays what file was uploaded.</span></span>

## <a name="clean-up"></a><span data-ttu-id="f169d-153">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="f169d-153">Clean up</span></span>

<span data-ttu-id="f169d-154">Per assicurarsi che non sono previsti costi per questa funzione, selezionare **Sospendi** sopra la finestra del log.</span><span class="sxs-lookup"><span data-stu-id="f169d-154">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![Sospendi](../media/4-pause-timer.png)


