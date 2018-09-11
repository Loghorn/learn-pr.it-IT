<span data-ttu-id="915cd-101">Uso dei due documenti aggiunti al database come destinazione delle query. Si esamineranno alcune nozioni di base sulle query.</span><span class="sxs-lookup"><span data-stu-id="915cd-101">Using the two documents you added to the database as the target of our queries, lets walk through some query basics.</span></span> <span data-ttu-id="915cd-102">Azure Cosmos DB usa query SQL, ad esempio SQL Server, per eseguire operazioni di query.</span><span class="sxs-lookup"><span data-stu-id="915cd-102">Azure Cosmos DB uses SQL queries, just like SQL Server, to perform query operations.</span></span> <span data-ttu-id="915cd-103">Tutte le proprietà vengono automaticamente indicizzate per impostazione predefinita. In questo modo tutti i dati nel database sono immediatamente disponibili per eseguire una query.</span><span class="sxs-lookup"><span data-stu-id="915cd-103">All properties are automatically indexed by default, so all data in the database is instantly available to query.</span></span>

## <a name="sql-query-basics"></a><span data-ttu-id="915cd-104">Nozioni di base sulle query SQL</span><span class="sxs-lookup"><span data-stu-id="915cd-104">SQL query basics</span></span>
<span data-ttu-id="915cd-105">Ogni query SQL è costituita da una clausola SELECT e clausole FROM e WHERE facoltative.</span><span class="sxs-lookup"><span data-stu-id="915cd-105">Every SQL query consists of a SELECT clause and optional FROM and WHERE clauses.</span></span> <span data-ttu-id="915cd-106">È possibile aggiungere anche altre clausole, ad esempio ORDER BY e JOIN per ottenere le informazioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="915cd-106">In addition, you can add other clauses like ORDER BY and JOIN to get the info you need.</span></span>

<span data-ttu-id="915cd-107">Il formato di una query SQL è il seguente.</span><span class="sxs-lookup"><span data-stu-id="915cd-107">A SQL query has the following format.</span></span>

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a><span data-ttu-id="915cd-108">Clausola SELECT</span><span class="sxs-lookup"><span data-stu-id="915cd-108">SELECT clause</span></span>

<span data-ttu-id="915cd-109">La clausola SELECT determina il tipo di valori che saranno generati quando viene eseguita la query.</span><span class="sxs-lookup"><span data-stu-id="915cd-109">The SELECT clause determines the type of values that will be produced when the query is executed.</span></span> <span data-ttu-id="915cd-110">Un valore `SELECT *` indica che viene restituito l'intero documento JSON.</span><span class="sxs-lookup"><span data-stu-id="915cd-110">A value of `SELECT *` indicates that the entire JSON document is returned.</span></span>

<span data-ttu-id="915cd-111">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-111">**Query**</span></span>
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="915cd-112">**Restituisce**</span><span class="sxs-lookup"><span data-stu-id="915cd-112">**Returns**</span></span>
```
[
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
        },
        "_rid": "glAZAJFm0VsBAAAAAAAAAA==",
        "_self": "dbs/glAZAA==/colls/glAZAJFm0Vs=/docs/glAZAJFm0VsBAAAAAAAAAA==/",
        "_etag": "\"00006000-0000-0000-0000-5b71be760000\"",
        "_attachments": "attachments/",
        "_ts": 1534180982
    }
]
```

<span data-ttu-id="915cd-113">In alternativa, è possibile limitare l'output in modo da includere solo determinate proprietà, includendo un elenco di proprietà nella clausola SELECT.</span><span class="sxs-lookup"><span data-stu-id="915cd-113">Or, you can limit the output to only include certain properties by including a list of properties in the SELECT clause.</span></span> <span data-ttu-id="915cd-114">Nella query seguente vengono restituiti solo ID, produttore e descrizione del prodotto.</span><span class="sxs-lookup"><span data-stu-id="915cd-114">In the following query, only the id, manufacturer, and product description are returned.</span></span>

<span data-ttu-id="915cd-115">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-115">**Query**</span></span>
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="915cd-116">**Restituisce**</span><span class="sxs-lookup"><span data-stu-id="915cd-116">**Returns**</span></span>
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a><span data-ttu-id="915cd-117">Clausola FROM</span><span class="sxs-lookup"><span data-stu-id="915cd-117">FROM clause</span></span>

