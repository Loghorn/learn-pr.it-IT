<span data-ttu-id="a5d53-101">In questo esercizio si userà il portale di Azure per creare un account di archiviazione appropriato per un'app Web fittizia di previsioni per il surf nella California del sud.</span><span class="sxs-lookup"><span data-stu-id="a5d53-101">In this exercise, you will use the Azure portal to create a storage account that is appropriate for a fictitious southern California surf report Web App.</span></span>

## <a name="design-goals"></a><span data-ttu-id="a5d53-102">Obiettivi di progettazione</span><span class="sxs-lookup"><span data-stu-id="a5d53-102">Design goals</span></span>

<span data-ttu-id="a5d53-103">Il sito di previsioni per il surf consente agli utenti di caricare foto e video delle condizioni delle spiagge locali.</span><span class="sxs-lookup"><span data-stu-id="a5d53-103">The surf-report site lets users upload photos and videos of their local beach conditions.</span></span> <span data-ttu-id="a5d53-104">I visualizzatori useranno il contenuto per scegliere la spiaggia con le condizioni migliori per il surf.</span><span class="sxs-lookup"><span data-stu-id="a5d53-104">Viewers will use the content to help them choose the beach with the best surfing conditions.</span></span> <span data-ttu-id="a5d53-105">Ecco l'elenco degli obiettivi di progettazione e funzionalità:</span><span class="sxs-lookup"><span data-stu-id="a5d53-105">Your list of design and feature goals is:</span></span>

- <span data-ttu-id="a5d53-106">Il contenuto video deve caricarsi rapidamente</span><span class="sxs-lookup"><span data-stu-id="a5d53-106">Video content must load quickly</span></span>
- <span data-ttu-id="a5d53-107">Il sito deve essere in grado di gestire i picchi imprevisti nei volumi di caricamento</span><span class="sxs-lookup"><span data-stu-id="a5d53-107">The site must handle unexpected spikes in upload volume</span></span>
- <span data-ttu-id="a5d53-108">Il contenuto non aggiornato deve essere rimosso man mano che cambiano le condizioni per il surf, in modo che il sito mostri sempre le condizioni attuali</span><span class="sxs-lookup"><span data-stu-id="a5d53-108">Outdated content will be removed as surf conditions change so the site always shows current conditions</span></span>

<span data-ttu-id="a5d53-109">Si opta per un'implementazione che memorizza nel buffer il contenuto caricato in una coda di Azure per l'elaborazione e quindi lo sposta in un BLOB di Azure per l'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="a5d53-109">You decide on an implementation that buffers uploaded content in an Azure Queue for processing and then moves it into an Azure Blob for storage.</span></span> <span data-ttu-id="a5d53-110">A questo scopo è necessario un account di archiviazione in grado di contenere sia le code che i BLOB fornendo al contempo un accesso con bassa latenza al contenuto.</span><span class="sxs-lookup"><span data-stu-id="a5d53-110">You need a storage account that can hold both Queues and Blobs while delivering low-latency access to your content.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="a5d53-111">Passaggi dell'esercizio</span><span class="sxs-lookup"><span data-stu-id="a5d53-111">Exercise steps</span></span>

### <a name="launch-the-blade"></a><span data-ttu-id="a5d53-112">Avviare il pannello</span><span class="sxs-lookup"><span data-stu-id="a5d53-112">Launch the blade</span></span>

1. <span data-ttu-id="a5d53-113">Nel Web browser passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e accedere al proprio account.</span><span class="sxs-lookup"><span data-stu-id="a5d53-113">In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.</span></span>

1. <span data-ttu-id="a5d53-114">Nella barra laterale sinistra selezionare **Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-114">In the left sidebar, select **Create a resource**.</span></span>

1. <span data-ttu-id="a5d53-115">Selezionare il titolo **Archiviazione** in Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a5d53-115">Select the **Storage** heading in the Azure Marketplace.</span></span>

1. <span data-ttu-id="a5d53-116">Selezionare **Account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-116">Select **Storage account**.</span></span> <span data-ttu-id="a5d53-117">Nel portale viene visualizzato il pannello **Crea account di archiviazione**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-117">The portal will display the **Create storage account** blade.</span></span>

