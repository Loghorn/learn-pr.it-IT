<span data-ttu-id="5325f-101">In questa unità si creerà una funzione di Azure per visualizzare il nome e le dimensioni di un BLOB quando viene creato o aggiornato.</span><span class="sxs-lookup"><span data-stu-id="5325f-101">In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="5325f-102">Creare un trigger BLOB</span><span class="sxs-lookup"><span data-stu-id="5325f-102">Create a blob trigger</span></span>

<span data-ttu-id="5325f-103">Anche in questo caso, continuare a usare l'applicazione Funzioni di Azure esistente e aggiungere un trigger BLOB.</span><span class="sxs-lookup"><span data-stu-id="5325f-103">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="5325f-104">Assicurarsi di aver eseguito l'accesso al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato il sandbox.</span><span class="sxs-lookup"><span data-stu-id="5325f-104">Make sure you are signed into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="5325f-105">Passare alla schermata **Tutte le risorse** e selezionare l'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="5325f-105">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="5325f-106">Selezionare **Funzioni** e quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="5325f-106">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="5325f-107">Selezionare **Trigger BLOB**.</span><span class="sxs-lookup"><span data-stu-id="5325f-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="5325f-108">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="5325f-108">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="5325f-109">Lasciare **Percorso** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="5325f-109">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="5325f-110">Selezionare il collegamento _nuovo_ accanto all'elenco a discesa **Connessione dell'account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="5325f-110">Select the _new_ link next to the **Storage account connection** dropdown.</span></span> <span data-ttu-id="5325f-111">Nel pannello che viene visualizzato fare clic su **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="5325f-111">In the blade that pops up, click **Create new**.</span></span> <span data-ttu-id="5325f-112">Immettere un nome univoco per il nuovo account di archiviazione e selezionare **OK** per creare l'account e chiudere il riquadro.</span><span class="sxs-lookup"><span data-stu-id="5325f-112">Enter a unique name for the new storage account and select **OK** to create the storage account and close pane.</span></span>

1. <span data-ttu-id="5325f-113">Nella schermata Nuova funzione che viene visualizzata di nuovo fare clic su **Crea** per creare la funzione.</span><span class="sxs-lookup"><span data-stu-id="5325f-113">Once you have returned to the New Function screen, select **Create** to create the function.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="5325f-114">Creare un contenitore BLOB</span><span class="sxs-lookup"><span data-stu-id="5325f-114">Create a blob container</span></span>

<span data-ttu-id="5325f-115">Ora che è stato creato un trigger di BLOB, si userà Storage Explorer per creare un BLOB e attivare la funzione.</span><span class="sxs-lookup"><span data-stu-id="5325f-115">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="5325f-116">Aprire l'account di archiviazione usato (o creato) in una nuova scheda.</span><span class="sxs-lookup"><span data-stu-id="5325f-116">Open the storage account you used (or created) in a new tab.</span></span>

    > [!TIP]
    > <span data-ttu-id="5325f-117">È possibile duplicare una scheda nella maggior parte dei browser facendo clic con il pulsante destro del mouse sulla scheda in questione e selezionando **Duplica** dal menu visualizzato.</span><span class="sxs-lookup"><span data-stu-id="5325f-117">You can duplicate a tab in most browsers by right-clicking on the tab in question and selecting **Duplicate** from the menu that appears.</span></span> <span data-ttu-id="5325f-118">Si sceglie di aprire una nuova scheda per poter passare da uno all'altro dei due servizi usati.</span><span class="sxs-lookup"><span data-stu-id="5325f-118">We want to use a new tab so we can switch between the two services we are working with.</span></span>

1. <span data-ttu-id="5325f-119">Nella barra laterale selezionare **Account di archiviazione** o **Tutte le risorse** e quindi applicare il filtro in base al nome.</span><span class="sxs-lookup"><span data-stu-id="5325f-119">Select **Storage accounts** in the sidebar, or select **All resources** in the sidebar and then filter by the name.</span></span>

