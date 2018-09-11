<span data-ttu-id="388f2-101">In questo modulo verrà creata una semplice un'app console tramite il terminale integrato, verranno installati i pacchetti NuGet e si userà l'estensione Azure Cosmos DB per visualizzare i database e le raccolte creati nel modulo precedente.</span><span class="sxs-lookup"><span data-stu-id="388f2-101">In this module you will create a simple console app using the integrated terminal, install NuGet packages, and use the Azure Cosmos DB extension to see databases and collections created in the previous module.</span></span> <span data-ttu-id="388f2-102">Si recupererà la stringa di connessione di Azure Cosmos DB dall'estensione e quindi si inizierà a configurare la connessione ad Azure Cosmos DB per creare il database utente.</span><span class="sxs-lookup"><span data-stu-id="388f2-102">You'll retrieve your Azure Cosmos DB connection string from the extension, and then start configuring the connection to Azure Cosmos DB to create your User database.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="388f2-103">Creare un'app console</span><span class="sxs-lookup"><span data-stu-id="388f2-103">Create a console app</span></span>

1. <span data-ttu-id="388f2-104">Creare una cartella di lavoro.</span><span class="sxs-lookup"><span data-stu-id="388f2-104">Create a folder where you will be working.</span></span>

1. <span data-ttu-id="388f2-105">Aprire un prompt dei comandi e passare a tale cartella.</span><span class="sxs-lookup"><span data-stu-id="388f2-105">Open a command prompt and navigate into the folder.</span></span>

1. <span data-ttu-id="388f2-106">Creare una nuova applicazione console .NET Core</span><span class="sxs-lookup"><span data-stu-id="388f2-106">Create a new .NET Core console application</span></span>

```bash
dotnet new console 
```

1. <span data-ttu-id="388f2-107">Aprire Visual Studio Code e quindi selezionare **File** > **Apri cartella**.</span><span class="sxs-lookup"><span data-stu-id="388f2-107">Open Visual Studio Code, and then select **File** > **Open Folder**.</span></span>

1. <span data-ttu-id="388f2-108">Creare una nuova cartella denominata in cui inserire il nuovo progetto C# e quindi fare clic su **Seleziona cartella**.</span><span class="sxs-lookup"><span data-stu-id="388f2-108">Create a new folder where you want your new C# project to be, and then click **Select Folder**.</span></span>

1. <span data-ttu-id="388f2-109">Assicurarsi che il salvataggio automatico dei file sia abilitato. Fare clic sul menu File e selezionare l'opzione per il salvataggio automatico se è deselezionata.</span><span class="sxs-lookup"><span data-stu-id="388f2-109">Ensure that file auto save is enabled by clicking on the File menu and checking Auto Save if it is blank.</span></span>

1. <span data-ttu-id="388f2-110">Aprire il terminale integrato da Visual Studio Code selezionando **Visualizza** > **Terminale integrato** nel menu principale.</span><span class="sxs-lookup"><span data-stu-id="388f2-110">Open the integrated terminal from Visual Studio Code by selecting **View** > **Integrated Terminal** from the main menu.</span></span>

1. <span data-ttu-id="388f2-111">Nella finestra del terminale digitare **dotnet new console**.</span><span class="sxs-lookup"><span data-stu-id="388f2-111">In the terminal window, type **dotnet new console**.</span></span>

    <span data-ttu-id="388f2-112">Questo comando crea un file **Program.cs** nella cartella con un semplice programma "Hello World" già scritto, oltre a un file di progetto C# denominato **learning-module.csproj**.</span><span class="sxs-lookup"><span data-stu-id="388f2-112">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

1. <span data-ttu-id="388f2-113">Nella finestra del terminale digitare il comando seguente per eseguire il programma "Hello World".</span><span class="sxs-lookup"><span data-stu-id="388f2-113">In the terminal window, type the following command to run the "Hello World" program.</span></span> 

    ```
    dotnet run
    ```

    <span data-ttu-id="388f2-114">La finestra del terminale visualizza "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="388f2-114">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="388f2-115">come output.</span><span class="sxs-lookup"><span data-stu-id="388f2-115">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="388f2-116">Connettere l'app ad Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="388f2-116">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="388f2-117">Accedere ad Azure facendo clic su **Visualizza** > **Riquadro comandi** e digitando **Azure: Accedi**.</span><span class="sxs-lookup"><span data-stu-id="388f2-117">Sign in to Azure by clicking **View** > **Command Palette** and typing **Azure: Sign In**.</span></span>

    <span data-ttu-id="388f2-118">Seguire le istruzioni per copiare e incollare il codice fornito nel Web browser, che autentica la sessione di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="388f2-118">Follow the prompts to copy and paste the code provided in the web browser, which authenticates your Visual Studio Code session.</span></span>

1. <span data-ttu-id="388f2-119">Fare clic sull'![icona di Esplora risorse](../media/2-setup/visual-studio-code-explorer-icon.png) **Esplora risorse** nel menu a sinistra e quindi espandere **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="388f2-119">Click the ![Explorer icon](../media/2-setup/visual-studio-code-explorer-icon.png) **Explorer** icon on the left menu, and then expand **Azure Cosmos DB**.</span></span>

