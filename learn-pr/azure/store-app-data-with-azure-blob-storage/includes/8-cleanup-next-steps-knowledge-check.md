<span data-ttu-id="a5313-101">In questo modulo è stato illustrato come usare l'archivio BLOB di Azure per archiviare i dati delle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="a5313-101">In this module, you learned how to use Azure Blob storage to store web application data.</span></span> <span data-ttu-id="a5313-102">Sono stati offerti suggerimenti per creare una strategia per l'uso dell'archivio BLOB in un'app Web ed è stato descritto come usare Azure Storage SDK per .NET Core per la scrittura e la lettura nei BLOB.</span><span class="sxs-lookup"><span data-stu-id="a5313-102">We discussed tips for creating a strategy to use Blob storage in a web app and how to use the Azure Storage SDK for .NET Core to write to and read from blobs.</span></span> <span data-ttu-id="a5313-103">L'app che è stata creata accetta file caricati dagli utenti, li archivia nell'archivio BLOB e li rende disponibili per il download.</span><span class="sxs-lookup"><span data-stu-id="a5313-103">The app we made accepts uploaded files from users, stores them in Blob storage, and makes them available for download.</span></span>

## <a name="cleanup"></a><span data-ttu-id="a5313-104">Pulizia</span><span class="sxs-lookup"><span data-stu-id="a5313-104">Cleanup</span></span>

<span data-ttu-id="a5313-105">Per effettuare la pulizia della sottoscrizione di Azure, eseguire questo comando in Azure Cloud Shell per eliminare il gruppo di risorse contenente tutte le risorse create in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="a5313-105">To clean up your Azure subscription, run the following in Azure Cloud Shell to delete the resource group containing all the resources we created in this module.</span></span>

```console
az group delete --name blob-exercise-group
```

<span data-ttu-id="a5313-106">Per effettuare la pulizia dell'archivio di Cloud Shell, eliminare la directory `TODO` con `rm -rf TODO`.</span><span class="sxs-lookup"><span data-stu-id="a5313-106">To clean up your Cloud Shell storage, delete the `TODO` directory with `rm -rf TODO`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5313-107">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a5313-107">Additional resources</span></span>

<span data-ttu-id="a5313-108">**TODO cross-docset links?**</span><span class="sxs-lookup"><span data-stu-id="a5313-108">**TODO cross-docset links?**</span></span>

* <span data-ttu-id="a5313-109">**Archiviazione sicura delle chiavi dell'account di archiviazione**: la soluzione end-to-end più affidabile per archiviare valori di configurazione segreti è Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a5313-109">**Securely storing storage account keys**: The most robust end-to-end solution for storing secret configuration values is Azure Key Vault.</span></span> <span data-ttu-id="a5313-110">Per informazioni sull'uso di Key Vault in un'applicazione ASP.NET Core, vedere [qui](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a5313-110">See [here](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x) for information about using Key Vault in an ASP.NET Core application.</span></span> <span data-ttu-id="a5313-111">In alternativa, è possibile archiviare in modo sicuro le stringhe di connessione nelle impostazioni delle applicazioni del servizio app e usare lo [strumento Secret Manager per ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) per supportare gli ambienti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="a5313-111">Alternatively, you can safely store connection strings in App Service application settings and use the [ASP.NET Core Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) to support developer environments.</span></span>
* [<span data-ttu-id="a5313-112">Caricamento di file di grandi dimensioni tramite streaming in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5313-112">Uploading large files with streaming in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming)
* [<span data-ttu-id="a5313-113">Concorrenza nei BLOB: AccessCondition e lease di blob</span><span class="sxs-lookup"><span data-stu-id="a5313-113">Blob concurrency: AccessConditions and blob leases</span></span>](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)
* [<span data-ttu-id="a5313-114">Concessione di accesso limitato a un oggetto di archiviazione di Azure con firme di accesso condiviso</span><span class="sxs-lookup"><span data-stu-id="a5313-114">Granting limited access to Azure Storage object with shared access signatures</span></span>](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
* [<span data-ttu-id="a5313-115">Indicizzazione dell'archivio BLOB con Ricerca di Azure</span><span class="sxs-lookup"><span data-stu-id="a5313-115">Indexing Blob storage with Azure Search</span></span>](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage)
* [<span data-ttu-id="a5313-116">Restrizioni per i nomi di contenitori e BLOB</span><span class="sxs-lookup"><span data-stu-id="a5313-116">Container and blob name restrictions</span></span>](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names)