<span data-ttu-id="df9a3-101">Dopo aver valutato il database e corretto gli eventuali errori segnalati, si è pronti per eseguire la migrazione del database.</span><span class="sxs-lookup"><span data-stu-id="df9a3-101">Now that you have assessed your database and fixed any errors reported, you're ready to migrate your database.</span></span> <span data-ttu-id="df9a3-102">In questo esercizio si eseguirà la migrazione di un database SQL a un database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="df9a3-102">In this exercise, you will migrate your SQL database to Azure SQL Database.</span></span>

## <a name="setup"></a><span data-ttu-id="df9a3-103">Configurazione</span><span class="sxs-lookup"><span data-stu-id="df9a3-103">Setup</span></span>

<span data-ttu-id="df9a3-104">Se non è già stato fatto, scaricare i dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="df9a3-104">If you haven't already, download the sample data.</span></span>

1. <span data-ttu-id="df9a3-105">Aprire un browser Internet e passare all'indirizzo https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span><span class="sxs-lookup"><span data-stu-id="df9a3-105">Start an internet browser and navigate to https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

2. <span data-ttu-id="df9a3-106">In **Download OLTP** fare clic su **AdventureWorks2008R2.bak**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-106">In **OLTP downloads**, click **AdventureWorks2008R2.bak**.</span></span>

3. <span data-ttu-id="df9a3-107">In Management Studio ripristinare l'istanza predefinita di *AdventureWorks 2008R2*.</span><span class="sxs-lookup"><span data-stu-id="df9a3-107">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="df9a3-108">Creare un database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="df9a3-108">Create an Azure SQL Database</span></span>

1. <span data-ttu-id="df9a3-109">Accedere all'account del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="df9a3-109">Sign in to your Azure portal account.</span></span>

2. <span data-ttu-id="df9a3-110">Fare clic su **Crea una risorsa** nell'angolo superiore sinistro del portale.</span><span class="sxs-lookup"><span data-stu-id="df9a3-110">Click **Create a resource** in the upper left-hand corner of the portal.</span></span>

3. <span data-ttu-id="df9a3-111">Fare clic su **Database** > **Database SQL**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-111">Click **Databases** > **SQL Database**.</span></span>

4. <span data-ttu-id="df9a3-112">Compilare i seguenti campi nel modulo Database SQL:</span><span class="sxs-lookup"><span data-stu-id="df9a3-112">Enter the following fields on the SQL Database form:</span></span>

    |<span data-ttu-id="df9a3-113">Campo</span><span class="sxs-lookup"><span data-stu-id="df9a3-113">Field</span></span>|<span data-ttu-id="df9a3-114">Descrizione</span><span class="sxs-lookup"><span data-stu-id="df9a3-114">Description</span></span>|
    |-----|---|
    |<span data-ttu-id="df9a3-115">Nome database</span><span class="sxs-lookup"><span data-stu-id="df9a3-115">Database name</span></span>|<span data-ttu-id="df9a3-116">Immettere un nome per il database.</span><span class="sxs-lookup"><span data-stu-id="df9a3-116">Enter a name for your database.</span></span> <span data-ttu-id="df9a3-117">Può essere il nome del database esistente di cui si sta eseguendo la migrazione o un nuovo nome di propria scelta</span><span class="sxs-lookup"><span data-stu-id="df9a3-117">This can be the name of your existing database that you're migrating, or any new name of your choice</span></span>|
    |<span data-ttu-id="df9a3-118">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="df9a3-118">Resource group</span></span>|<span data-ttu-id="df9a3-119">Selezionare un gruppo di risorse o immettere un nome per il nuovo gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="df9a3-119">Select a resource group or enter a new resource group name</span></span>|
    |<span data-ttu-id="df9a3-120">Selezionare l'origine</span><span class="sxs-lookup"><span data-stu-id="df9a3-120">Select source</span></span>|<span data-ttu-id="df9a3-121">Selezionare *Database vuoto*</span><span class="sxs-lookup"><span data-stu-id="df9a3-121">Select *Blank database*</span></span>|

