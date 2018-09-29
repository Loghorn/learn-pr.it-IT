<span data-ttu-id="8080b-101">Visual Studio Code consente di creare un'applicazione console usando il terminale integrato e alcuni brevi comandi.</span><span class="sxs-lookup"><span data-stu-id="8080b-101">Visual Studio Code enables you to create a console application by using the integrated terminal and a few short commands.</span></span>

<span data-ttu-id="8080b-102">In questa unità si creerà una semplice app console usando il terminale integrato, dall'estensione si recupererà la stringa di connessione di Azure Cosmos DB e dall'applicazione si configurerà la connessione ad Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8080b-102">In this unit, you will create a simple console app using the integrated terminal, retrieve your Azure Cosmos DB connection string from the extension, and then configure the connection from your application to Azure Cosmos DB.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="8080b-103">Creare un'app console</span><span class="sxs-lookup"><span data-stu-id="8080b-103">Create a console app</span></span>

1. <span data-ttu-id="8080b-104">In Visual Studio Code selezionare **File** > **Apri cartella**.</span><span class="sxs-lookup"><span data-stu-id="8080b-104">In Visual Studio Code, select **File** > **Open Folder**.</span></span>

1. <span data-ttu-id="8080b-105">Creare una nuova cartella denominata `learning-module` in un percorso a scelta e quindi fare clic su **Seleziona cartella**.</span><span class="sxs-lookup"><span data-stu-id="8080b-105">Create a new folder named `learning-module` in the location of your choice, and then click **Select Folder**.</span></span>

1. <span data-ttu-id="8080b-106">Assicurarsi che il salvataggio automatico dei file sia abilitato. Fare quindi clic sul menu File e selezionare **Salvataggio automatico** se l'opzione non è selezionata.</span><span class="sxs-lookup"><span data-stu-id="8080b-106">Ensure that file auto-save is enabled by clicking on the File menu and checking **Auto Save** if it is blank.</span></span> <span data-ttu-id="8080b-107">Verranno copiati vari blocchi di codice e ciò garantirà di operare sempre sulle modifiche più recenti dei file.</span><span class="sxs-lookup"><span data-stu-id="8080b-107">You will be copying in several blocks of code, and this will ensure you are always operating against the latest edits of your files.</span></span>

1. <span data-ttu-id="8080b-108">Aprire il terminale integrato da Visual Studio Code selezionando **Visualizza** > **Terminale** nel menu principale.</span><span class="sxs-lookup"><span data-stu-id="8080b-108">Open the integrated terminal from Visual Studio Code by selecting **View** > **Terminal** from the main menu.</span></span>

1. <span data-ttu-id="8080b-109">Copiare e incollare il comando seguente nella finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="8080b-109">In the terminal window, copy and paste the following command.</span></span>

    ```bash
    dotnet new console
    ```

    <span data-ttu-id="8080b-110">Questo comando crea un file **Program.cs** nella cartella con un semplice programma "Hello World" già scritto, oltre a un file di progetto C# denominato **learning-module.csproj**.</span><span class="sxs-lookup"><span data-stu-id="8080b-110">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

1. <span data-ttu-id="8080b-111">Nella finestra del terminale copiare e incollare il comando seguente per eseguire il programma "Hello World".</span><span class="sxs-lookup"><span data-stu-id="8080b-111">In the terminal window, copy and paste the following command to run the "Hello World" program.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="8080b-112">La finestra del terminale visualizza "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="8080b-112">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="8080b-113">come output.</span><span class="sxs-lookup"><span data-stu-id="8080b-113">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="8080b-114">Connettere l'app ad Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8080b-114">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="8080b-115">Al prompt del terminale, copiare e incollare il blocco dei comandi seguente per installare i pacchetti NuGet necessari.</span><span class="sxs-lookup"><span data-stu-id="8080b-115">At the terminal prompt, copy and paste the following command block to install the required NuGet packages.</span></span>

    ```bash
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet restore
    ```

1. <span data-ttu-id="8080b-116">Nella parte superiore del riquadro Esplora risorse fare clic su **Program.cs** per aprire il file.</span><span class="sxs-lookup"><span data-stu-id="8080b-116">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

1. <span data-ttu-id="8080b-117">Aggiungere le istruzioni using seguenti dopo `using System;`.</span><span class="sxs-lookup"><span data-stu-id="8080b-117">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    <span data-ttu-id="8080b-118">Se viene visualizzato un messaggio in cui si chiede di aggiungere gli asset mancanti necessari, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="8080b-118">If you get a message about adding required missing assets, click **Yes**.</span></span>

