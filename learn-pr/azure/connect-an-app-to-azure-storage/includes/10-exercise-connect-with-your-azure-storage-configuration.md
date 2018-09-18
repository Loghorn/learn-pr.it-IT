<span data-ttu-id="a4fc3-101">::: zone pivot="csharp" Si procederà all'aggiunta di codice per recuperare la stringa di connessione dalla configurazione, per poi usarla per connettersi all'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-101">::: zone pivot="csharp" Let's add code to retrieve the connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="a4fc3-102">Recuperare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="a4fc3-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="a4fc3-103">Selezionare **Program.cs** per aprirlo nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-103">Select **Program.cs** to open it in the code editor.</span></span>

1. <span data-ttu-id="a4fc3-104">Aggiungere un'istruzione `using` all'inizio del file per fare riferimento allo spazio dei nomi `Microsoft.WindowsAzure.Storage`:</span><span class="sxs-lookup"><span data-stu-id="a4fc3-104">Add a `using` statement at the top of the file to reference the `Microsoft.WindowsAzure.Storage` namespace:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="a4fc3-105">Alla fine del metodo `Main` aggiungere la riga seguente per recuperare la stringa di connessione dell'account di archiviazione di Azure dal file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-105">At the end of the `Main` method, add the following line to retrieve the Azure storage account connection string from the configuration file.</span></span> <span data-ttu-id="a4fc3-106">Il valore _key_ passato deve corrispondere al nome usato nel file **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-106">The passed _key_ must match the name used in your **appsettings.json** file.</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="a4fc3-107">Creare un client BLOB</span><span class="sxs-lookup"><span data-stu-id="a4fc3-107">Create a blob client</span></span>

1. <span data-ttu-id="a4fc3-108">Usare il metodo `CloudStorageAccount.TryParse` statico per creare un oggetto `CloudStorageAccount`; accetta la stringa di connessione e un parametro `out` per restituire l'oggetto creato.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-108">Use the static `CloudStorageAccount.TryParse` method to create a `CloudStorageAccount` object - it takes the connection string and an `out` parameter to return the created object.</span></span> <span data-ttu-id="a4fc3-109">Restituisce un valore `bool` che indica se la stringa è stata analizzata correttamente.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-109">It returns a `bool` value indicating whether it successfully parsed the string.</span></span>
    - <span data-ttu-id="a4fc3-110">In caso di errore, visualizzare un messaggio per la console e restituire un valore dal metodo.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-110">If it fails, output a message to the console and return from the method.</span></span>

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. <span data-ttu-id="a4fc3-111">Usare l'oggetto `CloudStorageAccount` restituito per creare un client BLOB.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-111">Use the returned `CloudStorageAccount` object to create a blob client.</span></span>

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="a4fc3-112">Usare quindi il client BLOB per recuperare un riferimento a un contenitore denominato "photoblobs".</span><span class="sxs-lookup"><span data-stu-id="a4fc3-112">Next, use the blob client to retrieve a reference to a container named "photoblobs".</span></span> <span data-ttu-id="a4fc3-113">Come i nomi di account, i nomi dei contenitori BLOB devono essere in minuscolo ed essere composti da lettere e numeri.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-113">Much like the account names, the Blob container names must be lowercase and composed of letters and numbers.</span></span>

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. <span data-ttu-id="a4fc3-114">Usare il metodo `CreateIfNotExistsAsync` per creare il contenitore.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-114">Use the `CreateIfNotExistsAsync` method to create the container.</span></span> <span data-ttu-id="a4fc3-115">Restituisce un valore `bool` che indica se il contenitore è stato creato.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-115">This returns a `bool` indicating whether the container was created.</span></span> <span data-ttu-id="a4fc3-116">Archiviare questo valore in una variabile denominata `created`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-116">Store this in a variable named `created`.</span></span>
    - <span data-ttu-id="a4fc3-117">Si noti che questo è un metodo **asincrono**, il che significa che eseguirà una chiamata di rete effettiva.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-117">Notice that this is an **async** method - that means it will perform an actual network call.</span></span>
    - <span data-ttu-id="a4fc3-118">Sarà necessario usare le parole chiave `await` per ottenere il risultato `bool`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-118">You will need to use the `await` keywords to get the `bool` result.</span></span>

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. <span data-ttu-id="a4fc3-119">Dal momento che si sta usando la parola chiave `await`, proseguire e modificare la firma affinché il metodo `Main` sia `async` e restituisca un valore `Task`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-119">Because we are using the `await` keyword, go ahead and change the signature for the `Main` method to be `async` and return a `Task`.</span></span>

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. <span data-ttu-id="a4fc3-120">Visualizzare infine se il contenitore BLOB è stato creato.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-120">Finally, output whether we created the Blob container.</span></span>

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. <span data-ttu-id="a4fc3-121">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-121">Save the file.</span></span>