5. <span data-ttu-id="df9a3-122">In **Server** selezionare un server esistente oppure immettere un **nome server** univoco a livello globale, un **nome di accesso amministratore server**, una **password** (che è necessario confermare) e una **posizione**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-122">Under **Server**, either select an existing server or, enter a globally unique **Server name**, a **Server admin login**, a **Password** (which you should confirm), and a **Location**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df9a3-123">Il nome del server, il nome di accesso e la password verranno usati per connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="df9a3-123">You will use the server name, login name, and password to connect to your database.</span></span>

6. <span data-ttu-id="df9a3-124">Fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-124">Click **Select**.</span></span>

7. <span data-ttu-id="df9a3-125">In **Piano tariffario** è possibile selezionare un database con prestazioni superiori, ma per questa esercitazione selezionare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="df9a3-125">In **Pricing tier**, you can select a database with higher performance, but for this tutorial, select default.</span></span>

8. <span data-ttu-id="df9a3-126">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-126">Click **Create**.</span></span> <span data-ttu-id="df9a3-127">Attendere il completamento del provisioning del database prima di continuare l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="df9a3-127">Wait until the database has been provisioned before continuing the tutorial.</span></span>

    <span data-ttu-id="df9a3-128">Lo screenshot seguente mostra una potenziale configurazione per il nuovo database SQL.</span><span class="sxs-lookup"><span data-stu-id="df9a3-128">The following screenshot shows a potential configuration for your new SQL database.</span></span>

    ![Screenshot del pannello di un nuovo database SQL nel portale di Azure con una configurazione di esempio.](../media-draft/5-create-azure-sql-db.png)

## <a name="create-a-new-migration-project"></a><span data-ttu-id="df9a3-130">Creare un nuovo progetto di migrazione</span><span class="sxs-lookup"><span data-stu-id="df9a3-130">Create a new migration project</span></span>

1. <span data-ttu-id="df9a3-131">Avviare **Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-131">Start **Data Migration Assistant**.</span></span>

2. <span data-ttu-id="df9a3-132">Scegliere l'icona **Nuovo** e specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="df9a3-132">Click the **New** icon, and specify the following options:</span></span>
    - <span data-ttu-id="df9a3-133">**Tipo di progetto**: selezionare l'opzione *Migrazione*.</span><span class="sxs-lookup"><span data-stu-id="df9a3-133">**Project type** - Select  the *Migration* option.</span></span>
    - <span data-ttu-id="df9a3-134">**Nome progetto**: immettere un nome facile da ricordare per il progetto.</span><span class="sxs-lookup"><span data-stu-id="df9a3-134">**Project name** - Enter a memorable name for your project.</span></span>
    - <span data-ttu-id="df9a3-135">**Tipo del server di origine**: selezionare *SQL Server*.</span><span class="sxs-lookup"><span data-stu-id="df9a3-135">**Source server type** - Select *SQL Server*.</span></span>
    - <span data-ttu-id="df9a3-136">**Tipo del server di destinazione**: selezionare *Database SQL di Azure*.</span><span class="sxs-lookup"><span data-stu-id="df9a3-136">**Target server type** - Select *Azure SQL Database*.</span></span>
    - <span data-ttu-id="df9a3-137">**Ambito migrazione**: selezionare *Schema e dati*.</span><span class="sxs-lookup"><span data-stu-id="df9a3-137">**Migration scope** - Select *Schema and data*.</span></span>

3. <span data-ttu-id="df9a3-138">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-138">Click **Create**.</span></span>

## <a name="specify-the-source-server-and-database"></a><span data-ttu-id="df9a3-139">Specificare il server e il database di origine</span><span class="sxs-lookup"><span data-stu-id="df9a3-139">Specify the source server and database</span></span>

1. <span data-ttu-id="df9a3-140">In **Connect to source server** (Connessione al server di origine), nel campo **Nome server**, immettere il nome dell'istanza di SQL Server di origine.</span><span class="sxs-lookup"><span data-stu-id="df9a3-140">Under **Connect to source server**, in the **Server name** field, enter the name of the source SQL Server instance.</span></span>

