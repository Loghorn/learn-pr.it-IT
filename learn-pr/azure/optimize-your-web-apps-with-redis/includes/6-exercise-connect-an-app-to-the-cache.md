<span data-ttu-id="87c42-101">Dopo avere creato una cache Redis in Azure, è possibile creare un'applicazione per usarla.</span><span class="sxs-lookup"><span data-stu-id="87c42-101">Now that we have a Redis cache created in Azure, let's create an application to use it.</span></span> <span data-ttu-id="87c42-102">Assicurarsi che siano disponibili le informazioni sulla stringa di connessione dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="87c42-102">Make sure you have your connection string information from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="87c42-103">Il servizio Cloud Shell integrato è disponibile a destra.</span><span class="sxs-lookup"><span data-stu-id="87c42-103">The integrated Cloud Shell is available on the right.</span></span> <span data-ttu-id="87c42-104">È possibile usare tale prompt dei comandi per creare ed eseguire il codice dell'esempio compilato qui oppure è possibile eseguire la procedura in locale, se si ha una configurazione con ambiente di sviluppo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="87c42-104">You can use that command prompt to create and run the example code we are building here, or perform these steps locally if you have a .NET Core development environment setup.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="87c42-105">Creare un'applicazione console</span><span class="sxs-lookup"><span data-stu-id="87c42-105">Create a Console Application</span></span>

<span data-ttu-id="87c42-106">Verrà usata una semplice applicazione console, per consentire di concentrarsi sull'implementazione di Redis.</span><span class="sxs-lookup"><span data-stu-id="87c42-106">We'll use a simple Console Application so we can focus on the Redis implementation.</span></span>

1. <span data-ttu-id="87c42-107">In Cloud Shell creare una nuova applicazione console .NET Core e assegnare all'applicazione il nome "SportsStatsTracker"</span><span class="sxs-lookup"><span data-stu-id="87c42-107">In the Cloud Shell, create a new .NET Core Console Application, name it "SportsStatsTracker"</span></span>

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. <span data-ttu-id="87c42-108">Verrà creata una cartella per il progetto. Procedere e cambiare la directory corrente.</span><span class="sxs-lookup"><span data-stu-id="87c42-108">This will create a folder for the project, go ahead and change the current directory.</span></span>

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a><span data-ttu-id="87c42-109">Aggiungere la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="87c42-109">Add the connection string</span></span>

<span data-ttu-id="87c42-110">È ora possibile aggiungere al codice la stringa di connessione ottenuta dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="87c42-110">Let's add the connection string we got from the Azure portal into the code.</span></span> <span data-ttu-id="87c42-111">Non archiviare mai credenziali di questo tipo nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="87c42-111">Never store credentials like this in your source code.</span></span> <span data-ttu-id="87c42-112">Per non complicare l'esempio, verrà usato un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="87c42-112">To keep this sample simple, we're going to use a configuration file.</span></span> <span data-ttu-id="87c42-113">Un approccio migliore per un'applicazione lato server in Azure consiste nell'usare Azure Key Vault con certificati.</span><span class="sxs-lookup"><span data-stu-id="87c42-113">A better approach for a server-side application in Azure would be to use Azure Key Vault with certificates.</span></span>

1. <span data-ttu-id="87c42-114">Creare un nuovo file **appsettings.json** da aggiungere al progetto.</span><span class="sxs-lookup"><span data-stu-id="87c42-114">Create a new **appsettings.json** file to add to the project.</span></span>

    ```bash
    touch appsettings.json
    ```

1. <span data-ttu-id="87c42-115">Aprire l'editor di codice digitando `code .` nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="87c42-115">Open the code editor by typing `code .` in the project folder.</span></span> <span data-ttu-id="87c42-116">Se si lavora in locale, è consigliabile usare **Visual Studio Code**.</span><span class="sxs-lookup"><span data-stu-id="87c42-116">If you are working locally, we recommend using **Visual Studio Code**.</span></span> <span data-ttu-id="87c42-117">Questa procedura sarà allineata per la maggior parte al rispettivo utilizzo.</span><span class="sxs-lookup"><span data-stu-id="87c42-117">The steps here will mostly align with it's usage.</span></span>

