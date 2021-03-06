Ora che si è appreso quali tipi di query è possibile creare, si userà Esplora dati nel portale di Azure per recuperare e filtrare i dati dei prodotti.

Si noti che per impostazione predefinita nella finestra Esplora dati la query nella scheda **Documento** è impostata su `SELECT * FROM c`, come illustrato nella figura seguente. La query predefinita recupera e visualizza tutti i documenti nella raccolta.

![La query predefinita in Esplora dati è SELECT * FROM c](../media/5-azure-cosmosdb-data-explorer-query.png)

## <a name="create-a-new-query"></a>Creare una nuova query

1. In Esplora dati fare clic su **Nuova query SQL**. Si noti che la query predefinita nella nuova scheda **Query 1** è nuovamente `SELECT * from c`, che restituirà tutti i documenti nella raccolta. 

1. Fare clic su **Esegui query**. Questa query restituisce tutti i risultati nel database.

    ![Modificare la query predefinita aggiungendo ORDER BY c._ts DESC e facendo clic su Applica filtro](../media/5-azure-cosmosdb-data-explorer-edit-query.png)

2. A questo punto è possibile eseguire alcune query illustrate nell'unità precedente. Nella scheda della query eliminare `SELECT * from c`, copiare e incollare la query seguente e quindi fare clic su **Esegui query**:

    ```sql
    SELECT * 
    FROM Products p 
    WHERE p.id ="1"
    ```

    I risultati restituiscono il prodotto dove `productId` è 1.

    ![Eseguire una query per recuperare l'ID 1](../media/5-azure-cosmosdb-data-explorer-query-by-id.png)

3. Eliminare la query precedente, copiare e incollare la query seguente e fare clic su **Esegui query**. Questa query restituisce il prezzo, la descrizione e l'ID prodotto per tutti i prodotti, ordinati in base al prezzo, in ordine crescente.
 
    ```sql
    SELECT p.price, p.description, p.productId 
    FROM Products p 
    ORDER BY p.price ASC
    ```

## <a name="summary"></a>Riepilogo

Sono state ora completate alcune query di base sui dati in Azure Cosmos DB. 