<span data-ttu-id="aa629-101">::: zone pivot="csharp" Si procederà all'aggiunta di codice per recuperare la stringa di connessione dalla configurazione, per poi usarla per connettersi all'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa629-101">::: zone pivot="csharp" Let's add code to retrieve the connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="aa629-102">Recuperare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="aa629-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="aa629-103">Aprire **Program.cs** nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="aa629-103">Open **Program.cs** in the code editor.</span></span>

1. <span data-ttu-id="aa629-104">Aggiungere un'istruzione `using` all'inizio del file per fare riferimento allo spazio dei nomi `Microsoft.WindowsAzure.Storage`:</span><span class="sxs-lookup"><span data-stu-id="aa629-104">Add a `using` statement at the top of the file to reference the `Microsoft.WindowsAzure.Storage` namespace:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="aa629-105">Alla fine del metodo `Main` aggiungere la riga seguente per recuperare la stringa di connessione dell'account di archiviazione di Azure dal file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="aa629-105">At the end of the `Main` method, add the following line to retrieve the Azure storage account connection string from the configuration file.</span></span> <span data-ttu-id="aa629-106">Il valore _key_ passato deve corrispondere al nome usato nel file **appsettings.json**.</span><span class="sxs-lookup"><span data-stu-id="aa629-106">The passed _key_ must match the name used in your **appsettings.json** file.</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="aa629-107">Creare un client BLOB</span><span class="sxs-lookup"><span data-stu-id="aa629-107">Create a blob client</span></span>

1. <span data-ttu-id="aa629-108">Usare il metodo `CloudStorageAccount.TryParse` statico per creare un oggetto `CloudStorageAccount`; accetta la stringa di connessione e un parametro `out` per restituire l'oggetto creato.</span><span class="sxs-lookup"><span data-stu-id="aa629-108">Use the static `CloudStorageAccount.TryParse` method to create a `CloudStorageAccount` object - it takes the connection string and an `out` parameter to return the created object.</span></span> <span data-ttu-id="aa629-109">Restituisce un valore `bool` che indica se la stringa è stata analizzata correttamente.</span><span class="sxs-lookup"><span data-stu-id="aa629-109">It returns a `bool` value indicating whether it successfully parsed the string.</span></span>
    - <span data-ttu-id="aa629-110">In caso di errore, visualizzare un messaggio per la console e restituire un valore dal metodo.</span><span class="sxs-lookup"><span data-stu-id="aa629-110">If it fails, output a message to the console and return from the method.</span></span>

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString,
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. <span data-ttu-id="aa629-111">Usare l'oggetto `CloudStorageAccount` restituito per creare un client BLOB.</span><span class="sxs-lookup"><span data-stu-id="aa629-111">Use the returned `CloudStorageAccount` object to create a blob client.</span></span>

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="aa629-112">Usare quindi il client BLOB per recuperare un riferimento a un contenitore denominato "photoblobs".</span><span class="sxs-lookup"><span data-stu-id="aa629-112">Next, use the blob client to retrieve a reference to a container named "photoblobs".</span></span> <span data-ttu-id="aa629-113">Come i nomi di account, i nomi dei contenitori BLOB devono essere in minuscolo ed essere composti da lettere e numeri.</span><span class="sxs-lookup"><span data-stu-id="aa629-113">Much like the account names, the Blob container names must be lowercase and composed of letters and numbers.</span></span>

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. <span data-ttu-id="aa629-114">Usare il metodo `CreateIfNotExistsAsync` per creare il contenitore.</span><span class="sxs-lookup"><span data-stu-id="aa629-114">Use the `CreateIfNotExistsAsync` method to create the container.</span></span> <span data-ttu-id="aa629-115">Restituisce un valore `bool` che indica se il contenitore è stato creato.</span><span class="sxs-lookup"><span data-stu-id="aa629-115">This returns a `bool` indicating whether the container was created.</span></span> <span data-ttu-id="aa629-116">Archiviare questo valore in una variabile denominata `created`.</span><span class="sxs-lookup"><span data-stu-id="aa629-116">Store this in a variable named `created`.</span></span>
    - <span data-ttu-id="aa629-117">Si noti che questo è un metodo **asincrono**, il che significa che eseguirà una chiamata di rete effettiva.</span><span class="sxs-lookup"><span data-stu-id="aa629-117">Notice that this is an **async** method - that means it will perform an actual network call.</span></span>
    - <span data-ttu-id="aa629-118">Sarà necessario usare le parole chiave `await` per ottenere il risultato `bool`.</span><span class="sxs-lookup"><span data-stu-id="aa629-118">You will need to use the `await` keywords to get the `bool` result.</span></span>

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. <span data-ttu-id="aa629-119">Dal momento che si sta usando la parola chiave `await`, modificare la firma in modo che il metodo `Main` sia `async` e restituisca un valore `Task`.</span><span class="sxs-lookup"><span data-stu-id="aa629-119">Because we are using the `await` keyword, go ahead and change the signature for the `Main` method to be `async` and return a `Task`.</span></span>

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

    <span data-ttu-id="aa629-120">È anche necessario aggiungere una nuova istruzione `using` all'inizio:</span><span class="sxs-lookup"><span data-stu-id="aa629-120">You'll also need to add a new `using` statement at the top:</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="aa629-121">Visualizzare infine se il contenitore BLOB è stato creato.</span><span class="sxs-lookup"><span data-stu-id="aa629-121">Finally, output whether we created the Blob container.</span></span>

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. <span data-ttu-id="aa629-122">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="aa629-122">Save the file.</span></span>

