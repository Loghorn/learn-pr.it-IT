<!--TODO: explain Etag in knowledge needed-->

<span data-ttu-id="355c7-101">Dopo che è stata stabilita una connessione ad Azure Cosmos DB, il passaggio successivo consiste nel creare, leggere, sostituire ed eliminare i documenti archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="355c7-101">Once the connection to Azure Cosmos DB has been made, the next step is to create, read, replace, and delete the documents that are stored in the database.</span></span> <span data-ttu-id="355c7-102">In questa unità verranno creati documenti degli utenti nella raccolta WebCustomer</span><span class="sxs-lookup"><span data-stu-id="355c7-102">In this unit, you will create User documents in your WebCustomer collection.</span></span> <span data-ttu-id="355c7-103">e verranno poi recuperati in base all'ID, sostituiti ed eliminati.</span><span class="sxs-lookup"><span data-stu-id="355c7-103">Then, you'll retrieve them by ID, replace them, and delete them.</span></span>

## <a name="working-with-documents-programmatically"></a><span data-ttu-id="355c7-104">Utilizzo dei documenti a livello di codice</span><span class="sxs-lookup"><span data-stu-id="355c7-104">Working with documents programmatically</span></span>

<span data-ttu-id="355c7-105">I dati vengono archiviati in documenti JSON in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="355c7-105">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="355c7-106">I [documenti](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) possono essere creati, recuperati, sostituiti o eliminati nel portale, come illustrato nel modulo precedente, oppure a livello di programmazione, come descritto in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="355c7-106">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="355c7-107">Azure Cosmos DB offre SDK lato client per .NET, .NET Core, Java, Node.js e Python, ognuno dei quali supporta queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="355c7-107">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="355c7-108">In questo modulo si userà .NET Core SDK per eseguire operazioni CRUD (Create, Retrieve, Update, Delete) sui dati NoSQL archiviati in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="355c7-108">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations on the NoSQL data stored in Azure Cosmos DB.</span></span>

