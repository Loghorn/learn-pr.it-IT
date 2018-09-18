::: zone pivot="csharp" Si procederà all'aggiunta di codice per recuperare la stringa di connessione dalla configurazione, per poi usarla per connettersi all'account di archiviazione di Azure.

## <a name="retrieve-the-connection-string"></a>Recuperare la stringa di connessione

1. Selezionare **Program.cs** per aprirlo nell'editor di codice.

1. Aggiungere un'istruzione `using` all'inizio del file per fare riferimento allo spazio dei nomi `Microsoft.WindowsAzure.Storage`:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. Alla fine del metodo `Main` aggiungere la riga seguente per recuperare la stringa di connessione dell'account di archiviazione di Azure dal file di configurazione. Il valore _key_ passato deve corrispondere al nome usato nel file **appsettings.json**.

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Creare un client BLOB

1. Usare il metodo `CloudStorageAccount.TryParse` statico per creare un oggetto `CloudStorageAccount`; accetta la stringa di connessione e un parametro `out` per restituire l'oggetto creato. Restituisce un valore `bool` che indica se la stringa è stata analizzata correttamente.
    - In caso di errore, visualizzare un messaggio per la console e restituire un valore dal metodo.

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString, 
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. Usare l'oggetto `CloudStorageAccount` restituito per creare un client BLOB.

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Usare quindi il client BLOB per recuperare un riferimento a un contenitore denominato "photoblobs". Come i nomi di account, i nomi dei contenitori BLOB devono essere in minuscolo ed essere composti da lettere e numeri.

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. Usare il metodo `CreateIfNotExistsAsync` per creare il contenitore. Restituisce un valore `bool` che indica se il contenitore è stato creato. Archiviare questo valore in una variabile denominata `created`.
    - Si noti che questo è un metodo **asincrono**, il che significa che eseguirà una chiamata di rete effettiva.
    - Sarà necessario usare le parole chiave `await` per ottenere il risultato `bool`.

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. Dal momento che si sta usando la parola chiave `await`, proseguire e modificare la firma affinché il metodo `Main` sia `async` e restituisca un valore `Task`.

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

1. Visualizzare infine se il contenitore BLOB è stato creato.

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. Salvare il file.

Il file finale dovrebbe essere simile al seguente se si vuole verificare il lavoro.

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

## <a name="use-c-71-to-build-our-app"></a>Usare C# 7.1 per compilare l'app

Il supporto per `async` e `await` nei metodi `Main` è stato aggiunto a C# 7.1. Ma questa potrebbe non essere la versione predefinita del compilatore in uso. Verificare di stare usando tale versione del compilatore impostandola nel file di configurazione.

1. Aprire `PhotoSharingApp.csproj` e aggiungere `<LangVersion>7.1</LangVersion>` a `PropertyGroup` che specifica `TargetFramework`. Al termine, il file dovrebbe avere un aspetto analogo al seguente:

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

1. Salvare il file.

## <a name="run-the-app"></a>Eseguire l'app

1. Compilare ed eseguire l'applicazione. **Nota:** assicurarsi di essere nella directory di lavoro corretta.

    ```bash
    dotnet run
    ```

::: zone-end

::: zone-pivot="javascript" Si procederà all'aggiunta di codice per connettersi all'account di archiviazione di Azure tramite la stringa di connessione archiviata. La libreria client di Azure userà automaticamente la variabile di ambiente **AZURE_STORAGE_CONNECTION_STRING** per ottenere la stringa di connessione.

## <a name="create-a-blob-client"></a>Creare un client BLOB

1. Aprire **index.js** nell'editor di codice.

1. Iniziare includendo il modulo **azure-storage**. Archiviare il modulo in una variabile denominata **storage**.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();
    
    const storage = require('azure-storage');
    ```

1. Quindi, subito dopo questa operazione, usare l'oggetto **storage** per creare l'oggetto `BlobService` e archiviarlo in un oggetto globale denominato **blobService**. Tenere presente che questi sono oggetti leggeri che rappresentano l'accesso all'account di archiviazione.

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. Aggiungere una costante per rappresentare il contenitore che si vuole creare. Il contenitore sarà denominato "photoblobs".

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a>Creare un contenitore

È possibile usare l'oggetto `BlobService` per utilizzare le API BLOB nell'archiviazione di Azure. Come accennato in precedenza, tutte le API che effettuano chiamate di rete sono asincrone per mantenere l'app reattiva. Il metodo `createContainerIfNotExists` è uno di questi metodi. Verranno usate _promesse_ per gestire il callback contenente la risposta.

1. Usare `util.promisify` per prendere la versione di callback di `createContainerIfNotExists` e trasformarla in un metodo che restituisce una promessa.
    - Poiché il metodo di callback è in un oggetto, assicurarsi di aggiungere una chiamata `bind` alla fine per connetterlo a tale contesto.
    - Assegnare il valore restituito a una costante all'inizio del file denominato `createContainerAsync`, come illustrato di seguito.

```javascript
const storage = require('azure-storage');
const blobService = storage.createBlobService();

const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';
```

1. Rimuovere l'output "Hello World!" da `main()`.

1. Chiamare la nuova promessa `createContainerAsync`.
    - Passare la costante **containerName**.
    - Applicare la parola chiave `await` alla chiamata.
    - Eseguire il wrapping della chiamata in un costrutto `try` / `catch` e restituire eventuali errori.

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```
    
1. La promessa `createContainerAsync` restituisce il primo valore dal valore `createContainerIfNotExists` sottostante che è il risultato del contenitore. Sono incluse informazioni sul fatto che il contenitore sia stato creato o meno. Acquisire il risultato in una variabile e restituire se il contenitore è stato creato in base alla proprietà `result.created`.

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

1. Decorare infine la funzione `main` con la parola chiave `async`.
        
1. Salvare il file.

Il file finale dovrebbe essere simile al seguente se si vuole verificare il lavoro.

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

## <a name="run-the-app"></a>Eseguire l'app

1. Compilare ed eseguire l'applicazione. **Nota:** assicurarsi di essere nella directory di lavoro corretta.

    ```bash
    node index.js
    ```

::: zone-end

Dovrebbe segnalare che il contenitore BLOB è stato creato. Se la si esegue una seconda volta, dovrebbe indicare che esiste già.

Per verificare il contenitore:

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Passare all'account di archiviazione. È possibile usare la sezione **Tutte le risorse** per trovare l'account di archiviazione oppure eseguire una ricerca per nome nella _casella di ricerca_ nella parte superiore della finestra del portale. 

1. Selezionare la voce **BLOB** dell'account di archiviazione nella sezione **Servizi BLOB**.

1. Nel pannello BLOB dovrebbe essere visualizzato il contenitore **photoblobs**. È possibile eliminare il contenitore usando il menu "..." a destra della voce per provare a crearne uno nuovo con l'app.

> [!NOTE]
> Il contenitore scomparirà dal portale molto rapidamente, ma sono necessari alcuni minuti per eliminarlo effettivamente. Si otterrà una risposta di errore da Azure mentre è in corso l'eliminazione del contenitore se si proverà a ricrearlo.

## <a name="delete-the-app"></a>Eliminare l'app
Se si decide che non si vuole mantenere il codice sorgente dell'applicazione nell'ambiente Cloud Shell, è possibile usare il comando seguente per rimuovere la cartella e tutto il contenuto.

```bash
rm -r PhotoSharingApp/
```