1. <span data-ttu-id="87c42-118">Selezionare il file **appsettings.json** file nell'editor e aggiungere il testo seguente.</span><span class="sxs-lookup"><span data-stu-id="87c42-118">Select the **appsettings.json** file in the editor and add the following text.</span></span> <span data-ttu-id="87c42-119">Incollare la stringa di connessione nell'elemento **value** dell'impostazione.</span><span class="sxs-lookup"><span data-stu-id="87c42-119">Paste your connection string into the **value** of the setting.</span></span>

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. <span data-ttu-id="87c42-120">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="87c42-120">Save the changes.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="87c42-121">Ogni volta che si incolla o si modifica codice in un file nell'editor, assicurarsi di salvare le modifiche tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux o <kbd>Comando+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="87c42-121">Whenever you paste or change code into a file in the editor, make sure to save afterwards using the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="87c42-122">Fare clic sul file **SportsStatsTracker.csproj** nell'editor per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="87c42-122">Click on the **SportsStatsTracker.csproj** file in the editor to open it.</span></span>

1. <span data-ttu-id="87c42-123">Aggiungere il blocco di configurazione `<ItemGroup>` seguente nell'elemento radice `<Project>` per includere il nuovo file nel progetto e copiarlo nella cartella di output.</span><span class="sxs-lookup"><span data-stu-id="87c42-123">Add the following `<ItemGroup>` configuration block into the root `<Project>` element to include the new file in the project and copy it to the output folder.</span></span> <span data-ttu-id="87c42-124">In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="87c42-124">This ensures that the app configuration file is placed in the output directory when the app is compiled/built.</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. <span data-ttu-id="87c42-125">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="87c42-125">Save the file.</span></span> <span data-ttu-id="87c42-126">Assicurarsi di eseguire questa operazione. In caso contrario le modifiche andranno perse quando si aggiunge il pacchetto più avanti.</span><span class="sxs-lookup"><span data-stu-id="87c42-126">(Make sure you do this or you will lose the change when you add the package below!)</span></span>

## <a name="add-support-to-read-a-json-configuration-file"></a><span data-ttu-id="87c42-127">Aggiungere il supporto per la lettura di un file di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="87c42-127">Add support to read a JSON configuration file</span></span>

<span data-ttu-id="87c42-128">Un'applicazione .NET Core richiede pacchetti NuGet aggiuntivi per leggere un file di configurazione JSON.</span><span class="sxs-lookup"><span data-stu-id="87c42-128">A .NET Core application requires additional NuGet packages to read a JSON configuration file.</span></span>

1. <span data-ttu-id="87c42-129">Nella sezione della finestra relativa al prompt dei comandi aggiungere un riferimento al pacchetto NuGet **Microsoft.Extensions.Configuration.Json**.</span><span class="sxs-lookup"><span data-stu-id="87c42-129">In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.</span></span>

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a><span data-ttu-id="87c42-130">Aggiungere codice per leggere il file di configurazione</span><span class="sxs-lookup"><span data-stu-id="87c42-130">Add code to read the configuration file</span></span>

<span data-ttu-id="87c42-131">Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="87c42-131">Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.</span></span>

1. <span data-ttu-id="87c42-132">Selezionare **Program.cs** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="87c42-132">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="87c42-133">Nella parte superiore del file è presente la riga `using System`.</span><span class="sxs-lookup"><span data-stu-id="87c42-133">At the top of the file, a `using System` line is present.</span></span> <span data-ttu-id="87c42-134">Al di sotto di questa riga aggiungere le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="87c42-134">Underneath that line, add the following lines of code:</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="87c42-135">Sostituire il contenuto del metodo **Main** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="87c42-135">Replace the contents of the **Main** method with the following code.</span></span> <span data-ttu-id="87c42-136">Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="87c42-136">This code initializes the configuration system to read from the **appsettings.json** file.</span></span>

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