<span data-ttu-id="915cd-118">La clausola FROM specifica l'origine dati in cui opera la query.</span><span class="sxs-lookup"><span data-stu-id="915cd-118">The FROM clause specifies the data source upon which the query operates.</span></span> <span data-ttu-id="915cd-119">L'origine della query può essere l'intera raccolta, oppure è possibile specificare un subset della raccolta.</span><span class="sxs-lookup"><span data-stu-id="915cd-119">You can make the whole collection the source of the query, or you can specify a subset of the collection instead.</span></span> <span data-ttu-id="915cd-120">La clausola FROM è facoltativa, a meno che l'origine non sia filtrata o proiettata più avanti nella query.</span><span class="sxs-lookup"><span data-stu-id="915cd-120">The FROM clause is optional unless the source is filtered or projected later in the query.</span></span>

<span data-ttu-id="915cd-121">Una query come `SELECT * FROM Products` indica che l'intera raccolta Products corrisponde all'origine in base alla quale viene eseguita l'enumerazione.</span><span class="sxs-lookup"><span data-stu-id="915cd-121">A query such as `SELECT * FROM Products` indicates that the entire Products collection is the source over which to enumerate the query.</span></span>

<span data-ttu-id="915cd-122">È possibile eseguire l'aliasing della raccolta, come in `SELECT p.id FROM Products AS p` o semplicemente in `SELECT p.id FROM Products p`, dove `p` è l'equivalente di `Products`.</span><span class="sxs-lookup"><span data-stu-id="915cd-122">A collection can be aliased, such as `SELECT p.id FROM Products AS p` or simply `SELECT p.id FROM Products p`, where `p` is the equivalent of `Products`.</span></span> <span data-ttu-id="915cd-123">`AS` è una parola chiave facoltativa per eseguire l'aliasing dell'identificatore.</span><span class="sxs-lookup"><span data-stu-id="915cd-123">`AS` is an optional keyword to alias the identifier.</span></span>

<span data-ttu-id="915cd-124">Dopo aver eseguito l'aliasing, non sarà più possibile associare l'origine iniziale.</span><span class="sxs-lookup"><span data-stu-id="915cd-124">Once aliased, the original source cannot be bound.</span></span> <span data-ttu-id="915cd-125">Ad esempio, la sintassi di `SELECT Products.id FROM Products p` non è valida perché non è più possibile risolvere l'identificatore "Products".</span><span class="sxs-lookup"><span data-stu-id="915cd-125">For example, `SELECT Products.id FROM Products p` is syntactically invalid since the identifier "Products" cannot be resolved anymore.</span></span>

<span data-ttu-id="915cd-126">Tutte le proprietà a cui è necessario fare riferimento devono essere complete.</span><span class="sxs-lookup"><span data-stu-id="915cd-126">All properties that need to be referenced must be fully qualified.</span></span> <span data-ttu-id="915cd-127">In assenza di una rigorosa aderenza allo schema, questo comportamento viene applicato per evitare eventuali associazioni ambigue.</span><span class="sxs-lookup"><span data-stu-id="915cd-127">In the absence of strict schema adherence, this is enforced to avoid any ambiguous bindings.</span></span> <span data-ttu-id="915cd-128">Di conseguenza, la sintassi di `SELECT id FROM Products p` non è valida perché la proprietà `id` non è associata.</span><span class="sxs-lookup"><span data-stu-id="915cd-128">Therefore, `SELECT id FROM Products p` is syntactically invalid since the property `id` is not bound.</span></span>

### <a name="subdocuments-in-a-from-clause"></a><span data-ttu-id="915cd-129">Documenti secondari in una clausola FROM</span><span class="sxs-lookup"><span data-stu-id="915cd-129">Subdocuments in a FROM clause</span></span>
<span data-ttu-id="915cd-130">È anche possibile ridurre l'origine a un subset di dimensioni inferiori.</span><span class="sxs-lookup"><span data-stu-id="915cd-130">The source can also be reduced to a smaller subset.</span></span> <span data-ttu-id="915cd-131">Ad esempio, per enumerare un solo sottoalbero in ogni documento, è possibile che la sottoradice diventi l'origine, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="915cd-131">For instance, to enumerate only a subtree in each document, the subroot could then become the source, as shown in the following example.</span></span>

