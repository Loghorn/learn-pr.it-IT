<!--TODO: explain Etag in knowledge needed-->

Dopo che è stata stabilita una connessione ad Azure Cosmos DB, il passaggio successivo consiste nel creare, leggere, sostituire ed eliminare i documenti archiviati nel database. In questo modulo verranno creati documenti degli utenti nella raccolta WebCustomer e verranno poi recuperati in base all'ID, sostituiti ed eliminati.

## <a name="working-with-documents-programmatically"></a>Uso dei documenti a livello di codice

I dati vengono archiviati in documenti JSON in Azure Cosmos DB. I [documenti](https://docs.microsoft.com/azure/cosmos-db/sql-api-resources#documents) possono essere creati, recuperati, sostituiti o eliminati nel portale, come illustrato nel modulo precedente, oppure a livello di programmazione, come descritto in questo modulo. Azure Cosmos DB offre SDK lato client per .NET, .NET Core, Java, Node.js e Python, ognuno dei quali supporta queste operazioni. In questo modulo si userà .NET Core SDK per eseguire operazioni CRUD (Create, Retrieve, Update, Delete). 

Le operazioni principali per i documenti di Azure Cosmos DB fanno parte della classe [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet):
* [CreateDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet)
* [ReadDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync?view=azure-dotnet)
* [ReplaceDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync?view=azure-dotnet)
* [UpsertDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync?view=azure-dotnet). Upsert esegue un'operazione di creazione o sostituzione a seconda del fatto che il documento esista già.
* [DeleteDocumentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync?view=azure-dotnet)

Per eseguire queste operazioni, è necessario creare una classe che rappresenta l'oggetto archiviato nel database. Dato che l'oggetto dell'esempio è un database di utenti, è opportuno creare una classe **User** per archiviare i dati primari, ad esempio il nome, il cognome e l'ID degli utenti (l'ID è obbligatorio perché è la chiave di partizione per abilitare la scalabilità orizzontale) e sottoclassi per le preferenze di spedizione e la cronologia degli ordini.

Dopo aver creato queste classi per rappresentare gli utenti, verranno creati nuovi documenti degli utenti per ogni istanza e quindi si eseguiranno alcune semplici operazioni CRUD sui documenti.

## <a name="create-documents"></a>Creare i documenti

1. Creare prima di tutto una classe **User** che rappresenta gli oggetti da archiviare in Azure Cosmos DB. Verranno create anche le sottoclassi **OrderHistory** e **ShippingPreference** usate all'interno di **User**. Si noti che i documenti devono avere una proprietà **Id** serializzata come **id** in JSON.

    Per creare queste classi, copiare e incollare le classi seguenti **User**, **OrderHistory**, **ShippingPreference** sotto il metodo **BasicOperations**.

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

2. Nel terminale integrato digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

3. A questo punto copiare e incollare l'attività **CreateUserDocumentIfNotExists** sotto la classe **ShippingPreference**.

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

4. Aggiungere quindi quanto segue al metodo **BasicOperations**.

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

5. Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

    Il terminale visualizza l'output seguente, a indicare che sono stati creati correttamente entrambi i record utente.

    ```
    Database and collection validation complete
    Created User 1
    Press any key to continue ...
    Created User 2
    Press any key to continue ...
    End of demo, press any key to exit.
    ```

## <a name="read-documents"></a>Leggere i documenti

1. Per leggere i documenti dal database, copiare il codice seguente e inserirlo alla fine del file Program.cs.
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

2.  Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**, dopo la riga `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.

    ```csharp
    await this.ReadUserDocument("Users", "WebCustomers", yanhe);
    ```

4. Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.

    ```
    dotnet run
    ```
    Il terminale visualizza l'output seguente, in cui l'output "Found user 1" indica che il documento è stato trovato.

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

## <a name="replace-documents"></a>Sostituire i documenti
Azure Cosmos DB supporta la sostituzione di documenti JSON. In questo caso, verrà aggiornato un record utente per registrare una modifica al cognome.

1. Copiare e incollare il metodo **ReplaceFamilyDocument** alla fine del file Program.cs.
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

2. Copiare e incollare il codice seguente alla fine del metodo **BasicOperations**, dopo la riga `await this.CreateUserDocumentIfNotExists("Users", "WebCustomers", nelapin);`.

    ```csharp
    yanhe.LastName = "Suh";
    await this.ReplaceUserDocument("Users", "WebCustomers", yanhe.LastName, yanhe);
    ```

4. Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.

    ```
    dotnet run
    ```
    Il terminale visualizza l'output seguente, in cui l'output "Replaced last name for Suh" indica che il documento è stato sostituito.

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

## <a name="delete-documents"></a>Eliminare i documenti

1. Copiare e incollare il metodo **DeleteUserDocument** sotto il metodo **ReplaceUserDocument**.
    
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

2. Copiare e incollare il codice seguente nel metodo **BasicOperations** sotto il codice di esecuzione della seconda query.

    ```csharp
    await this.DeleteUserDocument("Users", "WebCustomers", "1", yanhe);
    ```

3. Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.

    ```
    dotnet run
    ```

    Il terminale visualizza l'output seguente, in cui l'output "Replaced last name for Suh" indica che il documento è stato sostituito.

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

    Congratulazioni! L'eliminazione di un documento di Azure Cosmos DB è stata completata.

## <a name="summary"></a>Riepilogo

In questa unità sono stati creati, sostituiti ed eliminati documenti nel database di Azure Cosmos DB.