<span data-ttu-id="aa629-123">Il file finale dovrebbe essere simile al seguente se si vuole verificare il lavoro.</span><span class="sxs-lookup"><span data-stu-id="aa629-123">The final file should look like this if you'd like to check your work.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using Microsoft.WindowsAzure.Storage;
using System.Threading.Tasks;

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

## <a name="use-c-71-to-build-our-app"></a><span data-ttu-id="aa629-124">Usare C# 7.1 per compilare l'app</span><span class="sxs-lookup"><span data-stu-id="aa629-124">Use C# 7.1 to build our app</span></span>

<span data-ttu-id="aa629-125">Il supporto per `async` e `await` nei metodi `Main` è stato aggiunto a C# 7.1.</span><span class="sxs-lookup"><span data-stu-id="aa629-125">Support for `async` and `await` on `Main` methods was added to C# 7.1.</span></span> <span data-ttu-id="aa629-126">Ma questa potrebbe non essere la versione predefinita del compilatore in uso.</span><span class="sxs-lookup"><span data-stu-id="aa629-126">This might not be the default version of the compiler you are using.</span></span> <span data-ttu-id="aa629-127">Verificare di stare usando tale versione del compilatore impostandola nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="aa629-127">Let's make sure we use that version of the compiler by setting it in our configuration file.</span></span>

1. <span data-ttu-id="aa629-128">Aprire `PhotoSharingApp.csproj` e aggiungere `<LangVersion>7.1</LangVersion>` a `PropertyGroup` che specifica `TargetFramework`.</span><span class="sxs-lookup"><span data-stu-id="aa629-128">Open the `PhotoSharingApp.csproj` and add `<LangVersion>7.1</LangVersion>` to the `PropertyGroup` that specifies the `TargetFramework`.</span></span> <span data-ttu-id="aa629-129">Al termine, il file dovrebbe avere un aspetto analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="aa629-129">It should look like this when you're finished:</span></span>

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

1. <span data-ttu-id="aa629-130">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="aa629-130">Save the file.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="aa629-131">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="aa629-131">Run the app</span></span>

1. <span data-ttu-id="aa629-132">Compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa629-132">Build and run the application.</span></span> <span data-ttu-id="aa629-133">**Nota:** assicurarsi di essere nella directory di lavoro corretta.</span><span class="sxs-lookup"><span data-stu-id="aa629-133">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="aa629-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="aa629-134">::: zone-end</span></span>