<span data-ttu-id="a4fc3-122">Il file finale dovrebbe essere simile al seguente se si vuole verificare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-122">The final file should look like this if you'd like to check your work.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a><span data-ttu-id="a4fc3-123">Usare C# 7.1 per compilare l'app</span><span class="sxs-lookup"><span data-stu-id="a4fc3-123">Use C# 7.1 to build our app</span></span>

<span data-ttu-id="a4fc3-124">Il supporto per `async` e `await` nei metodi `Main` è stato aggiunto a C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-124">Support for `async` and `await` on `Main` methods was added to C# 7.1.</span></span> <span data-ttu-id="a4fc3-125">Ma questa potrebbe non essere la versione predefinita del compilatore in uso.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-125">This might not be the default version of the compiler you are using.</span></span> <span data-ttu-id="a4fc3-126">Verificare di stare usando tale versione del compilatore impostandola nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-126">Let's make sure we use that version of the compiler by setting it in our configuration file.</span></span>

1. <span data-ttu-id="a4fc3-127">Aprire `PhotoSharingApp.csproj` e aggiungere `<LangVersion>7.1</LangVersion>` a `PropertyGroup` che specifica `TargetFramework`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-127">Open the `PhotoSharingApp.csproj` and add `<LangVersion>7.1</LangVersion>` to the `PropertyGroup` that specifies the `TargetFramework`.</span></span> <span data-ttu-id="a4fc3-128">Al termine, il file dovrebbe avere un aspetto analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="a4fc3-128">It should look like this when you're finished:</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion> 
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. <span data-ttu-id="a4fc3-129">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-129">Save the file.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a4fc3-130">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="a4fc3-130">Run the app</span></span>

1. <span data-ttu-id="a4fc3-131">Compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-131">Build and run the application.</span></span> <span data-ttu-id="a4fc3-132">**Nota:** assicurarsi di essere nella directory di lavoro corretta.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-132">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="a4fc3-133">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="a4fc3-133">::: zone-end</span></span>

<span data-ttu-id="a4fc3-134">::: zone-pivot="javascript" Si procederà all'aggiunta di codice per connettersi all'account di archiviazione di Azure tramite la stringa di connessione archiviata.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-134">::: zone-pivot="javascript" Let's add code to connect to the Azure storage account using our stored connection string.</span></span> <span data-ttu-id="a4fc3-135">La libreria client di Azure userà automaticamente la variabile di ambiente **AZURE_STORAGE_CONNECTION_STRING** per ottenere la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-135">The Azure client library will automatically use the **AZURE_STORAGE_CONNECTION_STRING** environment variable to get the connection string.</span></span>

## <a name="create-a-blob-client"></a><span data-ttu-id="a4fc3-136">Creare un client BLOB</span><span class="sxs-lookup"><span data-stu-id="a4fc3-136">Create a blob client</span></span>

1. <span data-ttu-id="a4fc3-137">Aprire **index.js** nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-137">Open **index.js** in the code editor.</span></span>

