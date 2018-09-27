<span data-ttu-id="afa83-101">In questa unità si creerà una funzione di Azure per visualizzare il nome e le dimensioni di un BLOB quando viene creato o aggiornato.</span><span class="sxs-lookup"><span data-stu-id="afa83-101">In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="afa83-102">Creare un trigger BLOB</span><span class="sxs-lookup"><span data-stu-id="afa83-102">Create a blob trigger</span></span>

<span data-ttu-id="afa83-103">Anche in questo caso, continuiamo a usare l'applicazione Funzioni di Azure esistente e aggiungiamo un trigger BLOB.</span><span class="sxs-lookup"><span data-stu-id="afa83-103">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="afa83-104">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="afa83-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="afa83-105">Passare alla schermata **Tutte le risorse** e selezionare l'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="afa83-105">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="afa83-106">Selezionare **Funzioni** e quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="afa83-106">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="afa83-107">Selezionare **Trigger BLOB**.</span><span class="sxs-lookup"><span data-stu-id="afa83-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="afa83-108">Selezionare il linguaggio di programmazione **C#**.</span><span class="sxs-lookup"><span data-stu-id="afa83-108">Select **C#** as the language.</span></span>

1. <span data-ttu-id="afa83-109">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="afa83-109">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="afa83-110">Lasciare **Percorso** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="afa83-110">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="afa83-111">Selezionare il collegamento _nuovo_ accanto all'elenco a discesa **Connessione dell'account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="afa83-111">Select the _new_ link next to the **Storage account connection** dropdown.</span></span> <span data-ttu-id="afa83-112">Nel pannello che viene visualizzato fare clic su **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="afa83-112">In the blade that pops up, click **Create new**.</span></span> <span data-ttu-id="afa83-113">Immettere un nome univoco per il nuovo account di archiviazione e selezionare **OK** per creare l'account e chiudere il riquadro.</span><span class="sxs-lookup"><span data-stu-id="afa83-113">Enter a unique name for the new storage account and select **OK** to create the storage account and close pane.</span></span>

1. <span data-ttu-id="afa83-114">Nella schermata Nuova funzione che viene visualizzata di nuovo fare clic su **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="afa83-114">Once you have returned to the New Function screen, select **Create** to create the function.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="afa83-115">Creare un contenitore BLOB</span><span class="sxs-lookup"><span data-stu-id="afa83-115">Create a blob container</span></span>

<span data-ttu-id="afa83-116">Ora che è stato creato un trigger di BLOB, si userà Storage Explorer per creare un BLOB e attivare la funzione.</span><span class="sxs-lookup"><span data-stu-id="afa83-116">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="afa83-117">Aprire l'account di archiviazione usato (o creato) in una nuova scheda.</span><span class="sxs-lookup"><span data-stu-id="afa83-117">Open the storage account you used (or created) in a new tab.</span></span>

    > [!TIP]
    > <span data-ttu-id="afa83-118">È possibile duplicare una scheda nella maggior parte dei browser facendo clic con il pulsante destro del mouse sulla scheda in questione e selezionando **Duplica** dal menu visualizzato.</span><span class="sxs-lookup"><span data-stu-id="afa83-118">You can duplicate a tab in most browsers by right-clicking on the tab in question and selecting **Duplicate** from the menu that appears.</span></span> <span data-ttu-id="afa83-119">Si sceglie di aprire una nuova scheda per poter passare da uno all'altro dei due servizi usati.</span><span class="sxs-lookup"><span data-stu-id="afa83-119">We want to use a new tab so we can switch between the two services we are working with.</span></span>

1. <span data-ttu-id="afa83-120">Nella barra laterale selezionare **Account di archiviazione** o **Tutte le risorse** e quindi applicare il filtro in base al nome.</span><span class="sxs-lookup"><span data-stu-id="afa83-120">Select **Storage accounts** in the sidebar, or select **All resources** in the sidebar and then filter by the name.</span></span>

