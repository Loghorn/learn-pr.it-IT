<span data-ttu-id="e5a4f-101">Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> 

<span data-ttu-id="e5a4f-102">Nel caso di un'applicazione di vendita online, quando un utente invia un ordine e decide di usare un codice promozionale, un credito o un bonus (o tutti e tre in una volta sola), è necessario eseguire una query sul relativo account per verificare queste opzioni, eseguire gli aggiornamenti necessari per indicare che queste opzioni sono state usate, aggiornare il totale dell'ordine ed elaborare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-102">For your online retail application, when a user places an order and wants to use a coupon code, a credit, or a dividend (or all three at once), you need to query their account for those options, make updates to their account indicating they used them, update the order total, and process the order.</span></span>

<span data-ttu-id="e5a4f-103">Tutte queste operazioni devono essere eseguite contemporaneamente, all'interno di una singola transazione.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-103">All of these actions need to happen at the same time, within a single transaction.</span></span> <span data-ttu-id="e5a4f-104">Se l'utente sceglie di annullare l'ordine, sarà necessario eseguire il rollback delle modifiche e non modificare le informazioni dell'account, in modo che eventuali codici promozionali, crediti e bonus siano disponibili per l'acquisto successivo.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-104">If the user chooses to cancel the order, you want to roll back the changes and not modify their account information, so that their coupon codes, credits, and dividends are available for their next purchase.</span></span>

<span data-ttu-id="e5a4f-105">Per eseguire queste transazioni in Azure Cosmos DB è possibile usare stored procedure e funzioni definite dall'utente.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-105">The way to perform these transactions in Azure Cosmos DB is by using stored procedures and user-defined functions (UDFs).</span></span> <span data-ttu-id="e5a4f-106">Le stored procedure sono l'unico modo per assicurare transazioni ACID (atomicità, coerenza, isolamento, durabilità), dal momento che vengono eseguite nel server e sono pertanto indicate come programmazione sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-106">Stored procedures are the only way to ensure ACID (Atomicity, Consistency, Isolation, Durability) transactions because they are run on the server, and are thus referred to as server-side programming.</span></span> <span data-ttu-id="e5a4f-107">Anche le funzioni definite dall'utente sono archiviate nel server e vengono usate durante le query per eseguire la logica di calcolo sui valori o i documenti all'interno della query.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-107">UDFs are also stored on the server and are used during queries to perform computational logic on values or documents within the query.</span></span> 

<span data-ttu-id="e5a4f-108">In questo modulo si otterranno informazioni sulle stored procedure e le funzioni definite dall'utente e quindi ne verranno eseguite alcune nel portale.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-108">In this module, you'll learn about stored procedures and UDFs, and then run some in the portal.</span></span>

## <a name="stored-procedure-basics"></a><span data-ttu-id="e5a4f-109">Nozioni di base sulle stored procedure</span><span class="sxs-lookup"><span data-stu-id="e5a4f-109">Stored procedure basics</span></span>

<span data-ttu-id="e5a4f-110">Le stored procedure consentono di eseguire transazioni complesse su documenti e proprietà.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-110">Stored procedures perform complex transactions on documents and properties.</span></span> <span data-ttu-id="e5a4f-111">Le stored procedure vengono scritte in Javascript e sono archiviate in una raccolta in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-111">Stored procedures are written in JavaScript and are stored in a collection on Azure Cosmos DB.</span></span> <span data-ttu-id="e5a4f-112">Eseguendo le stored procedure sul motore di database e in prossimità dei dati, è possibile migliorare le prestazioni rispetto alla programmazione sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-112">By performing the stored procedures on the database engine and close to the data, you can improve performance over client-side programming.</span></span>

<span data-ttu-id="e5a4f-113">Le stored procedure sono l'unico modo per ottenere transazioni atomiche all'interno di Azure Cosmos DB. Gli SDK sul lato client non supportano le transazioni.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-113">Stored procedures are the only way to achieve atomic transactions within Azure Cosmos DB; the client-side SDKs do not support transactions.</span></span>

<span data-ttu-id="e5a4f-114">È anche consigliabile eseguire operazioni batch nelle stored procedure per via della minore necessità di creare transazioni distinte.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-114">Performing batch operations in stored procedures is also recommended because of the reduced need to create separate transactions.</span></span>

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a><span data-ttu-id="e5a4f-115">Esempio di stored procedure</span><span class="sxs-lookup"><span data-stu-id="e5a4f-115">Stored procedure example</span></span>

