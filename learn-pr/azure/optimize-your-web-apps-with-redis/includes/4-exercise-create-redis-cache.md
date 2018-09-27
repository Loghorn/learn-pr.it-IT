<span data-ttu-id="20a8e-101">È ora possibile creare un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.</span><span class="sxs-lookup"><span data-stu-id="20a8e-101">Let's create an Azure Redis Cache instance to store and return commonly used values.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a><span data-ttu-id="20a8e-102">Creare una cache Redis in Azure</span><span class="sxs-lookup"><span data-stu-id="20a8e-102">Create a Redis cache in Azure</span></span>

1. <span data-ttu-id="20a8e-103">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="20a8e-103">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="20a8e-104">Fare quindi clic su **Crea una risorsa**, su **Database** e quindi su **Cache Redis**.</span><span class="sxs-lookup"><span data-stu-id="20a8e-104">Click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="20a8e-105">Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="20a8e-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a><span data-ttu-id="20a8e-107">Configurare la cache</span><span class="sxs-lookup"><span data-stu-id="20a8e-107">Configure your cache</span></span>

<span data-ttu-id="20a8e-108">Applicare le impostazioni seguenti nella cache.</span><span class="sxs-lookup"><span data-stu-id="20a8e-108">Apply the following settings on the cache.</span></span>

1. <span data-ttu-id="20a8e-109">**Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp [nnn]**, dove `[nnn]` viene sostituito con numeri casuali.</span><span class="sxs-lookup"><span data-stu-id="20a8e-109">**DNS Name:** Create a globally unique name such as **ContosoSportsApp[nnn]**, where `[nnn]` is replaced with random numbers.</span></span>

1. <span data-ttu-id="20a8e-110">**Sottoscrizione:** selezionare la sottoscrizione Concierge.</span><span class="sxs-lookup"><span data-stu-id="20a8e-110">**Subscription:** Select the Concierge subscription.</span></span>

1. <span data-ttu-id="20a8e-111">**Gruppo di risorse:** selezionare <rgn>[nome del gruppo di risorse sandbox]</rgn> per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="20a8e-111">**Resource group:** Select <rgn>[sandbox resource group name]</rgn> for the Resource Group.</span></span>

1. <span data-ttu-id="20a8e-112">**Località:** è in genere consigliabile selezionare una località vicina ai clienti, in questo caso la costa orientale degli Stati Uniti.</span><span class="sxs-lookup"><span data-stu-id="20a8e-112">**Location:** Normally, you would select a location near your customers - in this case, the East Coast.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. <span data-ttu-id="20a8e-113">**Piano tariffario:** selezionare **Basic C0**.</span><span class="sxs-lookup"><span data-stu-id="20a8e-113">**Pricing tier:** Select **Basic C0**.</span></span> <span data-ttu-id="20a8e-114">Questo è il livello minimo consentito.</span><span class="sxs-lookup"><span data-stu-id="20a8e-114">This is the lowest tier you can use.</span></span> <span data-ttu-id="20a8e-115">È probabile che le app di produzione richiedano l'archiviazione di una quantità maggiore di dati e l'utilizzo di alcune funzionalità Premium come il clustering, che necessita di un livello superiore.</span><span class="sxs-lookup"><span data-stu-id="20a8e-115">Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.</span></span>

1. <span data-ttu-id="20a8e-116">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="20a8e-116">Click **Create**.</span></span> <span data-ttu-id="20a8e-117">Azure creerà e distribuirà l'istanza di Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="20a8e-117">Azure will create and deploy the Redis Cache instance for you.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="20a8e-118">In genere, la risorsa di Cache Redis sarà creata e visibile molto rapidamente nel portale di Azure, ma non sarà disponibile per alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="20a8e-118">Usually, the Redis cache resource will be created and viewable in the Azure portal very quickly, but the cache itself will not be available for a few minutes.</span></span> <span data-ttu-id="20a8e-119">I passaggi successivi illustrano come verificare lo stato della cache.</span><span class="sxs-lookup"><span data-stu-id="20a8e-119">The next steps show how to check the status of your cache.</span></span>

## <a name="verify-your-data"></a><span data-ttu-id="20a8e-120">Verificare i dati</span><span class="sxs-lookup"><span data-stu-id="20a8e-120">Verify your data</span></span>

