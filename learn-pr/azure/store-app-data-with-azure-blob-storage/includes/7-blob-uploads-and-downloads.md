Dopo aver ottenuto un riferimento a un blob, è possibile caricare e scaricare i dati. Gli oggetti `ICloudBlob` dispongono di metodi `Upload` e `Download` che supportano le matrici di byte, i flussi e i file come origini e destinazioni. Altri tipi specifici dispongono di metodi aggiuntivi per motivi di praticità &mdash;, ad esempio, `CloudBlockBlob` supporta il caricamento e download di stringhe con `UploadTextAsync` e `DownloadTextAsync`.

## <a name="creating-new-blobs"></a>Creazione di nuovi BLOB

Per creare un nuovo BLOB, chiamare uno dei metodi `Upload` su un riferimento a un BLOB che non esiste nella risorsa di archiviazione. Ciò comporta due operazioni: creazione del BLOB nella risorsa di archiviazione e caricamento dei dati.

## <a name="moving-data-to-and-from-blobs"></a>Spostamento dei dati da e verso i BLOB

Lo spostamento dei dati da e verso un BLOB è un'operazione di rete che richiede tempo. In Azure Storage SDK per .NET Core, tutti i metodi che richiedono attività di rete restituiscono `Task`s, in modo da assicurarsi che i metodi del controller usino `await` in modo appropriato.

Un consiglio comune quando si lavora con oggetti dati di grandi dimensioni consiste nell'usare flussi anziché strutture in memoria, ad esempio stringhe o matrici di byte. Questo evita di memorizzare nel buffer l'intero contenuto in memoria prima di inviarlo alla destinazione. ASP.NET Core supporta la lettura e la scrittura di flussi da richieste e risposte.

## <a name="concurrent-access"></a>Accesso simultaneo

È possibile che altri processi possano aggiungere, modificare o eliminare i BLOB usati dall'app. Codificare sempre in modo sicuro e considerare i problemi causati dalla simultaneità, come i BLOB eliminati proprio quando si sta provando a scaricare o i BLOB il cui contenuto viene modificato quando non previsto. Vedere la sezione Altre informazioni, alla fine di questo modulo, sull'uso di lease di BLOB e AccessConditions per gestire l'accesso simultaneo ai BLOB.

## <a name="exercise"></a>Esercizio

È possibile completare l'app mediante l'aggiunta di codice per il caricamento e il download e quindi distribuirla nel servizio app di Azure per effettuare il test.

### <a name="upload"></a>Caricamento

Per caricare un blob, verrà implementato il metodo `BlobStorage.Save` che usa `GetBlockBlobReference` per ottenere un `CloudBlockBlob` dal contenitore. `FilesController.Upload` passa il flusso di file a `Save`, quindi è possibile usare `UploadFromStreamAsync` per eseguire il caricamento per la massima efficienza.

Nell'editor sostituire `Save` in `BlobStorage.cs` con il codice seguente:

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
> Il codice di caricamento basato sul flusso illustrato di seguito è più efficiente rispetto alla lettura del file in una matrice di byte prima dell'invio all'archiviazione BLOB di Azure. Tuttavia, la tecnica ASP.NET Core `IFormFile` usata per ottenere il file dal client non è una vera implementazione di flusso end-to-end ed è adatta solo per la gestione di caricamenti di file di piccole dimensioni. Vedere la sezione Altre informazioni, alla fine di questo modulo, sui caricamenti di file con flusso completo.

### <a name="download"></a>Download

`BlobStorage.Load` restituisce un `Stream`, vale a dire che il codice non deve spostare fisicamente i byte da un archivio BLOB &mdash; è sufficiente restituire un riferimento al flusso del blob. A questo scopo, usare `OpenReadAsync`. ASP.NET Core gestirà la lettura e la chiusura del flusso una volta compilata la risposta del client.

Sostituire `Load` con questo codice e salvare il lavoro:

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a>Distribuire ed eseguire l'app in Azure

L'app è stata completata &mdash; è possibile distribuirla e visualizzarne il funzionamento. Creare un'app del servizio app e configurarla con le impostazioni dell'applicazione per la stringa di connessione dell'account di archiviazione e il nome del contenitore. Ottenere la stringa di connessione dell'account di archiviazione con `az storage account show-connection-string` e impostare il nome del contenitore su `files`.

Il nome dell'app deve essere globalmente univoco, quindi è necessario sceglierne uno per riempire `<your-unique-app-name>`.

```azurecli
az appservice plan create --name blob-exercise-plan --resource-group <rgn>[Sandbox resource group name]</rgn>
az webapp create --name <your-unique-app-name> --plan blob-exercise-plan --resource-group <rgn>[Sandbox resource group name]</rgn>
CONNECTIONSTRING=$(az storage account show-connection-string --name <your-unique-storage-account-name> --output tsv)
az webapp config appsettings set --name <your-unique-app-name> --resource-group <rgn>[Sandbox resource group name]</rgn> --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
```

A questo punto l'app viene distribuita. I comandi seguenti pubblicheranno il sito nella cartella `pub`, la comprimeranno in `site.zip` e quindi distribuiranno il file con estensione zip nel servizio app.

> [!NOTE]
> Verificare che la shell sia ancora nella directory `mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start` prima di eseguire i comandi seguenti.

```azurecli
dotnet publish -o pub
cd pub
zip -r ../site.zip *
az webapp deployment source config-zip --src ../site.zip --name <your-unique-app-name> --resource-group <rgn>[Sandbox resource group name]</rgn>
```

Aprire `https://<your-unique-app-name>.azurewebsites.net` in un browser per visualizzare l'app in esecuzione. L'aspetto dovrebbe essere simile a quanto riportato di seguito.

![Screenshot dell'app Web FileUploader](../media/7-fileuploader-empty.PNG)

Provare a caricare e scaricare alcuni file per testare l'app. Dopo aver caricato dei file, eseguire il comando seguente nella shell per visualizzare i BLOB che sono stati caricati nel contenitore:

```console
az storage blob list --account-name <your-unique-storage-account-name> --container-name files --query [].{Name:name} --output table
```