1. <span data-ttu-id="388f2-120">Espandere la sottoscrizione di Azure > account Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="388f2-120">Expand your Azure subscription > Azure Cosmos DB account.</span></span> <span data-ttu-id="388f2-121">Se nei moduli precedenti sono stati creati il database **Products** e la raccolta **Clothing**, l'estensione li visualizza.</span><span class="sxs-lookup"><span data-stu-id="388f2-121">If you created the **Products** database and **Clothing** collection in the previous modules, the extension displays them.</span></span>

   ![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

1. <span data-ttu-id="388f2-123">Verranno ora creati un nuovo database e una raccolta per i clienti.</span><span class="sxs-lookup"><span data-stu-id="388f2-123">Now let's create a new database and collection for your customers.</span></span>

    <span data-ttu-id="388f2-124">Nella finestra Esplora risorse fare clic con il pulsante destro del mouse sul proprio account e quindi fare clic su **Crea database**.</span><span class="sxs-lookup"><span data-stu-id="388f2-124">In the Explorer window, right-click your account, and then click **Create Database**.</span></span> 
    
    <span data-ttu-id="388f2-125">Nella casella di testo nella parte superiore della schermata, digitare **Users** per il nome del database > **INVIO** > **WebCustomers** per il nome della raccolta >  **INVIO** > **userId** per la chiave di partizione > **INVIO** > **1000** per la capacità di unità elaborate iniziale > **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="388f2-125">In the text box at the top of the screen, type **Users** for the database name > **Enter** > **WebCustomers** for the collection name > **Enter** > **userId** for the partition key > **Enter** > **1000** for the initial throughput capacity > **Enter**.</span></span>

    <span data-ttu-id="388f2-126">![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span><span class="sxs-lookup"><span data-stu-id="388f2-126">![Azure Cosmos DB Visual Studio Code extension](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span></span>

    <span data-ttu-id="388f2-127">Il nuovo database Users e la raccolta WebCustomers vengono visualizzati nella finestra Esplora risorse.</span><span class="sxs-lookup"><span data-stu-id="388f2-127">The new Users database and WebCustomers collection are displayed in the Explorer window.</span></span>

1. <span data-ttu-id="388f2-128">Nel terminale integrato eseguire ognuno dei comandi seguenti da un nuovo prompt dei comandi per installare i pacchetti NuGet necessari.</span><span class="sxs-lookup"><span data-stu-id="388f2-128">In the integrated terminal, run each of the following commands at a new prompt to install the required NuGet packages.</span></span>

    ```
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet add package Bogus
    dotnet restore
    ```

1. <span data-ttu-id="388f2-129">Nella parte superiore del riquadro Esplora risorse fare clic su **Program.cs** per aprire il file.</span><span class="sxs-lookup"><span data-stu-id="388f2-129">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

1. <span data-ttu-id="388f2-130">Aggiungere le istruzioni using seguenti dopo `using System;`.</span><span class="sxs-lookup"><span data-stu-id="388f2-130">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="388f2-131">Creare un nuovo file denominato App.config nella cartella del modulo di apprendimento e aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="388f2-131">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. <span data-ttu-id="388f2-132">Copiare la stringa di connessione dall'estensione Azure Cosmos DB facendo clic con il pulsante destro del mouse sull'account del modulo di apprendimento e scegliendo **Copy Connection String** (Copia stringa di connessione).</span><span class="sxs-lookup"><span data-stu-id="388f2-132">Copy your connection string from the Azure Cosmos DB extension by right-clicking the learning-module account, and clicking **Copy Connection String**.</span></span>

    ![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/vs-code-copy-connection-string.gif) 

1. <span data-ttu-id="388f2-134">Incollare la stringa di connessione in un file di testo e quindi copiare la parte **AccountEndpoint** dal file di testo in **accountEndpoint** nel file App.config.</span><span class="sxs-lookup"><span data-stu-id="388f2-134">Paste the connection string into a text file, and then copy the **AccountEndpoint** portion from the text file into the **accountEndpoint** in App.config.</span></span>

    <span data-ttu-id="388f2-135">AccountEndpoint dovrebbe avere un aspetto analogo al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="388f2-135">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. <span data-ttu-id="388f2-136">A questo punto copiare il valore **AccountKey** dal valore di testo nel valore **accountKey** in App.config.</span><span class="sxs-lookup"><span data-stu-id="388f2-136">Now copy the **AccountKey** value from the text value into the **accountKey** value in App.config.</span></span>

1. <span data-ttu-id="388f2-137">Nel terminale integrato digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="388f2-137">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="388f2-138">Aggiungere una nuova attività asincrona per creare un nuovo client e verificare se il database Users esiste aggiungendo il metodo seguente dopo il metodo principale.</span><span class="sxs-lookup"><span data-stu-id="388f2-138">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the main method.</span></span>
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpointUrl"]), ConfigurationManager.AppSettings["primaryKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

1. <span data-ttu-id="388f2-139">Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="388f2-139">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="388f2-140">Copiare e incollare il codice seguente nel metodo **Main**, sovrascrivendo la riga `Console.WriteLine("Hello World!");` corrente.</span><span class="sxs-lookup"><span data-stu-id="388f2-140">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

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

1. <span data-ttu-id="388f2-141">Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="388f2-141">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="388f2-142">La console visualizza l'output seguente.</span><span class="sxs-lookup"><span data-stu-id="388f2-142">The console displays the following output.</span></span>
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a><span data-ttu-id="388f2-143">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="388f2-143">Summary</span></span>

<span data-ttu-id="388f2-144">In questa unità sono state poste le basi per creare l'applicazione di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="388f2-144">In this unit, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="388f2-145">Dopo aver configurato l'ambiente di sviluppo in Visual Studio Code, è stato creato un semplice progetto HelloWorld, è stato connesso il progetto all'endpoint di Azure Cosmos DB ed è stata verificata l'esistenza di raccolta e database.</span><span class="sxs-lookup"><span data-stu-id="388f2-145">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>