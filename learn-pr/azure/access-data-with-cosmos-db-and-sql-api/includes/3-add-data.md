L'aggiunta di dati al database di Azure Cosmos DB è un'operazione semplice. Si apre il portale di Azure, si passa al database e si usa Esplora dati per aggiungere documenti JSON al database. Sono disponibili anche modalità più avanzate per l'aggiunta di dati, ma si inizierà con Esplora dati dal momento che è un ottimo strumento per acquisire familiarità con le funzionalità e i meccanismi interni di Azure Cosmos DB.

## <a name="what-is-the-data-explorer"></a>Che cos'è Esplora dati?
Esplora dati di Azure Cosmos DB è uno strumento incorporato nel portale di Azure che consente di gestire i dati archiviati in un database di Azure Cosmos DB. Offre un'interfaccia utente per la visualizzazione e l'esplorazione di raccolte di dati, nonché per la modifica di documenti all'interno del database, l'esecuzione di query sui dati e la creazione e l'esecuzione di stored procedure.

## <a name="add-data-using-the-data-explorer"></a>Aggiungere dati con Esplora dati

1. Accedere al [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

    > [!IMPORTANT]
    > Accedere al portale di Azure e alla sandbox con lo stesso account.
    >
    > Accedere al portale di Azure usando il collegamento precedente per assicurarsi di connettersi alla sandbox, che fornisce l'accesso a una sottoscrizione Concierge.

1. Fare clic su **Tutti i servizi** > **Database** > **Azure Cosmos DB**. Selezionare ora l'account, fare clic su **Esplora dati** e quindi su **Apri a schermo intero**.

   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media/3-azure-cosmosdb-data-explorer-full-screen.png)

2. Nella casella **Apri a schermo intero** fare clic su **Apri**.

    Il Web browser visualizza la nuova finestra Esplora dati a schermo intero, che offre maggiore spazio e un ambiente dedicato per l'uso del database.

3. Per creare un nuovo documento JSON, nel riquadro dell'API SQL espandere **Abbigliamento**, fare clic su **Documenti**, quindi su **Nuovo documento**.

   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media/3-azure-cosmosdb-data-explorer-new-document.png)

4. Aggiungere ora alla raccolta un documento con la struttura seguente. È sufficiente copiare e incollare il codice seguente nella scheda **Documenti**, sovrascrivendo il contenuto corrente:

     ```json
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "price": "14.99",
        "shipping": {
            "weight": 1,
            "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
           }
        }
    }
     ```

5. Una volta aggiunto il codice JSON alla scheda **Documenti**, fare clic su **Salva**.

    ![Copiare i dati JSON e fare clic su Salva in Esplora dati nel portale di Azure](../media/3-azure-cosmosdb-data-explorer-save-document.png)

6. Creare e salvare un altro documento facendo di nuovo clic su **Nuovo documento**, copiando l'oggetto JSON seguente in Esplora dati e facendo clic su **Salva**.

     ```json
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
        "price": "49.99",
        "shipping": {
            "weight": 2,
            "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
           }
        }
    }
     ```

7. Verificare che i documenti siano stati salvati facendo clic su **Documenti** nel menu a sinistra.

    In Esplora dati i due documenti vengono visualizzati nella scheda **Documenti**.

In questa unità tramite Esplora dati sono stati aggiunti al database due documenti, ognuno dei quali rappresenta un prodotto nel catalogo. Esplora dati è un buon metodo per creare e modificare documenti, oltre che per iniziare a usare Azure Cosmos DB.