<span data-ttu-id="915cd-132">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-132">**Query**</span></span>
```
SELECT * 
FROM Products.shipping
```

<span data-ttu-id="915cd-133">**Risultati**</span><span class="sxs-lookup"><span data-stu-id="915cd-133">**Results**</span></span>  

```
[
{
    "weight": 1,
    "dimensions": {
        "width": 6,
        "height": 8,
        "depth": 1
    }
},
{
    "weight": 1,
    "dimensions": {
        "width": 6,
        "height": 8,
        "depth": 1
    }
}
]
```

<span data-ttu-id="915cd-134">Se nell'esempio precedente si usasse una matrice come origine, si potrebbe usare anche un oggetto come origine, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="915cd-134">While the above example used an array as the source, an object could also be used as the source, which is what's shown in the following example.</span></span> <span data-ttu-id="915cd-135">Qualsiasi valore JSON valido (non definito) disponibile nell'origine viene considerato per essere incluso nel risultato della query.</span><span class="sxs-lookup"><span data-stu-id="915cd-135">Any valid JSON value (that's not undefined) that can be found in the source is considered for inclusion in the result of the query.</span></span> <span data-ttu-id="915cd-136">Se alcune prodotti non hanno un valore `shipping.weight`, vengono esclusi dal risultato della query.</span><span class="sxs-lookup"><span data-stu-id="915cd-136">If some products don’t have an `shipping.weight` value, they are excluded in the query result.</span></span>

<span data-ttu-id="915cd-137">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-137">**Query**</span></span>

    SELECT * 
    FROM Products.shipping.weight

<span data-ttu-id="915cd-138">**Risultati**</span><span class="sxs-lookup"><span data-stu-id="915cd-138">**Results**</span></span>

    [
        1,
        2
    ]

## <a name="where-clause"></a><span data-ttu-id="915cd-139">Clausola WHERE</span><span class="sxs-lookup"><span data-stu-id="915cd-139">WHERE clause</span></span>
<span data-ttu-id="915cd-140">La clausola WHERE specifica le condizioni che i documenti JSON specificati dall'origine devono soddisfare per essere inclusi come parte del risultato.</span><span class="sxs-lookup"><span data-stu-id="915cd-140">The WHERE clause specifies the conditions that the JSON documents provided by the source must satisfy in order to be included as part of the result.</span></span> <span data-ttu-id="915cd-141">Per essere inclusi nel risultato, i documenti JSON devono valutare le condizioni specificate come "true".</span><span class="sxs-lookup"><span data-stu-id="915cd-141">Any JSON document must evaluate the specified conditions to true to be considered for the result.</span></span> <span data-ttu-id="915cd-142">La clausola WHERE è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="915cd-142">The WHERE clause is optional.</span></span>

<span data-ttu-id="915cd-143">La query seguente richiede documenti che contengono un ID il cui valore è 1.</span><span class="sxs-lookup"><span data-stu-id="915cd-143">The following query requests documents that contain an id whose value is 1.</span></span>

<span data-ttu-id="915cd-144">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-144">**Query**</span></span>

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

<span data-ttu-id="915cd-145">**Risultati**</span><span class="sxs-lookup"><span data-stu-id="915cd-145">**Results**</span></span>

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a><span data-ttu-id="915cd-146">Clausola ORDER BY</span><span class="sxs-lookup"><span data-stu-id="915cd-146">ORDER BY clause</span></span>

<span data-ttu-id="915cd-147">La clausola ORDER BY consente di ordinare i risultati in ordine crescente o decrescente.</span><span class="sxs-lookup"><span data-stu-id="915cd-147">The ORDER BY clause enables you to order the results in ascending or descending order.</span></span>

<span data-ttu-id="915cd-148">La query ORDER BY seguente restituisce il prezzo, la descrizione e l'ID prodotto per tutti i prodotti, ordinati in base al prezzo, in ordine crescente.</span><span class="sxs-lookup"><span data-stu-id="915cd-148">The following ORDER BY query returns the price, description, and product ID for all products, ordered by price, in ascending order.</span></span>

