<span data-ttu-id="a9f1b-101">Aggiungere dati al database di Azure Cosmos DB dovrebbe essere semplice.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-101">Adding data to your Azure Cosmos DB database should be simple.</span></span> <span data-ttu-id="a9f1b-102">Si apre il portale di Azure, si passa al database e si usa Esplora dati per aggiungere documenti JSON al database.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-102">You open the Azure portal, navigate to your database, and use the Data Explorer to add JSON documents to the database.</span></span> <span data-ttu-id="a9f1b-103">Sono disponibili anche modalità più avanzate per l'aggiunta di dati, ma si inizierà con Esplora dati dal momento che è un ottimo strumento per acquisire familiarità con le funzionalità e i meccanismi interni di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-103">There are more advanced ways to add data, but we'll start here because the Data Explorer is a great tool to get you acquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="a9f1b-104">Che cos'è Esplora dati?</span><span class="sxs-lookup"><span data-stu-id="a9f1b-104">What is the Data Explorer?</span></span>
<span data-ttu-id="a9f1b-105">Esplora dati di Azure Cosmos DB è uno strumento incorporato nel portale di Azure che consente di gestire i dati archiviati in un database di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-105">The Azure Cosmos DB Data Explorer is a tool included in the Azure portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="a9f1b-106">Fornisce un'interfaccia utente per la visualizzazione e l'esplorazione di raccolte di dati, nonché per la modifica di documenti all'interno del database.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-106">It provides a UI for viewing and navigating data collections, as well as for editing documents within the database.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="a9f1b-107">Aggiungere dati con Esplora dati</span><span class="sxs-lookup"><span data-stu-id="a9f1b-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="a9f1b-108">Se si sta continuando dal modulo precedente, fare clic su **Esplora dati** nella finestra del portale di Azure e quindi fare clic su **Apri a schermo intero**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-108">If you're continuing from the previous module, click **Data Explorer** in the Azure portal window, and then click **Open Full Screen**.</span></span>

    <span data-ttu-id="a9f1b-109">In caso contrario, accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true), fare clic su **Tutti i servizi** > **Database** > **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-109">Otherwise, sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true), click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="a9f1b-110">Selezionare quindi l'account, fare clic su **Esplora dati** e quindi su **Apri a schermo intero**</span><span class="sxs-lookup"><span data-stu-id="a9f1b-110">Then select your account, click **Data Explorer**, and then click **Open Full Screen**</span></span>
 
   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media-draft/3-azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="a9f1b-112">Nella casella **Apri a schermo intero** fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-112">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="a9f1b-113">Il browser Web visualizza la nuova finestra di Esplora dati a schermo intero, che offre maggior spazio e un ambiente dedicato per l'uso del database.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-113">The web browser displays the new full-screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="a9f1b-114">Per creare un nuovo documento JSON, fare clic su **Nuovo documento**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-114">To create a new JSON document, click **New Document**.</span></span>

   ![Creare nuovi documenti in Esplora dati nel portale di Azure](../media-draft/3-azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="a9f1b-116">Aggiungere ora un documento alla raccolta con la struttura seguente.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-116">Now, add a document to the collection with the following structure.</span></span> <span data-ttu-id="a9f1b-117">Copiare e incollare il codice seguente nella scheda **Documenti**:</span><span class="sxs-lookup"><span data-stu-id="a9f1b-117">Just copy and paste the following code into the **Documents** tab:</span></span>

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

5. <span data-ttu-id="a9f1b-118">Dopo avere aggiunto il codice JSON alla scheda **Documenti**, fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-118">Once you've added the JSON to the **Documents** tab, click **Save**.</span></span>

    ![Copiare i dati JSON e fare clic su Salva in Esplora dati nel portale di Azure](../media-draft/3-azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="a9f1b-120">Creare e salvare un altro documento copiando l'oggetto JSON seguente in Esplora dati e facendo clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-120">Create and save one more document by copying the following JSON object into Data Explorer and clicking **Save**.</span></span>

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

7. <span data-ttu-id="a9f1b-121">Verificare che i documenti siano stati salvati facendo clic su **Documenti** nel menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-121">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span> 

    <span data-ttu-id="a9f1b-122">Esplora dati visualizza i due documenti nella scheda **Documenti**.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-122">Data Explorer displays the two documents in the **Documents** tab.</span></span>

## <a name="summary"></a><span data-ttu-id="a9f1b-123">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a9f1b-123">Summary</span></span>

<span data-ttu-id="a9f1b-124">In questo modulo sono stati aggiunti due documenti al database usando Esplora dati, ognuno dei quali rappresenta un prodotto nel catalogo.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-124">In this module, you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="a9f1b-125">Esplora dati è un buon metodo per creare e modificare documenti, oltre che per iniziare a usare Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a9f1b-125">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>  