1. <span data-ttu-id="afa83-121">Fare clic sulla sezione **Storage Explorer (anteprima)** per aprire un nuovo pannello in cui è possibile usare i BLOB e i file.</span><span class="sxs-lookup"><span data-stu-id="afa83-121">Click on the **Storage Explorer (preview)** section - this will open a new panel where you can work with blobs and files.</span></span>

<span data-ttu-id="afa83-122">Tenere presente che il trigger del BLOB monitora solo la posizione descritta nel campo **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="afa83-122">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="afa83-123">Per impostazione predefinita, il percorso deve essere:</span><span class="sxs-lookup"><span data-stu-id="afa83-123">By default, our path should be:</span></span>

> <span data-ttu-id="afa83-124">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="afa83-124">samples-workitems/{name}</span></span>

<span data-ttu-id="afa83-125">È necessario creare un contenitore denominato **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="afa83-125">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="afa83-126">Fare clic con il pulsante destro del mouse su **CONTENITORI BLOB** e scegliere **Crea contenitore BLOB**.</span><span class="sxs-lookup"><span data-stu-id="afa83-126">Right-click **BLOB CONTAINERS** and select **Create Blob Container**.</span></span>

1. <span data-ttu-id="afa83-127">Immettere **samples-workitems** come nome, lasciare l'impostazione predefinita **Privato** per il livello di accesso e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="afa83-127">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting, and select **OK**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="afa83-128">Attivare il trigger del BLOB</span><span class="sxs-lookup"><span data-stu-id="afa83-128">Turn on your blob trigger</span></span>

<span data-ttu-id="afa83-129">Dopo che è stato creato il contenitore per il monitoraggio, è possibile eseguire la funzione per visualizzare l'output quando il BLOB viene creato.</span><span class="sxs-lookup"><span data-stu-id="afa83-129">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="afa83-130">Tornare alla scheda del browser con Funzione di Azure (o aprirla di nuovo).</span><span class="sxs-lookup"><span data-stu-id="afa83-130">Switch back to the browser tab with your Azure Function (or reopen it).</span></span>

1. <span data-ttu-id="afa83-131">Selezionare il trigger del BLOB per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="afa83-131">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="afa83-132">Aprire la scheda **Log** nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="afa83-132">Open the **Logs** tab at the bottom of the screen.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="afa83-133">Creare un BLOB</span><span class="sxs-lookup"><span data-stu-id="afa83-133">Create a blob</span></span>

<span data-ttu-id="afa83-134">Il trigger del blob è ora attivo e in ascolto per l'attività.</span><span class="sxs-lookup"><span data-stu-id="afa83-134">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="afa83-135">È possibile creare un BLOB per vedere se viene visualizzato un messaggio di registro.</span><span class="sxs-lookup"><span data-stu-id="afa83-135">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="afa83-136">Tornare alla scheda del browser con Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="afa83-136">Switch back to the browser tab with Storage Explorer.</span></span>

1. <span data-ttu-id="afa83-137">In Storage Explorer selezionare il contenitore **samples-workitems** nell'elenco **CONTENITORI BLOB**.</span><span class="sxs-lookup"><span data-stu-id="afa83-137">In Storage Explorer, select the **samples-workitems** container from the **BLOB CONTAINERS** list.</span></span>

1. <span data-ttu-id="afa83-138">Nella barra degli strumenti selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="afa83-138">Select **Upload** from the toolbar.</span></span>

1. <span data-ttu-id="afa83-139">Selezionare un file qualsiasi nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="afa83-139">Select any file from your computer.</span></span>

1. <span data-ttu-id="afa83-140">In **Tipo di autenticazione** selezionare **SAS**.</span><span class="sxs-lookup"><span data-stu-id="afa83-140">Under **Authentication type**, select **SAS**.</span></span>

1. <span data-ttu-id="afa83-141">Selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="afa83-141">Select **Upload**.</span></span>

1. <span data-ttu-id="afa83-142">Tornare alla scheda Funzioni di Azure e cercare nei log di output un messaggio che visualizza il file caricato.</span><span class="sxs-lookup"><span data-stu-id="afa83-142">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>