1. <span data-ttu-id="a4fc3-138">Iniziare includendo il modulo **azure-storage**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-138">Start by including the **azure-storage** module.</span></span> <span data-ttu-id="a4fc3-139">Archiviare il modulo in una variabile denominata **storage**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-139">Store the module in a variable named **storage**.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. <span data-ttu-id="a4fc3-140">Quindi, subito dopo questa operazione, usare l'oggetto **storage** per creare l'oggetto `BlobService` e archiviarlo in un oggetto globale denominato **blobService**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-140">Next, right after that, use the **storage** object to create the `BlobService` object and store it in a global named **blobService**.</span></span> <span data-ttu-id="a4fc3-141">Tenere presente che questi sono oggetti leggeri che rappresentano l'accesso all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-141">Remember, these are light-weight objects representing access to the storage account.</span></span>

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. <span data-ttu-id="a4fc3-142">Aggiungere una costante per rappresentare il contenitore che si vuole creare.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-142">Add a constant to represent the container we want to create.</span></span> <span data-ttu-id="a4fc3-143">Il contenitore sarà denominato "photoblobs".</span><span class="sxs-lookup"><span data-stu-id="a4fc3-143">We'll name the container "photoblobs".</span></span>

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a><span data-ttu-id="a4fc3-144">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="a4fc3-144">Create a container</span></span>

<span data-ttu-id="a4fc3-145">È possibile usare l'oggetto `BlobService` per utilizzare le API BLOB nell'archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-145">We can use the `BlobService` object to work with blob APIs in Azure storage.</span></span> <span data-ttu-id="a4fc3-146">Come accennato in precedenza, tutte le API che effettuano chiamate di rete sono asincrone per mantenere l'app reattiva.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-146">As mentioned before, all the APIs that make network calls are asynchronous to keep the app responsive.</span></span> <span data-ttu-id="a4fc3-147">Il metodo `createContainerIfNotExists` è uno di questi metodi.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-147">The `createContainerIfNotExists` method is one such method.</span></span> <span data-ttu-id="a4fc3-148">Verranno usate _promesse_ per gestire il callback contenente la risposta.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-148">We'll use _promises_ to handle the callback which contains the response.</span></span>

1. <span data-ttu-id="a4fc3-149">Usare `util.promisify` per prendere la versione di callback di `createContainerIfNotExists` e trasformarla in un metodo che restituisce una promessa.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-149">Use `util.promisify` to take the callback version of `createContainerIfNotExists` and turn it into a promise-returning method.</span></span>
    - <span data-ttu-id="a4fc3-150">Poiché il metodo di callback è in un oggetto, assicurarsi di aggiungere una chiamata `bind` alla fine per connetterlo a tale contesto.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-150">Since the callback method is on an object, make sure to add a `bind` call at the end to connect it to that context.</span></span>
    - <span data-ttu-id="a4fc3-151">Assegnare il valore restituito a una costante all'inizio del file denominato `createContainerAsync`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-151">Assign the return value to a constant at the top of the file named `createContainerAsync` as shown below.</span></span>

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. <span data-ttu-id="a4fc3-152">Rimuovere l'output "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="a4fc3-152">Remove the "Hello, World!"</span></span> <span data-ttu-id="a4fc3-153">da `main()`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-153">output from `main()`.</span></span>

1. <span data-ttu-id="a4fc3-154">Chiamare la nuova promessa `createContainerAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-154">Call your new `createContainerAsync` promise.</span></span>
    - <span data-ttu-id="a4fc3-155">Passare la costante **containerName**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-155">Pass it the **containerName** constant.</span></span>
    - <span data-ttu-id="a4fc3-156">Applicare la parola chiave `await` alla chiamata.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-156">Apply the `await` keyword to the call.</span></span>
    - <span data-ttu-id="a4fc3-157">Eseguire il wrapping della chiamata in un costrutto `try` / `catch` e restituire eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-157">Wrap the call in a `try` / `catch` construct and output any error.</span></span>

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. <span data-ttu-id="a4fc3-158">La promessa `createContainerAsync` restituisce il primo valore dal valore `createContainerIfNotExists` sottostante che è il risultato del contenitore.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-158">The `createContainerAsync` promise returns the first value from the underlying `createContainerIfNotExists` which is the container result.</span></span> <span data-ttu-id="a4fc3-159">Sono incluse informazioni sul fatto che il contenitore sia stato creato o meno.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-159">This includes information on whether the container was created or not.</span></span> <span data-ttu-id="a4fc3-160">Acquisire il risultato in una variabile e restituire se il contenitore è stato creato in base alla proprietà `result.created`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-160">Capture the result in a variable and output whether the container was created based on the `result.created` property.</span></span>

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. <span data-ttu-id="a4fc3-161">Decorare infine la funzione `main` con la parola chiave `async`.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-161">Finally, decorate the `main` function with the `async` keyword.</span></span>
        
1. <span data-ttu-id="a4fc3-162">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-162">Save the file.</span></span>

<span data-ttu-id="a4fc3-163">Il file finale dovrebbe essere simile al seguente se si vuole verificare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-163">The final file should look like this if you'd like to check your work.</span></span>

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function run() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

run();
```