<span data-ttu-id="e5a4f-116">L'esempio seguente è una semplice stored procedure HelloWorld che ottiene il contesto corrente e invia una risposta che visualizza "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="e5a4f-116">The following sample is a simple HelloWorld stored procedure that gets the current context and sends a response that displays "Hello, World".</span></span> <span data-ttu-id="e5a4f-117">Si noti che la stored procedure contiene un valore ID, proprio come i documenti di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-117">Note that the stored procedure has an ID value, just like Azure Cosmos DB documents.</span></span>

```javascript
function helloWorld() {
    var context = getContext();
    var response = context.getResponse();

    response.setBody("Hello, World");
}
```

## <a name="user-defined-function-basics"></a><span data-ttu-id="e5a4f-118">Nozioni di base sulle funzioni definite dall'utente</span><span class="sxs-lookup"><span data-stu-id="e5a4f-118">User-defined function basics</span></span>

<span data-ttu-id="e5a4f-119">Le funzioni definite dall'utente consentono di estendere la grammatica del linguaggio di query SQL di Azure Cosmos DB e di implementare la logica di business personalizzata come i calcoli su proprietà e documenti.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-119">UDFs are used to extend the Azure Cosmos DB SQL query language grammar and implement custom business logic, such as calculations on properties and documents.</span></span> <span data-ttu-id="e5a4f-120">Le funzioni definite dall'utente possono essere chiamate solo dall'interno delle query e, a differenza delle stored procedure, non hanno accesso all'oggetto di contesto, pertanto non sono in grado di leggere o scrivere documenti.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-120">UDFs can be called only from inside queries and, unlike stored procedures, they do not have access to the context object, so they cannot read or write documents.</span></span>

<span data-ttu-id="e5a4f-121">In uno scenario di e-commerce si potrebbe usare una funzione definita dall'utente per determinare l'IVA da applicare al totale di un ordine o una percentuale di sconto da applicare a prodotti o ordini.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-121">In an online commerce scenario, a UDF could be used to determine the sales tax to apply to an order total or a percentage discount to apply to products or orders.</span></span>

## <a name="user-defined-function-example"></a><span data-ttu-id="e5a4f-122">Esempio di funzione definita dall'utente</span><span class="sxs-lookup"><span data-stu-id="e5a4f-122">User-defined function example</span></span>

<span data-ttu-id="e5a4f-123">L'esempio seguente crea una funzione definita dall'utente per calcolare le imposte su un prodotto in una società fittizia, in base al costo del prodotto:</span><span class="sxs-lookup"><span data-stu-id="e5a4f-123">The following sample creates a UDF to calculate tax on a product in a fictitious company based the product cost:</span></span>

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

## <a name="create-a-stored-procedure-in-the-portal"></a><span data-ttu-id="e5a4f-124">Creare una stored procedure nel portale</span><span class="sxs-lookup"><span data-stu-id="e5a4f-124">Create a stored procedure in the portal</span></span>

<span data-ttu-id="e5a4f-125">Verrà ora creata una nuova stored procedure nel portale.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-125">Let's create a new stored procedure in the portal.</span></span> <span data-ttu-id="e5a4f-126">Il portale inserisce automaticamente una semplice stored procedure che recupera il primo elemento nella raccolta, pertanto per prima cosa verrà eseguita questa stored procedure.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-126">The portal automatically populates a simple stored procedure that retrieves the first item in the collection, so we'll run this stored procedure first.</span></span>

1. <span data-ttu-id="e5a4f-127">In Esplora dati fare clic su **Nuova stored procedure**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-127">In the Data Explorer, click **New Stored Procedure**.</span></span>

    <span data-ttu-id="e5a4f-128">Esplora dati visualizza una nuova scheda con una stored procedure di esempio.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-128">Data Explorer displays a new tab with a sample stored procedure.</span></span>

2. <span data-ttu-id="e5a4f-129">Nella casella dell'**ID della stored procedure** immettere il nome *esempio*, fare clic su **Salva** e quindi su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-129">In the **Stored Procedure Id** box, enter the name *sample*, click **Save**, and then click **Execute**.</span></span>


