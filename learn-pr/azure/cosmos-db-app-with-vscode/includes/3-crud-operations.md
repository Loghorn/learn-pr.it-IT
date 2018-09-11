<!--TODO: explain Etag in knowledge needed-->

<span data-ttu-id="e921c-101">Dopo che è stata stabilita una connessione ad Azure Cosmos DB, il passaggio successivo consiste nel creare, leggere, sostituire ed eliminare i documenti archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="e921c-101">Once the connection to Azure Cosmos DB has been made, the next step is to create, read, replace and delete the documents that are stored in the database.</span></span> <span data-ttu-id="e921c-102">In questo modulo verranno creati documenti degli utenti nella raccolta WebCustomer e verranno poi recuperati in base all'ID, sostituiti ed eliminati.</span><span class="sxs-lookup"><span data-stu-id="e921c-102">In this module, you will create User documents in your WebCustomer collection, then you'll retrieve them by ID, replace them, and delete them.</span></span>

## <a name="working-with-documents-programmatically"></a><span data-ttu-id="e921c-103">Uso dei documenti a livello di codice</span><span class="sxs-lookup"><span data-stu-id="e921c-103">Working with documents programmatically</span></span>

<span data-ttu-id="e921c-104">I dati vengono archiviati in documenti JSON in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e921c-104">Data is stored in JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="e921c-105">I [documenti](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) possono essere creati, recuperati, sostituiti o eliminati nel portale, come illustrato nel modulo precedente, oppure a livello di programmazione, come descritto in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="e921c-105">[Documents](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) can be created, retrieved, replaced, or deleted in the portal, as shown in the previous module, or programmatically, as described in this module.</span></span> <span data-ttu-id="e921c-106">Azure Cosmos DB offre SDK lato client per .NET, .NET Core, Java, Node.js e Python, ognuno dei quali supporta queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="e921c-106">Azure Cosmos DB provides client-side SDKs for .NET, .NET Core, Java, Node.js, and Python, each of which supports these operations.</span></span> <span data-ttu-id="e921c-107">In questo modulo si userà .NET Core SDK per eseguire operazioni CRUD (Create, Retrieve, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="e921c-107">In this module we'll be using the .NET Core SDK to perform CRUD (create, retrieve, update, and delete) operations.</span></span> 

