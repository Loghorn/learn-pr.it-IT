<!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Dopo aver creato documenti nell'applicazione, è possibile eseguire query per recuperarli dall'applicazione. Azure Cosmos DB usa query SQL e query LINQ. Questa unità è incentrata sull'esecuzione di query SQL e di query LINQ dall'applicazione, anziché dal portale.

Per testare queste query verranno usati i documenti utente creati per l'applicazione del rivenditore online.

## <a name="linq-query-basics"></a>Nozioni di base sulle query LINQ

LINQ è un modello di programmazione .NET che esprime i calcoli come query su flussi di oggetti. È possibile creare un oggetto **IQueryable** che esegue query direttamente su Azure Cosmos DB, il quale a sua volta traduce la query LINQ in una query di Cosmos DB. La query viene quindi passata al server di Azure Cosmos DB per recuperare un set di risultati in formato JSON. I risultati restituiti vengono deserializzati in un flusso di oggetti .NET sul lato client. Molti sviluppatori preferiscono le query LINQ, in quanto offrono un singolo modello di programmazione coerente per l'utilizzo degli oggetti nel codice dell'applicazione e per esprimere la logica di query in esecuzione nel database.

La tabella seguente mostra come le query LINQ vengono convertite in SQL.

| Espressione LINQ | Conversione SQL |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a>Eseguire query SQL e LINQ

1. L'esempio seguente mostra come si potrebbe eseguire una query in SQL, LINQ o LINQ lambda dal codice .NET. Copiare il codice e aggiungerlo alla fine del file Program.cs.

    ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1, EnableCrossPartitionQuery = true };
    
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
                    "SELECT * FROM User WHERE User.lastName = 'Pindakova'", queryOptions );
    
            Console.WriteLine("Running direct SQL query...");
            foreach (User user in userQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", user);
            }
    
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }
    ```

1. Copiare e incollare il codice seguente nel metodo **BasicOperations**, prima della riga `await this.DeleteUserDocument("Users", "WebCustomers", "1");`.

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

1. Nel terminale integrato eseguire questo comando.
    
    ```
    dotnet run
    ```

    La console visualizza l'output delle query LINQ e SQL.

## <a name="summary"></a>Riepilogo

In questa unità sono state presentate informazioni generali sulle query LINQ e quindi è stata aggiunta una query LINQ e SQL all'applicazione per recuperare i record utente.