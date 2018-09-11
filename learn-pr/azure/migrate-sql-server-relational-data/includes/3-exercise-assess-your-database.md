<span data-ttu-id="1b430-101">In questo esercizio si valuterà un database esistente mediante Data Migration Assistant e si esamineranno le funzionalità usate nell'istanza di SQL Server locale che non sono attualmente supportate dal database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="1b430-101">In this exercise, you'll assess an existing database using Data Migration Assistant and review any features used in the local SQL Server instance that aren't currently supported in by Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="1b430-102">Configurazione</span><span class="sxs-lookup"><span data-stu-id="1b430-102">Setup</span></span>

1. <span data-ttu-id="1b430-103">[Installare **Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595), se non lo si è già fatto.</span><span class="sxs-lookup"><span data-stu-id="1b430-103">[Install the **Data Migration Assistant**](https://www.microsoft.com/en-us/download/details.aspx?id=53595) if you haven't done so already.</span></span>

1. <span data-ttu-id="1b430-104">Sarà necessaria un'istanza di SQL Server in esecuzione. Assicurarsi pertanto di avere i dettagli sulla connessione a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="1b430-104">You'll need a SQL Server instance running, ensure you have connection details available.</span></span>
1. <span data-ttu-id="1b430-105">[\*\*\*\* likely replace with an LOD VM \*\*\*\*\*] <!-- TODO: --></span><span class="sxs-lookup"><span data-stu-id="1b430-105">[\*\*\*\* likely replace with an LOD VM \*\*\*\*\*] <!-- TODO: --></span></span>

1. <span data-ttu-id="1b430-106">Aprire un browser Internet e passare all'indirizzo https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span><span class="sxs-lookup"><span data-stu-id="1b430-106">Start an Internet browser and navigate to https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="1b430-107">In **Download OLTP** fare clic su **AdventureWorks2008R2.bak** e salvarlo nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="1b430-107">In **OLTP downloads**, click **AdventureWorks2008R2.bak** and save it to your local machine.</span></span>

1. <span data-ttu-id="1b430-108">In Management Studio ripristinare l'istanza predefinita di *AdventureWorks 2008R2*.</span><span class="sxs-lookup"><span data-stu-id="1b430-108">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-assessment"></a><span data-ttu-id="1b430-109">Creare una valutazione</span><span class="sxs-lookup"><span data-stu-id="1b430-109">Create an assessment</span></span>

1. <span data-ttu-id="1b430-110">Avviare **Microsoft Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="1b430-110">Start the **Microsoft Data Migration Assistant**.</span></span>

1. <span data-ttu-id="1b430-111">Nel riquadro di spostamento sinistro dell'app fare clic su __+__ per creare un nuovo progetto DMA.</span><span class="sxs-lookup"><span data-stu-id="1b430-111">In the app's left-hand navigation, click __+__ to create a new DMA project.</span></span>

1. <span data-ttu-id="1b430-112">Specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1b430-112">Specify the following options:</span></span>
    - <span data-ttu-id="1b430-113">**Tipo di progetto**: selezionare *Valutazione*</span><span class="sxs-lookup"><span data-stu-id="1b430-113">**Project type** - Select *Assessment*</span></span>
    - <span data-ttu-id="1b430-114">**Nome progetto**: immettere un nome per il progetto, ad esempio "Bicycle DB Assessment"</span><span class="sxs-lookup"><span data-stu-id="1b430-114">**Project name** - Enter a name for your project - for example "Bicycle DB Assessment"</span></span>
    - <span data-ttu-id="1b430-115">**Tipo del server di origine**: selezionare *SQL Server*</span><span class="sxs-lookup"><span data-stu-id="1b430-115">**Source server type** - Select *SQL Server*</span></span>
    - <span data-ttu-id="1b430-116">**Tipo del server di destinazione**: selezionare *Database SQL di Azure*</span><span class="sxs-lookup"><span data-stu-id="1b430-116">**Target server type** - Select *Azure SQL Database*</span></span>

1. <span data-ttu-id="1b430-117">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="1b430-117">Click **Create**.</span></span>
    <span data-ttu-id="1b430-118">![Screenshot che mostra la configurazione descritta in Data Migration Assistant per i dati SQL Server AdventureWorks.](../media-draft/3-create-assessment.png)</span><span class="sxs-lookup"><span data-stu-id="1b430-118">![Screenshot showing the described configuration in the Data Migration Assistant for your AdventureWorks SQL Server data.](../media-draft/3-create-assessment.png)</span></span>

1. <span data-ttu-id="1b430-119">Selezionare il tipo di report di valutazione. Selezionare entrambe le opzioni:</span><span class="sxs-lookup"><span data-stu-id="1b430-119">Select the assessment report type - check both:</span></span>
    - <span data-ttu-id="1b430-120">Verifica la compatibilità del database</span><span class="sxs-lookup"><span data-stu-id="1b430-120">Check database compatibility</span></span>
    - <span data-ttu-id="1b430-121">Verifica parità delle funzionalità</span><span class="sxs-lookup"><span data-stu-id="1b430-121">Check feature parity</span></span>

1. <span data-ttu-id="1b430-122">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="1b430-122">Click **Next**.</span></span>

## <a name="add-databases-to-assess"></a><span data-ttu-id="1b430-123">Aggiungere i database da valutare</span><span class="sxs-lookup"><span data-stu-id="1b430-123">Add databases to assess</span></span>

1. <span data-ttu-id="1b430-124">Fare clic su **Aggiungi origini** per aprire il menu della connessione.</span><span class="sxs-lookup"><span data-stu-id="1b430-124">Click **Add Sources** to open the connection menu.</span></span>
2. <span data-ttu-id="1b430-125">Immettere le informazioni seguenti</span><span class="sxs-lookup"><span data-stu-id="1b430-125">Enter the following information</span></span>
    - <span data-ttu-id="1b430-126">Immettere il nome dell'istanza di SQL Server esistente</span><span class="sxs-lookup"><span data-stu-id="1b430-126">Enter your existing SQL server instance name</span></span>
    - <span data-ttu-id="1b430-127">Selezionare il tipo **Autenticazione**</span><span class="sxs-lookup"><span data-stu-id="1b430-127">Select the **Authentication** type</span></span>
    - <span data-ttu-id="1b430-128">Specificare le proprietà di connessione per il server</span><span class="sxs-lookup"><span data-stu-id="1b430-128">Specify the connection properties for your server</span></span>
3. <span data-ttu-id="1b430-129">Fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="1b430-129">CLick **Connect**.</span></span>
4. <span data-ttu-id="1b430-130">In **Aggiungi origini** selezionare i database da valutare.</span><span class="sxs-lookup"><span data-stu-id="1b430-130">In **Add sources**, select the databases to assess.</span></span> <span data-ttu-id="1b430-131">Per questo esercizio selezionare **AdventureWorks2008R2**.</span><span class="sxs-lookup"><span data-stu-id="1b430-131">For this exercise, select **AdventureWorks2008R2**.</span></span>
5. <span data-ttu-id="1b430-132">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="1b430-132">Click **Add**.</span></span>
    > [!NOTE]
    > <span data-ttu-id="1b430-133">Per aggiungere database da più istanze di SQL Server, usare il pulsante **Aggiungi origini**.</span><span class="sxs-lookup"><span data-stu-id="1b430-133">To add databases from multiple SQL Server instances, use the **Add Sources** button.</span></span> <span data-ttu-id="1b430-134">Per rimuovere più database, tenere premuto MAIUSC+CTRL e selezionare i database da rimuovere, quindi fare clic su **Rimuovi origini**.</span><span class="sxs-lookup"><span data-stu-id="1b430-134">To remove multiple databases, hold the SHIFT+CTRL keys and select the databases to remove then click **Remove Sources**.</span></span>

6. <span data-ttu-id="1b430-135">Fare clic su **Avvia valutazione**.</span><span class="sxs-lookup"><span data-stu-id="1b430-135">Click **Start Assessment**.</span></span>

## <a name="view-results"></a><span data-ttu-id="1b430-136">Visualizzare i risultati</span><span class="sxs-lookup"><span data-stu-id="1b430-136">View results</span></span>

<span data-ttu-id="1b430-137">Se sono presenti più database, i risultati relativi a ogni database verranno visualizzati appena disponibili.</span><span class="sxs-lookup"><span data-stu-id="1b430-137">If there are multiple databases, the results for each database will appear as soon as it is available.</span></span> <span data-ttu-id="1b430-138">Non è necessario attendere che venga completata la valutazione di tutti i database.</span><span class="sxs-lookup"><span data-stu-id="1b430-138">You don't need to wait for all database assessments to complete.</span></span>

1. <span data-ttu-id="1b430-139">Una volta completata la valutazione di **AdventureWorks**, fare clic su **Problemi di compatibilità** e su **Funzionalità consigliate** per visualizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="1b430-139">Once the assessment for **AdventureWorks** is complete, click **Compatibility issues** and **Feature recommendations** to view the results.</span></span>
    - <span data-ttu-id="1b430-140">La categoria di parità delle funzionalità di SQL Server elenca le funzionalità che potrebbero non essere completamente supportate e la procedura per risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="1b430-140">The SQL Server feature parity category lists features that might not be fully supported and steps to remedy these issues.</span></span> <span data-ttu-id="1b430-141">I problemi di parità delle funzionalità non interrompono una migrazione.</span><span class="sxs-lookup"><span data-stu-id="1b430-141">Feature parity issues will not stop a migration.</span></span>
    - <span data-ttu-id="1b430-142">La categoria dei problemi di compatibilità elenca le funzionalità che potrebbero bloccare una migrazione e la procedura per risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="1b430-142">The Compatibility issues category lists features that would block a migration and steps to remedy these issues.</span></span>

## <a name="summary"></a><span data-ttu-id="1b430-143">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1b430-143">Summary</span></span>

<span data-ttu-id="1b430-144">In questo esercizio è stato valutato un database di SQL Server installato in locale per verificare la presenza di eventuali funzionalità che non sarebbero disponibili dopo la migrazione del database al database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="1b430-144">In this exercise, you assessed a locally installed SQL Server database to verify if any features would be unavailable when you migrate the database to Azure SQL Database.</span></span>