### <a name="configure-the-basic-options"></a><span data-ttu-id="a5d53-118">Configurare le opzioni di base</span><span class="sxs-lookup"><span data-stu-id="a5d53-118">Configure the basic options</span></span>

1. <span data-ttu-id="a5d53-119">Selezionare la scheda **Informazioni di base** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="a5d53-119">Select the **Basics** tab at the top of the blade.</span></span>

1. <span data-ttu-id="a5d53-120">**Sottoscrizione:** selezionare una delle proprie sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="a5d53-120">**Subscription**: Select one of your subscriptions.</span></span>

1. <span data-ttu-id="a5d53-121">**Gruppo di risorse**: creare un nuovo gruppo di risorse denominato **SurfReportResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-121">**Resource group**: Create a new resource group named **SurfReportResourceGroup**.</span></span>

1. <span data-ttu-id="a5d53-122">**Nome account di archiviazione**: immettere un valore univoco globale come `surfreport` + le proprie iniziali + un numero.</span><span class="sxs-lookup"><span data-stu-id="a5d53-122">**Storage account name**: Enter a globally-unique value like `surfreport` + your initials + a number.</span></span>

 1. <span data-ttu-id="a5d53-123">**Posizione**: selezionare **Stati Uniti occidentali**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-123">**Location**: Select **West US**.</span></span>

    <span data-ttu-id="a5d53-124">Motivazione logica: l'applicazione è destinata agli utenti residenti nella California del sud.</span><span class="sxs-lookup"><span data-stu-id="a5d53-124">Rationale: The application is intended for users in southern California.</span></span> <span data-ttu-id="a5d53-125">Per ridurre al minimo la latenza durante il caricamento dei video, I BLOB devono essere ospitati nelle vicinanze di questi utenti, pertanto **Stati Uniti occidentali** è una buona scelta.</span><span class="sxs-lookup"><span data-stu-id="a5d53-125">To minimize latency when loading videos, the Blobs should be hosted close to these users; this makes **West US** a good choice.</span></span>

1. <span data-ttu-id="a5d53-126">**Modello di distribuzione**: selezionare **Gestione risorse**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-126">**Deployment model**: Select **Resource manager**.</span></span>
    
    <span data-ttu-id="a5d53-127">Motivazione logica: **Gestione risorse** è appropriato perché consente di usare un gruppo di risorse per gestire l'app Web, l'account di archiviazione e altri elementi per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a5d53-127">Rationale: **Resource manager** is appropriate because it will let you use a resource group to manage the Web App, storage account, etc. for the application.</span></span>

1. <span data-ttu-id="a5d53-128">**Prestazioni**: selezionare **Standard**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-128">**Performance**: Select **Standard**.</span></span>

    <span data-ttu-id="a5d53-129">Motivazione logica: non è possibile usare l'opzione **Premium** in quanto limiterebbe l'account di archiviazione ai BLOB di pagine.</span><span class="sxs-lookup"><span data-stu-id="a5d53-129">Rationale: You cannot use the **Premium** option because it would limit the storage account to page Blobs.</span></span> <span data-ttu-id="a5d53-130">Sono necessari BLOB in blocchi per i video e una coda per la memorizzazione nel buffer ed entrambi sono disponibili solo nell'opzione **Standard**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-130">You need block Blobs for your videos and a Queue for buffering both of which are only available in the **Standard** option.</span></span>

