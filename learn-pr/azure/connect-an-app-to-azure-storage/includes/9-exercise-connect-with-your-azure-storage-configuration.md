<span data-ttu-id="2aa4e-101">In questa unità si caricherà la stringa di connessione dell'account di archiviazione di Azure dalla configurazione, per poi usarla per connettersi all'account di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="2aa4e-101">In this unit, you'll load the Azure storage account connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="2aa4e-102">Recuperare la stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="2aa4e-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="2aa4e-103">Verificare che l'applicazione console creata nell'unità precedente venga caricata in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2aa4e-103">Ensure the console application from the previous unit is loaded into Visual Studio.</span></span>

1. <span data-ttu-id="2aa4e-104">Nel file **Program.cs** aggiungere un'istruzione *using* all'inizio del file per fare riferimento alla libreria **Microsoft.WindowsAzure.Storage**:</span><span class="sxs-lookup"><span data-stu-id="2aa4e-104">In the **Program.cs** file, add a *using* statement at the top of the file to reference the **Microsoft.WindowsAzure.Storage** library:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="2aa4e-105">Nella parte inferiore del metodo **Main** aggiungere la riga seguente per recuperare la stringa di connessione dell'account di archiviazione di Azure dal file di configurazione:</span><span class="sxs-lookup"><span data-stu-id="2aa4e-105">At the bottom of the **Main** method, add the following line to retrieve the Azure storage account connection string from the configuration file:</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="2aa4e-106">Creare un client BLOB</span><span class="sxs-lookup"><span data-stu-id="2aa4e-106">Create a blob client</span></span>

1. <span data-ttu-id="2aa4e-107">Inserire il codice seguente nella parte inferiore del metodo **Main** per analizzare la stringa di connessione e creare un client BLOB:</span><span class="sxs-lookup"><span data-stu-id="2aa4e-107">Insert the following code at the bottom of the **Main** method to parse the connection string and create a blob client:</span></span>

    ```csharp
    CloudStorageAccount storageAccount;
    if (!CloudStorageAccount.TryParse(connectionString,out storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="2aa4e-108">Inserire il codice seguente per creare un client BLOB e recuperare un riferimento a un contenitore, creandolo (operazione facoltativa) se non è presente nella parte inferiore del metodo **Main**:</span><span class="sxs-lookup"><span data-stu-id="2aa4e-108">Insert the following code to create a blob client and retrieve a reference to a container, optionally creating it if it does not exist at the bottom of the **Main** method:</span></span>

    ```csharp
    var containerName = "MyBlobContainer";
    var success = blobClient
        .GetContainerReference(containerName)
        .CreateIfNotExistsAsync()
        .Result;
    ```

  <span data-ttu-id="2aa4e-109">*Notare l'uso del metodo **async** **CreateIfNotExistsAsync()** e della proprietà **Result** corrispondente. In una normale applicazione verrebbe in genere usata la parola chiave **await**. Tuttavia, poiché questa applicazione è un'applicazione console e non asincrona, è necessario chiamare la proprietà **Result** per ottenere i risultati dell'attività asincrona creata dal metodo **CreateIfNotExistsAsync()**.*</span><span class="sxs-lookup"><span data-stu-id="2aa4e-109">*Note the use of the **async** method **CreateIfNotExistsAsync()** and corresponding **Result** property. In a typical application, we would normally use the **await** keyword. However, this application is a console application and is not async, so we need to call the **Result** property to get the results of the async task created by the **CreateIfNotExistsAsync()** method.*</span></span>

## <a name="print-a-status-message"></a><span data-ttu-id="2aa4e-110">Stampare un messaggio di stato</span><span class="sxs-lookup"><span data-stu-id="2aa4e-110">Print a status message</span></span>

1. <span data-ttu-id="2aa4e-111">Aggiungere il codice seguente nella parte inferiore del metodo **Main** per stampare un messaggio di operazione riuscita o non riuscita.</span><span class="sxs-lookup"><span data-stu-id="2aa4e-111">Add the following code at the bottom of the **Main** method to print a success or failure message.</span></span>

    ```csharp
    if (!success)
    {
        Console.WriteLine("Error: Could not connect to Azure Storage container");
        return;
    }
    Console.WriteLine("Successfully connected to Azure Storage container");
    ```
1. <span data-ttu-id="2aa4e-112">Infine, eseguire l'applicazione per verificare che la connessione sia riuscita e visualizzare il portale di Azure per assicurarsi che il contenitore di archiviazione di Azure sia stato creato se non esisteva in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2aa4e-112">Finally, run the application to verify that a successful connection is made, and view the Azure portal to ensure the Azure Storage container is created if it did not exist previously.</span></span>

1. <span data-ttu-id="2aa4e-113">**Facoltativo**: se non si prevede di usare il contenitore, eliminare il gruppo di risorse in cui si trova per evitare addebiti per le risorse inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="2aa4e-113">**Optional**: If you don't plan on using the container, delete the resource group where the container is located to ensure you are not charged for resources you are not using.</span></span>
