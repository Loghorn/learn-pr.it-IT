<span data-ttu-id="01a31-101">Aggiungere dati al database di Azure Cosmos DB dovrebbe essere semplice.</span><span class="sxs-lookup"><span data-stu-id="01a31-101">Adding data to your Azure Cosmos DB database should be simple.</span></span> <span data-ttu-id="01a31-102">Si apre il portale di Azure, si passa al database e si usa **Esplora dati** per aggiungere documenti JSON al database.</span><span class="sxs-lookup"><span data-stu-id="01a31-102">You open the Azure portal, navigate to your database, and use the **Data Explorer** to add json documents to the database.</span></span> <span data-ttu-id="01a31-103">Sono disponibili anche modalità più avanzate per l'aggiunta di dati, ma si inizierà con Esplora dati dal momento che è un ottimo strumento per acquisire familiarità con le funzionalità e i meccanismi interni di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="01a31-103">There are more advanced ways to add data, but we'll start here as the Data Explorer is a great tool to get you aquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="01a31-104">Introduzione a Esplora dati</span><span class="sxs-lookup"><span data-stu-id="01a31-104">What is the Data Explorer?</span></span>
<span data-ttu-id="01a31-105">Esplora dati di Azure Cosmos DB è uno strumento incorporato incluso nel portale di Azure che consente di gestire i dati archiviati in un database di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="01a31-105">The Azure Cosmos DB Data Explorer is a tool built included in the Azure Portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="01a31-106">Fornisce un'interfaccia utente per visualizzare ed esplorare raccolte dati, nonché modificare i documenti all'interno del database.</span><span class="sxs-lookup"><span data-stu-id="01a31-106">It provides UI to view and navigate data collections as well as edit documents within the db.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="01a31-107">Aggiungere dati con Esplora dati</span><span class="sxs-lookup"><span data-stu-id="01a31-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="01a31-108">Se si sta continuando dal modulo precedente, fare clic su **Esplora dati** nella finestra del portale di Azure e quindi fare clic su **Apri a schermo intero**.</span><span class="sxs-lookup"><span data-stu-id="01a31-108">If you're continuing from the previous module, click **Data Explorer** in the Azure portal window, and then click **Open Full Screen**.</span></span>

    <span data-ttu-id="01a31-109">In caso contrario, accedere al [portale di Azure](https://portal.azure.com/), fare clic su **Tutti i servizi** > **Database** > **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="01a31-109">Otherwise, sign in to the [Azure portal](https://portal.azure.com/), click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="01a31-110">Selezionare quindi l'account, fare clic su **Esplora dati** e quindi su **Apri a schermo intero**</span><span class="sxs-lookup"><span data-stu-id="01a31-110">Then select your account, click **Data Explorer**, and then click **Open Full Screen**</span></span>
 
   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media-draft/2-add-data/azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="01a31-112">Nella casella **Apri a schermo intero** fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="01a31-112">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="01a31-113">Il Web browser visualizza la nuova finestra di Esplora dati a schermo intero, che offre maggior spazio e un ambiente dedicato per l'uso del database.</span><span class="sxs-lookup"><span data-stu-id="01a31-113">The web browser displays the new full screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="01a31-114">Per creare un nuovo documento JSON, fare clic su **Nuovo documento**.</span><span class="sxs-lookup"><span data-stu-id="01a31-114">To create a new json document, click **New Document**.</span></span>

   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media-draft/2-add-data/azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="01a31-116">Aggiungere ora un documento alla raccolta con la struttura seguente: è sufficiente copiare e incollare il codice seguente nella scheda **Documenti**.</span><span class="sxs-lookup"><span data-stu-id="01a31-116">Now add a document to the collection with the following structure, just copy and paste the following code into the **Documents** tab.</span></span>

     ```
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "shipping": {
            "weight": 1,
            "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
           }
        }
     ```

5. <span data-ttu-id="01a31-117">Dopo avere aggiunto il codice JSON alla scheda **Documenti**, fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="01a31-117">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![Copiare i dati JSON e fare clic su Salva in Esplora dati nel portale di Azure](../media-draft/2-add-data/azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="01a31-119">Creare e salvare un altro documento copiando l'oggetto JSON seguente in Esplora dati e facendo clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="01a31-119">Create and save one more document by copying the following json object into Data Explorer and clicking **Save**.</span></span>

     ```
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
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

7. <span data-ttu-id="01a31-120">Verificare che i documenti siano stati salvati facendo clic su **Documenti** nel menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="01a31-120">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span> 

    <span data-ttu-id="01a31-121">In Esplora dati i due documenti vengono visualizzati nella scheda **Documenti**.</span><span class="sxs-lookup"><span data-stu-id="01a31-121">Data Explorer displays the two documents in the **Documents** tab.</span></span>

## <a name="summary"></a><span data-ttu-id="01a31-122">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="01a31-122">Summary</span></span>

<span data-ttu-id="01a31-123">In questo modulo tramite Esplora dati sono stati aggiunti due documenti al database, ognuno dei quali rappresenta un prodotto nel catalogo.</span><span class="sxs-lookup"><span data-stu-id="01a31-123">In this module you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="01a31-124">Esplora dati è un buon metodo per creare e modificare documenti, oltre che per iniziare a usare Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="01a31-124">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>  