<span data-ttu-id="aa629-135">::: zone pivot="javascript" Si procederà ora all'aggiunta di codice per la connessione all'account di archiviazione di Azure tramite la stringa di connessione archiviata.</span><span class="sxs-lookup"><span data-stu-id="aa629-135">::: zone pivot="javascript" Let's add code to connect to the Azure storage account using our stored connection string.</span></span> <span data-ttu-id="aa629-136">Per ottenere la stringa di connessione, la libreria client di Azure usa automaticamente la variabile di ambiente **AZURE_STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="aa629-136">The Azure client library will automatically use the **AZURE_STORAGE_CONNECTION_STRING** environment variable to get the connection string.</span></span>

## <a name="create-a-blob-client"></a><span data-ttu-id="aa629-137">Creare un client BLOB</span><span class="sxs-lookup"><span data-stu-id="aa629-137">Create a blob client</span></span>

1. <span data-ttu-id="aa629-138">Aprire **index.js** nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="aa629-138">Open **index.js** in the code editor.</span></span>

1. <span data-ttu-id="aa629-139">Iniziare includendo il modulo **azure-storage**.</span><span class="sxs-lookup"><span data-stu-id="aa629-139">Start by including the **azure-storage** module.</span></span> <span data-ttu-id="aa629-140">Archiviare il modulo in una variabile denominata **storage**.</span><span class="sxs-lookup"><span data-stu-id="aa629-140">Store the module in a variable named **storage**.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    const storage = require('azure-storage');
    ```

1. <span data-ttu-id="aa629-141">Subito dopo questa operazione, usare l'oggetto **storage** per creare l'oggetto `BlobService` e archiviarlo in un oggetto globale denominato **blobService**.</span><span class="sxs-lookup"><span data-stu-id="aa629-141">Next, right after that, use the **storage** object to create the `BlobService` object and store it in a global named **blobService**.</span></span> <span data-ttu-id="aa629-142">Tenere presente che questi sono oggetti leggeri che rappresentano l'accesso all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="aa629-142">Remember, these are lightweight objects representing access to the storage account.</span></span>

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. <span data-ttu-id="aa629-143">Aggiungere una costante per rappresentare il contenitore che si vuole creare.</span><span class="sxs-lookup"><span data-stu-id="aa629-143">Add a constant to represent the container we want to create.</span></span> <span data-ttu-id="aa629-144">Il contenitore sarà denominato "photoblobs".</span><span class="sxs-lookup"><span data-stu-id="aa629-144">We'll name the container "photoblobs".</span></span>

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a><span data-ttu-id="aa629-145">Creare un contenitore</span><span class="sxs-lookup"><span data-stu-id="aa629-145">Create a container</span></span>

<span data-ttu-id="aa629-146">È possibile usare l'oggetto `BlobService` per utilizzare le API BLOB nell'archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="aa629-146">We can use the `BlobService` object to work with blob APIs in Azure storage.</span></span> <span data-ttu-id="aa629-147">Come accennato in precedenza, tutte le API che effettuano chiamate di rete sono asincrone per mantenere l'app reattiva.</span><span class="sxs-lookup"><span data-stu-id="aa629-147">As mentioned before, all the APIs that make network calls are asynchronous to keep the app responsive.</span></span> <span data-ttu-id="aa629-148">Il metodo `createContainerIfNotExists` è un metodo di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="aa629-148">The `createContainerIfNotExists` method is one such method.</span></span> <span data-ttu-id="aa629-149">Per gestire il callback contenente la risposta, verranno usate _promesse_.</span><span class="sxs-lookup"><span data-stu-id="aa629-149">We'll use _promises_ to handle the callback that contains the response.</span></span>

1. <span data-ttu-id="aa629-150">Usare `util.promisify` per ottenere la versione di callback di `createContainerIfNotExists` e trasformarla in un metodo che restituisce una promessa.</span><span class="sxs-lookup"><span data-stu-id="aa629-150">Use `util.promisify` to take the callback version of `createContainerIfNotExists` and turn it into a promise-returning method.</span></span>
    - <span data-ttu-id="aa629-151">Poiché il metodo di callback agisce su un oggetto, assicurarsi di aggiungere una chiamata a `bind` alla fine per connetterlo al contesto.</span><span class="sxs-lookup"><span data-stu-id="aa629-151">Since the callback method is on an object, make sure to add a `bind` call at the end to connect it to that context.</span></span>
    - <span data-ttu-id="aa629-152">Assegnare il valore restituito a una costante all'inizio del file denominato `createContainerAsync`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="aa629-152">Assign the return value to a constant at the top of the file named `createContainerAsync`, as shown below.</span></span>
    - <span data-ttu-id="aa629-153">È anche necessario usare `require` per il modulo `util` nella parte iniziale del file.</span><span class="sxs-lookup"><span data-stu-id="aa629-153">You'll also need a `require` for the `util` module near the top of the file.</span></span>

    ```javascript
    const util = require('util');
    const storage = require('azure-storage');
    const blobService = storage.createBlobService();

    const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

    const containerName = 'photoblobs';
    ```

2. <span data-ttu-id="aa629-154">Rimuovere l'output "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="aa629-154">Remove the "Hello, World!"</span></span> <span data-ttu-id="aa629-155">da `main()`.</span><span class="sxs-lookup"><span data-stu-id="aa629-155">output from `main()`.</span></span>

3. <span data-ttu-id="aa629-156">Chiamare la nuova promessa `createContainerAsync`.</span><span class="sxs-lookup"><span data-stu-id="aa629-156">Call your new `createContainerAsync` promise.</span></span>
    - <span data-ttu-id="aa629-157">Passare la costante **containerName**.</span><span class="sxs-lookup"><span data-stu-id="aa629-157">Pass it the **containerName** constant.</span></span>
    - <span data-ttu-id="aa629-158">Applicare la parola chiave `await` alla chiamata.</span><span class="sxs-lookup"><span data-stu-id="aa629-158">Apply the `await` keyword to the call.</span></span>
    - <span data-ttu-id="aa629-159">Eseguire il wrapping della chiamata in un costrutto `try` / `catch` e restituire eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="aa629-159">Wrap the call in a `try` / `catch` construct and output any error.</span></span>

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. <span data-ttu-id="aa629-160">La promessa `createContainerAsync` restituisce il primo valore dalla funzione `createContainerIfNotExists` sottostante. Tale valore è il risultato corrispondente al contenitore.</span><span class="sxs-lookup"><span data-stu-id="aa629-160">The `createContainerAsync` promise returns the first value from the underlying `createContainerIfNotExists`, which is the container result.</span></span> <span data-ttu-id="aa629-161">Sono incluse informazioni sul fatto che il contenitore sia stato creato o meno.</span><span class="sxs-lookup"><span data-stu-id="aa629-161">This includes information on whether the container was created or not.</span></span> <span data-ttu-id="aa629-162">Acquisire il risultato in una variabile e restituire l'informazione relativa alla creazione o meno del contenitore in base alla proprietà `result.created`.</span><span class="sxs-lookup"><span data-stu-id="aa629-162">Capture the result in a variable, and output whether the container was created based on the `result.created` property.</span></span>

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

1. <span data-ttu-id="aa629-163">Decorare infine la funzione `main` con la parola chiave `async`.</span><span class="sxs-lookup"><span data-stu-id="aa629-163">Finally, decorate the `main` function with the `async` keyword.</span></span>

1. <span data-ttu-id="aa629-164">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="aa629-164">Save the file.</span></span>

<span data-ttu-id="aa629-165">Per verificare il lavoro, controllare che il file finale abbia l'aspetto seguente.</span><span class="sxs-lookup"><span data-stu-id="aa629-165">The final file should look like this, if you'd like to check your work.</span></span>

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function main() {
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

main();
```

