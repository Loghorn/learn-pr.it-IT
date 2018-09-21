In questo modulo è stato illustrato come usare l'archivio BLOB di Azure per archiviare i dati delle applicazioni Web. Sono stati offerti suggerimenti per creare una strategia per l'uso dell'archivio BLOB in un'app Web ed è stato descritto come usare Azure Storage SDK per .NET Core per la scrittura e la lettura nei BLOB. L'app che è stata creata accetta file caricati dagli utenti, li archivia nell'archiviazione BLOB e li rende disponibili per il download.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

Per eseguire la pulizia dell'archiviazione Cloud Shell, eliminare la directory `mslearn-store-data-in-azure`.

<!---TODO: Remove further reading
## Further reading

- **Securely storing secrets like connection strings**: The most robust end-to-end solution for storing secret configuration values is Azure Key Vault. See [here](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x) for information about using Key Vault in an ASP.NET Core application. Alternatively, you can safely store connection strings in App Service application settings and use the [ASP.NET Core Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) to support developer environments.
- [Uploading large files with streaming in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming)
- [Blob concurrency: AccessConditions and blob leases](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)
- [Granting limited access to Azure Storage object with shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Indexing Blob storage with Azure Search](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage)
- [Container and blob name restrictions](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names)
--->