<span data-ttu-id="355c7-109">Le operazioni principali per i documenti di Azure Cosmos DB fanno parte della classe [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):</span><span class="sxs-lookup"><span data-stu-id="355c7-109">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="355c7-110">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="355c7-110">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="355c7-111">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="355c7-111">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="355c7-112">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="355c7-112">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="355c7-113">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="355c7-113">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="355c7-114">Upsert esegue un'operazione di creazione o sostituzione a seconda del fatto che il documento esista già.</span><span class="sxs-lookup"><span data-stu-id="355c7-114">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="355c7-115">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="355c7-115">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="355c7-116">Per eseguire queste operazioni, è necessario creare una classe che rappresenta l'oggetto archiviato nel database.</span><span class="sxs-lookup"><span data-stu-id="355c7-116">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="355c7-117">Poiché l'oggetto dell'esempio è un database di utenti, è opportuno creare una classe **User** per archiviare i dati primari, ad esempio il nome, il cognome e l'ID degli utenti (l'ID è obbligatorio perché è la chiave di partizione per abilitare la scalabilità orizzontale) e sottoclassi per le preferenze di spedizione e la cronologia degli ordini.</span><span class="sxs-lookup"><span data-stu-id="355c7-117">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their first name, last name, and user id (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="355c7-118">Dopo aver creato queste classi per rappresentare gli utenti, verranno creati nuovi documenti degli utenti per ogni istanza e quindi si eseguiranno alcune semplici operazioni CRUD sui documenti.</span><span class="sxs-lookup"><span data-stu-id="355c7-118">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="355c7-119">Creare i documenti</span><span class="sxs-lookup"><span data-stu-id="355c7-119">Create documents</span></span>

1. <span data-ttu-id="355c7-120">Creare prima di tutto una classe **User** che rappresenta gli oggetti da archiviare in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="355c7-120">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="355c7-121">Verranno create anche le sottoclassi **OrderHistory** e **ShippingPreference** usate all'interno di **User**.</span><span class="sxs-lookup"><span data-stu-id="355c7-121">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="355c7-122">Si noti che i documenti devono avere una proprietà **Id** serializzata come **id** in JSON.</span><span class="sxs-lookup"><span data-stu-id="355c7-122">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span>

    <span data-ttu-id="355c7-123">Per creare queste classi, copiare e incollare le classi seguenti **User**, **OrderHistory** e **ShippingPreference** sotto il metodo **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="355c7-123">To create these classes, copy and paste the following **User**, **OrderHistory**, and **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

    ```csharp
    public class User
    {
        [JsonProperty("id")]
        public string Id { get; set; }
        [JsonProperty("userId")]
        public string UserId { get; set; }
        [JsonProperty("lastName")]
        public string LastName { get; set; }
        [JsonProperty("firstName")]
        public string FirstName { get; set; }
        [JsonProperty("email")]
        public string Email { get; set; }
        [JsonProperty("dividend")]
        public string Dividend { get; set; }
        [JsonProperty("OrderHistory")]
        public OrderHistory[] OrderHistory { get; set; }
        [JsonProperty("ShippingPreference")]
        public ShippingPreference[] ShippingPreference { get; set; }
        [JsonProperty("CouponsUsed")]
        public CouponsUsed[] Coupons { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class OrderHistory
    {
        public string OrderId { get; set; }
        public string DateShipped { get; set; }
        public string Total { get; set; }
    }

    public class ShippingPreference
    {
        public int Priority { get; set; }
        public string AddressLine1 { get; set; }
        public string AddressLine2 { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string ZipCode { get; set; }
        public string Country { get; set; }
    }

    public class CouponsUsed
    {
        public string CouponCode { get; set; }

    }

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
    ```

1. <span data-ttu-id="355c7-124">Nel terminale integrato digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="355c7-124">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="355c7-125">Copiare e incollare ora l'attività **CreateUserDocumentIfNotExists** nella funzione **WriteToConsoleAndPromptToContinue** alla fine del file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="355c7-125">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **WriteToConsoleAndPromptToContinue** function at the end of the Program.cs file.</span></span>

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            this.WriteToConsoleAndPromptToContinue("User {0} already exists in the database", user.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), user);
                this.WriteToConsoleAndPromptToContinue("Created User {0}", user.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. <span data-ttu-id="355c7-126">Tornare quindi al metodo **BasicOperations** e aggiungere il codice seguente alla fine del metodo.</span><span class="sxs-lookup"><span data-stu-id="355c7-126">Then, return to the **BasicOperations** method and add the following to the end of that method.</span></span>

    ```csharp
    User yanhe = new User
    {
        Id = "1",
        UserId = "yanhe",
        LastName = "He",
        FirstName = "Yan",
        Email = "yanhe@contoso.com",
        OrderHistory = new OrderHistory[]
            {
                new OrderHistory {
                    OrderId = "1000",
                    DateShipped = "08/17/2018",
                    Total = "52.49"
                }
            },
            ShippingPreference = new ShippingPreference[]
            {
                    new ShippingPreference {
                            Priority = 1,
                            AddressLine1 = "90 W 8th St",
                            City = "New York",
                            State = "NY",
                            ZipCode = "10001",
                            Country = "USA"
                    }
            },
    };

    await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", yanhe);

    User nelapin = new User
    {
        Id = "2",
        UserId = "nelapin",
        LastName = "Pindakova",
        FirstName = "Nela",
        Email = "nelapin@contoso.com",
        Dividend = "8.50",
        OrderHistory = new OrderHistory[]
        {
            new OrderHistory {
                OrderId = "1001",
                DateShipped = "08/17/2018",
                Total = "105.89"
            }
        },
         ShippingPreference = new ShippingPreference[]
        {
            new ShippingPreference {
                    Priority = 1,
                    AddressLine1 = "505 NW 5th St",
                    City = "New York",
                    State = "NY",
                    ZipCode = "10001",
                    Country = "USA"
            },
            new ShippingPreference {
                    Priority = 2,
                    AddressLine1 = "505 NW 5th St",
                    City = "New York",
                    State = "NY",
                    ZipCode = "10001",
                    Country = "USA"
            }
        },
        Coupons = new CouponsUsed[]
        {
            new CouponsUsed{
                CouponCode = "Fall2018"
            }
        }
    };

    await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);
    ```

1. <span data-ttu-id="355c7-127">Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="355c7-127">In the integrated terminal, again, type the following command to run the program.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="355c7-128">Il terminale visualizzerà l'output man mano che l'applicazione crea ogni nuovo documento utente.</span><span class="sxs-lookup"><span data-stu-id="355c7-128">The terminal will display output as the application creates each new user document.</span></span> <span data-ttu-id="355c7-129">Premere un tasto qualsiasi per completare il programma.</span><span class="sxs-lookup"><span data-stu-id="355c7-129">Press any key to complete the program.</span></span>

    ```output
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="355c7-130">Leggere i documenti</span><span class="sxs-lookup"><span data-stu-id="355c7-130">Read documents</span></span>

1. <span data-ttu-id="355c7-131">Per leggere i documenti dal database, copiare il codice seguente e inserirlo dopo il metodo WriteToConsoleAndPromptToContinue nel file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="355c7-131">To read documents from the database, copy in the following code and place after the WriteToConsoleAndPromptToContinue method in the Program.cs file.</span></span>

    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            this.WriteToConsoleAndPromptToContinue("Read user {0}", user.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not read", user.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. <span data-ttu-id="355c7-132">Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**, dopo la riga `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="355c7-132">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="355c7-133">Nel terminale integrato digitare il comando seguente per eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="355c7-133">In the integrated terminal, type the following command to run the program.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="355c7-134">Il terminale visualizza l'output seguente, in cui l'output "Read user 1" indica che il documento è stato recuperato.</span><span class="sxs-lookup"><span data-stu-id="355c7-134">The terminal displays the following output, where the output "Read user 1" indicates the document was retrieved.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="355c7-135">Sostituire i documenti</span><span class="sxs-lookup"><span data-stu-id="355c7-135">Replace documents</span></span>

<span data-ttu-id="355c7-136">Azure Cosmos DB supporta la sostituzione di documenti JSON.</span><span class="sxs-lookup"><span data-stu-id="355c7-136">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="355c7-137">In questo caso, verrà aggiornato un record utente per registrare una modifica al cognome.</span><span class="sxs-lookup"><span data-stu-id="355c7-137">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="355c7-138">Copiare e incollare il metodo **ReplaceFamilyDocument** dopo il metodo ReadUserDocument nel file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="355c7-138">Copy and paste the **ReplaceFamilyDocument** method after the ReadUserDocument method in the Program.cs file.</span></span>

    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, User updatedUser)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, updatedUser.Id), updatedUser, new RequestOptions { PartitionKey = new PartitionKey(updatedUser.UserId) });
            this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", updatedUser.LastName);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not found for replacement", updatedUser.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. <span data-ttu-id="355c7-139">Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**, dopo la riga `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="355c7-139">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="355c7-140">Nel terminale integrato eseguire questo comando.</span><span class="sxs-lookup"><span data-stu-id="355c7-140">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```
    <span data-ttu-id="355c7-141">Il terminale visualizza l'output seguente, in cui l'output "Replaced last name for Suh" indica che il documento è stato sostituito.</span><span class="sxs-lookup"><span data-stu-id="355c7-141">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="355c7-142">Eliminare i documenti</span><span class="sxs-lookup"><span data-stu-id="355c7-142">Delete documents</span></span>

