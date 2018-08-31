In questo modulo è stato illustrato come usare l'archivio BLOB di Azure per archiviare i dati delle applicazioni Web. Sono stati offerti suggerimenti per creare una strategia per l'uso dell'archivio BLOB in un'app Web ed è stato descritto come usare Azure Storage SDK per .NET Core per la scrittura e la lettura nei BLOB. L'app che è stata creata accetta file caricati dagli utenti, li archivia nell'archivio BLOB e li rende disponibili per il download.

## <a name="cleanup"></a>Pulizia

Per effettuare la pulizia della sottoscrizione di Azure, eseguire questo comando in Azure Cloud Shell per eliminare il gruppo di risorse contenente tutte le risorse create in questo modulo.

```console
az group delete --name blob-exercise-group
```

Per effettuare la pulizia dell'archivio di Cloud Shell, eliminare la directory `TODO` con `rm -rf TODO`.

## <a name="additional-resources"></a>Risorse aggiuntive

**TODO cross-docset links?**

* **Archiviazione sicura delle chiavi dell'account di archiviazione**: la soluzione end-to-end più affidabile per archiviare valori di configurazione segreti è Azure Key Vault. Per informazioni sull'uso di Key Vault in un'applicazione ASP.NET Core, vedere [qui](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x). In alternativa, è possibile archiviare in modo sicuro le stringhe di connessione nelle impostazioni delle applicazioni del servizio app e usare lo [strumento Secret Manager per ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) per supportare gli ambienti di sviluppo.
* [Caricamento di file di grandi dimensioni tramite streaming in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming)
* [Concorrenza nei BLOB: AccessCondition e lease di blob](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)
* [Concessione di accesso limitato a un oggetto di archiviazione di Azure con firme di accesso condiviso](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
* [Indicizzazione dell'archivio BLOB con Ricerca di Azure](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage)
* [Restrizioni per i nomi di contenitori e BLOB](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names)