<span data-ttu-id="87c42-137">Il file **Program.cs** avrà ora un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="87c42-137">Your **Program.cs** file should now look like the following:</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace SportsStatsTracker
{
    class Program
    {
        static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();
        }
    }
}
```

## <a name="get-the-connection-string-from-configuration"></a><span data-ttu-id="87c42-138">Ottenere la stringa di connessione dalla configurazione</span><span class="sxs-lookup"><span data-stu-id="87c42-138">Get the connection string from configuration</span></span>

1. <span data-ttu-id="87c42-139">In **Program.cs** usare alla fine del metodo **Main** la nuova variabile **config** per recuperare la stringa di connessione e archiviarla in una nuova variabile denominata **connectionString**.</span><span class="sxs-lookup"><span data-stu-id="87c42-139">In **Program.cs**, at the end of the **Main** method, use the new **config** variable to retrieve the connection string and store it in a new variable named **connectionString**.</span></span>
    - <span data-ttu-id="87c42-140">La variabile **config** include un indicizzatore, che consente di passare una stringa da recuperare dal file **appSettings.json**.</span><span class="sxs-lookup"><span data-stu-id="87c42-140">The **config** variable has an indexer where you can pass in a string to retrieve from your **appSettings.json** file.</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a><span data-ttu-id="87c42-141">Aggiungere il supporto per il client .NET della cache Redis</span><span class="sxs-lookup"><span data-stu-id="87c42-141">Add support for the Redis cache .NET client</span></span>

<span data-ttu-id="87c42-142">È ora possibile configurare l'applicazione console per usare il client **StackExchange.Redis** per .NET.</span><span class="sxs-lookup"><span data-stu-id="87c42-142">Next, let's configure the console application to use the **StackExchange.Redis** client for .NET.</span></span>

1. <span data-ttu-id="87c42-143">Aggiungere il pacchetto NuGet **StackExchange.Redis** al progetto usando il prompt dei comandi nella parte inferiore dell'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="87c42-143">Add the **StackExchange.Redis** NuGet package to the project using the command prompt at the bottom of the Cloud Shell editor.</span></span>

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. <span data-ttu-id="87c42-144">Selezionare**Program.cs** nell'editor e aggiungere `using` per lo spazio dei nomi **StackExchange.Redis**</span><span class="sxs-lookup"><span data-stu-id="87c42-144">Select **Program.cs** in the editor and add a `using` for the namespace **StackExchange.Redis**</span></span>

    ```csharp
    using StackExchange.Redis;
    ```
    
<span data-ttu-id="87c42-145">Dopo il completamento dell'installazione, il client della cache StackExchange.Redis è disponibile per l'uso con il progetto.</span><span class="sxs-lookup"><span data-stu-id="87c42-145">Once the installation is completed, the Redis cache client is available to use with your project.</span></span>

## <a name="connect-to-the-cache"></a><span data-ttu-id="87c42-146">Connettersi alla cache</span><span class="sxs-lookup"><span data-stu-id="87c42-146">Connect to the cache</span></span>

<span data-ttu-id="87c42-147">È ora possibile aggiungere il codice per la connessione alla cache.</span><span class="sxs-lookup"><span data-stu-id="87c42-147">Let's add the code to connect to the cache.</span></span>

1. <span data-ttu-id="87c42-148">Selezionare **Program.cs** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="87c42-148">Select **Program.cs** in the editor.</span></span>

1. <span data-ttu-id="87c42-149">Creare un oggetto `ConnectionMultiplexer` usando `ConnectionMultiplexer.Connect` mediante il passaggio della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="87c42-149">Create a `ConnectionMultiplexer` using `ConnectionMultiplexer.Connect` by passing it your connection string.</span></span> <span data-ttu-id="87c42-150">Assegnare il nome **cache** al valore restituito.</span><span class="sxs-lookup"><span data-stu-id="87c42-150">Name the returned value **cache**.</span></span>

1. <span data-ttu-id="87c42-151">Poiché la connessione creata è _eliminabile_, eseguirne il wrapping in un blocco `using`.</span><span class="sxs-lookup"><span data-stu-id="87c42-151">Since the created connection is _disposable_, wrap it in a `using` block.</span></span> <span data-ttu-id="87c42-152">Il codice dovrebbe essere simile a:</span><span class="sxs-lookup"><span data-stu-id="87c42-152">Your code should look something like:</span></span>

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> <span data-ttu-id="87c42-153">La connessione a Cache Redis di Azure è gestita dalla classe `ConnectionMultiplexer`.</span><span class="sxs-lookup"><span data-stu-id="87c42-153">The connection to Azure Redis Cache is managed by the `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="87c42-154">Questa classe deve essere condivisa e riutilizzata in tutta l'applicazione client.</span><span class="sxs-lookup"><span data-stu-id="87c42-154">This class should be shared and reused throughout your client application.</span></span> <span data-ttu-id="87c42-155">_Non_ creare una nuova connessione per ogni operazione.</span><span class="sxs-lookup"><span data-stu-id="87c42-155">We do _not_ want to create a new connection for each operation.</span></span> <span data-ttu-id="87c42-156">È consigliabile invece archiviare la connessione come campo nella classe e riutilizzarla per ogni operazione.</span><span class="sxs-lookup"><span data-stu-id="87c42-156">Instead, we want to store it off as a field in our class and reuse it for each operation.</span></span> <span data-ttu-id="87c42-157">In questo caso verrà usata solo nel metodo **Main**, ma in un'applicazione di produzione è necessario archiviarla in un campo della classe o in un singleton.</span><span class="sxs-lookup"><span data-stu-id="87c42-157">Here we are only going to use it in the **Main** method, but in a production application, it should be stored in a class field, or a singleton.</span></span>