1. <span data-ttu-id="a5d53-131">**Tipologia account**: selezionare **Archiviazione v2 (utilizzo generico v2)**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-131">**Account kind**: Select **StorageV2 (general purpose v2)**.</span></span>

    <span data-ttu-id="a5d53-132">Motivazione logica: **Archiviazione v2 (utilizzo generico v2)** è la scelta giusta in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="a5d53-132">Rationale: **StorageV2 (general purpose v2)** is the right choice here.</span></span> <span data-ttu-id="a5d53-133">Sono infatti necessari sia BLOB che una coda, quindi l'opzione **Archiviazione BLOB** non è adatta.</span><span class="sxs-lookup"><span data-stu-id="a5d53-133">You need a mix of Blobs and a Queue, so the **Blob storage** option will not work.</span></span> <span data-ttu-id="a5d53-134">La scelta di un account di tipo **Archiviazione (utilizzo generico v1)** non porterebbe alcun vantaggio a questa applicazione, poiché limiterebbe le funzionalità accessibili senza presumibilmente ridurre il costo del carico di lavoro previsto.</span><span class="sxs-lookup"><span data-stu-id="a5d53-134">For this application, there would be no benefit to choosing a **Storage (general purpose v1)** account since that would limit the features you could access and would be unlikely to reduce the cost of your expected workload.</span></span>

1. <span data-ttu-id="a5d53-135">**Replica**: selezionare **Archiviazione con ridondanza locale**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-135">**Replication**: Select **Locally-redundant storage (LRS)**.</span></span>

    <span data-ttu-id="a5d53-136">Motivazione logica: le immagini e i video diventano rapidamente obsoleti e vengono rimossi dal sito.</span><span class="sxs-lookup"><span data-stu-id="a5d53-136">Rationale: The images and videos quickly become out-of-date and are removed from the site.</span></span> <span data-ttu-id="a5d53-137">Non vale quindi la pena di pagare di più per la ridondanza globale.</span><span class="sxs-lookup"><span data-stu-id="a5d53-137">This means there is little value to paying extra for global redundancy.</span></span> <span data-ttu-id="a5d53-138">Nel caso in cui un evento catastrofico causi la perdita di dati, è possibile riavviare il sito con contenuti aggiornati caricati dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="a5d53-138">If a catastrophic event results in data loss, you can restart the site with fresh content from your users.</span></span>

1. <span data-ttu-id="a5d53-139">**Livello di accesso (predefinito)**: selezionare **Frequente**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-139">**Access tier (default)**: Select **Hot**.</span></span>
   
    <span data-ttu-id="a5d53-140">Motivazione logica: si vuole che i video vengano caricati rapidamente, quindi si userà l'opzione con prestazioni elevate per i BLOB.</span><span class="sxs-lookup"><span data-stu-id="a5d53-140">Rationale: You want the videos to load quickly so you will use the high-performance option for your Blobs.</span></span>
   
<span data-ttu-id="a5d53-141">Lo screenshot seguente mostra le impostazioni compilate per la scheda **Informazioni di base**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-141">The following screenshot shows the completed settings for the **Basics** tab.</span></span>

