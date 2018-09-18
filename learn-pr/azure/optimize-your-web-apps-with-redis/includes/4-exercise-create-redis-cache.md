<span data-ttu-id="e2afd-101">È ora possibile creare un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.</span><span class="sxs-lookup"><span data-stu-id="e2afd-101">Let's create an Azure Redis Cache instance to store and return commonly used values.</span></span>

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a><span data-ttu-id="e2afd-102">Creare una cache Redis in Azure</span><span class="sxs-lookup"><span data-stu-id="e2afd-102">Create a Redis cache in Azure</span></span>

1. <span data-ttu-id="e2afd-103">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="e2afd-103">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="e2afd-104">Fare quindi clic su **Crea una risorsa**, su **Database** e quindi su **Cache Redis**.</span><span class="sxs-lookup"><span data-stu-id="e2afd-104">Click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="e2afd-105">Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e2afd-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a><span data-ttu-id="e2afd-107">Identificare la posizione per la cache</span><span class="sxs-lookup"><span data-stu-id="e2afd-107">Identify the location for the cache</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a><span data-ttu-id="e2afd-108">Configurare la cache</span><span class="sxs-lookup"><span data-stu-id="e2afd-108">Configure your cache</span></span>

<span data-ttu-id="e2afd-109">Applicare le impostazioni seguenti nella cache.</span><span class="sxs-lookup"><span data-stu-id="e2afd-109">Apply the following settings on the cache.</span></span>

1. <span data-ttu-id="e2afd-110">**Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp1028**.</span><span class="sxs-lookup"><span data-stu-id="e2afd-110">**DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.</span></span>

1. <span data-ttu-id="e2afd-111">**Sottoscrizione:** selezionare la sottoscrizione di Azure Sandbox.</span><span class="sxs-lookup"><span data-stu-id="e2afd-111">**Subscription:** Select the Azure Sandbox subscription.</span></span>

1. <span data-ttu-id="e2afd-112">**Gruppo di risorse:** selezionare <rgn>[nome del gruppo di risorse del sandbox]</rgn> per il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e2afd-112">**Resource group:** Select <rgn>[Sandbox resource group name]</rgn> for the Resource Group.</span></span>

1. <span data-ttu-id="e2afd-113">**Località:** è in genere consigliabile selezionare una località vicina ai clienti, in questo caso la costa orientale degli Stati Uniti.</span><span class="sxs-lookup"><span data-stu-id="e2afd-113">**Location:** Normally, you would select a location near your customers - in this case, the East Coast.</span></span> <span data-ttu-id="e2afd-114">Azure Sandbox consente tuttavia di selezionare solo aree specifiche per le risorse, come indicato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e2afd-114">However, the Azure Sandbox only allows specific regions to be selected for resources as noted above.</span></span> <span data-ttu-id="e2afd-115">Selezionare una di tali località.</span><span class="sxs-lookup"><span data-stu-id="e2afd-115">Please select one of those locations.</span></span>

1. <span data-ttu-id="e2afd-116">**Piano tariffario:** selezionare **Basic C0**.</span><span class="sxs-lookup"><span data-stu-id="e2afd-116">**Pricing tier:** Select **Basic C0**.</span></span> <span data-ttu-id="e2afd-117">Questo è il livello minimo consentito.</span><span class="sxs-lookup"><span data-stu-id="e2afd-117">This is the lowest tier you can use.</span></span> <span data-ttu-id="e2afd-118">È probabile che le app di produzione richiedano l'archiviazione di una quantità maggiore di dati e l'utilizzo di alcune funzionalità Premium come il clustering, che necessita di un livello superiore.</span><span class="sxs-lookup"><span data-stu-id="e2afd-118">Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.</span></span>

1. <span data-ttu-id="e2afd-119">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e2afd-119">Click **Create**.</span></span>

    <span data-ttu-id="e2afd-120">Lo screenshot seguente mostra una configurazione rappresentativa usata per creare una nuova risorsa di Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="e2afd-120">The following screenshot shows a representative configuration used to create a new Redis Cache resource.</span></span> <span data-ttu-id="e2afd-121">Si noti che la configurazione specifica sarà leggermente diversa a causa di Azure Sandbox.</span><span class="sxs-lookup"><span data-stu-id="e2afd-121">Note that yours will be slightly different due to the Azure Sandbox.</span></span>

    ![Screenshot che mostra il pannello del portale di Azure quando si crea una nuova risorsa di Cache Redis, popolato con un nome DNS della configurazione di esempio, sottoscrizione, nuovo gruppo di risorse, area e piano tariffario.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> <span data-ttu-id="e2afd-123">Si dovrà attendere che la cache venga distribuita prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="e2afd-123">You will have to wait until the cache is deployed before continuing.</span></span> <span data-ttu-id="e2afd-124">Questo processo può richiedere del tempo.</span><span class="sxs-lookup"><span data-stu-id="e2afd-124">This process might take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="e2afd-125">Recuperare le chiavi di accesso e il nome host</span><span class="sxs-lookup"><span data-stu-id="e2afd-125">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="e2afd-126">Passare alla nuova istanza della cache nel portale di Azure e selezionare **Impostazioni** > **Chiavi di accesso**.</span><span class="sxs-lookup"><span data-stu-id="e2afd-126">Navigate to your new cache instance in the Azure portal and select **Settings** > **Access keys**.</span></span> 

1. <span data-ttu-id="e2afd-127">Copiare la **Stringa di connessione primaria (StackExchange.Redis)** in una posizione sicura. Sarà necessaria per l'esercizio successivo.</span><span class="sxs-lookup"><span data-stu-id="e2afd-127">Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.</span></span>

    <span data-ttu-id="e2afd-128">Questa chiave include la chiave primaria e il nome host in una stringa di connessione completa per l'uso all'interno delle impostazioni dell'applicazione per il pacchetto **StackExchange.Redis** che verrà usato.</span><span class="sxs-lookup"><span data-stu-id="e2afd-128">This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.</span></span>

<span data-ttu-id="e2afd-129">Nelle unità seguenti verranno illustrati i comandi disponibili per interrogare la cache.</span><span class="sxs-lookup"><span data-stu-id="e2afd-129">Next, let's learn about some of the commands we can use to interrogate the cache.</span></span>