## <a name="add-a-value-to-the-cache"></a><span data-ttu-id="87c42-158">Aggiungere un valore alla cache</span><span class="sxs-lookup"><span data-stu-id="87c42-158">Add a value to the cache</span></span>

<span data-ttu-id="87c42-159">Dopo avere creato la connessione è possibile aggiungere un valore alla cache.</span><span class="sxs-lookup"><span data-stu-id="87c42-159">Now that we have the connection, let's add a value to the cache.</span></span>

1. <span data-ttu-id="87c42-160">Al termine della creazione della connessione, nel blocco `using` usare il metodo `GetDatabase` per recuperare un'istanza di `IDatabase`.</span><span class="sxs-lookup"><span data-stu-id="87c42-160">Inside the `using` block after the connection has been created, use the `GetDatabase` method to retrieve an `IDatabase` instance.</span></span>

1. <span data-ttu-id="87c42-161">Chiamare `StringSet` nell'oggetto `IDatabase` per impostare la chiave "test:key" sul valore "some value".</span><span class="sxs-lookup"><span data-stu-id="87c42-161">Call `StringSet` on the `IDatabase` object to set the key "test:key" to the value "some value".</span></span>
    - <span data-ttu-id="87c42-162">Il valore restituito da `StringSet` è un valore `bool`, che indica se la chiave è stata aggiunta.</span><span class="sxs-lookup"><span data-stu-id="87c42-162">the return value from `StringSet` is a `bool` indicating whether the key was added.</span></span>

1. <span data-ttu-id="87c42-163">Visualizzare il valore restituito da `StringSet` nella console.</span><span class="sxs-lookup"><span data-stu-id="87c42-163">Display the return value from `StringSet` onto the console.</span></span>

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a><span data-ttu-id="87c42-164">Ottenere un valore dalla cache</span><span class="sxs-lookup"><span data-stu-id="87c42-164">Get a value from the cache</span></span>

1. <span data-ttu-id="87c42-165">Recuperare quindi il valore usando `StringGet`.</span><span class="sxs-lookup"><span data-stu-id="87c42-165">Next, retrieve the value using `StringGet`.</span></span> <span data-ttu-id="87c42-166">Viene acquisita la chiave da recuperare e viene restituito il valore.</span><span class="sxs-lookup"><span data-stu-id="87c42-166">This takes the key to retrieve and returns the value.</span></span>

