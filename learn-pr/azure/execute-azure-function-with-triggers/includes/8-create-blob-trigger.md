<span data-ttu-id="09756-101">In questa unità si creerà una funzione di Azure per visualizzare il nome e le dimensioni di un BLOB quando viene creato o aggiornato.</span><span class="sxs-lookup"><span data-stu-id="09756-101">In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="09756-102">Creare un trigger BLOB</span><span class="sxs-lookup"><span data-stu-id="09756-102">Create a blob trigger</span></span>

<span data-ttu-id="09756-103">Anche in questo caso, continuiamo a usare l'applicazione Funzioni di Azure esistente e aggiungiamo un trigger BLOB.</span><span class="sxs-lookup"><span data-stu-id="09756-103">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="09756-104">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="09756-104">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="09756-105">Selezionare **Funzioni** e quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="09756-105">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="09756-106">Selezionare **Funzione personalizzata** e quindi **Trigger BLOB**.</span><span class="sxs-lookup"><span data-stu-id="09756-106">Select **Custom Function** and then **Blob trigger**.</span></span>

1. <span data-ttu-id="09756-107">Selezionare il linguaggio di programmazione **C#**.</span><span class="sxs-lookup"><span data-stu-id="09756-107">Select **C#** as the language.</span></span>

1. <span data-ttu-id="09756-108">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="09756-108">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="09756-109">Lasciare **Percorso** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="09756-109">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="09756-110">Selezionare un account di archiviazione di Azure esistente oppure selezionare **Crea** se si vuole che Azure crei un nuovo account per l'utente.</span><span class="sxs-lookup"><span data-stu-id="09756-110">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="09756-111">Creare un contenitore BLOB</span><span class="sxs-lookup"><span data-stu-id="09756-111">Create a blob container</span></span>

<span data-ttu-id="09756-112">Ora che è stato creato un trigger di BLOB, si userà Storage Explorer per creare un BLOB e attivare la funzione.</span><span class="sxs-lookup"><span data-stu-id="09756-112">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="09756-113">Aprire l'account di archiviazione usato (o creato) in una nuova scheda. Un modo semplice per farlo consiste nell'aprire una nuova scheda del portale di Azure e fare clic su **Account di archiviazione** nella barra laterale oppure usare **Tutti i servizi** nella barra laterale e quindi filtrare per nome.</span><span class="sxs-lookup"><span data-stu-id="09756-113">Open the storage account you used (or created) in a new tab. An easy way to do this is to open a new Azure Portal tab and click on **Storage accounts** in the sidebar, or to use **All services** in the sidebar and then filter by the name.</span></span> <span data-ttu-id="09756-114">Si sceglie di aprire una nuova scheda per poter passare da uno all'altro dei due servizi usati.</span><span class="sxs-lookup"><span data-stu-id="09756-114">We want to use a new tab so we can switch between the two services we are working with.</span></span>

1. <span data-ttu-id="09756-115">Fare clic sulla sezione **Storage Explorer (anteprima)** per aprire un nuovo pannello in cui è possibile usare i BLOB e i file.</span><span class="sxs-lookup"><span data-stu-id="09756-115">Click on the **Storage Explorer (preview)** section - this will open a new blade where you can work with blobs and files.</span></span>

<span data-ttu-id="09756-116">Tenere presente che il trigger del BLOB monitora solo la posizione descritta nel campo **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="09756-116">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="09756-117">Per impostazione predefinita, il percorso deve essere:</span><span class="sxs-lookup"><span data-stu-id="09756-117">By default, our path should be:</span></span>

> <span data-ttu-id="09756-118">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="09756-118">samples-workitems/{name}</span></span>

<span data-ttu-id="09756-119">È necessario creare un contenitore denominato **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="09756-119">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="09756-120">Fare clic con il pulsante destro del mouse su **CONTENITORI BLOB** e scegliere **Crea contenitore BLOB**.</span><span class="sxs-lookup"><span data-stu-id="09756-120">Right-click **BLOB CONTAINERS** and select **Create blob container**.</span></span>

1. <span data-ttu-id="09756-121">Immettere **samples-workitems** come nome e lasciare l'impostazione predefinita **Privato** per il livello di accesso.</span><span class="sxs-lookup"><span data-stu-id="09756-121">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="09756-122">Attivare il trigger del BLOB</span><span class="sxs-lookup"><span data-stu-id="09756-122">Turn on your blob trigger</span></span>

<span data-ttu-id="09756-123">Ora che è stato creato il contenitore per il monitoraggio, è possibile eseguire la funzione per visualizzare l'output quando il BLOB viene creato.</span><span class="sxs-lookup"><span data-stu-id="09756-123">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="09756-124">Tornare alla scheda Funzione di Azure (o riaprirla).</span><span class="sxs-lookup"><span data-stu-id="09756-124">Switch back to the Azure Function tab (or reopen it).</span></span>

1. <span data-ttu-id="09756-125">Selezionare il trigger del BLOB per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="09756-125">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="09756-126">Selezionare **Esegui** per aprire la finestra di output.</span><span class="sxs-lookup"><span data-stu-id="09756-126">Select **Run** - this will open the output window.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="09756-127">Creare un BLOB</span><span class="sxs-lookup"><span data-stu-id="09756-127">Create a blob</span></span>

<span data-ttu-id="09756-128">Il trigger del blob è ora attivo e in ascolto per l'attività.</span><span class="sxs-lookup"><span data-stu-id="09756-128">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="09756-129">È possibile creare un blob per vedere se viene visualizzato un messaggio di registro.</span><span class="sxs-lookup"><span data-stu-id="09756-129">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="09756-130">In Storage Explorer selezionare il contenitore **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="09756-130">In Storage explorer, select the **samples-workitems** container.</span></span>

1. <span data-ttu-id="09756-131">Selezionare **Carica** dalla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="09756-131">Select **Upload** from the toolbar.</span></span>

1. <span data-ttu-id="09756-132">Selezionare i file dal computer.</span><span class="sxs-lookup"><span data-stu-id="09756-132">Select any file from your computer.</span></span>

1. <span data-ttu-id="09756-133">Selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="09756-133">Select **Upload**.</span></span>

1. <span data-ttu-id="09756-134">Tornare alla scheda Funzioni di Azure e cercare nei log di output un messaggio che visualizza il file caricato.</span><span class="sxs-lookup"><span data-stu-id="09756-134">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>