Usando i due documenti aggiunti al database come destinazione delle query, verranno ora esaminate alcune nozioni di base sulle query. Azure Cosmos DB usa le query SQL, analogamente a SQL Server, per eseguire operazioni di query. Tutte le proprietà vengono indicizzate automaticamente per impostazione predefinita, quindi tutti i dati nel database sono immediatamente disponibili per le query.

## <a name="sql-query-basics"></a>Nozioni di base sulle query SQL
Ogni query SQL è costituita da una clausola SELECT e da clausole FROM e WHERE facoltative. È possibile aggiungere anche altre clausole, ad esempio ORDER BY e JOIN, per ottenere le informazioni necessarie.

Il formato di una query SQL è il seguente:

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a>Clausola SELECT

La clausola SELECT determina il tipo di valori prodotti quando viene eseguita la query. Un valore `SELECT *` indica che viene restituito l'intero documento JSON.

**Query**
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

**Risultati restituiti**
```
[
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
        },
        "_rid": "iAEeANrzNAAJAAAAAAAAAA==",
        "_self": "dbs/iAEeAA==/colls/iAEeANrzNAA=/docs/iAEeANrzNAAJAAAAAAAAAA==/",
        "_etag": "\"00003a02-0000-0000-0000-5b9208440000\"",
        "_attachments": "attachments/",
        "_ts": 1536297028
    }
]
```

In alternativa, è possibile limitare l'output in modo da includere solo determinate proprietà, includendo un elenco di proprietà nella clausola SELECT. Nella query seguente vengono restituiti solo ID, produttore e descrizione del prodotto.

**Query**
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

**Risultati restituiti**
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a>Clausola FROM

La clausola FROM specifica l'origine dati su cui opera la query. L'origine della query può essere l'intera raccolta oppure è possibile specificare un subset della raccolta. La clausola FROM è facoltativa, a meno che l'origine non sia filtrata o proiettata più avanti nella query.

Una query come `SELECT * FROM Products` indica che l'intera raccolta Products corrisponde all'origine in base alla quale viene eseguita l'enumerazione della query.

È possibile usare un alias per la raccolta, come in `SELECT p.id FROM Products AS p` o semplicemente in `SELECT p.id FROM Products p`, dove `p` è l'equivalente di `Products`. `AS` è una parola chiave facoltativa per applicare un alias per l'identificatore.

Una volta usato un alias, non sarà più possibile associare l'origine iniziale. Ad esempio, la sintassi di `SELECT Products.id FROM Products p` non è valida perché non è più possibile risolvere l'identificatore "Products".

Tutte le proprietà a cui è necessario fare riferimento devono essere complete. In assenza di una rigorosa aderenza allo schema, questo requisito viene applicato per evitare eventuali associazioni ambigue. Di conseguenza, la sintassi di `SELECT id FROM Products p` non è valida perché la proprietà `id` non è associata.

### <a name="subdocuments-in-a-from-clause"></a>Documenti secondari in una clausola FROM
È anche possibile ridurre l'origine a un subset di dimensioni inferiori. Ad esempio, per enumerare solo un sottoalbero in ogni documento, la sottoradice può diventare l'origine, come nell'esempio seguente:

**Query**
```
SELECT * 
FROM Products.shipping
```

**Risultati**  

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
        "weight": 2,
        "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
        }
    }
]
```

Anche se nell'esempio precedente è stata usata una matrice come origine, è anche possibile usare un oggetto come origine, come illustrato nell'esempio seguente. Qualsiasi valore JSON valido (non definito) disponibile nell'origine viene considerato per essere incluso nel risultato della query. Se alcuni prodotti non hanno un valore `shipping.weight`, vengono esclusi dal risultato della query.

**Query**

    SELECT * 
    FROM Products.shipping.weight

**Risultati**

    [
        1,
        2
    ]

## <a name="where-clause"></a>Clausola WHERE
La clausola WHERE specifica le condizioni che i documenti JSON forniti dall'origine devono soddisfare per essere inclusi come parte del risultato. Per essere considerato per il risultato, qualsiasi documento JSON deve restituire **true** per le condizioni specificate. La clausola WHERE è facoltativa.

La query seguente richiede documenti che contengono un ID il cui valore è 1:

**Query**

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

**Risultati**

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a>Clausola ORDER BY

La clausola ORDER BY consente di disporre i risultati in ordine crescente o decrescente.

La query ORDER BY seguente restituisce il prezzo, la descrizione e l'ID prodotto per tutti i prodotti, con i risultati in ordine crescente in base al prezzo:

**Query**

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

**Risultati**

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

## <a name="join-clause"></a>Clausola JOIN

La clausola JOIN consente di eseguire inner join con il documento e le sottoradici del documento. Nel database dei prodotti è quindi possibile, ad esempio, associare i documenti ai dati di spedizione.  

Nella query seguente vengono restituiti gli ID prodotto per ogni prodotto che ha un metodo di spedizione. Se si aggiungesse un terzo prodotto senza proprietà relativa alla spedizione, si otterrebbe lo stesso risultato perché il terzo prodotto verrebbe escluso per l'assenza della proprietà relativa alla spedizione.

**Query**

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

**Risultati**

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a>Query geospaziali

Le query geospaziali consentono di eseguire query spaziali tramite punti GeoJSON. Usando le coordinate nel database, è possibile calcolare la distanza tra due punti e determinare se un punto, un poligono o una linea spezzata si trova all'interno di un altro punto, poligono o linea spezzata.

In caso di dati di un catalogo prodotti, questa opzione consente agli utenti di immettere le informazioni sulla propria posizione e determinare se nel raggio di 50 miglia sono presenti negozi che hanno a disposizione l'articolo che stanno cercando. 

## <a name="summary"></a>Riepilogo

Grazie alla possibilità di eseguire rapidamente query su tutti i dati aggiunti ad Azure Cosmos DB e di usare tecniche di query SQL familiari, utenti e clienti possono esplorare i dati archiviati e ottenere da essi informazioni dettagliate.