1. <span data-ttu-id="87c42-167">Usare come output il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="87c42-167">Output the returned value.</span></span>

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="87c42-168">Il codice dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="87c42-168">Your code should look like this:</span></span>

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;
    using StackExchange.Redis;
    
    namespace SportsStatsTracker
    {
        class Program
        {
            static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json")
                    .Build();
    
                string connectionString = config["CacheConnection"];
    
                using (var cache = ConnectionMultiplexer.Connect(connectionString))
                {
                    var db = cache.GetDatabase();
    
                    bool setValue = db.StringSet("test:key", "some value");
                    Console.WriteLine($"SET: {setValue}");
    
                    string getValue = db.StringGet("test:key");
                    Console.WriteLine($"GET: {getValue}");
                }
            }
        }
    }
    ```
    
1. <span data-ttu-id="87c42-169">Eseguire l'applicazione per visualizzare il risultato.</span><span class="sxs-lookup"><span data-stu-id="87c42-169">Run the application to see the result.</span></span> <span data-ttu-id="87c42-170">Digitare `dotnet run` nella finestra del terminale sotto l'editor.</span><span class="sxs-lookup"><span data-stu-id="87c42-170">Type `dotnet run` into the terminal window below the editor.</span></span> <span data-ttu-id="87c42-171">Assicurarsi di trovarsi nella cartella del progetto. In caso contrario, non sarà possibile trovare il codice da compilare ed eseguire.</span><span class="sxs-lookup"><span data-stu-id="87c42-171">Make sure you are in the project folder or it won't find your code to build and run.</span></span>
    
    ```bash
    dotnet run
    ```
    
> [!TIP] 
> <span data-ttu-id="87c42-172">Se il programma non esegue le operazioni previste, ma viene compilato, è possibile le modifiche apportate nell'editor non siano state salvate.</span><span class="sxs-lookup"><span data-stu-id="87c42-172">If the program doesn't do what you expect, but compiles, it may be because you have not saved changes in the editor.</span></span> <span data-ttu-id="87c42-173">Ricordare sempre di salvare le modifiche quando ci si sposta tra la finestra del terminale e quella dell'editor</span><span class="sxs-lookup"><span data-stu-id="87c42-173">Always remember to save changes as you switch between the terminal and the editor windows</span></span> 

## <a name="use-the-async-versions-of-the-methods"></a><span data-ttu-id="87c42-174">Usare le versioni asincrone dei metodi</span><span class="sxs-lookup"><span data-stu-id="87c42-174">Use the async versions of the methods</span></span>

<span data-ttu-id="87c42-175">È stato possibile ottenere e configurare i valori dalla cache, ma vengono usate le versioni sincrone meno recenti.</span><span class="sxs-lookup"><span data-stu-id="87c42-175">We have been able to get and set values from the cache, but we are using the older synchronous versions.</span></span> <span data-ttu-id="87c42-176">Nelle applicazioni lato server tali versioni non consentono un uso efficiente dei thread.</span><span class="sxs-lookup"><span data-stu-id="87c42-176">In server-side applications, these are not an efficient use of our threads.</span></span> <span data-ttu-id="87c42-177">È invece consigliabile usare le versioni _asincrone_ dei metodi.</span><span class="sxs-lookup"><span data-stu-id="87c42-177">Instead, we want to use the _asynchronous_ versions of the methods.</span></span> <span data-ttu-id="87c42-178">È possibile individuarle con facilità, perché terminano tutte con **Async**.</span><span class="sxs-lookup"><span data-stu-id="87c42-178">You can easily spot them - they all end in **Async**.</span></span>

<span data-ttu-id="87c42-179">Per semplificare l'utilizzo di questi metodi, è possibile usare le parole chiave `async` e `await` di C#.</span><span class="sxs-lookup"><span data-stu-id="87c42-179">To make these methods easy to work with, we can use the C# `async` and `await` keywords.</span></span> <span data-ttu-id="87c42-180">Sarà tuttavia necessario usare _almeno_ C# 7.1 per potere applicare tali parole chiave al metodo **Main**.</span><span class="sxs-lookup"><span data-stu-id="87c42-180">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="87c42-181">Passare a C# 7.1</span><span class="sxs-lookup"><span data-stu-id="87c42-181">Switch to C# 7.1</span></span>

<span data-ttu-id="87c42-182">Le parole chiave `async` e `await` di C# non sono parole chiave valide nei metodi **Main** nelle versioni precedenti a C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="87c42-182">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="87c42-183">È possibile passare con facilità a tale compilatore tramite un flag nel file **.csproj**.</span><span class="sxs-lookup"><span data-stu-id="87c42-183">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="87c42-184">Aprire il file **SportsStatsTracker.csproj** nell'editor.</span><span class="sxs-lookup"><span data-stu-id="87c42-184">Open the **SportsStatsTracker.csproj** file in the editor.</span></span>

1. <span data-ttu-id="87c42-185">Aggiungere `<LangVersion>7.1</LangVersion>` nel primo elemento `PropertyGroup` del file di compilazione.</span><span class="sxs-lookup"><span data-stu-id="87c42-185">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="87c42-186">Al termine il risultato dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="87c42-186">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="87c42-187">Applicare la parola chiave async</span><span class="sxs-lookup"><span data-stu-id="87c42-187">Apply the async keyword</span></span>

<span data-ttu-id="87c42-188">Applicare quindi la parola chiave `async` al metodo **Main**.</span><span class="sxs-lookup"><span data-stu-id="87c42-188">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="87c42-189">Sarà necessario eseguire tre operazioni.</span><span class="sxs-lookup"><span data-stu-id="87c42-189">We will have to do three things.</span></span>

1. <span data-ttu-id="87c42-190">Aggiungere la parola chiave `async` alla firma del metodo **Main**.</span><span class="sxs-lookup"><span data-stu-id="87c42-190">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="87c42-191">Cambiare il tipo restituito da `void` a `Task`.</span><span class="sxs-lookup"><span data-stu-id="87c42-191">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="87c42-192">Aggiungere un'istruzione `using` per includere `System.Threading.Tasks`.</span><span class="sxs-lookup"><span data-stu-id="87c42-192">Add a `using` statement to include `System.Threading.Tasks`.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
        ...
```