1. <span data-ttu-id="355c7-143">Copiare e incollare il metodo **DeleteUserDocument** sotto il metodo **ReplaceUserDocument**.</span><span class="sxs-lookup"><span data-stu-id="355c7-143">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>

    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, User deletedUser)
    {
        try
        {
            await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, deletedUser.Id), new RequestOptions { PartitionKey = new PartitionKey(deletedUser.UserId) });
            Console.WriteLine("Deleted user {0}", deletedUser.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not found for deletion", deletedUser.Id);
            }
            else
            {
                throw;
            }
        }
    }
    ```

1. <span data-ttu-id="355c7-144">Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="355c7-144">Copy and paste the following code in the end of the **BasicOperations** method.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="355c7-145">Nel terminale integrato eseguire questo comando.</span><span class="sxs-lookup"><span data-stu-id="355c7-145">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="355c7-146">Il terminale visualizza l'output seguente, in cui l'output "Deleted user 1" indica che il documento è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="355c7-146">The terminal displays the following output, where the output "Deleted user 1" indicates the document was deleted.</span></span>

    ```output
    Database and collection validation complete
    User 1 already exists in the database
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    User 2 already exists in the database
    Press any key to continue ...
    Read user 1
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

<span data-ttu-id="355c7-147">In questa unità sono stati creati, sostituiti ed eliminati documenti nel database di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="355c7-147">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>
