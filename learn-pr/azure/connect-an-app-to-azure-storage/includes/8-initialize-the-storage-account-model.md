La libreria client di Archiviazione di Azure offre un modello a oggetti usato per interagire con account di archiviazione di Azure. Questo modello viene usato per connettersi rapidamente a un account di archiviazione di Azure e usare le API dei servizi di Archiviazione di Azure.

Qui si userà il modello a oggetti per l'account nel codice per connettersi all'account di archiviazione di Azure.

## <a name="azure-storage-client-library-object-model"></a>Modello a oggetti della libreria client di Archiviazione di Azure

La possibilità di usare il modello a oggetti della libreria client di Archiviazione di Azure è un fattore chiave per la semplicità di implementazione quando ci si connette all'account di archiviazione di Azure.

Alla base del modello a oggetti per l'account di archiviazione nella libreria client .NET Core vi è **CloudStorageAccount**. Il modo più semplice per inizializzare il modello a oggetti consiste nell'usare `CloudStorageAccount.Parse` o `CloudStorageAccount.TryParse` per analizzare la stringa di connessione.

La libreria client non tenterà di connettersi finché non viene richiamata un'operazione che la richiede. `Parse()` e `TryParse()` garantiscono solo che la stringa di connessione abbia un formato corretto, ma non verificano che l'account sia presente o che la chiave sia corretta. L'istanza `CloudStorageAccount` risultante restituita dalla chiamata al metodo `Parse()` o `TryParse()` espone metodi per creare un client per i tipi di archiviazione BLOB, file, tabelle o code. Questo viene quindi usato per creare istanze di oggetti client del servizio per i servizi di archiviazione BLOB, file, code e tabelle. Il frammento di codice seguente mostra un esempio di creazione di un client per l'uso dell'archiviazione BLOB:

```c#
using Microsoft.WindowsAzure.Storage;

var storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");
var blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` e gli oggetti client sono oggetti leggeri e possono essere creati on demand o inizialmente per essere poi condivisi nell'applicazione. Un approccio standard consiste nell'usare `CloudStorageAccount.TryParse()` accanto al punto di ingresso dell'applicazione per creare un'istanza e renderla disponibile nell'applicazione per creare istanze client.

## <a name="summary"></a>Riepilogo

Il modello a oggetti della libreria client di Archiviazione di Azure offre un semplice metodo per connettersi all'account di archiviazione di Azure. È necessario fornire una stringa di connessione, di cui il modello a oggetti controllerà il formato e verificherà l'esistenza dell'account. Quando è presente un'istanza `CloudStorageAccount`, è possibile creare client per tipi di archiviazione BLOB, file, tabelle o code.
