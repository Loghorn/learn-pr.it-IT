<span data-ttu-id="cbc40-101">L'aggiunta di dati al database di Azure Cosmos DB è un'operazione semplice.</span><span class="sxs-lookup"><span data-stu-id="cbc40-101">Adding data to your Azure Cosmos DB database is simple.</span></span> <span data-ttu-id="cbc40-102">Si apre il portale di Azure, si passa al database e si usa Esplora dati per aggiungere documenti JSON al database.</span><span class="sxs-lookup"><span data-stu-id="cbc40-102">You open the Azure portal, navigate to your database, and use the Data Explorer to add JSON documents to the database.</span></span> <span data-ttu-id="cbc40-103">Sono disponibili anche modalità più avanzate per l'aggiunta di dati, ma si inizierà con Esplora dati dal momento che è un ottimo strumento per acquisire familiarità con le funzionalità e i meccanismi interni di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cbc40-103">There are more advanced ways to add data, but we'll start here because the Data Explorer is a great tool to get you acquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="cbc40-104">Che cos'è Esplora dati?</span><span class="sxs-lookup"><span data-stu-id="cbc40-104">What is the Data Explorer?</span></span>
<span data-ttu-id="cbc40-105">Esplora dati di Azure Cosmos DB è uno strumento incorporato nel portale di Azure che consente di gestire i dati archiviati in un database di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cbc40-105">The Azure Cosmos DB Data Explorer is a tool included in the Azure portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="cbc40-106">Offre un'interfaccia utente per la visualizzazione e l'esplorazione di raccolte di dati, nonché per la modifica di documenti all'interno del database, l'esecuzione di query sui dati e la creazione e l'esecuzione di stored procedure.</span><span class="sxs-lookup"><span data-stu-id="cbc40-106">It provides a UI for viewing and navigating data collections, as well as for editing documents within the database, querying data, and creating and running stored procedures.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="cbc40-107">Aggiungere dati con Esplora dati</span><span class="sxs-lookup"><span data-stu-id="cbc40-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="cbc40-108">Accedere al [portale di Azure per sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="cbc40-108">Sign into the [Azure portal for sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cbc40-109">Accedere al portale di Azure e alla sandbox con lo stesso account.</span><span class="sxs-lookup"><span data-stu-id="cbc40-109">Login to the Azure portal and the sandbox with the same account.</span></span>
    >
    > <span data-ttu-id="cbc40-110">Accedere al portale di Azure usando il collegamento precedente per assicurarsi di connettersi alla sandbox, che fornisce l'accesso a una sottoscrizione Concierge.</span><span class="sxs-lookup"><span data-stu-id="cbc40-110">Login to the Azure portal using the link above to ensure you are connected to the sandbox, which provides access to a Concierge Subscription.</span></span>

1. <span data-ttu-id="cbc40-111">Fare clic su **Tutti i servizi** > **Database** > **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-111">Click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="cbc40-112">Selezionare ora l'account, fare clic su **Esplora dati** e quindi su **Apri a schermo intero**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-112">Then select your account, click **Data Explorer**, and then click **Open Full Screen**.</span></span>

   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media/3-azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="cbc40-114">Nella casella **Apri a schermo intero** fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-114">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="cbc40-115">Il Web browser visualizza la nuova finestra Esplora dati a schermo intero, che offre maggiore spazio e un ambiente dedicato per l'uso del database.</span><span class="sxs-lookup"><span data-stu-id="cbc40-115">The web browser displays the new full-screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="cbc40-116">Per creare un nuovo documento JSON, nel riquadro dell'API SQL espandere **Abbigliamento**, fare clic su **Documenti**, quindi su **Nuovo documento**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-116">To create a new JSON document, in the SQL API pane, expand **Clothing**, click **Documents**, then click **New Document**.</span></span>

   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media/3-azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="cbc40-118">Aggiungere ora alla raccolta un documento con la struttura seguente.</span><span class="sxs-lookup"><span data-stu-id="cbc40-118">Now, add a document to the collection with the following structure.</span></span> <span data-ttu-id="cbc40-119">È sufficiente copiare e incollare il codice seguente nella scheda **Documenti**, sovrascrivendo il contenuto corrente:</span><span class="sxs-lookup"><span data-stu-id="cbc40-119">Just copy and paste the following code into the **Documents** tab, overwriting the current content:</span></span>

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

5. <span data-ttu-id="cbc40-120">Una volta aggiunto il codice JSON alla scheda **Documenti**, fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-120">Once you've added the JSON to the **Documents** tab, click **Save**.</span></span>

    ![Copiare i dati JSON e fare clic su Salva in Esplora dati nel portale di Azure](../media/3-azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="cbc40-122">Creare e salvare un altro documento facendo di nuovo clic su **Nuovo documento**, copiando l'oggetto JSON seguente in Esplora dati e facendo clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-122">Create and save one more document clicking **New Document** again, and copying the following JSON object into Data Explorer and clicking **Save**.</span></span>

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

7. <span data-ttu-id="cbc40-123">Verificare che i documenti siano stati salvati facendo clic su **Documenti** nel menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="cbc40-123">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span>

    <span data-ttu-id="cbc40-124">In Esplora dati i due documenti vengono visualizzati nella scheda **Documenti**.</span><span class="sxs-lookup"><span data-stu-id="cbc40-124">Data Explorer displays the two documents in the **Documents** tab.</span></span>

<span data-ttu-id="cbc40-125">In questa unità tramite Esplora dati sono stati aggiunti al database due documenti, ognuno dei quali rappresenta un prodotto nel catalogo.</span><span class="sxs-lookup"><span data-stu-id="cbc40-125">In this unit, you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="cbc40-126">Esplora dati è un buon metodo per creare e modificare documenti, oltre che per iniziare a usare Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cbc40-126">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>
