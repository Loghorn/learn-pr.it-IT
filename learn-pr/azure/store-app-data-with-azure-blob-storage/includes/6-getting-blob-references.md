Per usare un singolo BLOB in Azure Storage SDK per .NET Core è necessario un *riferimento a BLOB*, ossia un'istanza di un oggetto `ICloudBlob`.

È possibile ottenere un `ICloudBlob` richiedendolo con il nome del BLOB oppure selezionandolo in un elenco dei BLOB nel contenitore. In entrambi i casi è necessario un oggetto `CloudBlobContainer`, il cui recupero è stato illustrato nell'unità precedente.

## <a name="getting-blobs-by-name"></a>Recupero di BLOB in base al nome

Chiamare uno dei metodi `GetXXXReference` in un `CloudBlobContainer` per ottenere un `ICloudBlob` in base al nome. Se si conosce il tipo di BLOB che si sta recuperando, usare uno dei metodi specifici (`GetBlockBlobReference`, `GetAppendBlobReference`, o `GetPageBlobReference`) per ottenere un oggetto che include i metodi e le proprietà specifici per quel tipo di BLOB.

Nessuno di questi metodi esegue una chiamata di rete o verifica se il BLOB di destinazione effettivamente esista. Creano solo un oggetto di riferimento al BLOB in locale, che può quindi essere usato per chiamare i metodi che *operano* nella rete e interagiscono con i BLOB nell'archiviazione. Un metodo separato, `GetBlobReferenceFromServerAsync`, chiama l'API di archiviazione BLOB e genera un'eccezione se il BLOB non esiste.

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

> [!IMPORTANT]
> Non dare mai per scontato che i risultati di `ListBlobsSegmentedAsync` vengano recapitati in una singola pagina. Verificare sempre la presenza di un token di continuità e usarlo se è presente.

### <a name="processing-list-results"></a>Elaborazione dei risultati elencati

L'oggetto ottenuto da `ListBlobsSegmentedAsync` contiene una proprietà `Results` di tipo `IEnumerable<IListBlobItem>`. L'interfaccia `IListBlobItem` include solo un numero limitato di proprietà relative al contenitore e all'URL del BLOB e non è molto utile da sola.

Per ottenere gli oggetti BLOB utili al di fuori di `Results`, è possibile usare il metodo `OfType<>` per filtrare ed eseguire il cast dei risultati per tipi di oggetti BLOB più specifici. Di seguito sono disponibili alcuni esempi:

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

> [!TIP]
> L'uso di `OfType<>` richiede un riferimento allo spazio dei nomi `System.Linq` (`using System.Linq;`).

## <a name="exercise"></a>Esercizio

Una delle funzionalità dell'app richiede il recupero di un elenco di BLOB dall'API. Si userà il modello illustrato sopra per elencare tutti i BLOB nel contenitore. Elaborando l'elenco, si ottiene il nome di ogni BLOB.

Aprire `BlobStorage.cs` nell'editor, sostituire `GetNames` con il codice seguente e salvare le modifiche.

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

> [!TIP]
> Si noti che la firma del metodo deve ora specificare `async`.

I nomi restituiti da questo metodo vengono elaborati da `FilesController` per la trasformazione in URL. Quando vengono restituiti al client, ne viene eseguito il rendering come collegamenti ipertestuali nella pagina.