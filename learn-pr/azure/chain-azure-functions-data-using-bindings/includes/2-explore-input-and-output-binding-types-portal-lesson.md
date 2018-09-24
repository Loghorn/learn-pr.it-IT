L'accesso e l'elaborazione dei dati sono le attività principali in molte soluzioni software. Prendere in considerazione alcuni di questi scenari:

* È stato richiesto di implementare un modo per spostare i dati in ingresso dall'archiviazione BLOB ad Azure Cosmos DB.
* Si vogliono pubblicare i messaggi in arrivo in una coda per elaborarli tramite un altro componente aziendale.
* Il servizio deve recuperare i punteggi dei giocatori da una coda e aggiornare un tabellone online.

Tutti questi esempi riguardano lo spostamento dei dati. L'origine dati e le destinazioni sono diverse a seconda dello scenario, ma il modello è simile. Ci si connette a un'origine dati e si leggono e scrivono dati. Funzioni di Azure consente l'integrazione con dati e servizi tramite le associazioni. 

## <a name="what-is-a-binding"></a>Che cos'è un'associazione?

In Funzioni di Azure le associazioni forniscono una modalità dichiarativa per connettersi ai dati dall'interno del codice. Rendono più facile un'integrazione coerente con i flussi dei dati in una funzione. È possibile avere più associazioni che forniscono accesso a elementi dati diversi. Si tratta di una funzionalità molto efficace perché è possibile connettersi alle origini dati senza dover scrivere codice per una logica di connessione specifica (come connessioni di database o interfacce API Web).

### <a name="types-of-bindings"></a>Tipi di associazioni

Ci sono due tipi di associazioni che è possibile usare con le funzioni:

1. **Associazione di input** Un'associazione di input è una connessione a un'**origine** dati. La funzione può leggere i dati da tali input.

1. **Associazione di output** Un'associazione di output è una connessione a una **destinazione** dati. La funzione scrive i dati in tali destinazioni.

Ci sono anche i trigger. I trigger sono tipi speciali di associazioni di input che causano l'esecuzione di una funzione. Ad esempio, una notifica di Griglia di eventi di Azure può essere configurata come trigger. Quando si verifica un evento, la funzione viene eseguita.

### <a name="types-of-supported-bindings"></a>Tipi di associazioni supportate

Il *tipo* di associazione definisce dove si leggono o si inviano i dati. Sono disponibili un'associazione per rispondere alle richieste Web e un'ampia gamma di associazioni per interagire direttamente con diversi servizi di Azure e servizi di terze parti.

Un tipo di associazione può essere usato come input, output o entrambi. Una funzione, ad esempio, può scrivere nell'associazione di output di Archiviazione BLOB di Azure, ma un aggiornamento dell'archiviazione BLOB può attivare un'altra funzione.

Di seguito sono elencati alcuni tipi di associazioni comuni:
- Archiviazione BLOB
- Code del bus di servizio di Azure
- Azure Cosmos DB
- Hub eventi di Azure
- File esterni
- Tabelle esterne
- Endpoint HTTP

Questi tipi rappresentano solo alcuni esempi. Ci sono altri tipi e le funzioni hanno inoltre un modello di estendibilità che permette di aggiungere altre associazioni.

### <a name="binding-properties"></a>Proprietà delle associazioni

Ci sono tre proprietà obbligatorie in tutte le associazioni. Può essere necessario specificare proprietà aggiuntive in base al tipo di associazione e alla risorsa di archiviazione usata.

1. **Name** Definisce il parametro della funzione tramite cui si accede ai dati. In un'associazione di input di coda, ad esempio, si tratta del nome del parametro della funzione che riceve il contenuto del messaggio della coda. 

1. **Type** Identifica il tipo di associazione, ad esempio il tipo di dati o il servizio con cui si vuole interagire.

1. **Direction** Indica la direzione del flusso dei dati, ovvero se si tratta di un'associazione di input o di output.

La maggior parte dei tipi di associazione necessita anche di una quarta proprietà: 

4. **Connection** Fornisce il nome di una chiave di impostazione dell'app che contiene la stringa di connessione. Le associazioni usano stringhe di connessione archiviate nelle impostazioni dell'app per mantenere i segreti all'esterno del codice della funzione. Ciò agevola la configurazione del codice e lo rende più sicuro.

## <a name="create-a-binding"></a>Creare un'associazione

Le associazioni sono definite in JSON. Un'associazione viene configurata nel file di configurazione della funzione, denominato *function.json*, e si trova nella stessa cartella del codice della funzione.

 Verrà ora esaminato un esempio di *associazione di input*:

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

Per creare questa associazione, è necessario:

1. Creare un'associazione nel file *function.json*.

1. Specificare il valore per la variabile `name`. In questo esempio la variabile contiene i dati BLOB.

1. Specificare la proprietà `type` per l'associazione. Nell'esempio precedente viene usata l'archiviazione BLOB.

1. Specificare la proprietà `path`, che indica il contenitore e il nome dell'elemento inserito. La proprietà `path` è obbligatoria per i BLOB.

1. Specificare il nome dell'impostazione della stringa `connection` definito nel file delle impostazioni dell'applicazione. Viene usato come chiave per trovare la stringa di connessione per connettersi all'account di archiviazione.

1. Definire `direction` come `in`. Vengono letti i dati dal BLOB.

Le associazioni vengono usate per connettersi ai dati nella funzione. In questo esempio è stata usata un'associazione di input per connettere le immagini degli utenti da elaborare con la funzione come anteprime.