2. <span data-ttu-id="df9a3-141">Selezionare il **tipo di autenticazione** supportato dall'istanza di SQL Server di origine.</span><span class="sxs-lookup"><span data-stu-id="df9a3-141">Select the **Authentication type** supported by the source SQL Server instance.</span></span>
    > [!TIP]
    > <span data-ttu-id="df9a3-142">È consigliabile selezionare la casella di controllo **Crittografa connessione** in Proprietà connessione per crittografare la connessione.</span><span class="sxs-lookup"><span data-stu-id="df9a3-142">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

3. <span data-ttu-id="df9a3-143">Selezionare **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-143">Select **Connect**.</span></span>

4. <span data-ttu-id="df9a3-144">Selezionare un singolo database di origine per eseguire la migrazione al database SQL di Azure e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-144">Select a single source database to migrate to Azure SQL Database and click **Next**.</span></span>

## <a name="specify-the-target-server-and-database"></a><span data-ttu-id="df9a3-145">Specificare il server e il database di destinazione</span><span class="sxs-lookup"><span data-stu-id="df9a3-145">Specify the target server and database</span></span>

1. <span data-ttu-id="df9a3-146">In **Connessione al server di destinazione**, nel campo **Nome server**, immettere il nome dell'istanza del database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="df9a3-146">Under **Connect to target server**, in the **Server name** field, enter the name of the Azure SQL Database instance.</span></span> <span data-ttu-id="df9a3-147">Aggiungere **database.windows.net** all'istanza del database.</span><span class="sxs-lookup"><span data-stu-id="df9a3-147">Add **database.windows.net** to the database instance.</span></span>

2. <span data-ttu-id="df9a3-148">Selezionare il tipo di autenticazione supportato dall'istanza del database SQL di Azure di destinazione.</span><span class="sxs-lookup"><span data-stu-id="df9a3-148">Select the Authentication type supported by the target Azure SQL Database instance.</span></span> <span data-ttu-id="df9a3-149">Per questa esercitazione, selezionare **Autenticazione di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-149">For this tutorial, select **SQL Server Authentication**.</span></span>
    > [!TIP]
    > <span data-ttu-id="df9a3-150">È consigliabile selezionare la casella di controllo **Crittografa connessione** in Proprietà connessione per crittografare la connessione.</span><span class="sxs-lookup"><span data-stu-id="df9a3-150">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

3. <span data-ttu-id="df9a3-151">Immettere il **nome utente** e la **password** definiti al momento della creazione del database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="df9a3-151">Enter the **Username** and **Password** that you defined when you created the Azure SQL Database.</span></span>

4. <span data-ttu-id="df9a3-152">Fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-152">Click **Connect**.</span></span>

5. <span data-ttu-id="df9a3-153">Selezionare un singolo database di destinazione in cui eseguire la migrazione.</span><span class="sxs-lookup"><span data-stu-id="df9a3-153">Select a single target database to which to migrate.</span></span>
    > <span data-ttu-id="df9a3-154">[NOTA] Se si prevede di eseguire la migrazione di utenti di Windows, nella casella di testo Nome di dominio dell'utente esterno di destinazione assicurarsi che il nome di dominio dell'utente esterno di destinazione sia specificato correttamente.</span><span class="sxs-lookup"><span data-stu-id="df9a3-154">[NOTE] If you intend to migrate Windows users, in the Target external user domain name text box, make sure that the target external user domain name is specified correctly.</span></span>

6. <span data-ttu-id="df9a3-155">Fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-155">Click **Next**.</span></span>

## <a name="select-schema-objects"></a><span data-ttu-id="df9a3-156">Selezionare gli oggetti dello schema</span><span class="sxs-lookup"><span data-stu-id="df9a3-156">Select schema objects</span></span>

