Per usare un singolo BLOB in Azure Storage SDK per .NET Core è necessario un *riferimento a BLOB*, ossia un'istanza di un oggetto `ICloudBlob`.

È possibile ottenere un `ICloudBlob` richiedendolo con il nome del BLOB oppure selezionandolo in un elenco dei BLOB nel contenitore. In entrambi i casi è necessario un oggetto `CloudBlobContainer`, il cui recupero è stato illustrato nell'unità precedente.

## <a name="getting-blobs-by-name"></a>Recupero di BLOB in base al nome

Chiamare uno dei metodi `GetXXXReference` in un `CloudBlobContainer` per ottenere un `ICloudBlob` in base al nome. Se si conosce il tipo di BLOB da recuperare, è preferibile usare un metodo più specifico, ossia `GetBlockBlobReference`, `GetAppendBlobReference` o `GetPageBlobReference`.

Nessuno di questi metodi effettua una chiamata di rete o verifica se il BLOB effettivamente esista. Un metodo separato, `GetBlobReferenceFromServerAsync`, chiama l'API di archiviazione BLOB e genera un'eccezione se il BLOB non esiste.

## <a name="listing-blobs-in-a-container"></a>Elenco dei BLOB in un contenitore

È possibile ottenere un elenco dei BLOB in un contenitore usando il metodo `ListBlobsSegmentedAsync` di `CloudBlobContainer`. *Segmented* fa riferimento alle pagine di risultati separate restituite. Non è mai garantito che una singola chiamata a `ListBlobsSegmentedAsync` restituisca tutti i risultati in una singola pagina. Per scorrere le pagine potrebbe essere necessario ripetere la chiamata con il valore di `ContinuationToken` restituito. Il codice per elencare i BLOB diventa così un po' più complesso rispetto al codice per il caricamento o il download, ma è disponibile un modello standard che può essere usato per ottenere ogni BLOB in un contenitore:

```csharp
BlobContinuationToken continuationToken = null;
BlobResultSegment resultSegment = null; 

do
{
    resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

    // Do work here on resultSegment.Results

    continuationToken = resultSegment.ContinuationToken;
} while (continuationToken != null);
```

In questo modo, `ListBlobsSegmentedAsync` verrà chiamato ripetutamente finché il valore di `continuationToken` non è `null`, che indica la fine dei risultati.

### <a name="processing-list-results"></a>Elaborazione dei risultati elencati

L'oggetto ottenuto da `ListBlobsSegmentedAsync` contiene una proprietà `Results` di tipo `IEnumerable<IListBlobItem>`. Gli oggetti `IListBlobItem` contengono alcune proprietà relative al contenitore e all'URL del BLOB, ma nessun metodo di caricamento o download. Ciò è dovuto al fatto che alcuni oggetti dei risultati potrebbero essere `CloudBlobDirectory` che rappresentano directory virtuali anziché singoli BLOB.

Se si è interessati solo ai singoli BLOB, si può usare il metodo `OfType<>` per filtrare i risultati. Di seguito sono disponibili alcuni esempi:

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

L'uso di `OfType<>` richiederà un riferimento allo spazio dei nomi `System.Linq` (`using System.Linq;`).

## <a name="exercise"></a>Esercizio

Una delle funzionalità dell'app richiede il recupero di un elenco di BLOB dall'API. Si userà il modello illustrato sopra per elencare tutti i BLOB nel contenitore. Elaborando l'elenco, si ottiene il nome di ogni BLOB.

Aprire `BlobStorage.cs` nell'editor e inserire il codice seguente in `GetNames`:

```csharp
public async Task<IEnumerable<string>> GetNames()
{
    List<string> names = new List<string>();

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);

    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    do
    {
        resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

        // Get the name of each blob.
        names.AddRange(resultSegment.Results.OfType<ICloudBlob>().Select(b => b.Name));

        continuationToken = resultSegment.ContinuationToken;
    } while (continuationToken != null);

    return names;
}
```

I nomi restituiti da questo metodo vengono elaborati da `FilesController` per la trasformazione in URL. Quando vengono restituiti al client, ne viene eseguito il rendering come collegamenti ipertestuali nella pagina.