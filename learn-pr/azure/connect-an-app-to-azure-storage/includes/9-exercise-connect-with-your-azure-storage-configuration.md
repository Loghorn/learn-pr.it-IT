In questa unità si caricherà la stringa di connessione dell'account di archiviazione di Azure dalla configurazione, per poi usarla per connettersi all'account di archiviazione di Azure.

## <a name="retrieve-the-connection-string"></a>Recuperare la stringa di connessione

1. Verificare che l'applicazione console creata nell'unità precedente venga caricata in Visual Studio.

1. Nel file **Program.cs** aggiungere un'istruzione *using* all'inizio del file per fare riferimento alla libreria **Microsoft.WindowsAzure.Storage**:

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. Nella parte inferiore del metodo **Main** aggiungere la riga seguente per recuperare la stringa di connessione dell'account di archiviazione di Azure dal file di configurazione:

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Creare un client BLOB

1. Inserire il codice seguente nella parte inferiore del metodo **Main** per analizzare la stringa di connessione e creare un client BLOB:

    ```csharp
    CloudStorageAccount storageAccount;
    if (!CloudStorageAccount.TryParse(connectionString,out storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Inserire il codice seguente per creare un client BLOB e recuperare un riferimento a un contenitore, creandolo (operazione facoltativa) se non è presente nella parte inferiore del metodo **Main**:

    ```csharp
    var containerName = "MyBlobContainer";
    var success = blobClient
        .GetContainerReference(containerName)
        .CreateIfNotExistsAsync()
        .Result;
    ```

  *Notare l'uso del metodo **async** **CreateIfNotExistsAsync()** e della proprietà **Result** corrispondente. In una normale applicazione verrebbe in genere usata la parola chiave **await**. Tuttavia, poiché questa applicazione è un'applicazione console e non asincrona, è necessario chiamare la proprietà **Result** per ottenere i risultati dell'attività asincrona creata dal metodo **CreateIfNotExistsAsync()**.*

## <a name="print-a-status-message"></a>Stampare un messaggio di stato

1. Aggiungere il codice seguente nella parte inferiore del metodo **Main** per stampare un messaggio di operazione riuscita o non riuscita.

    ```csharp
    if (!success)
    {
        Console.WriteLine("Error: Could not connect to Azure Storage container");
        return;
    }
    Console.WriteLine("Successfully connected to Azure Storage container");
    ```
1. Infine, eseguire l'applicazione per verificare che la connessione sia riuscita e visualizzare il portale di Azure per assicurarsi che il contenitore di archiviazione di Azure sia stato creato se non esisteva in precedenza.

1. **Facoltativo**: se non si prevede di usare il contenitore, eliminare il gruppo di risorse in cui si trova per evitare addebiti per le risorse inutilizzate.