<span data-ttu-id="20a8e-121">È possibile usare la funzionalità **Console** nel portale di Azure per inviare comandi all'istanza di Cache Redis dopo la creazione.</span><span class="sxs-lookup"><span data-stu-id="20a8e-121">You can use the **Console** feature in the Azure portal to issue commands to your Redis cache instance after it has been created.</span></span>

1. <span data-ttu-id="20a8e-122">Al termine della distribuzione, individuare la cache Redis tramite il popup **Notifica** selezionando **Tutte le risorse** nella barra laterale sinistra e usando la casella di filtro a sinistra per selezionare le istanze di Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="20a8e-122">Locate your Redis cache through the **Notification** popup when it finishes deployment, or by selecting **All Resources** in the left-hand sidebar and using the filter box on the left to select Redis Cache instances.</span></span> <span data-ttu-id="20a8e-123">In alternativa, è possibile usare la casella di ricerca nella parte superiore e digitare il nome della cache.</span><span class="sxs-lookup"><span data-stu-id="20a8e-123">Alternatively, you can use the search box at the top and type the name of the cache.</span></span>

1. <span data-ttu-id="20a8e-124">Selezionare l'istanza della cache Redis.</span><span class="sxs-lookup"><span data-stu-id="20a8e-124">Select your Redis cache instance.</span></span>

1. <span data-ttu-id="20a8e-125">Controllare il valore del campo "Stato".</span><span class="sxs-lookup"><span data-stu-id="20a8e-125">Check the value of the "Status" field.</span></span> <span data-ttu-id="20a8e-126">La cache non è pronta finché lo stato non è "In esecuzione".</span><span class="sxs-lookup"><span data-stu-id="20a8e-126">The cache is not ready until the status is "Running".</span></span> <span data-ttu-id="20a8e-127">Potrebbe essere necessario attendere qualche minuto prima di procedere.</span><span class="sxs-lookup"><span data-stu-id="20a8e-127">You might have to wait for a few minutes before proceeding.</span></span>

1. <span data-ttu-id="20a8e-128">Quando la cache è in esecuzione, fare clic sul pulsante `>_ Console` sulla barra degli strumenti nel pannello **Panoramica** della Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="20a8e-128">Once the cache is running, Click the `>_ Console` button in the toolbar on the **Overview** blade for your Redis Cache.</span></span> <span data-ttu-id="20a8e-129">Verrà aperta una console di Redis, che consente di immettere i comandi di Redis a basso livello.</span><span class="sxs-lookup"><span data-stu-id="20a8e-129">This will open a Redis console, which allows you to enter low-level Redis commands.</span></span> <span data-ttu-id="20a8e-130">Provare alcune delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="20a8e-130">Try some of the following:</span></span>

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

<span data-ttu-id="20a8e-131">Tornare al pannello **Panoramica** tramite la barra di navigazione nella parte superiore oppure usare la barra di scorrimento per scorrere la visualizzazione verso sinistra.</span><span class="sxs-lookup"><span data-stu-id="20a8e-131">Switch back to the **Overview** panel either through the breadcrumb bar on the top, or use the scrollbar to slide the view back to the left.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="20a8e-132">Recuperare le chiavi di accesso e il nome host</span><span class="sxs-lookup"><span data-stu-id="20a8e-132">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="20a8e-133">Selezionare **Impostazioni** > **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="20a8e-133">Select **Settings** > **Access keys**.</span></span>

1. <span data-ttu-id="20a8e-134">Copiare la **Stringa di connessione primaria (StackExchange.Redis)** in una posizione sicura. Sarà necessaria per l'esercizio successivo.</span><span class="sxs-lookup"><span data-stu-id="20a8e-134">Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.</span></span>

    <span data-ttu-id="20a8e-135">Questa chiave include la chiave primaria e il nome host in una stringa di connessione completa per l'uso all'interno delle impostazioni dell'applicazione per il pacchetto **StackExchange.Redis** che verrà usato.</span><span class="sxs-lookup"><span data-stu-id="20a8e-135">This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.</span></span>

<span data-ttu-id="20a8e-136">Nelle unità seguenti verranno illustrati i comandi disponibili per interrogare la cache.</span><span class="sxs-lookup"><span data-stu-id="20a8e-136">Next, let's learn about some of the commands we can use to interrogate the cache.</span></span>