1. <span data-ttu-id="df9a3-157">Selezionare gli oggetti dello schema dal database di origine di cui si vuole eseguire la migrazione al database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="df9a3-157">Select the schema objects from the source database that you want to migrate to Azure SQL Database.</span></span>

    > [!NOTE]
    > <span data-ttu-id="df9a3-158">Vengono presentati alcuni degli oggetti che non possono essere convertiti così come sono, offrendo l'opportunità di eseguire la correzione automatica.</span><span class="sxs-lookup"><span data-stu-id="df9a3-158">Some of the objects that cannot be converted as-is are presented with automatic fix opportunities.</span></span> <span data-ttu-id="df9a3-159">Facendo clic su questi oggetti nel riquadro sinistro, vengono visualizzate le correzioni suggerite nel riquadro destro.</span><span class="sxs-lookup"><span data-stu-id="df9a3-159">Clicking these objects on the left pane displays the suggested fixes on the right pane.</span></span> <span data-ttu-id="df9a3-160">Esaminare le correzioni e scegliere se applicare o ignorare tutte le modifiche, per ogni singolo oggetto.</span><span class="sxs-lookup"><span data-stu-id="df9a3-160">Review the fixes and choose to either apply or ignore all changes, object by object.</span></span> <span data-ttu-id="df9a3-161">Si noti che il fatto di applicare o ignorare tutte le modifiche per un oggetto non influisce sulle modifiche degli altri oggetti di database.</span><span class="sxs-lookup"><span data-stu-id="df9a3-161">Note that applying or ignoring all changes for one object does not affect changes to other database objects.</span></span> <span data-ttu-id="df9a3-162">Le istruzioni che non possono essere convertite o corrette automaticamente vengono riprodotte nel database di destinazione e impostate come commenti.</span><span class="sxs-lookup"><span data-stu-id="df9a3-162">Statements that cannot be converted or automatically fixed are reproduced to the target database and commented.</span></span>

    ![Screenshot di Data Migration Assistant che mostra l'elenco dei problemi di migrazione relativi ai dati SQL Server AdventureWorks con il problema "Stored procedure: dbo.uspSearchCandidateResumes" selezionato e i dettagli sul problema visualizzati.](../media-draft/5-suggested-fix.png)

2. <span data-ttu-id="df9a3-164">Fare clic su **Genera script SQL**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-164">Click **Generate SQL script**.</span></span>

## <a name="deploy-schema"></a><span data-ttu-id="df9a3-165">Distribuire lo schema</span><span class="sxs-lookup"><span data-stu-id="df9a3-165">Deploy schema</span></span>

1. <span data-ttu-id="df9a3-166">Fare clic su **Distribuisci schema**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-166">Click **Deploy schema**.</span></span>

2. <span data-ttu-id="df9a3-167">Esaminare i risultati della distribuzione dello schema.</span><span class="sxs-lookup"><span data-stu-id="df9a3-167">Review the results of the schema deployment.</span></span>

## <a name="migrate-data"></a><span data-ttu-id="df9a3-168">Eseguire la migrazione dei dati</span><span class="sxs-lookup"><span data-stu-id="df9a3-168">Migrate data</span></span>

1. <span data-ttu-id="df9a3-169">Fare clic su **Esegui la migrazione dei dati** per avviare il processo di migrazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="df9a3-169">Click **Migrate data** to start the data migration process.</span></span>

2. <span data-ttu-id="df9a3-170">Selezionare le tabelle con i dati di cui eseguire la migrazione.</span><span class="sxs-lookup"><span data-stu-id="df9a3-170">Select the tables with the data you want to migrate.</span></span>

3. <span data-ttu-id="df9a3-171">Fare clic su **Esegui la migrazione dei dati**.</span><span class="sxs-lookup"><span data-stu-id="df9a3-171">Click **Start data migration**.</span></span>

<span data-ttu-id="df9a3-172">La schermata finale mostra lo stato complessivo.</span><span class="sxs-lookup"><span data-stu-id="df9a3-172">The final screen shows the overall status.</span></span>

## <a name="summary"></a><span data-ttu-id="df9a3-173">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="df9a3-173">Summary</span></span>

<span data-ttu-id="df9a3-174">In questo esercizio è stato creato un database SQL di Azure vuoto ed è stata eseguita la migrazione di un database di SQL Server locale in questo nuovo database.</span><span class="sxs-lookup"><span data-stu-id="df9a3-174">In this exercise, you created an empty Azure SQL Database and migrated a local SQL Server database to this new database.</span></span>