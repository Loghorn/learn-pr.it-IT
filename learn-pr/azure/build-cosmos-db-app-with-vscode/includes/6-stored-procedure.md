Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente. Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.

## <a name="create-a-stored-procedure-in-your-app"></a>Creare una stored procedure nell'app

In questa stored procedure OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine. Il totale degli ordini viene calcolato sommando gli articoli nell'ordine, sottraendo eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.

1. Nella scheda **Azure: Cosmos DB** in Visual Studio Code espandere l'account di Azure Cosmos DB > **Utenti** > **WebCustomers**, fare clic con il pulsante destro del mouse su **Stored procedure** e quindi scegliere **Crea stored procedure**.

1. Nella casella di testo nella parte superiore della schermata digitare `UpdateOrderTotal` e premere INVIO per assegnare un nome alla stored procedure.

1. Nella scheda **Azure: Cosmos DB** espandere **Stored procedure** e fare clic su **UpdateOrderTotal**.

    Per impostazione predefinita, viene offerta una stored procedure che recupera il primo elemento.

1. Per eseguire questa stored procedure predefinita dall'applicazione, aggiungere il codice seguente al file **Program.cs**.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
        Console.WriteLine("Stored procedure complete");
    }
    ```

1. Copiare ora il codice seguente e incollarlo prima della riga `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` nel metodo **BasicOperations**.

    ```csharp
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. Eseguire il comando seguente nel terminale integrato per eseguire l'esempio con la stored procedure.

    ```bash
    dotnet run
    ```

La console visualizza l'output indicando il completamento della stored procedure.
