Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente. Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.

## <a name="create-a-stored-procedure-in-your-app"></a>Creare una stored procedure nell'app

In questa stored procedure, OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine. Il totale degli ordini viene calcolato dalla somma degli articoli nell'ordine, meno eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.

1. In Visual Studio Code, nella scheda Azure, espandere **modulo di apprendimento (SQL)** > **Utenti** > **WebCustomers**, fare clic con il pulsante destro del mouse su **Stored procedure** e quindi fare clic su **Crea Stored Procedure**.

1. Nella casella di testo nella parte superiore della schermata, digitare *UpdateOrderTotal* e premere Invio per assegnare un nome alla stored procedure.

1. Espandere **Stored procedure** e fare clic su **UpdateOrderTotal**.

1. Per impostazione predefinita, viene fornita una stored procedure che recupera il primo elemento.

1. Per eseguire questa stored procedure dall'applicazione, aggiungere il codice seguente al file Program.cs.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "sample"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```
    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->

1. Copiare ora il codice seguente e incollarlo alla fine del metodo **BasicOperations**.

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. Eseguire il comando seguente nel terminale integrato per eseguire l'esempio con la stored procedure.

    ```
    dotnet run
    ```
    La console mostra l'output che indica il completamento della stored procedure.

## <a name="clean-up"></a>Eseguire la pulizia

Se si prevede di continuare a utilizzare i moduli in questo percorso di apprendimento, ignorare il processo di pulizia, altrimenti usare la procedura seguente per eliminare le risorse per evitare di incorrere in addebiti per l'uso del servizio.

1. Nel portale di Azure selezionare **Gruppi di risorse** all'estrema sinistra e quindi selezionare il gruppo di risorse creato.  

    Se il menu a sinistra è compresso, fare clic sul ![pulsante Espandi](../media/5-javascript-programming/expand.png) per espanderlo.

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources-select.png)

1. Nella nuova finestra selezionare il gruppo di risorse e quindi fare clic su **Elimina gruppo di risorse**.

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources.png)

1. Nella nuova finestra digitare il nome del gruppo di risorse da eliminare e quindi fare clic su **Elimina**.

## <a name="summary"></a>Riepilogo

In questo modulo è stata creata un'applicazione console .NET Core che crea, aggiorna ed elimina i record utente, esegue query per gli utenti con SQL e LINQ ed esegue una stored procedure per eseguire query per gli elementi nel database.