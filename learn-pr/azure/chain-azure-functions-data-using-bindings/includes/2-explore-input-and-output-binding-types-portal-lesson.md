L'accesso e l'elaborazione dei dati sono le attività principali in molte soluzioni software. Prendere in considerazione alcuni di questi scenari:

* È stato richiesto per implementare un modo per spostare i dati in ingresso dall'archivio blob di Azure Cosmos DB.
* Si desidera registrare i messaggi in ingresso a una coda per l'elaborazione da un altro componente in ambito aziendale.
* Il servizio deve ottenere punteggi ottenuti da una coda e aggiornare una tabella di stato online.

Tutti questi esempi riguardano lo spostamento dei dati. L'origine dati e destinazioni diverso da uno scenario allo scenario, ma il modello è simile. Connettersi a un'origine dati, leggere e scrivere dati. Funzioni di Azure consente di integrare con i dati e servizi tramite associazioni. 

## <a name="what-is-a-binding"></a>Che cos'è un'associazione?

Nelle funzioni associazioni forniscono una modalità dichiarativa per connettersi ai dati dall'interno del codice. Rendono più facile da integrare con i flussi di dati in modo coerente in una funzione. Un trigger definisce come viene richiamata una funzione. È possibile avere solo un trigger, ma è possibile avere più associazioni in una funzione. Ciò risulta utile poiché è possibile connettersi alle origini dati senza la necessità per impostare come hardcoded i valori, come ad esempio, il *stringa di connessione*.

### <a name="two-kinds-of-bindings"></a>Due tipi di associazioni

Esistono due tipi di associazioni che è possibile usare per le funzioni:

1. **Associazione di input** un'associazione di input è una connessione a una data **origine**. La funzione legge i dati dall'origine.

1. **Associazione di output** un'associazione di output è una connessione a una data **destinazione**. La funzione scrive i dati a questa destinazione.

### <a name="types-of-supported-bindings"></a>Tipi di associazioni supportate

Il *tipo* di associazione definisce dove è corso la lettura o l'invio dei dati. È presente un'associazione per rispondere alle richieste web e un'ampia selezione di associazioni per interagire con l'archiviazione.

Ecco un elenco di associazioni di usate comune:
- BLOB
- Coda
- Cosmos DB
- Hub eventi
- File esterni
- Tabelle esterne
- HTTP

### <a name="binding-properties"></a>Proprietà di binding

Esistono quattro proprietà che sono necessari in tutte le associazioni. È possibile specificare proprietà aggiuntive in base al tipo di associazione e/o di archiviazione in che uso.

- **Nome** il `name` proprietà definisce il parametro della funzione attraverso il quale si accede ai dati. In un'associazione di input di coda, ad esempio, questo è il nome del parametro della funzione che riceve il contenuto del messaggio della coda. 

- **Tipo di** il `type` proprietà identifica il tipo di associazione, ad esempio, il tipo di dati o un servizio a cui si vuole interagire con.

- **Direzione** il `direction` proprietà identifica la direzione flusso dei dati Internet Explorer. si tratta di un'associazione di input o output?

- **Connessione** il `connection` proprietà corrisponde al nome di un'impostazione app contenente la stringa di connessione, non la stringa di connessione. Le associazioni usano stringhe di connessione archiviate nelle impostazioni dell'app per applicare le procedure consigliate che Function. JSON non contiene segreti del servizio.

## <a name="create-a-binding"></a>Creare un'associazione

Le associazioni sono definite in JSON. È configurata un'associazione nel file di configurazione della funzione, chiamata ScriptHelpers `function.json` mentre si trova nella stessa cartella del codice della funzione.

 Verrà ora esaminato un esempio di un' *associazione di input*:

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

Per creare questa associazione è:

1. Creare un'associazione nel nostro `function.json` file.

1. Specificare il valore per il `name` variabile. In questo esempio, la variabile conterrà i dati blob.

1. Specificare lo spazio di archiviazione `type`, nell'esempio precedente, viene usato nell'archivio Blob.

1. Fornire il `path`, che consente di specificare il contenitore e il nome dell'elemento che si estende in esso. Il `path` proprietà è obbligatoria per i BLOB.

1. Fornire il `connection` nome dell'impostazione stringa definita nel file di impostazioni dell'applicazione. Viene utilizzato come una ricerca per trovare la stringa di connessione per la connessione a risorsa di archiviazione.

1. Definire le `direction` come `in`, lo leggerà i dati dal Blob.

Le associazioni vengono utilizzate per connettersi ai dati alla funzione. Nell'esempio che è stata esaminata, abbiamo utilizzato un'associazione di input per la connessione deve essere elaborato tramite la funzione come le anteprime delle immagini dell'utente.