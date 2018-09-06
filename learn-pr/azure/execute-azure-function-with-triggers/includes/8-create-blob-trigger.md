<span data-ttu-id="1f74a-101">In questo esercizio si creerà una funzione di Azure per visualizzare il nome e le dimensioni di un BLOB quando viene creato o aggiornato.</span><span class="sxs-lookup"><span data-stu-id="1f74a-101">In this exercise, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span> 

> [!NOTE]
> <span data-ttu-id="1f74a-102">Per completare questo esercizio, assicurarsi di essere connessi al [portale di Azure](https://portal.azure.com?azure-portal=true) con un account valido.</span><span class="sxs-lookup"><span data-stu-id="1f74a-102">To complete this exercise, make sure you're signed into the [Azure portal](https://portal.azure.com?azure-portal=true) with a valid account.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="1f74a-103">Creare un trigger del BLOB</span><span class="sxs-lookup"><span data-stu-id="1f74a-103">Create a blob trigger</span></span>

<span data-ttu-id="1f74a-104">Anche in questo caso, continuiamo a usare l'applicazione di Funzioni di Azure esistente e aggiungiamo un trigger del blob.</span><span class="sxs-lookup"><span data-stu-id="1f74a-104">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="1f74a-105">Selezionare **Funzioni**, quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="1f74a-105">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Selezionare Funzioni, quindi il segno più](../media-drafts/4-hover-function.png)

2. <span data-ttu-id="1f74a-107">Selezionare **Funzione personalizzata** e quindi **Trigger BLOB**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-107">Select **Custom Function** and then **Blob trigger**.</span></span>

3. <span data-ttu-id="1f74a-108">Selezionare il linguaggio di programmazione **C#**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-108">Select **C#** as the language.</span></span> 

4. <span data-ttu-id="1f74a-109">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="1f74a-109">Leave the **Name** set to the default value.</span></span>

5. <span data-ttu-id="1f74a-110">Lasciare **Percorso** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="1f74a-110">Leave the **Path** set to the default value.</span></span>

6. <span data-ttu-id="1f74a-111">Selezionare un account di archiviazione di Azure esistente oppure selezionare **Crea** se si vuole che Azure crei un nuovo account per l'utente.</span><span class="sxs-lookup"><span data-stu-id="1f74a-111">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="1f74a-112">Creare un contenitore BLOB</span><span class="sxs-lookup"><span data-stu-id="1f74a-112">Create a blob container</span></span>

<span data-ttu-id="1f74a-113">Ora che è stato creato un trigger di BLOB, si userà Storage Explorer per creare un BLOB e attivare la funzione.</span><span class="sxs-lookup"><span data-stu-id="1f74a-113">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="1f74a-114">Aprire l'account di archiviazione usato (o creato) in una nuova scheda. Un modo semplice per farlo consiste nell'aprire una nuova scheda del portale di Azure e fare clic su **Account di archiviazione** nella barra laterale oppure usare **Tutti i servizi** nella barra laterale e quindi filtrare per nome.</span><span class="sxs-lookup"><span data-stu-id="1f74a-114">Open the storage account you used (or created) in a new tab. An easy way to do this is to open a new Azure Portal tab and click on **Storage accounts** in the sidebar, or to use **All services** in the sidebar and then filter by the name.</span></span> <span data-ttu-id="1f74a-115">Si sceglie di aprire una nuova scheda per poter passare da uno all'altro dei due servizi usati.</span><span class="sxs-lookup"><span data-stu-id="1f74a-115">We want to use a new tab so we can switch between the two services we are working with.</span></span>

2. <span data-ttu-id="1f74a-116">Fare clic sulla sezione **Storage Explorer (anteprima)** per aprire un nuovo pannello in cui è possibile usare i BLOB e i file.</span><span class="sxs-lookup"><span data-stu-id="1f74a-116">Click on the **Storage Explorer (preview)** section - this will open a new blade where you can work with blobs and files.</span></span>

<span data-ttu-id="1f74a-117">Tenere presente che il trigger del BLOB monitora solo la posizione descritta nel campo **Percorso**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-117">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="1f74a-118">Per impostazione predefinita, il percorso deve essere:</span><span class="sxs-lookup"><span data-stu-id="1f74a-118">By default, our path should be:</span></span>

> <span data-ttu-id="1f74a-119">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="1f74a-119">samples-workitems/{name}</span></span>

<span data-ttu-id="1f74a-120">È necessario creare un contenitore denominato **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-120">We need to create a container called **samples-workitems**.</span></span>

3. <span data-ttu-id="1f74a-121">Fare clic con il pulsante destro del mouse su **CONTENITORI BLOB** e scegliere **Crea contenitore BLOB**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-121">Right-click **BLOB CONTAINERS** and select **Create blob container**.</span></span>

4. <span data-ttu-id="1f74a-122">Immettere **samples-workitems** come nome e lasciare l'impostazione predefinita **Privato** per il livello di accesso.</span><span class="sxs-lookup"><span data-stu-id="1f74a-122">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="1f74a-123">Attivare il trigger del BLOB</span><span class="sxs-lookup"><span data-stu-id="1f74a-123">Turn on your blob trigger</span></span>

<span data-ttu-id="1f74a-124">Ora che è stato creato il contenitore per il monitoraggio, è possibile eseguire la funzione per visualizzare l'output quando il BLOB viene creato.</span><span class="sxs-lookup"><span data-stu-id="1f74a-124">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="1f74a-125">Tornare alla scheda Funzione di Azure (o riaprirla).</span><span class="sxs-lookup"><span data-stu-id="1f74a-125">Switch back to the Azure Function tab (or reopen it).</span></span>

2. <span data-ttu-id="1f74a-126">Selezionare il trigger del BLOB per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="1f74a-126">Select your blob trigger to open the code screen.</span></span>

3. <span data-ttu-id="1f74a-127">Selezionare **Esegui** per aprire la finestra di output.</span><span class="sxs-lookup"><span data-stu-id="1f74a-127">Select **Run** - this will open the output window.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="1f74a-128">Creare un BLOB</span><span class="sxs-lookup"><span data-stu-id="1f74a-128">Create a blob</span></span>

<span data-ttu-id="1f74a-129">Il trigger del blob è ora attivo e in ascolto per l'attività.</span><span class="sxs-lookup"><span data-stu-id="1f74a-129">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="1f74a-130">È possibile creare un blob per vedere se viene visualizzato un messaggio di registro.</span><span class="sxs-lookup"><span data-stu-id="1f74a-130">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="1f74a-131">In Storage Explorer selezionare il contenitore **samples-workitems**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-131">In Storage explorer, select the **samples-workitems** container.</span></span>

2. <span data-ttu-id="1f74a-132">Selezionare **Carica** dalla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="1f74a-132">Select **Upload** from the toolbar.</span></span>

3. <span data-ttu-id="1f74a-133">Selezionare i file dal computer.</span><span class="sxs-lookup"><span data-stu-id="1f74a-133">Select any file from your computer.</span></span>

4. <span data-ttu-id="1f74a-134">Selezionare **Carica**.</span><span class="sxs-lookup"><span data-stu-id="1f74a-134">Select **Upload**.</span></span>

5. <span data-ttu-id="1f74a-135">Tornare alla scheda Funzione di Azure e cercare nei log di output un messaggio che visualizza il file caricato.</span><span class="sxs-lookup"><span data-stu-id="1f74a-135">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>

## <a name="pause-the-function"></a><span data-ttu-id="1f74a-136">Sospendere la funzione</span><span class="sxs-lookup"><span data-stu-id="1f74a-136">Pause the Function</span></span>

<span data-ttu-id="1f74a-137">Per assicurarsi che non verranno applicati costi per richieste aggiuntive, è possibile fare clic su **Sospendi** sopra la finestra del log.</span><span class="sxs-lookup"><span data-stu-id="1f74a-137">To ensure that you aren't charged for additional requests, you can click **Pause** above the log window.</span></span>

![Sospendere la funzione](../media-drafts/4-pause-timer.png)


