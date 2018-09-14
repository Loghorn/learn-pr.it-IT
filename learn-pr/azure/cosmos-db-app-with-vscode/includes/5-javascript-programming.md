Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente. Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.

## <a name="create-a-stored-procedure-in-your-app"></a>Creare una stored procedure nell'app

In questa stored procedure, OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine. Il totale degli ordini viene calcolato dalla somma degli articoli nell'ordine, meno eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.

1. Copiare il codice seguente e incollarlo alla fine del file Program.cs.

    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->
    ```csharp
    private async Task UpdateOrderTotal(string databaseName, string collectionName, Order orderId)
    {
    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
    }
    ```

2. Copiare ora il codice seguente e incollarlo alla fine del metodo **BasicOperations**.

    ```
    await this.UpdateOrderTotal("Users", "WebCustomers", "5-6235");
    ```

3. Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.

    ```
    dotnet run
    ```

    La console visualizza l'output seguente.

    ```
    Order total is 90.85.
    Press any key to continue ...
    ```

## <a name="clean-up"></a>Eseguire la pulizia

Se si prevede di continuare a utilizzare i moduli in questo percorso di apprendimento, ignorare il processo di pulizia, altrimenti usare la procedura seguente per eliminare le risorse per evitare di incorrere in addebiti per l'uso del servizio.

1. Nel portale di Azure selezionare **Gruppi di risorse** all'estrema sinistra, quindi selezionare il gruppo di risorse creato.  

    Se il menu a sinistra è compresso, fare clic sul ![pulsante Espandi](../media/5-javascript-programming/expand.png) per espanderlo.

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources-select.png)

2. Nella nuova finestra selezionare il gruppo di risorse e fare clic su **Elimina gruppo di risorse**.

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources.png)

3. Nella nuova finestra digitare il nome del gruppo di risorse da eliminare, quindi fai clic su **Elimina**.

## <a name="summary"></a>Riepilogo

In questo modulo è stata creata un'applicazione console .NET Core che crea, aggiorna ed elimina i record utente, esegue query per gli utenti con SQL e LINQ ed esegue una stored procedure per creare un totale degli ordini che tiene conto dei bonus degli utenti e dei coupon.