![Screenshot di un pannello Crea account di archiviazione con la scheda **Informazioni di base** selezionata.](../media-drafts/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a><span data-ttu-id="a5d53-143">Configurare le opzioni avanzate</span><span class="sxs-lookup"><span data-stu-id="a5d53-143">Configure the advanced options</span></span>

1. <span data-ttu-id="a5d53-144">Selezionare la scheda **Avanzate** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="a5d53-144">Select the **Advanced** tab at the top of the blade.</span></span>

1. <span data-ttu-id="a5d53-145">**Trasferimento sicuro obbligatorio**: selezionare **Abilitato**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-145">**Secure transfer required**: Select **Enabled**.</span></span>

    <span data-ttu-id="a5d53-146">Motivazione logica: Https via cavo è generalmente considerato come la procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="a5d53-146">Rationale: Https across the wire is generally considered best practice.</span></span>

1. <span data-ttu-id="a5d53-147">**Reti virtuali**: selezionare **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-147">**Virtual Networks**: Select **Disabled**.</span></span>

    <span data-ttu-id="a5d53-148">Motivazione logica: il contenuto è esposto al pubblico ed è necessario consentire l'accesso da client pubblici.</span><span class="sxs-lookup"><span data-stu-id="a5d53-148">Rationale: The content is public facing and you need to allow access from public clients.</span></span>

<span data-ttu-id="a5d53-149">Lo screenshot seguente mostra le impostazioni compilate per la scheda **Avanzate**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-149">The following screenshot shows the completed settings for the **Advanced** tab.</span></span>

![Screenshot di un pannello Crea account di archiviazione con la scheda **Avanzate** selezionata.](../media-drafts/5-create-storage-account-advanced.png)

### <a name="create"></a><span data-ttu-id="a5d53-151">Creare</span><span class="sxs-lookup"><span data-stu-id="a5d53-151">Create</span></span>

1. <span data-ttu-id="a5d53-152">Fare clic sul pulsante **Rivedi e crea** nella parte inferiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="a5d53-152">Click the **Review + create** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="a5d53-153">Nella schermata successiva fare clic sul pulsante **Crea** nella parte inferiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="a5d53-153">On the next screen, click the **Create** button at the bottom of the blade.</span></span>

1. <span data-ttu-id="a5d53-154">Attendere che la risorsa venga creata.</span><span class="sxs-lookup"><span data-stu-id="a5d53-154">Wait for the resource to be created.</span></span>

### <a name="verify"></a><span data-ttu-id="a5d53-155">Verificare</span><span class="sxs-lookup"><span data-stu-id="a5d53-155">Verify</span></span>

1. <span data-ttu-id="a5d53-156">Selezionare il collegamento **Account di archiviazione** nella barra laterale sinistra.</span><span class="sxs-lookup"><span data-stu-id="a5d53-156">Select the **Storage accounts** link in the left sidebar.</span></span>

1. <span data-ttu-id="a5d53-157">Individuare il nuovo account di archiviazione nell'elenco per verificare che la creazione sia riuscita.</span><span class="sxs-lookup"><span data-stu-id="a5d53-157">Locate the new storage account in the list to verify that creation succeeded.</span></span>

### <a name="clean-up"></a><span data-ttu-id="a5d53-158">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="a5d53-158">Clean up</span></span>

1. <span data-ttu-id="a5d53-159">Selezionare il collegamento **Gruppo di risorse** nella barra laterale sinistra.</span><span class="sxs-lookup"><span data-stu-id="a5d53-159">Select the **Resource groups** link in the left sidebar.</span></span>

1. <span data-ttu-id="a5d53-160">Individuare **SurfReportResourceGroup** nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="a5d53-160">Locate **SurfReportResourceGroup** in the list.</span></span>

1. <span data-ttu-id="a5d53-161">Fare clic con il pulsante destro del mouse sulla voce **SurfReportResourceGroup** e scegliere **Elimina gruppo di risorse** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="a5d53-161">Right-click on the **SurfReportResourceGroup** entry and select **Delete resource group** from the context menu.</span></span>

1. <span data-ttu-id="a5d53-162">Digitare il nome del gruppo di risorse nel campo di conferma.</span><span class="sxs-lookup"><span data-stu-id="a5d53-162">Type the resource group name into the confirmation field.</span></span>

1. <span data-ttu-id="a5d53-163">Fare clic sul pulsante **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="a5d53-163">Click the **Delete** button.</span></span>

## <a name="summary"></a><span data-ttu-id="a5d53-164">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a5d53-164">Summary</span></span>

<span data-ttu-id="a5d53-165">È stato creato un account di archiviazione con le impostazioni più appropriate per i propri requisiti aziendali.</span><span class="sxs-lookup"><span data-stu-id="a5d53-165">You created a storage account with settings driven by your business requirements.</span></span> <span data-ttu-id="a5d53-166">Ad esempio, è stato selezionato il data center Stati Uniti occidentali perché i clienti risiedevano principalmente nella California del sud.</span><span class="sxs-lookup"><span data-stu-id="a5d53-166">For example, you selected a West US datacenter because your customers were primarily located in southern California.</span></span> <span data-ttu-id="a5d53-167">Il flusso descritto è un flusso tipico: prima si analizzano i dati e gli obiettivi e poi si configurano le opzioni dell'account di archiviazione più appropriate.</span><span class="sxs-lookup"><span data-stu-id="a5d53-167">This is a typical flow: first analyze your data and goals and then configure the storage account options to match.</span></span>