<span data-ttu-id="e921c-108">Le operazioni principali per i documenti di Azure Cosmos DB fanno parte della classe [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):</span><span class="sxs-lookup"><span data-stu-id="e921c-108">The main operations for Azure Cosmos DB documents are part of the [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) class:</span></span>
* [<span data-ttu-id="e921c-109">CreateDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="e921c-109">CreateDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="e921c-110">ReadDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="e921c-110">ReadDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [<span data-ttu-id="e921c-111">ReplaceDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="e921c-111">ReplaceDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* <span data-ttu-id="e921c-112">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="e921c-112">[UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet).</span></span> <span data-ttu-id="e921c-113">Upsert esegue un'operazione di creazione o sostituzione a seconda del fatto che il documento esista già.</span><span class="sxs-lookup"><span data-stu-id="e921c-113">Upsert performs a create or replace operation depending on whether the document already exists.</span></span>
* [<span data-ttu-id="e921c-114">DeleteDocumentAsync</span><span class="sxs-lookup"><span data-stu-id="e921c-114">DeleteDocumentAsync</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

<span data-ttu-id="e921c-115">Per eseguire queste operazioni, è necessario creare una classe che rappresenta l'oggetto archiviato nel database.</span><span class="sxs-lookup"><span data-stu-id="e921c-115">To perform any of these operations, you need to create a class that represents the object stored in the database.</span></span> <span data-ttu-id="e921c-116">Dato che l'oggetto dell'esempio è un database di utenti, è opportuno creare una classe **User** per archiviare i dati primari, ad esempio il nome, il cognome e l'ID degli utenti (l'ID è obbligatorio perché è la chiave di partizione per abilitare la scalabilità orizzontale) e sottoclassi per le preferenze di spedizione e la cronologia degli ordini.</span><span class="sxs-lookup"><span data-stu-id="e921c-116">Because we're working with a database of users, you'll want to create a **User** class to store primary data such as their first name, last name and user id (which is required, as that's the partition key to enable horizontal scaling) and subclasses for shipping preferences and order history.</span></span>

<span data-ttu-id="e921c-117">Dopo aver creato queste classi per rappresentare gli utenti, verranno creati nuovi documenti degli utenti per ogni istanza e quindi si eseguiranno alcune semplici operazioni CRUD sui documenti.</span><span class="sxs-lookup"><span data-stu-id="e921c-117">Once you have those classes created to represent your users, you'll create new user documents for each instance, and then we'll perform some simple CRUD operations on the documents.</span></span>

## <a name="create-documents"></a><span data-ttu-id="e921c-118">Creare i documenti</span><span class="sxs-lookup"><span data-stu-id="e921c-118">Create documents</span></span>

1. <span data-ttu-id="e921c-119">Creare prima di tutto una classe **User** che rappresenta gli oggetti da archiviare in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e921c-119">First, create a **User** class that represents the objects to store in Azure Cosmos DB.</span></span> <span data-ttu-id="e921c-120">Verranno create anche le sottoclassi **OrderHistory** e **ShippingPreference** usate all'interno di **User**.</span><span class="sxs-lookup"><span data-stu-id="e921c-120">We will also create **OrderHistory** and **ShippingPreference** subclasses that are used within **User**.</span></span> <span data-ttu-id="e921c-121">Si noti che i documenti devono avere una proprietà **Id** serializzata come **id** in JSON.</span><span class="sxs-lookup"><span data-stu-id="e921c-121">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span>

    <span data-ttu-id="e921c-122">Per creare queste classi, copiare e incollare le classi seguenti **User**, **OrderHistory**, **ShippingPreference** sotto il metodo **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="e921c-122">To create these classes, copy and paste the following **User**, **OrderHistory**, **ShippingPreference** classes underneath the **BasicOperations** method.</span></span>

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

2. <span data-ttu-id="e921c-123">Nel terminale integrato digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="e921c-123">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

3. <span data-ttu-id="e921c-124">A questo punto copiare e incollare l'attività **CreateUserDocumentIfNotExists** sotto la classe **ShippingPreference**.</span><span class="sxs-lookup"><span data-stu-id="e921c-124">Now copy and paste the **CreateUserDocumentIfNotExists** task under the **ShippingPreference** class.</span></span>

    ```csharp
    private async Task CreateUserDocumentIfNotExists(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey("user.userId") });
            this.WriteToConsoleAndPromptToContinue("Found {0}", user.Id);
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

4. <span data-ttu-id="e921c-125">Aggiungere quindi quanto segue al metodo **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="e921c-125">Then add the following to the **BasicOperations** method.</span></span>

    ```csharp
     User yanhe = new User
                {
                    Id = "1",
                    UserId = "yanhe",
                    LastName = "He",
                    FirstName = "Yan",
                    Email = "yanhe@todo.com",
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
                    Email = "nelapin@todo.com",
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

5. <span data-ttu-id="e921c-126">Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="e921c-126">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="e921c-127">Il terminale visualizza l'output seguente, a indicare che sono stati creati correttamente entrambi i record utente.</span><span class="sxs-lookup"><span data-stu-id="e921c-127">The terminal displays the following output, indicating that both user records were successfully created.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a><span data-ttu-id="e921c-128">Leggere i documenti</span><span class="sxs-lookup"><span data-stu-id="e921c-128">Read documents</span></span>

1. <span data-ttu-id="e921c-129">Per leggere i documenti dal database, copiare il codice seguente e inserirlo alla fine del file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e921c-129">To read documents from the database, copy in the following code and place it at the end of the Program.cs file.</span></span>
    <!--TODO: This is not finding the user-->

    ```csharp
    private async Task ReadUserDocument(string databaseName, string collectionName, User user)
    {
            try
            {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, user.Id), new RequestOptions { PartitionKey = new PartitionKey("user.userId") });
            this.WriteToConsoleAndPromptToContinue("Found user {0}", user.Id);
            }
            catch (DocumentClientException de)
            {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                this.WriteToConsoleAndPromptToContinue("User {0} not retrieved", user.Id);
            }
            else
            {
                throw;
            }
            }
    }
    ```

2.  <span data-ttu-id="e921c-130">Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**, dopo la riga `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="e921c-130">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

4. <span data-ttu-id="e921c-131">Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="e921c-131">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="e921c-132">Il terminale visualizza l'output seguente, in cui l'output "Found user 1" indica che il documento è stato trovato.</span><span class="sxs-lookup"><span data-stu-id="e921c-132">The terminal displays the following output, where the output "Found user 1" indicates the document was found.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="replace-documents"></a><span data-ttu-id="e921c-133">Sostituire i documenti</span><span class="sxs-lookup"><span data-stu-id="e921c-133">Replace documents</span></span>
<span data-ttu-id="e921c-134">Azure Cosmos DB supporta la sostituzione di documenti JSON.</span><span class="sxs-lookup"><span data-stu-id="e921c-134">Azure Cosmos DB supports replacing JSON documents.</span></span> <span data-ttu-id="e921c-135">In questo caso, verrà aggiornato un record utente per registrare una modifica al cognome.</span><span class="sxs-lookup"><span data-stu-id="e921c-135">In this case, we'll update a user record to account for a change to their last name.</span></span>

1. <span data-ttu-id="e921c-136">Copiare e incollare il metodo **ReplaceFamilyDocument** alla fine del file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e921c-136">Copy and paste the **ReplaceFamilyDocument** method at the end of the Program.cs file.</span></span>
    <!--TODO: This is not finding user to replace-->
    ```csharp
    private async Task ReplaceUserDocument(string databaseName, string collectionName, string LastName, User updatedUser)
    {
        try
        {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, LastName), new RequestOptions { PartitionKey = new PartitionKey("updatedUser.UserId") });
        this.WriteToConsoleAndPromptToContinue("Replaced last name for {0}", updatedUser);
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

2. <span data-ttu-id="e921c-137">Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**, dopo la riga `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.</span><span class="sxs-lookup"><span data-stu-id="e921c-137">Copy and paste the following code to the end of the **BasicOperations** method, after the `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);` line.</span></span>

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe.LastName, yanhe);
    ```

4. <span data-ttu-id="e921c-138">Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="e921c-138">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="e921c-139">Il terminale visualizza l'output seguente, in cui l'output "Replaced last name for Suh" indica che il documento è stato sostituito.</span><span class="sxs-lookup"><span data-stu-id="e921c-139">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    Replaced last name for Suh
    End of demo, press any key to exit.
    ```

## <a name="delete-documents"></a><span data-ttu-id="e921c-140">Eliminare i documenti</span><span class="sxs-lookup"><span data-stu-id="e921c-140">Delete documents</span></span>

1. <span data-ttu-id="e921c-141">Copiare e incollare il metodo **DeleteUserDocument** sotto il metodo **ReplaceUserDocument**.</span><span class="sxs-lookup"><span data-stu-id="e921c-141">Copy and paste the **DeleteUserDocument** method underneath your **ReplaceUserDocument** method.</span></span>
    
    ```csharp
    private async Task DeleteUserDocument(string databaseName, string collectionName, string id, User deletedUser)
    {
        try
        {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, id), new RequestOptions { PartitionKey = new PartitionKey("User.UserId") });
        Console.WriteLine("Deleted user {0}", id);
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

2. <span data-ttu-id="e921c-142">Copiare e incollare il codice seguente nel metodo **BasicOperations** sotto il codice di esecuzione della seconda query.</span><span class="sxs-lookup"><span data-stu-id="e921c-142">Copy and paste the following code to your **BasicOperations** method underneath the second query execution.</span></span>

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", "1", yanhe);
    ```

3. <span data-ttu-id="e921c-143">Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="e921c-143">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="e921c-144">Il terminale visualizza l'output seguente, in cui l'output "Replaced last name for Suh" indica che il documento è stato sostituito.</span><span class="sxs-lookup"><span data-stu-id="e921c-144">The terminal displays the following output, where the output "Replaced last name for Suh" indicates the document was replaced.</span></span>

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    Found user 1
    Press any key to continue ...
    Replaced last name for Suh
    Press any key to continue ...
    Deleted user 1
    End of demo, press any key to exit.
    ```

    <span data-ttu-id="e921c-145">Congratulazioni!</span><span class="sxs-lookup"><span data-stu-id="e921c-145">Congratulations!</span></span> <span data-ttu-id="e921c-146">L'eliminazione di un documento di Azure Cosmos DB è stata completata.</span><span class="sxs-lookup"><span data-stu-id="e921c-146">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <a name="summary"></a><span data-ttu-id="e921c-147">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e921c-147">Summary</span></span>

<span data-ttu-id="e921c-148">In questa unità sono stati creati, sostituiti ed eliminati documenti nel database di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e921c-148">In this unit you created, replaced, and deleted documents in your Azure Cosmos DB database.</span></span>