## <a name="run-the-app"></a><span data-ttu-id="aa629-166">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="aa629-166">Run the app</span></span>

1. <span data-ttu-id="aa629-167">Compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aa629-167">Build and run the application.</span></span> <span data-ttu-id="aa629-168">**Nota:** assicurarsi di essere nella directory PhotoSharingApp.</span><span class="sxs-lookup"><span data-stu-id="aa629-168">**Note:** make sure you're in the PhotoSharingApp directory.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="aa629-169">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="aa629-169">::: zone-end</span></span>

<span data-ttu-id="aa629-170">Dovrebbe segnalare che il contenitore BLOB è stato creato.</span><span class="sxs-lookup"><span data-stu-id="aa629-170">It should report that the blob container was created.</span></span> <span data-ttu-id="aa629-171">Se la si esegue una seconda volta, dovrebbe indicare che esiste già.</span><span class="sxs-lookup"><span data-stu-id="aa629-171">If you run it a second time, it should tell you it already exists.</span></span>

<span data-ttu-id="aa629-172">Per verificare il contenitore:</span><span class="sxs-lookup"><span data-stu-id="aa629-172">To verify the container:</span></span>

1. <span data-ttu-id="aa629-173">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="aa629-173">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="aa629-174">Passare all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="aa629-174">Navigate to your storage account.</span></span> <span data-ttu-id="aa629-175">Per trovare l'account di archiviazione, è possibile usare la sezione **Tutte le risorse** oppure eseguire una ricerca in base al nome nella _casella di ricerca_ nella parte superiore della finestra del portale.</span><span class="sxs-lookup"><span data-stu-id="aa629-175">You can use the **All Resources** section to find the storage account or search by name from the _search box_ at the top of the portal window.</span></span>

