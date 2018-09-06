<span data-ttu-id="a1b8f-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Dopo aver creato documenti nell'applicazione, è possibile eseguire query per recuperarli dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Now that you've created documents in your application, let's query them from your application.</span></span> <span data-ttu-id="a1b8f-102">Azure Cosmos DB usa query SQL e query LINQ.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-102">Azure Cosmos DB uses SQL queries and LINQ queries.</span></span> <span data-ttu-id="a1b8f-103">È possibile trovare informazioni generali sulle query SQL supportate da Azure Cosmos DB nel modulo **Add data and query data in your database** (Aggiungere dati e dati di query nel database). Questa unità è incentrata sull'esecuzione di query SQL e LINQ dall'applicazione invece che dal portale.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-103">General information about the SQL queries Azure Cosmos DB supports is in **Add data and query data in your database** module; this unit focuses on running SQL queries and LINQ queries from your application, as opposed to the portal.</span></span>

<span data-ttu-id="a1b8f-104">Per testare queste query verranno usati i documenti utente creati per l'applicazione del rivenditore online.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-104">We'll use the user documents you've created for your online retailer application to test these queries.</span></span>

## <a name="linq-query-basics"></a><span data-ttu-id="a1b8f-105">Nozioni di base sulle query LINQ</span><span class="sxs-lookup"><span data-stu-id="a1b8f-105">LINQ query basics</span></span>

<span data-ttu-id="a1b8f-106">LINQ è un modello di programmazione .NET che esprime i calcoli come query su flussi di oggetti.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-106">LINQ is a .NET programming model that expresses computations as queries on streams of objects.</span></span> <span data-ttu-id="a1b8f-107">È possibile creare un oggetto **IQueryable** che esegue query direttamente su Azure Cosmos DB, il quale a sua volta traduce la query LINQ in una query di Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-107">You can create an **IQueryable** object that directly queries Azure Cosmos DB, which translates the LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="a1b8f-108">La query viene quindi passata al server di Azure Cosmos DB per recuperare un set di risultati in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-108">The query is then passed to the Azure Cosmos DB server to retrieve a set of results in JSON format.</span></span> <span data-ttu-id="a1b8f-109">I risultati restituiti vengono deserializzati in un flusso di oggetti .NET sul lato client.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-109">The returned results are deserialized into a stream of .NET objects on the client side.</span></span> <span data-ttu-id="a1b8f-110">Molti sviluppatori preferiscono le query LINQ, in quanto offrono un singolo modello di programmazione coerente per l'utilizzo degli oggetti nel codice dell'applicazione e per esprimere la logica di query in esecuzione nel database.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-110">Many developers prefer LINQ queries, as they provide a single consistent programming model across how they work with objects in application code and how they express query logic running in the database.</span></span>

<span data-ttu-id="a1b8f-111">La tabella seguente mostra come le query LINQ vengono convertite in SQL.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-111">The following table shows how LINQ queries are translated into SQL.</span></span>

| <span data-ttu-id="a1b8f-112">Espressione LINQ</span><span class="sxs-lookup"><span data-stu-id="a1b8f-112">LINQ expression</span></span> | <span data-ttu-id="a1b8f-113">Conversione SQL</span><span class="sxs-lookup"><span data-stu-id="a1b8f-113">SQL translation</span></span> |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a><span data-ttu-id="a1b8f-114">Eseguire query SQL e LINQ</span><span class="sxs-lookup"><span data-stu-id="a1b8f-114">Run SQL and LINQ queries</span></span>

1. <span data-ttu-id="a1b8f-115">L'esempio seguente mostra come si potrebbe eseguire una query in SQL, LINQ o LINQ lambda dal codice .NET.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-115">The following sample shows how a query could be performed in SQL, LINQ, or LINQ lambda from your .NET code.</span></span> <span data-ttu-id="a1b8f-116">Copiare il codice e aggiungerlo alla fine del file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-116">Copy the code and add it to the end of the Program.cs file.</span></span>

    ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };
    
            // Here we find nelapin via their LastName
            IQueryable<User> userQuery = this.client.CreateDocumentQuery<User>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(u => u.LastName == "Pindakova");
    
            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (User user in userQuery)
            {
                Console.WriteLine("\tRead {0}", user);
            }
    
            // Now execute the same query via direct SQL
            IQueryable<User> userQueryInSql = this.client.CreateDocumentQuery<User>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM User WHERE User.LastName = 'Pindakova'",
                    queryOptions);
    
            Console.WriteLine("Running direct SQL query...");
            foreach (User user in userQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", user);
            }
    
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }
    ```

2. <span data-ttu-id="a1b8f-117">Copiare e incollare il codice seguente nel metodo **BasicOperations**, prima della riga `await this.DeleteUserDocument("Users", "WebCustomers", "1");`.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-117">Copy and paste the following code to your **BasicOperations** method, before the `await this.DeleteUserDocument("Users", "WebCustomers", "1");` line.</span></span>

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

3. <span data-ttu-id="a1b8f-118">Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-118">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>
    
    ```
    dotnet run
    ```

    <span data-ttu-id="a1b8f-119">La console visualizza l'output seguente.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-119">The console displays the following output.</span></span>

    ```
    Running LINQ query...
    Running direct SQL query...
    Press any key to continue ...
    ```

    <span data-ttu-id="a1b8f-120">Congratulazioni!</span><span class="sxs-lookup"><span data-stu-id="a1b8f-120">Congratulations!</span></span> <span data-ttu-id="a1b8f-121">È stata completata correttamente la raccolta di utenti di Azure Cosmos DB tramite query SQL e LINQ.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-121">You have successfully your Azure Cosmos DB collection of users by using SQL and LINQ queries.</span></span>

## <a name="summary"></a><span data-ttu-id="a1b8f-122">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a1b8f-122">Summary</span></span>

<span data-ttu-id="a1b8f-123">In questa unità sono state presentate informazioni generali sulle query LINQ e quindi è stata aggiunta una query LINQ e SQL all'applicazione per recuperare i record utente.</span><span class="sxs-lookup"><span data-stu-id="a1b8f-123">In this unit you learned about LINQ queries, and then added a LINQ and SQL query to your application to retrieve user records.</span></span>