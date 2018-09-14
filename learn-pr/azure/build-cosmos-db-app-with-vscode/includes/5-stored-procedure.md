Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente. Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.

## <a name="create-a-stored-procedure-in-your-app"></a>Creare una stored procedure nell'app

In questa stored procedure OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine. Il totale degli ordini viene calcolato sommando gli articoli nell'ordine, sottraendo eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.

1. In Visual Studio Code nel **Azure: Cosmos DB** scheda, quindi espandere l'account Azure Cosmos DB > **Users** > **WebCustomers** e quindi fare doppio clic su  **Le stored procedure** e quindi fare clic su **Create Stored Procedure**.

1. Nella casella di testo nella parte superiore della schermata digitare *UpdateOrderTotal* e premere Invio per assegnare un nome alla stored procedure.

1. Nel **Azure: Cosmos DB** scheda, quindi espandere **Stored procedure** e fare clic su **UpdateOrderTotal**.

    Per impostazione predefinita, viene offerta una stored procedure che recupera il primo elemento.

1. Per eseguire questa stored procedure dall'applicazione, aggiungere il codice seguente al file Program.cs.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```

1. A questo punto copiare il codice seguente e incollarlo prima la `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` riga nel **BasicOperations** (metodo).

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. Eseguire il comando seguente nel terminale integrato per eseguire l'esempio con la stored procedure.

    ```
    dotnet run
    ```
    La console visualizza l'output indicando il completamento della stored procedure.

## <a name="summary"></a>Riepilogo

In questo modulo è stata creata un'applicazione console .NET Core che crea, aggiorna ed elimina i record utente, esegue query per gli utenti con SQL e LINQ ed esegue una stored procedure per eseguire query per gli elementi nel database.

[!include[](../../../includes/azure-sandbox-cleanup.md)]