1. <span data-ttu-id="aa629-176">Selezionare la voce **BLOB** dell'account di archiviazione nella sezione **Servizi BLOB**.</span><span class="sxs-lookup"><span data-stu-id="aa629-176">Select the **Blobs** entry of the storage account in the **Blob services** section.</span></span>

1. <span data-ttu-id="aa629-177">Nel pannello BLOB dovrebbe essere visualizzato il contenitore **photoblobs**.</span><span class="sxs-lookup"><span data-stu-id="aa629-177">You should see your **photoblobs** container in the Blobs panel.</span></span> <span data-ttu-id="aa629-178">È possibile eliminare il contenitore usando il menu "..." a destra della voce e provare a crearne uno nuovo con l'app.</span><span class="sxs-lookup"><span data-stu-id="aa629-178">You can delete the container through the "..." menu on the right-hand side of the entry to try recreating it with your app.</span></span>

> [!NOTE]
> <span data-ttu-id="aa629-179">Il contenitore scomparirà dal portale molto rapidamente, ma sono necessari alcuni minuti per eliminarlo effettivamente.</span><span class="sxs-lookup"><span data-stu-id="aa629-179">The container will disappear from the portal very quickly, but it takes a few minutes to actually delete.</span></span> <span data-ttu-id="aa629-180">Si otterrà una risposta di errore da Azure mentre è in corso l'eliminazione del contenitore se si proverà a ricrearlo.</span><span class="sxs-lookup"><span data-stu-id="aa629-180">You will get an error response from Azure while it is being deleted if you attempt to recreate it.</span></span>

## <a name="delete-the-app"></a><span data-ttu-id="aa629-181">Eliminare l'app</span><span class="sxs-lookup"><span data-stu-id="aa629-181">Delete the app</span></span>
<span data-ttu-id="aa629-182">Se si decide che non si vuole mantenere il codice sorgente dell'applicazione nell'ambiente Cloud Shell, è possibile usare il comando seguente per rimuovere la cartella e tutto il contenuto.</span><span class="sxs-lookup"><span data-stu-id="aa629-182">If you decide you don't want to keep the application source code in your Cloud Shell environment, you can use the following command to remove the folder and all the contents.</span></span>

```bash
rm -r PhotoSharingApp/
```