### <a name="get-and-set-values-asynchronously"></a><span data-ttu-id="87c42-193">Ottenere e impostare i valori in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="87c42-193">Get and set values asynchronously</span></span>

<span data-ttu-id="87c42-194">È possibile lasciare i metodi sincroni così come sono e aggiungere una chiamata ai metodi `StringSetAsync` e `StringGetAsync` per aggiungere un altro valore alla cache.</span><span class="sxs-lookup"><span data-stu-id="87c42-194">We can leave the synchronous methods in place, let's add a call to the `StringSetAsync` and `StringGetAsync` methods to add another value to the cache.</span></span> <span data-ttu-id="87c42-195">Impostare "counter" sul valore "100".</span><span class="sxs-lookup"><span data-stu-id="87c42-195">Set "counter" to the value "100".</span></span>  

1. <span data-ttu-id="87c42-196">Usare i metodi `StringSetAsync` e `StringGetAsync` per impostare e recuperare una chiave denominata "counter".</span><span class="sxs-lookup"><span data-stu-id="87c42-196">Use the `StringSetAsync` and `StringGetAsync` methods to set and retrieve a key named "counter".</span></span> <span data-ttu-id="87c42-197">Impostare il valore su "100".</span><span class="sxs-lookup"><span data-stu-id="87c42-197">Set the value to "100".</span></span>

1. <span data-ttu-id="87c42-198">Applicare la parola chiave `await` per ottenere i risultati da ogni metodo.</span><span class="sxs-lookup"><span data-stu-id="87c42-198">Apply the `await` keyword to get the results from each method.</span></span>

1. <span data-ttu-id="87c42-199">Restituire come output i risultati alla finestra della console, analogamente alle versioni sincrone.</span><span class="sxs-lookup"><span data-stu-id="87c42-199">Output the results to the console window - just as you did with the synchronous versions.</span></span>

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. <span data-ttu-id="87c42-200">Eseguire di nuovo l'applicazione. Dovrebbe funzionare comunque e includere ora due valori.</span><span class="sxs-lookup"><span data-stu-id="87c42-200">Run the application again - it should still work and now have two values.</span></span>

#### <a name="increment-the-value"></a><span data-ttu-id="87c42-201">Incrementare il valore</span><span class="sxs-lookup"><span data-stu-id="87c42-201">Increment the value</span></span>

1. <span data-ttu-id="87c42-202">Usare il metodo `StringIncrementAsync` per incrementare il valore **counter**.</span><span class="sxs-lookup"><span data-stu-id="87c42-202">Use the `StringIncrementAsync` method to increment your **counter** value.</span></span> <span data-ttu-id="87c42-203">Passare il numero **50** da aggiungere al valore counter.</span><span class="sxs-lookup"><span data-stu-id="87c42-203">Pass the number **50** to add to the counter.</span></span>
    - <span data-ttu-id="87c42-204">Si noti che il metodo accetta la chiave _e_ anche `long` o `double`.</span><span class="sxs-lookup"><span data-stu-id="87c42-204">Notice that the method takes the key _and_ either a `long` or `double`.</span></span>
    - <span data-ttu-id="87c42-205">In base ai parametri passati, restituisce `long` o `double`.</span><span class="sxs-lookup"><span data-stu-id="87c42-205">Depending on the parameters passed, it either returns a `long` or `double`.</span></span>

1. <span data-ttu-id="87c42-206">Restituire come output i risultati del metodo alla console.</span><span class="sxs-lookup"><span data-stu-id="87c42-206">Output the results of the method to the console.</span></span>

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a><span data-ttu-id="87c42-207">Altre operazioni</span><span class="sxs-lookup"><span data-stu-id="87c42-207">Other operations</span></span>

