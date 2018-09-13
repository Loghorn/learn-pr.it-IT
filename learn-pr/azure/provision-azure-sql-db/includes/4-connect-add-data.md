<span data-ttu-id="dfe16-101">Prima di connettere il database all'app, è necessario controllare che sia possibile connettersi, aggiungere una tabella di base e lavorare con dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="dfe16-101">Before you connect the database to your app, you want to check that you can connect to it, add a basic table, and work with sample data.</span></span>

<span data-ttu-id="dfe16-102">L'infrastruttura, gli aggiornamenti software e le patch per il database SQL di Azure sono gestiti da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="dfe16-102">We maintain the infrastructure, software updates, and patches for your Azure SQL database.</span></span> <span data-ttu-id="dfe16-103">A parte questo, è possibile gestire il database SQL di Azure come qualsiasi altra installazione di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dfe16-103">Beyond that, you can treat your Azure SQL database like you would any other SQL Server installation.</span></span> <span data-ttu-id="dfe16-104">È ad esempio possibile usare Visual Studio, SQL Server Management Studio o altri strumenti per gestire il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-104">For example, you can use Visual Studio, SQL Server Management Studio, or other tools to manage your Azure SQL database.</span></span>

<span data-ttu-id="dfe16-105">È possibile scegliere come accedere al database e connetterlo all'app.</span><span class="sxs-lookup"><span data-stu-id="dfe16-105">How you access your database and connect it to your app is up to you.</span></span> <span data-ttu-id="dfe16-106">Tuttavia, per acquisire familiarità con il database, verrà descritto come connettersi direttamente al database dal portale, creare una tabella ed eseguire alcune operazioni CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="dfe16-106">But to get some experience working with your database, here you'll connect to it directly from the portal, create a table, and run a few basic CRUD operations.</span></span> <span data-ttu-id="dfe16-107">Si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="dfe16-107">You'll learn:</span></span>

- <span data-ttu-id="dfe16-108">Che cos'è Cloud Shell e come accedervi dal portale.</span><span class="sxs-lookup"><span data-stu-id="dfe16-108">What Cloud Shell is and how to access it from the portal.</span></span>
- <span data-ttu-id="dfe16-109">Come accedere alle informazioni sul database dall'interfaccia della riga di comando di Azure, incluse le stringhe di connessione.</span><span class="sxs-lookup"><span data-stu-id="dfe16-109">How to access information about your database from the Azure CLI, including connection strings.</span></span>
- <span data-ttu-id="dfe16-110">Come connettersi al database tramite `sqlcmd`.</span><span class="sxs-lookup"><span data-stu-id="dfe16-110">How to connect to your database using `sqlcmd`.</span></span>
- <span data-ttu-id="dfe16-111">Come inizializzare il database con una tabella di base e alcuni dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="dfe16-111">How to initialize your database with a basic table and some sample data.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="dfe16-112">Che cos'è Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="dfe16-112">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="dfe16-113">Azure Cloud Shell è un'esperienza shell basata su browser per gestire e sviluppare risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-113">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span> <span data-ttu-id="dfe16-114">Cloud Shell può essere paragonato a una console interattiva che viene eseguita nel cloud.</span><span class="sxs-lookup"><span data-stu-id="dfe16-114">Think of Cloud Shell as an interactive console that runs in the cloud.</span></span>

<span data-ttu-id="dfe16-115">In background, Cloud Shell viene eseguito su Linux.</span><span class="sxs-lookup"><span data-stu-id="dfe16-115">Behind the scenes, Cloud Shell runs on Linux.</span></span> <span data-ttu-id="dfe16-116">Tuttavia, a seconda del fatto che si preferisca un ambiente Linux o Windows, è possibile scegliere tra due esperienze: Bash e PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dfe16-116">But depending on whether you prefer a Linux or Windows environment, you have two experiences to choose from: Bash and PowerShell.</span></span>