<span data-ttu-id="915cd-149">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-149">**Query**</span></span>

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

<span data-ttu-id="915cd-150">**Risultati**</span><span class="sxs-lookup"><span data-stu-id="915cd-150">**Results**</span></span>

```
[
    {
        "price": "14.99",
        "description": "Quick dry crew neck t-shirt",
        "productId": "33218896"
    },
    {
        "price": "49.99",
        "description": "Black wool pea-coat",
        "productId": "33218897"
    }
]
```

## <a name="join-clause"></a><span data-ttu-id="915cd-151">Clausola JOIN</span><span class="sxs-lookup"><span data-stu-id="915cd-151">JOIN clause</span></span>

<span data-ttu-id="915cd-152">La clausola JOIN consente di eseguire inner join con il documento e le radici secondarie del documento.</span><span class="sxs-lookup"><span data-stu-id="915cd-152">The JOIN clause enables you to perform inner joins with the document and the document sub-roots.</span></span> <span data-ttu-id="915cd-153">Quindi, nel database del prodotto è ad esempio possibile associare i documenti ai dati di spedizione.</span><span class="sxs-lookup"><span data-stu-id="915cd-153">So in the product database, for example, you can join the documents with the shipping data.</span></span>  

<span data-ttu-id="915cd-154">Nella query seguente vengono restituiti gli ID prodotto per ogni prodotto che ha un metodo di spedizione.</span><span class="sxs-lookup"><span data-stu-id="915cd-154">In the following query, the product IDs are returned for each product that has a shipping method.</span></span> <span data-ttu-id="915cd-155">Se si aggiungesse un terzo prodotto senza proprietà di spedizione, si otterrebbe lo stesso risultato di escludere un terzo elemento senza proprietà di spedizione.</span><span class="sxs-lookup"><span data-stu-id="915cd-155">If you added a third product that didn't have a shipping property, the result would be the same as the third item would be excluded for not having a shipping property.</span></span>

<span data-ttu-id="915cd-156">**Query**</span><span class="sxs-lookup"><span data-stu-id="915cd-156">**Query**</span></span>

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

<span data-ttu-id="915cd-157">**Risultati**</span><span class="sxs-lookup"><span data-stu-id="915cd-157">**Results**</span></span>

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a><span data-ttu-id="915cd-158">Query geospaziali</span><span class="sxs-lookup"><span data-stu-id="915cd-158">Geospatial queries</span></span>

<span data-ttu-id="915cd-159">Le query geospaziali consentono di eseguire query spaziali tramite punti GeoJSON.</span><span class="sxs-lookup"><span data-stu-id="915cd-159">Geospatial queries enable you to perform spatial queries using GeoJSON points.</span></span> <span data-ttu-id="915cd-160">Usando le coordinate del database, è possibile calcolare la distanza tra due punti, determinare se un punto, un poligono o una linea spezzata si trova all'interno di un altro punto, poligono o linea spezzata.</span><span class="sxs-lookup"><span data-stu-id="915cd-160">Using the coordinates in the database, you can calculate the distance between two points, determine whether a point, polygon, or linestring is within another point, polygon or linestring.</span></span>

<span data-ttu-id="915cd-161">Nel caso di dati del catalogo prodotti, ciò consentirebbe agli utenti di immettere informazioni sulla propria posizione e determinare se nel raggio di 50 miglia siano presenti negozi che hanno a disposizione l'articolo necessario.</span><span class="sxs-lookup"><span data-stu-id="915cd-161">For product catalog data this would enable your users to enter their location information and determine whether there were any stores within a 50 mile radius that have the item they're looking for.</span></span> 

## <a name="summary"></a><span data-ttu-id="915cd-162">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="915cd-162">Summary</span></span>

<span data-ttu-id="915cd-163">Grazie alla possibilità di eseguire rapidamente query su tutti i dati aggiunti ad Azure Cosmos DB e di usare tecniche di query SQL comuni, utenti e clienti possono esplorare e ottenere informazioni dettagliate sui dati archiviati.</span><span class="sxs-lookup"><span data-stu-id="915cd-163">Being able to quickly query all your data after adding it to Azure Cosmos DB, and using familiar SQL query techniques, can help you and your customers to explore and gain insights into your stored data.</span></span>