<span data-ttu-id="87c42-208">È infine possibile provare a eseguire altri metodi con il supporto `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="87c42-208">Finally, let's try executing a few additional methods with the `ExecuteAsync` support.</span></span>

1. <span data-ttu-id="87c42-209">Eseguire "PING" per testare la connessione al server.</span><span class="sxs-lookup"><span data-stu-id="87c42-209">Execute "PING" to test the server connection.</span></span> <span data-ttu-id="87c42-210">Dovrebbe restituire una risposta "PONG".</span><span class="sxs-lookup"><span data-stu-id="87c42-210">It should respond with "PONG".</span></span>
1. <span data-ttu-id="87c42-211">Eseguire "FLUSHDB" per cancellare i valori del database.</span><span class="sxs-lookup"><span data-stu-id="87c42-211">Execute "FLUSHDB" to clear the database values.</span></span> <span data-ttu-id="87c42-212">Dovrebbe restituire una risposta "OK".</span><span class="sxs-lookup"><span data-stu-id="87c42-212">It should respond with "OK".</span></span>

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

<span data-ttu-id="87c42-213">Il codice finale dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="87c42-213">The final code should look something like:</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();

            string connectionString = config["CacheConnection"];

            using (var cache = ConnectionMultiplexer.Connect(connectionString))
            {
                var db = cache.GetDatabase();

                bool setValue = db.StringSet("test:key", "some value");
                Console.WriteLine($"SET: {setValue}");

                string getValue = db.StringGet("test:key");
                Console.WriteLine($"GET: {getValue}");

                setValue = await db.StringSetAsync("test", "100");
                Console.WriteLine($"SET: {setValue}");

                getValue = await db.StringGetAsync("test");
                Console.WriteLine($"GET: {getValue}");

                var result = await db.ExecuteAsync("ping");
                Console.WriteLine($"PING = {result.Type} : {result}");
                
                result = await db.ExecuteAsync("flushdb");
                Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
            }
        }
    }
}
```

## <a name="challenge"></a><span data-ttu-id="87c42-214">Verifica</span><span class="sxs-lookup"><span data-stu-id="87c42-214">Challenge</span></span>

<span data-ttu-id="87c42-215">Come verifica, provare a serializzare un tipo di oggetto nella cache.</span><span class="sxs-lookup"><span data-stu-id="87c42-215">As a challenge, try serializing an object type to the cache.</span></span> <span data-ttu-id="87c42-216">Ecco la procedura di base.</span><span class="sxs-lookup"><span data-stu-id="87c42-216">Here are the basic steps.</span></span>

1. <span data-ttu-id="87c42-217">Creare un nuovo valore `class` con alcune proprietà pubbliche.</span><span class="sxs-lookup"><span data-stu-id="87c42-217">Create a new `class` with some public properties.</span></span> <span data-ttu-id="87c42-218">È possibile inventare una proprietà personalizzata, come "Person" o "Car", oppure usare l'esempio "GameStats" fornito nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="87c42-218">You can invent one of your own ("Person" or "Car" are popular), or use the "GameStats" example given in the previous unit.</span></span>

1. <span data-ttu-id="87c42-219">Aggiungere il supporto per il pacchetto NuGet **Newtonsoft.Json** mediante `dotnet add package`.</span><span class="sxs-lookup"><span data-stu-id="87c42-219">Add support for the **Newtonsoft.Json** NuGet package using `dotnet add package`.</span></span>

1. <span data-ttu-id="87c42-220">Aggiungere `using` per lo spazio dei nomi `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="87c42-220">Add a `using` for the `Newtonsoft.Json` namespace.</span></span>

1. <span data-ttu-id="87c42-221">Creare uno degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="87c42-221">Create one of your objects.</span></span>

1. <span data-ttu-id="87c42-222">Serializzarlo con `JsonConvert.SerializeObject` e usare `StringSetAsync` per eseguirne il push nella cache.</span><span class="sxs-lookup"><span data-stu-id="87c42-222">Serialize it with `JsonConvert.SerializeObject` and use `StringSetAsync` to push it into the cache.</span></span>

1. <span data-ttu-id="87c42-223">Recuperarlo dalla cache con `StringGetAsync` e quindi deserializzarlo con `JsonConvert.DeserializeObject<T>`.</span><span class="sxs-lookup"><span data-stu-id="87c42-223">Get it back from the cache with `StringGetAsync` and then deserialize it with `JsonConvert.DeserializeObject<T>`.</span></span>