1. <span data-ttu-id="5325f-120">Fare clic sulla sezione **Storage Explorer (anteprima)** per aprire un nuovo pannello in cui è possibile usare i BLOB e i file.</span><span class="sxs-lookup"><span data-stu-id="5325f-120">Click on the **Storage Explorer (preview)** section - this will open a new panel where you can work with blobs and files.</span></span>

<span data-ttu-id="5325f-121">Tenere presente che il trigger del BLOB monitora solo la posizione descritta nel campo **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="5325f-121">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="5325f-122">Per impostazione predefinita, il percorso deve essere:</span><span class="sxs-lookup"><span data-stu-id="5325f-122">By default, our path should be:</span></span>

> <span data-ttu-id="5325f-123">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="5325f-123">samples-workitems/{name}</span></span>

<span data-ttu-id="5325f-124">È necessario creare un contenitore denominato **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="5325f-124">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="5325f-125">Fare clic con il pulsante destro del mouse su **CONTENITORI BLOB** e scegliere **Crea contenitore BLOB**.</span><span class="sxs-lookup"><span data-stu-id="5325f-125">Right-click **BLOB CONTAINERS** and select **Create Blob Container**.</span></span>

1. <span data-ttu-id="5325f-126">Immettere **samples-workitems** come nome, lasciare l'impostazione predefinita **Privato** per il livello di accesso e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="5325f-126">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting, and select **OK**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="5325f-127">Attivare il trigger del BLOB</span><span class="sxs-lookup"><span data-stu-id="5325f-127">Turn on your blob trigger</span></span>

<span data-ttu-id="5325f-128">Dopo che è stato creato il contenitore per il monitoraggio, è possibile eseguire la funzione per visualizzare l'output quando il BLOB viene creato.</span><span class="sxs-lookup"><span data-stu-id="5325f-128">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="5325f-129">Tornare alla scheda del browser con Funzione di Azure (o aprirla di nuovo).</span><span class="sxs-lookup"><span data-stu-id="5325f-129">Switch back to the browser tab with your Azure Function (or reopen it).</span></span>

1. <span data-ttu-id="5325f-130">Selezionare il trigger del BLOB per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="5325f-130">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="5325f-131">Aprire la scheda **Log** nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="5325f-131">Open the **Logs** tab at the bottom of the screen.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="5325f-132">Creare un BLOB</span><span class="sxs-lookup"><span data-stu-id="5325f-132">Create a blob</span></span>

<span data-ttu-id="5325f-133">Il trigger del blob è ora attivo e in ascolto per l'attività.</span><span class="sxs-lookup"><span data-stu-id="5325f-133">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="5325f-134">È possibile creare un BLOB per vedere se viene visualizzato un messaggio di registro.</span><span class="sxs-lookup"><span data-stu-id="5325f-134">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="5325f-135">Tornare alla scheda del browser con Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="5325f-135">Switch back to the browser tab with Storage Explorer.</span></span>

1. <span data-ttu-id="5325f-136">In Storage Explorer selezionare il contenitore **samples-workitems** nell'elenco **CONTENITORI BLOB**.</span><span class="sxs-lookup"><span data-stu-id="5325f-136">In Storage Explorer, select the **samples-workitems** container from the **BLOB CONTAINERS** list.</span></span>

1. <span data-ttu-id="5325f-137">Nella barra degli strumenti selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="5325f-137">Select **Upload** from the toolbar.</span></span>

1. <span data-ttu-id="5325f-138">Selezionare un file qualsiasi nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="5325f-138">Select any file from your computer.</span></span>

1. <span data-ttu-id="5325f-139">In **Tipo di autenticazione** selezionare **SAS**.</span><span class="sxs-lookup"><span data-stu-id="5325f-139">Under **Authentication type**, select **SAS**.</span></span>

1. <span data-ttu-id="5325f-140">Selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="5325f-140">Select **Upload**.</span></span>

1. <span data-ttu-id="5325f-141">Tornare alla scheda Funzioni di Azure e cercare nei log di output un messaggio che visualizza il file caricato.</span><span class="sxs-lookup"><span data-stu-id="5325f-141">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>