1. <span data-ttu-id="8080b-119">Creare un nuovo file denominato App.config nella cartella del modulo di apprendimento e aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="8080b-119">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. <span data-ttu-id="8080b-120">Copiare la stringa di connessione facendo clic sull'![icona di Azure](../media/2-setup/visual-studio-code-explorer-icon.png) a sinistra, espandere la sottoscrizione Concierge, fare clic con il pulsante destro del mouse sul nuovo account di Azure Cosmos DB e scegliere **Copy Connection String** (Copia stringa di connessione).</span><span class="sxs-lookup"><span data-stu-id="8080b-120">Copy your connection string by clicking the ![Azure icon](../media/2-setup/visual-studio-code-explorer-icon.png) Azure icon on the left, expanding your Concierge Subscription, right-clicking your new Azure Cosmos DB account, and then clicking **Copy Connection String**.</span></span>

1. <span data-ttu-id="8080b-121">Incollare la stringa di connessione alla fine del file App.config, quindi copiare la parte **AccountEndpoint** dalla stringa di connessione in **accountEndpoint** nel file App.config.</span><span class="sxs-lookup"><span data-stu-id="8080b-121">Paste the connection string into the end of the App.config file, and then copy the **AccountEndpoint** portion from the connection string into the **accountEndpoint** value in App.config.</span></span>

    <span data-ttu-id="8080b-122">AccountEndpoint dovrebbe avere un aspetto analogo al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8080b-122">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. <span data-ttu-id="8080b-123">A questo punto copiare il valore **AccountKey** dalla stringa di connessione nel valore **accountKey**, quindi eliminare la stringa di connessione originale che è stata copiata.</span><span class="sxs-lookup"><span data-stu-id="8080b-123">Now copy the **AccountKey** value from the connection string into the **accountKey** value, and then delete the original connection string you copied in.</span></span>

1. <span data-ttu-id="8080b-124">Al prompt del terminale copiare e incollare il comando seguente per eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="8080b-124">At the terminal prompt, copy and paste the following command to run the program.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="8080b-125">Il programma visualizzerà Hello World!</span><span class="sxs-lookup"><span data-stu-id="8080b-125">The program displays Hello World!</span></span> <span data-ttu-id="8080b-126">nel terminale.</span><span class="sxs-lookup"><span data-stu-id="8080b-126">in the terminal.</span></span>

## <a name="create-the-documentclient"></a><span data-ttu-id="8080b-127">Creare un'istanza di DocumentClient</span><span class="sxs-lookup"><span data-stu-id="8080b-127">Create the DocumentClient</span></span>

<span data-ttu-id="8080b-128">A questo punto è possibile creare un'istanza di DocumentClient, ovvero la rappresentazione lato client del servizio Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8080b-128">Now it's time to create an instance of the DocumentClient, which is the client-side representation of the Azure Cosmos DB service.</span></span> <span data-ttu-id="8080b-129">Questo client viene usato per configurare ed eseguire richieste nel servizio.</span><span class="sxs-lookup"><span data-stu-id="8080b-129">This client is used to configure and execute requests against the service.</span></span>

1. <span data-ttu-id="8080b-130">Aggiungere quanto segue all'inizio della classe Program in Program.cs.</span><span class="sxs-lookup"><span data-stu-id="8080b-130">In Program.cs, add the following to the beginning of the Program class.</span></span>

    ```csharp
    private DocumentClient client;
    ```

1. <span data-ttu-id="8080b-131">Aggiungere una nuova attività asincrona per creare un nuovo client e verificare se il database Users esiste aggiungendo il metodo seguente dopo il metodo `Main`.</span><span class="sxs-lookup"><span data-stu-id="8080b-131">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the `Main` method.</span></span>

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. <span data-ttu-id="8080b-132">Sempre nel terminale integrato copiare e incollare il comando seguente per eseguire il programma e assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="8080b-132">In the integrated terminal, again, copy and paste the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="8080b-133">Copiare e incollare il codice seguente nel metodo **Main**, sovrascrivendo la riga `Console.WriteLine("Hello World!");` corrente.</span><span class="sxs-lookup"><span data-stu-id="8080b-133">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

    ```csharp
    try
    {
        Program p = new Program();
        p.BasicOperations().Wait();
    }
    catch (DocumentClientException de)
    {
        Exception baseException = de.GetBaseException();
        Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
    }
    catch (Exception e)
    {
        Exception baseException = e.GetBaseException();
        Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
    }
    finally
    {
        Console.WriteLine("End of demo, press any key to exit.");
        Console.ReadKey();
    }
    ```

1. <span data-ttu-id="8080b-134">Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="8080b-134">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="8080b-135">La console visualizza l'output seguente.</span><span class="sxs-lookup"><span data-stu-id="8080b-135">The console displays the following output.</span></span>

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

<span data-ttu-id="8080b-136">In questa unità sono state poste le basi per creare l'applicazione di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8080b-136">In this unit, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="8080b-137">Dopo aver configurato l'ambiente di sviluppo in Visual Studio Code, è stato creato un semplice progetto HelloWorld, è stato connesso il progetto all'endpoint di Azure Cosmos DB ed è stata verificata l'esistenza di raccolta e database.</span><span class="sxs-lookup"><span data-stu-id="8080b-137">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>
