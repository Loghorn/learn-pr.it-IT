Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente. 

Nel caso di un'applicazione di vendita online, quando un utente invia un ordine e decide di usare un codice promozionale, un credito o un bonus (o tutti e tre in una volta sola), è necessario eseguire una query sul relativo account per verificare queste opzioni, eseguire gli aggiornamenti necessari per indicare che queste opzioni sono state usate, aggiornare il totale dell'ordine ed elaborare l'ordine.

Tutte queste operazioni devono essere eseguite contemporaneamente, all'interno di una singola transazione. Se l'utente sceglie di annullare l'ordine, sarà necessario eseguire il rollback delle modifiche e non modificare le informazioni dell'account, in modo che eventuali codici promozionali, crediti e bonus siano disponibili per l'acquisto successivo.

Per eseguire queste transazioni in Azure Cosmos DB è possibile usare stored procedure e funzioni definite dall'utente. Le stored procedure sono l'unico modo per assicurare transazioni ACID (atomicità, coerenza, isolamento, durabilità), dal momento che vengono eseguite nel server e sono pertanto indicate come programmazione sul lato server. Anche le funzioni definite dall'utente sono archiviate nel server e vengono usate durante le query per eseguire la logica di calcolo sui valori o i documenti all'interno della query. 

In questo modulo si otterranno informazioni sulle stored procedure e le funzioni definite dall'utente e quindi ne verranno eseguite alcune nel portale.

## <a name="stored-procedure-basics"></a>Nozioni di base sulle stored procedure

Le stored procedure consentono di eseguire transazioni complesse su documenti e proprietà. Le stored procedure vengono scritte in Javascript e sono archiviate in una raccolta in Azure Cosmos DB. Eseguendo le stored procedure sul motore di database e in prossimità dei dati, è possibile migliorare le prestazioni rispetto alla programmazione sul lato client.

Le stored procedure sono l'unico modo per ottenere transazioni atomiche all'interno di Azure Cosmos DB. Gli SDK sul lato client non supportano le transazioni.

È anche consigliabile eseguire operazioni batch nelle stored procedure per via della minore necessità di creare transazioni distinte.

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a>Esempio di stored procedure

L'esempio seguente è una semplice stored procedure HelloWorld che ottiene il contesto corrente e invia una risposta che visualizza "Hello, World". Si noti che la stored procedure contiene un valore ID, proprio come i documenti di Azure Cosmos DB.

```javascript
function helloWorld() {
    var context = getContext();
    var response = context.getResponse();

    response.setBody("Hello, World");
}
```

## <a name="user-defined-function-basics"></a>Nozioni di base sulle funzioni definite dall'utente

Le funzioni definite dall'utente consentono di estendere la grammatica del linguaggio di query SQL di Azure Cosmos DB e di implementare la logica di business personalizzata come i calcoli su proprietà e documenti. Le funzioni definite dall'utente possono essere chiamate solo dall'interno delle query e, a differenza delle stored procedure, non hanno accesso all'oggetto di contesto, pertanto non sono in grado di leggere o scrivere documenti.

In uno scenario di e-commerce si potrebbe usare una funzione definita dall'utente per determinare l'IVA da applicare al totale di un ordine o una percentuale di sconto da applicare a prodotti o ordini.

## <a name="user-defined-function-example"></a>Esempio di funzione definita dall'utente

L'esempio seguente crea una funzione definita dall'utente per calcolare le imposte su un prodotto in una società fittizia basato il costo del prodotto:

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

## <a name="create-a-stored-procedure-in-the-portal"></a>Creare una stored procedure nel portale

Verrà ora creata una nuova stored procedure nel portale. Il portale inserisce automaticamente una semplice stored procedure che recupera il primo elemento nella raccolta, pertanto per prima cosa verrà eseguita questa stored procedure.

1. In Esplora dati fare clic su **Nuova stored procedure**.

    In Esplora dati viene visualizzata una nuova scheda con una stored procedure di esempio.

2. Nella casella dell'**ID della stored procedure** immettere il nome *esempio*, fare clic su **Salva** e quindi su **Esegui**.


3. Nella casella **Parametri di input** digitare il nome di una chiave di partizione *33218896* e quindi fare clic su **Esegui**. Si noti che le stored procedure funzionano all'interno di una singola partizione.

    ![Eseguire una stored procedure nel portale](../media/6-stored-procedure.gif)

    Il riquadro **Risultato** visualizza il feed dal primo documento nella raccolta.

## <a name="create-a-stored-procedure-that-creates-documents"></a>Creare una stored procedure per la creazione di documenti

Ora si procederà alla creazione di una stored procedure che crea documenti.

1. In Esplora dati fare clic su **Nuova stored procedure**. Assegnare a questa stored procedure il nome *createDocuments*, fare clic su **Salva** e quindi su **Esegui**.

```javascript
function createMyDocument(id, productid, name, description, price) {
    var context = getContext();
    var collection = context.getCollection();

    var doc = {
        "id": id,
        "productId": productid,
        "description": description,
        "price": price    
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

2. Nella casella dei parametri di Input, immettere un valore della chiave di partizione *999* e quindi fare clic su **aggiungere nuovo param** e immettere un valore per l'id, quindi premere **aggiungere nuovo param** nuovamente e immettere un valore per il productId. Eseguire la stessa operazione per il nome del nome dell'account, descrizione e prezzo, in questo ordine e quindi fare clic su **Execute**.

    Esplora dati, verrà visualizzato il documento appena creato. 

## <a name="create-a-user-defined-function"></a>Creare una funzione definita dall'utente

Ora si procederà alla creazione di una funzione definita dall'utente in Esplora dati.

In Esplora dati fare clic su **Nuova funzione definita dall'utente**. Copiare il codice seguente nella finestra di, denominare la UDF *producttax*, quindi fare clic su **salvare**.

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

Dopo aver definito la funzione definita dall'utente, quindi eseguirla nella raccolta, eseguire la query seguente:

```sql
SELECT c.id, c.productId, c.price, udf.producttax(c.price) AS producttax FROM c
```
