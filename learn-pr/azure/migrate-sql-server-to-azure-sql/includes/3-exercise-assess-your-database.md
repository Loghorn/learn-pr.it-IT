<span data-ttu-id="2be61-101">In questa unità si valuterà un database esistente mediante Data Migration Assistant e si rivedranno le funzionalità usate nell'istanza di SQL Server locale che non sono attualmente supportate dal database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="2be61-101">In this unit, you'll assess an existing database using the Data Migration Assistant and review any features used in the local SQL Server instance that aren't currently supported by Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="2be61-102">Configurazione</span><span class="sxs-lookup"><span data-stu-id="2be61-102">Setup</span></span>

1. <span data-ttu-id="2be61-103">[Installare **Data Migration Assistant**](https://www.microsoft.com/download/details.aspx?id=53595), se non lo si è già fatto.</span><span class="sxs-lookup"><span data-stu-id="2be61-103">[Install the **Data Migration Assistant**](https://www.microsoft.com/download/details.aspx?id=53595) if you haven't done so already.</span></span>

1. <span data-ttu-id="2be61-104">Sarà necessaria un'istanza di SQL Server in esecuzione. Assicurarsi pertanto di avere i dettagli sulla connessione a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="2be61-104">You'll need a SQL Server instance running, ensure you have connection details available.</span></span>

<!-- 1. [**** likely replace with an LOD VM *****] TODO: -->

1. <span data-ttu-id="2be61-105">Aprire un browser Internet e passare all'indirizzo https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span><span class="sxs-lookup"><span data-stu-id="2be61-105">Start an Internet browser and navigate to https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

1. <span data-ttu-id="2be61-106">In **Download OLTP** fare clic su **AdventureWorks2008R2.bak** e salvarlo nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="2be61-106">In **OLTP downloads**, click **AdventureWorks2008R2.bak** and save it to your local machine.</span></span>

1. <span data-ttu-id="2be61-107">In Management Studio ripristinare l'istanza predefinita di *AdventureWorks 2008R2*.</span><span class="sxs-lookup"><span data-stu-id="2be61-107">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-assessment"></a><span data-ttu-id="2be61-108">Creare una valutazione</span><span class="sxs-lookup"><span data-stu-id="2be61-108">Create an assessment</span></span>

1. <span data-ttu-id="2be61-109">Avviare **Microsoft Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="2be61-109">Start the **Microsoft Data Migration Assistant**.</span></span>

1. <span data-ttu-id="2be61-110">Nel riquadro di spostamento sinistro dell'app fare clic su __+__ per creare un nuovo progetto Data Migration Assistant.</span><span class="sxs-lookup"><span data-stu-id="2be61-110">In the app's left-hand navigation, click __+__ to create a new Data Migration Assistant project.</span></span>

1. <span data-ttu-id="2be61-111">Specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2be61-111">Specify the following options:</span></span>

    - <span data-ttu-id="2be61-112">**Tipo di progetto**: selezionare *Valutazione*</span><span class="sxs-lookup"><span data-stu-id="2be61-112">**Project type** - Select *Assessment*</span></span>
    - <span data-ttu-id="2be61-113">**Nome progetto**: immettere un nome per il progetto, ad esempio "Bicycle DB Assessment"</span><span class="sxs-lookup"><span data-stu-id="2be61-113">**Project name** - Enter a name for your project - for example, "Bicycle DB Assessment"</span></span>
    - <span data-ttu-id="2be61-114">**Tipo del server di origine**: selezionare *SQL Server*</span><span class="sxs-lookup"><span data-stu-id="2be61-114">**Source server type** - Select *SQL Server*</span></span>
    - <span data-ttu-id="2be61-115">**Tipo del server di destinazione**: selezionare *Database SQL di Azure*</span><span class="sxs-lookup"><span data-stu-id="2be61-115">**Target server type** - Select *Azure SQL Database*</span></span>

1. <span data-ttu-id="2be61-116">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2be61-116">Click **Create**.</span></span>
    <span data-ttu-id="2be61-117">![Screenshot che mostra la configurazione descritta in Data Migration Assistant per i dati SQL Server AdventureWorks.](../media-draft/3-create-assessment.png)</span><span class="sxs-lookup"><span data-stu-id="2be61-117">![Screenshot showing the described configuration in the Data Migration Assistant for your AdventureWorks SQL Server data.](../media-draft/3-create-assessment.png)</span></span>

1. <span data-ttu-id="2be61-118">Selezionare il tipo di report di valutazione. Selezionare entrambe le opzioni:</span><span class="sxs-lookup"><span data-stu-id="2be61-118">Select the assessment report type - check both:</span></span>
    - <span data-ttu-id="2be61-119">Verifica la compatibilità del database</span><span class="sxs-lookup"><span data-stu-id="2be61-119">Check database compatibility</span></span>
    - <span data-ttu-id="2be61-120">Verifica parità delle funzionalità</span><span class="sxs-lookup"><span data-stu-id="2be61-120">Check feature parity</span></span>

1. <span data-ttu-id="2be61-121">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2be61-121">Click **Next**.</span></span>

## <a name="add-databases-to-assess"></a><span data-ttu-id="2be61-122">Aggiungere i database da valutare</span><span class="sxs-lookup"><span data-stu-id="2be61-122">Add databases to assess</span></span>

1. <span data-ttu-id="2be61-123">Fare clic su **Aggiungi origini** per aprire il menu della connessione.</span><span class="sxs-lookup"><span data-stu-id="2be61-123">Click **Add Sources** to open the connection menu.</span></span>

1. <span data-ttu-id="2be61-124">Eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2be61-124">Do the following:</span></span>

    - <span data-ttu-id="2be61-125">Immettere il nome dell'istanza di SQL Server esistente</span><span class="sxs-lookup"><span data-stu-id="2be61-125">Enter your existing SQL server instance name</span></span>
    - <span data-ttu-id="2be61-126">Selezionare il tipo **Autenticazione**</span><span class="sxs-lookup"><span data-stu-id="2be61-126">Select the **Authentication** type</span></span>
    - <span data-ttu-id="2be61-127">Specificare le proprietà di connessione per il server</span><span class="sxs-lookup"><span data-stu-id="2be61-127">Specify the connection properties for your server</span></span>

1. <span data-ttu-id="2be61-128">Fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="2be61-128">CLick **Connect**.</span></span>

1. <span data-ttu-id="2be61-129">In **Aggiungi origini** selezionare i database da valutare.</span><span class="sxs-lookup"><span data-stu-id="2be61-129">In **Add sources**, select the databases to assess.</span></span> <span data-ttu-id="2be61-130">Per questo esercizio selezionare **AdventureWorks2008R2**.</span><span class="sxs-lookup"><span data-stu-id="2be61-130">For this exercise, select **AdventureWorks2008R2**.</span></span>

1. <span data-ttu-id="2be61-131">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="2be61-131">Click **Add**.</span></span>
    > [!NOTE]
    > <span data-ttu-id="2be61-132">Per aggiungere database da più istanze di SQL Server, usare il pulsante **Aggiungi origini**.</span><span class="sxs-lookup"><span data-stu-id="2be61-132">To add databases from multiple SQL Server instances, use the **Add Sources** button.</span></span> <span data-ttu-id="2be61-133">Per rimuovere più database, tenere premuto MAIUSC+CTRL per selezionare i database da rimuovere, quindi fare clic su **Rimuovi origini**.</span><span class="sxs-lookup"><span data-stu-id="2be61-133">To remove multiple databases, hold the SHIFT+CTRL keys to select the databases you want to remove, then click **Remove Sources**.</span></span>

1. <span data-ttu-id="2be61-134">Fare clic su **Avvia valutazione**.</span><span class="sxs-lookup"><span data-stu-id="2be61-134">Click **Start Assessment**.</span></span>

## <a name="view-results"></a><span data-ttu-id="2be61-135">Visualizzare i risultati</span><span class="sxs-lookup"><span data-stu-id="2be61-135">View results</span></span>

<span data-ttu-id="2be61-136">Se sono presenti più database, i risultati relativi a ogni database vengono visualizzati appena disponibili.</span><span class="sxs-lookup"><span data-stu-id="2be61-136">If there are multiple databases, the results for each database appears as soon as it is available.</span></span> <span data-ttu-id="2be61-137">Non è necessario attendere che venga completata la valutazione di tutti i database.</span><span class="sxs-lookup"><span data-stu-id="2be61-137">You don't need to wait for all database assessments to complete.</span></span>

1. <span data-ttu-id="2be61-138">Una volta completata la valutazione di **AdventureWorks**, fare clic su **Problemi di compatibilità** e su **Funzionalità consigliate** per visualizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="2be61-138">Once the assessment for **AdventureWorks** is complete, click **Compatibility issues** and **Feature recommendations** to view the results.</span></span>

    - <span data-ttu-id="2be61-139">La categoria di parità delle funzionalità di SQL Server elenca le funzionalità che potrebbero non essere completamente supportate e la procedura per risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="2be61-139">The SQL Server feature parity category lists features that might not be fully supported and steps to remedy these issues.</span></span> <span data-ttu-id="2be61-140">I problemi di parità delle funzionalità non interrompono una migrazione.</span><span class="sxs-lookup"><span data-stu-id="2be61-140">Feature parity issues will not stop a migration.</span></span>
    - <span data-ttu-id="2be61-141">La categoria dei problemi di compatibilità elenca le funzionalità che potrebbero bloccare una migrazione e la procedura per risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="2be61-141">The Compatibility issues category lists features that would block a migration and steps to remedy these issues.</span></span>

## <a name="summary"></a><span data-ttu-id="2be61-142">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2be61-142">Summary</span></span>

<span data-ttu-id="2be61-143">In questa unità è stato valutato un database di SQL Server installato in locale per verificare la presenza di eventuali funzionalità che non sarebbero disponibili dopo la migrazione del database al database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="2be61-143">In this unit, you assessed a locally installed SQL Server database to verify if any features would be unavailable when you migrate the database to Azure SQL Database.</span></span>