3. <span data-ttu-id="e5a4f-130">Nella casella **Parametri di input** digitare il nome di una chiave di partizione *33218896* e quindi fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-130">In the **Input parameters** box, type the name of a partition key, *33218896*, and then click **Execute**.</span></span> <span data-ttu-id="e5a4f-131">Si noti che le stored procedure funzionano all'interno di una singola partizione.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-131">Note that stored procedures work within a single partition.</span></span>

    ![Eseguire una stored procedure nel portale](../media/6-stored-procedure.gif)

    <span data-ttu-id="e5a4f-133">Il riquadro **Risultato** visualizza il feed dal primo documento nella raccolta.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-133">The **Result** pane displays the feed from the first document in the collection.</span></span>

## <a name="create-a-stored-procedure-that-creates-documents"></a><span data-ttu-id="e5a4f-134">Creare una stored procedure per la creazione di documenti</span><span class="sxs-lookup"><span data-stu-id="e5a4f-134">Create a stored procedure that creates documents</span></span>

<span data-ttu-id="e5a4f-135">Ora si procederà alla creazione di una stored procedure che crea documenti.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-135">Now, let's create a stored procedure that creates documents.</span></span>

1. <span data-ttu-id="e5a4f-136">In Esplora dati fare clic su **Nuova stored procedure**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-136">In the Data Explorer, click **New Stored Procedure**.</span></span> <span data-ttu-id="e5a4f-137">Assegnare alla stored procedure il nome *createMyDocument*, copiare e incollare il codice seguente nella casella del **corpo della stored procedure**, fare clic su **Salva** e quindi su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-137">Name this stored procedure *createMyDocument*, copy and paste the following code into the **Stored Procedure Body** box, click **Save**, and then click **Execute**.</span></span>

    ```javascript
    function createMyDocument() {
        var context = getContext();
        var collection = context.getCollection();

        var doc = {
            "id": "3",
            "productId": "33218898",
            "description": "Contoso microfleece zip-up jacket",
            "price": "44.99"
        };

        var accepted = collection.createDocument(collection.getSelfLink(),
            doc,
            function (err, documentCreated) {
                if (err) throw new Error('Error' + err.message);
                context.getResponse().setBody(documentCreated)
            });
        if (!accepted) return;
    }
    ```

2. <span data-ttu-id="e5a4f-138">Nella casella dei parametri di input digitare il valore della chiave di partizione *33218898*, quindi fare clic su **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-138">In the Input parameters box, enter a Partition Key Value of *33218898*, and then click **Execute**.</span></span>

    <span data-ttu-id="e5a4f-139">Esplora dati visualizza il documento appena creato nell'area del risultato.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-139">Data Explorer displays the newly created document in the Result area.</span></span>

    <span data-ttu-id="e5a4f-140">È possibile tornare alla scheda Documenti, fare clic sul pulsante **Aggiorna** e visualizzare il nuovo documento.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-140">You can go back to the Documents tab, click the **Refresh** button and see the new document.</span></span> 

## <a name="create-a-user-defined-function"></a><span data-ttu-id="e5a4f-141">Creare una funzione definita dall'utente</span><span class="sxs-lookup"><span data-stu-id="e5a4f-141">Create a user-defined function</span></span>

<span data-ttu-id="e5a4f-142">Ora si procederà alla creazione di una funzione definita dall'utente in Esplora dati.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-142">Now, let's create a UDF in Data Explorer.</span></span>

<span data-ttu-id="e5a4f-143">In Esplora dati fare clic su **Nuova funzione definita dall'utente**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-143">In the Data Explorer, click **New UDF**.</span></span> <span data-ttu-id="e5a4f-144">Per visualizzare **Nuova funzione definita dall'utente** potrebbe essere necessario fare clic sulla freccia in giù accanto a **Nuova stored procedure**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-144">You may need to click the down arrow next to **New Stored Prodedure** to see **New UDF**.</span></span> <span data-ttu-id="e5a4f-145">Copiare il codice seguente nella finestra, assegnare alla funzione definita dall'utente il nome *producttax*, quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-145">Copy the following code into the window, name the UDF *producttax*, and then click **Save**.</span></span>

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

<span data-ttu-id="e5a4f-146">Dopo aver definito la funzione definita dall'utente, per eseguirla passare alla scheda **Query 1** e copiare e incollare la query seguente nella relativa area.</span><span class="sxs-lookup"><span data-stu-id="e5a4f-146">Once you have defined the UDF, go to the **Query 1** tab and copy and paste the following query into the query area to run the UDF.</span></span>

```sql
SELECT c.id, c.productId, c.price, udf.producttax(c.price) AS producttax FROM c
```