## <a name="run-the-app"></a><span data-ttu-id="a4fc3-164">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="a4fc3-164">Run the app</span></span>

1. <span data-ttu-id="a4fc3-165">Compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-165">Build and run the application.</span></span> <span data-ttu-id="a4fc3-166">**Nota:** assicurarsi di essere nella directory di lavoro corretta.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-166">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="a4fc3-167">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="a4fc3-167">::: zone-end</span></span>

<span data-ttu-id="a4fc3-168">Dovrebbe segnalare che il contenitore BLOB è stato creato.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-168">It should report that the blob container was created.</span></span> <span data-ttu-id="a4fc3-169">Se la si esegue una seconda volta, dovrebbe indicare che esiste già.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-169">If you run it a second time, it should tell you it already exists.</span></span>

<span data-ttu-id="a4fc3-170">Per verificare il contenitore:</span><span class="sxs-lookup"><span data-stu-id="a4fc3-170">To verify the container:</span></span>

1. <span data-ttu-id="a4fc3-171">Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="a4fc3-171">Sign in to the [Azure Portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="a4fc3-172">Passare all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-172">Navigate to your storage account.</span></span> <span data-ttu-id="a4fc3-173">È possibile usare la sezione **Tutte le risorse** per trovare l'account di archiviazione oppure eseguire una ricerca per nome nella _casella di ricerca_ nella parte superiore della finestra del portale.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-173">You can use the **All Resources** section to find the storage account, or search by name from the _search box_ at the top of the portal window.</span></span> 

1. <span data-ttu-id="a4fc3-174">Selezionare la voce **BLOB** dell'account di archiviazione nella sezione **Servizi BLOB**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-174">Select the **Blobs** entry of the storage account in the **Blob services** section.</span></span>

1. <span data-ttu-id="a4fc3-175">Nel pannello BLOB dovrebbe essere visualizzato il contenitore **photoblobs**.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-175">You should see your **photoblobs** container in the Blobs panel.</span></span> <span data-ttu-id="a4fc3-176">È possibile eliminare il contenitore usando il menu "..." a destra della voce per provare a crearne uno nuovo con l'app.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-176">You can delete the container through the "..." menu on the right hand side of the entry to try re-creating it with your app.</span></span>

> [!NOTE]
> <span data-ttu-id="a4fc3-177">Il contenitore scomparirà dal portale molto rapidamente, ma sono necessari alcuni minuti per eliminarlo effettivamente.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-177">The container will disappear from the portal very quickly, but it takes a few minutes to actually delete.</span></span> <span data-ttu-id="a4fc3-178">Si otterrà una risposta di errore da Azure mentre è in corso l'eliminazione del contenitore se si proverà a ricrearlo.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-178">You will get an error response from Azure while it is being deleted if you attempt to recreate it.</span></span>

## <a name="delete-the-app"></a><span data-ttu-id="a4fc3-179">Eliminare l'app</span><span class="sxs-lookup"><span data-stu-id="a4fc3-179">Delete the app</span></span>
<span data-ttu-id="a4fc3-180">Se si decide che non si vuole mantenere il codice sorgente dell'applicazione nell'ambiente Cloud Shell, è possibile usare il comando seguente per rimuovere la cartella e tutto il contenuto.</span><span class="sxs-lookup"><span data-stu-id="a4fc3-180">If you decide you don't want to keep the application source code in your Cloud Shell environment, you can use the following command to remove the folder and all the contents.</span></span>

```bash
rm -r PhotoSharingApp/
```