<span data-ttu-id="dfe16-117">Cloud Shell è accessibile da qualsiasi posizione.</span><span class="sxs-lookup"><span data-stu-id="dfe16-117">Cloud Shell is accessible from anywhere.</span></span> <span data-ttu-id="dfe16-118">Oltre che dal portale, è possibile accedere a Cloud Shell anche da [shell.azure.com](https://shell.azure.com/), dall'app per dispositivi mobili di Azure o da Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dfe16-118">Besides the portal, you can also access Cloud Shell from [shell.azure.com](https://shell.azure.com/), the Azure mobile app, or from Visual Studio Code.</span></span>

<span data-ttu-id="dfe16-119">Cloud Shell include strumenti ed editor di testo molto diffusi.</span><span class="sxs-lookup"><span data-stu-id="dfe16-119">Cloud Shell includes popular tools and text editors.</span></span> <span data-ttu-id="dfe16-120">Di seguito è riportata una breve descrizione di `az`, `jq` e `sqlcmd`, tre strumenti che verranno usati per l'attività corrente.</span><span class="sxs-lookup"><span data-stu-id="dfe16-120">Here's a brief look at `az`, `jq`, and `sqlcmd`, three tools you'll use for our current task.</span></span>

- <span data-ttu-id="dfe16-121">`az` è anche noto come interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-121">`az` is also known as the Azure CLI.</span></span> <span data-ttu-id="dfe16-122">Si tratta dell'interfaccia della riga di comando per l'uso delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-122">It's the command-line interface for working with Azure resources.</span></span> <span data-ttu-id="dfe16-123">Verrà usata per ottenere informazioni sul database, compresa la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="dfe16-123">You'll use this to get information about your database, including the connection string.</span></span>
- <span data-ttu-id="dfe16-124">`jq` è un parser JSON della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="dfe16-124">`jq` is a command-line JSON parser.</span></span> <span data-ttu-id="dfe16-125">È possibile inviare tramite pipe l'output dei comandi `az` a questo strumento per estrarre i campi importanti dall'output JSON.</span><span class="sxs-lookup"><span data-stu-id="dfe16-125">You'll pipe output from `az` commands to this tool to extract important fields from JSON output.</span></span>
- <span data-ttu-id="dfe16-126">`sqlcmd` consente di eseguire istruzioni in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dfe16-126">`sqlcmd` enables you to execute statements on SQL Server.</span></span> <span data-ttu-id="dfe16-127">Si userà `sqlcmd` per creare una sessione interattiva con il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-127">You'll use `sqlcmd` to create an interactive session with your Azure SQL database.</span></span>

## <a name="get-information-about-your-azure-sql-database"></a><span data-ttu-id="dfe16-128">Ottenere informazioni sul database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="dfe16-128">Get information about your Azure SQL database</span></span>

<span data-ttu-id="dfe16-129">Prima di connettersi al database, è consigliabile verificare che esista e che sia online.</span><span class="sxs-lookup"><span data-stu-id="dfe16-129">Before you connect to your database, it's a good idea to verify it exists and is online.</span></span>

<span data-ttu-id="dfe16-130">In questo caso, si aprirà Cloud Shell e si userà l'utilità `az` per elencare i database e visualizzare alcune informazioni sul database **Logistics**, tra cui la dimensione massima e lo stato.</span><span class="sxs-lookup"><span data-stu-id="dfe16-130">Here, you bring up Cloud Shell and use the `az` utility to list your databases and show some information about the **Logistics** database, including its maximum size and status.</span></span>

1. <span data-ttu-id="dfe16-131">Nel portale fare clic su **Cloud Shell** nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="dfe16-131">From the portal, at the top, click **Cloud Shell**.</span></span>

    ![Apertura di Cloud Shell](../media-draft/open-cloud-shell.png)

1. <span data-ttu-id="dfe16-133">I comandi `az` da eseguire richiedono il nome del gruppo di risorse e il nome del server logico SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-133">The `az` commands you'll run require the name of your resource group and the name of your Azure SQL logical server.</span></span> <span data-ttu-id="dfe16-134">Per evitare di digitarli, eseguire questo comando `azure configure` per specificare i valori come valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="dfe16-134">To save typing, run this `azure configure` command to specify them as default values.</span></span>
    <span data-ttu-id="dfe16-135">Sostituire `contoso-logistics` con il nome del server logico SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-135">Replace `contoso-logistics` with the name of your Azure SQL logical server.</span></span>

    ```azurecli
    az configure --defaults group=logistics-db-rg sql-server=contoso-logistics
    ```

1. <span data-ttu-id="dfe16-136">Eseguire `az sql db list` per elencare tutti i database nel server logico SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-136">Run `az sql db list` to list all databases on your Azure SQL logical server.</span></span>

    ```azurecli
    az sql db list
    ```
    <span data-ttu-id="dfe16-137">Si noterà un blocco di codice JSON di grandi dimensioni come output.</span><span class="sxs-lookup"><span data-stu-id="dfe16-137">You see a large block of JSON as output.</span></span> 

1. <span data-ttu-id="dfe16-138">Poiché si è interessati solo ai nomi dei database, eseguire il comando una seconda volta.</span><span class="sxs-lookup"><span data-stu-id="dfe16-138">Since we want just the database names, run the command a second time.</span></span> <span data-ttu-id="dfe16-139">Questa volta, inviare tramite pipe l'output a `jq` per visualizzare solo i campi del nome.</span><span class="sxs-lookup"><span data-stu-id="dfe16-139">This time, pipe the output to `jq` to print out only the name fields.</span></span>
    ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    <span data-ttu-id="dfe16-140">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="dfe16-140">You see this.</span></span>
    ```console
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```
    <span data-ttu-id="dfe16-141">**Logistics** è il database.</span><span class="sxs-lookup"><span data-stu-id="dfe16-141">**Logistics** is your database.</span></span> <span data-ttu-id="dfe16-142">Come in SQL Server, **master** include i metadati del server, come gli account di accesso e le impostazioni di configurazione di sistema.</span><span class="sxs-lookup"><span data-stu-id="dfe16-142">Like SQL Server, **master** includes server metadata, such as logon accounts and system configuration settings.</span></span>

1. <span data-ttu-id="dfe16-143">Eseguire questo comando `az sql db show` per ottenere informazioni dettagliate sul database **Logistics**.</span><span class="sxs-lookup"><span data-stu-id="dfe16-143">Run this `az sql db show` command to get details about the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics
    ```
    <span data-ttu-id="dfe16-144">Come nel caso precedente, si noterà un blocco di codice JSON di grandi dimensioni come output.</span><span class="sxs-lookup"><span data-stu-id="dfe16-144">As before, you see a large block of JSON as output.</span></span>

1. <span data-ttu-id="dfe16-145">Eseguire il comando una seconda volta.</span><span class="sxs-lookup"><span data-stu-id="dfe16-145">Run the command a second time.</span></span> <span data-ttu-id="dfe16-146">Questa volta inviare tramite pipe l'output a `jq` per limitare l'output solo al nome, alla dimensione massima e allo stato del database **Logistics**.</span><span class="sxs-lookup"><span data-stu-id="dfe16-146">This time, pipe the output to `jq` to limit output to only the name, maximum size, and status of the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```
    <span data-ttu-id="dfe16-147">Si noterà che il database è online e può contenere circa 2 GB di dati.</span><span class="sxs-lookup"><span data-stu-id="dfe16-147">You see that the database is online and can hold around 2 GB of data.</span></span>
    ```console
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a><span data-ttu-id="dfe16-148">Connettersi al database</span><span class="sxs-lookup"><span data-stu-id="dfe16-148">Connect to your database</span></span>

<span data-ttu-id="dfe16-149">Dopo aver ottenuto queste informazioni sul database, è possibile connettersi tramite `sqlcmd`, creare una tabella con informazioni sugli autisti ed eseguire alcune operazioni CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="dfe16-149">Now that you understand a bit about your database, let's connect to it using `sqlcmd`, create a table that holds information about transportation drivers, and perform a few basic CRUD operations.</span></span>

<span data-ttu-id="dfe16-150">CRUD è l'acronimo di **create**, **read**, **update** e **delete**, ovvero le operazioni di creazione, lettura, aggiornamento ed eliminazione.</span><span class="sxs-lookup"><span data-stu-id="dfe16-150">Remember that CRUD stands for **create**, **read**, **update**, and **delete**.</span></span> <span data-ttu-id="dfe16-151">Si tratta delle operazioni eseguite sui dati di una tabella e sono le quattro operazioni di base necessarie per l'app.</span><span class="sxs-lookup"><span data-stu-id="dfe16-151">These terms refer to operations you perform on table data and are the four basic operations you need for your app.</span></span> <span data-ttu-id="dfe16-152">In questa fase, è consigliabile verificare che sia possibile eseguire ognuna di esse.</span><span class="sxs-lookup"><span data-stu-id="dfe16-152">Now's a good time to verify you can perform each of them.</span></span>

1. <span data-ttu-id="dfe16-153">Eseguire questo comando `az sql db show-connection-string` per ottenere la stringa di connessione per il database **Logistics** in un formato utilizzabile da `sqlcmd`.</span><span class="sxs-lookup"><span data-stu-id="dfe16-153">Run this `az sql db show-connection-string` command to get the connection string to the **Logistics** database in a format that `sqlcmd` can use.</span></span>

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```
    <span data-ttu-id="dfe16-154">L'output è simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="dfe16-154">Your output resembles this.</span></span>
    ```console
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. <span data-ttu-id="dfe16-155">Eseguire l'istruzione `sqlcmd` ottenuta dal passaggio precedente per creare una sessione interattiva.</span><span class="sxs-lookup"><span data-stu-id="dfe16-155">Run the `sqlcmd` statement from the previous step to create an interactive session.</span></span>
    <span data-ttu-id="dfe16-156">Rimuovere le virgolette e sostituire `<username>` e `<password>` con il nome utente e la password specificati durante la creazione del database.</span><span class="sxs-lookup"><span data-stu-id="dfe16-156">Remove the surrounding quotes and replace `<username>` and `<password>` with the username and password you specified when you created your database.</span></span> <span data-ttu-id="dfe16-157">Ecco un esempio.</span><span class="sxs-lookup"><span data-stu-id="dfe16-157">Here's an example.</span></span>

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > <span data-ttu-id="dfe16-158">Inserire la password tra virgolette in modo che "&" e altri caratteri speciali non vengano interpretati come istruzioni di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="dfe16-158">Place your password in quotes so that "&" and other special characters aren't interpreted as processing instructions.</span></span>

1. <span data-ttu-id="dfe16-159">Dalla sessione `sqlcmd` creare una tabella denominata `Drivers`.</span><span class="sxs-lookup"><span data-stu-id="dfe16-159">From your `sqlcmd` session, create a table named `Drivers`.</span></span>

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255) );
    GO
    ```

    <span data-ttu-id="dfe16-160">La tabella contiene quattro colonne: un identificatore univoco, il nome e il cognome dell'autista e la città di origine dell'autista.</span><span class="sxs-lookup"><span data-stu-id="dfe16-160">The table contains four columns: a unique identifier, the driver's last and first name, and the driver's city of origin.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dfe16-161">Il linguaggio visualizzato qui è Transact-SQL, o T-SQL.</span><span class="sxs-lookup"><span data-stu-id="dfe16-161">The language you see here is Transact-SQL, or T-SQL.</span></span>

1. <span data-ttu-id="dfe16-162">Verificare che la tabella `Drivers` esista.</span><span class="sxs-lookup"><span data-stu-id="dfe16-162">Verify that the `Drivers` table exists.</span></span>

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    <span data-ttu-id="dfe16-163">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="dfe16-163">You see this.</span></span>

    ```console
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers
    
    (1 rows affected)
    ```

1. <span data-ttu-id="dfe16-164">Eseguire questa istruzione T-SQL `INSERT` per aggiungere una riga di esempio per la tabella.</span><span class="sxs-lookup"><span data-stu-id="dfe16-164">Run this `INSERT` T-SQL statement to add a sample row to the table.</span></span> <span data-ttu-id="dfe16-165">Questa è l'operazione **create**.</span><span class="sxs-lookup"><span data-stu-id="dfe16-165">This is the **create** operation.</span></span>

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Orton', 'Erick', 'Springfield');
    GO
    ```

    <span data-ttu-id="dfe16-166">Questo indica che l'operazione è riuscita.</span><span class="sxs-lookup"><span data-stu-id="dfe16-166">You see this to indicate the operation succeeded.</span></span>

    ```console
    (1 rows affected)
    ```

1. <span data-ttu-id="dfe16-167">Eseguire questo elenco di istruzioni T-SQL `SELECT` nella colonna `DriverID` di tutte le righe della tabella.</span><span class="sxs-lookup"><span data-stu-id="dfe16-167">Run this `SELECT` T-SQL statement list in the `DriverID` column from all rows in the table.</span></span> <span data-ttu-id="dfe16-168">Questa è l'operazione **read**.</span><span class="sxs-lookup"><span data-stu-id="dfe16-168">This is the **read** operation.</span></span>

    ```sql
    SELECT DriverID FROM Drivers;
    GO
    ```

    <span data-ttu-id="dfe16-169">Viene visualizzato un solo risultato, il valore `DriverID` per la riga che è stata creata nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="dfe16-169">You see one result, the `DriverID` for the row you created in the previous step.</span></span>

    ```console
    DriverID
    -----------
            123
    
    (1 rows affected)
    ```

1. <span data-ttu-id="dfe16-170">Eseguire questa istruzione T-SQL `UPDATE` per modificare la città di origine da "Springfield" in "Springfield, OR" per l'autista con un valore `DriverID` pari a 123.</span><span class="sxs-lookup"><span data-stu-id="dfe16-170">Run this `UPDATE` T-SQL statement to change the city of origin from "Springfield" to "Springfield, OR" for the driver with a `DriverID` of 123.</span></span> <span data-ttu-id="dfe16-171">Questa è l'operazione **update**.</span><span class="sxs-lookup"><span data-stu-id="dfe16-171">This is the **update** operation.</span></span>

    ```sql
    UPDATE Drivers SET OriginCity='Springfield, OR' WHERE DriverID=123;
    GO
    ```

    <span data-ttu-id="dfe16-172">Verrà visualizzato quanto segue.</span><span class="sxs-lookup"><span data-stu-id="dfe16-172">You see this.</span></span>

    ```console
    (1 rows affected)
    ```

1. <span data-ttu-id="dfe16-173">Eseguire questa istruzione T-SQL `DELETE` per eliminare il record.</span><span class="sxs-lookup"><span data-stu-id="dfe16-173">Run this `DELETE` T-SQL statement to delete the record.</span></span> <span data-ttu-id="dfe16-174">Questa è l'operazione **delete**.</span><span class="sxs-lookup"><span data-stu-id="dfe16-174">This is the **delete** operation.</span></span>

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```
    
    ```console
    (1 rows affected)
    ```

1. <span data-ttu-id="dfe16-175">Eseguire questa istruzione T-SQL `SELECT` per verificare che la tabella `Drivers` sia vuota.</span><span class="sxs-lookup"><span data-stu-id="dfe16-175">Run this `SELECT` T-SQL statement to verify the `Drivers` table is empty.</span></span>

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    <span data-ttu-id="dfe16-176">È possibile notare che la tabella non contiene righe.</span><span class="sxs-lookup"><span data-stu-id="dfe16-176">You see that the table contains no rows.</span></span>

    ```console
    -----------
              0
    
    (1 rows affected)
    ```

## <a name="summary"></a><span data-ttu-id="dfe16-177">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="dfe16-177">Summary</span></span>

<span data-ttu-id="dfe16-178">Dopo aver acquisito familiarità con l'uso del database SQL di Azure da Cloud Shell, è possibile ottenere la stringa di connessione per il proprio strumento di gestione SQL preferito, che si tratti di SQL Server Management Studio, Visual Studio o un altro strumento.</span><span class="sxs-lookup"><span data-stu-id="dfe16-178">Now that you have the hang of working with Azure SQL Database from Cloud Shell, you can get the connection string for your favorite SQL management tool &ndash; whether that's from SQL Server Management Studio, Visual Studio, or something else.</span></span>

<span data-ttu-id="dfe16-179">Cloud Shell rende molto semplice accedere e usare le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-179">Cloud Shell makes it easy to access and work with your Azure resources.</span></span> <span data-ttu-id="dfe16-180">Poiché Cloud Shell è basato su browser, è possibile accedervi da Windows, macOS o Linux: essenzialmente da qualsiasi sistema dotato di un Web browser.</span><span class="sxs-lookup"><span data-stu-id="dfe16-180">Because Cloud Shell is browser-based, you can access it from Windows, macOS, or Linux &ndash; essentially any system with a web browser.</span></span>

<span data-ttu-id="dfe16-181">È stata acquisita esperienza pratica su come eseguire i comandi dell'interfaccia della riga di comando di Azure per ottenere informazioni sul database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe16-181">You gained some hands-on experience running Azure CLI commands to get information about your Azure SQL database.</span></span> <span data-ttu-id="dfe16-182">Sono inoltre state applicate le competenze relative a T-SQL.</span><span class="sxs-lookup"><span data-stu-id="dfe16-182">As a bonus, you practiced your T-SQL skills.</span></span>

<span data-ttu-id="dfe16-183">Per concludere, vediamo come eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="dfe16-183">Finally, let's wrap up and see how to tear down your database.</span></span>