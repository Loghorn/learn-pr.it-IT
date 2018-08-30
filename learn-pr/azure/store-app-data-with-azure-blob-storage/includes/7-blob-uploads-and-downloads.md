Dopo aver ottenuto un riferimento a un blob, è possibile caricare e scaricare i dati. Gli oggetti `ICloudBlob` dispongono di metodi `Upload` e `Download` che supportano le matrici di byte, i flussi e i file come origini e destinazioni. Altri tipi specifici dispongono di metodi aggiuntivi per motivi di praticità &mdash;, ad esempio, `CloudBlockBlob` supporta il caricamento e download di stringhe con `UploadTextAsync` e `DownloadTextAsync`.

## <a name="creating-new-blobs"></a>Creazione di nuovi blob

Per creare un nuovo blob, chiamare uno dei metodi `Upload` su un blob che non esiste. Ciò comporta due operazioni: crea il blob e carica i dati. 

## <a name="moving-data-to-and-from-blobs"></a>Spostamento dei dati da e verso i blob

Lo spostamento dei dati da e verso un blob è un'operazione di rete che richiede tempo. Nell'archivio SDK di Azure per .NET Core, tutti i metodi che richiedono attività di rete restituiscono `Task`s, in modo da assicurarsi che i metodi del controller vengano `async` in modo appropriato e che si sta `await`ando le chiamate dei metodi e non `Wait`ando su di esse.

Un consiglio comune quando si lavora con gli oggetti dati di grandi dimensioni consiste nell'usare flussi anziché strutture in memoria, ad esempio stringhe o matrici di byte. Questo evita di memorizzare nel buffer l'intero contenuto in memoria prima di inviarlo alla destinazione. ASP.NET Core supporta la lettura e la scrittura di flussi da richieste e risposte.

## <a name="concurrent-access"></a>Accesso simultaneo

È possibile che altri processi possano aggiungere, modificare o eliminare i blob usati dall'app. Codificare sempre in modo sicuro e considerare i problemi causati dalla concorrenza, come i blob eliminati proprio quando si prova a scaricare o i blob il cui contenuto viene modificato quando non previsto. Consultare la sezione Risorse aggiuntive alla fine di questo modulo per informazioni sull'uso di lease di blob e AccessConditions per gestire l'accesso a blob simultanei.

## <a name="exercise"></a>Esercizio

È possibile completare l'app mediante l'aggiunta di codice per il caricamento e il download e quindi distribuirla nel servizio app di Azure per effettuare il test.

### <a name="upload"></a>Caricamento

Per caricare un blob, verrà implementato il metodo `BlobStorage.Save` che usa `GetBlockBlobReference` per ottenere un `CloudBlockBlob` dal contenitore. `FilesController.Upload` passa il flusso di file a `Save`, quindi è possibile usare `UploadFromStreamAsync` per eseguire il caricamento per la massima efficienza.

Aprire `BlobStorage.cs` nell'editor e inserire l'implementazione di `Save` con il codice seguente:

```csharp
public Task Save(Stream fileStream, string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(name);
    return blockBlob.UploadFromStreamAsync(fileStream);
}
```

> [!NOTE]
> Il codice di caricamento basato sul flusso illustrato di seguito è più efficiente rispetto alla lettura del file in una matrice di byte prima dell'invio all'archivio BLOB di Azure. Tuttavia, la tecnica `IFormFile` usata per ottenere il file dal client non è una vera implementazione di flusso end-to-end ed è adatta solo per la gestione di caricamenti di file di piccole dimensioni. Consultare la sezione Risorse aggiuntive alla fine di questo modulo per informazioni sui caricamenti di file con flusso completo.

### <a name="download"></a>Download

`BlobStorage.Load` restituisce un `Stream`, vale a dire che il codice non deve spostare fisicamente i byte da un archivio BLOB &mdash; è sufficiente restituire un riferimento al flusso del blob. A questo scopo, usare `OpenReadAsync`. ASP.NET Core gestirà la lettura e la chiusura del flusso una volta compilata la risposta del client.

Inserire `Load` con questo codice:

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a>Pubblicare ed eseguire l'app su Azure

L'app è stata completata &mdash; è possibile distribuirla e visualizzarne il funzionamento. Eseguire il codice seguente sul terminale di Azure Cloud Shell per compilare il codice e distribuirla in una nuova istanza del Servizio app. È anche possibile aggiungere la stringa di connessione dell'account di archiviazione alla configurazione.

```console

```

...

Caricare e scaricare alcuni file per testare l'app, quindi eseguire il comando seguente nella shell per visualizzare i blob che sono stati caricati:

```console

```

**Imm TODO dal portale**