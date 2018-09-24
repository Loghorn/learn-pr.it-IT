Per connettersi a un'origine dati, è necessario configurare un'*associazione di input*. Questa associazione permette di scrivere una quantità minima di codice per creare un messaggio. Non è necessario scrivere codice per attività come l'apertura di una connessione di archiviazione. Queste attività vengono eseguite dal runtime di Funzioni di Azure e dall'associazione.

## <a name="input-binding-types"></a>Tipi di associazioni di input

Ci sono diversi tipi di input. Non tutti i tipi, tuttavia, supportano sia l'input che l'output. Sarà possibile usare questi tipi ogni volta che si vogliono inserire dati di tale tipo. In questo modulo si prenderanno in esame i tipi che supportano le associazioni di input e verrà spiegato quando usarli.

- **Archiviazione BLOB** Le associazioni di archiviazione BLOB consentono di leggere da un BLOB.

- **Azure Cosmos DB** L'associazione di input di Azure Cosmos DB usa l'API SQL per recuperare uno o più documenti di Azure Cosmos DB e li passa al parametro di input della funzione. L'ID documento o i parametri di query possono essere determinati in base al trigger che richiama la funzione.

- **Microsoft Graph** Le associazioni di input di Microsoft Graph consentono di leggere file da OneDrive, di leggere dati da Excel e di ottenere token di autenticazione per poter interagire con qualsiasi API Microsoft Graph.

- **App per dispositivi mobili** L'associazione di input di App per dispositivi mobili carica un record da un endpoint tabella per dispositivi mobili e lo passa alla funzione.

- **Archiviazione tabelle** È possibile leggere i dati e usare l'archiviazione tabelle di Azure.

Per creare un'associazione come input, è necessario definire `direction` come `in`.
I parametri per ogni tipo di associazione possono variare.

## <a name="what-is-a-binding-expression"></a>Che cos'è un'espressione di associazione?

Un'espressione di associazione è un testo specializzato in **function.json**, nei parametri della funzione o nel codice che viene valutato quando la funzione viene richiamata per restituire un valore. Se, ad esempio, si ha un'associazione di coda del bus di servizio, è possibile usare un'espressione di associazione per ottenere il nome della coda dalle impostazioni dell'app.

### <a name="types-of-binding-expressions"></a>Tipi di espressioni di associazione

- Impostazioni app
- Nome file del trigger
- Metadati del trigger
- Payload JSON
- Nuovo GUID
- Data e ora correnti

La maggior parte delle espressioni viene identificata racchiudendo l'espressione tra parentesi graffe. Le espressioni di associazione delle impostazioni dell'app vengono tuttavia racchiuse tra segni di percentuale anziché tra parentesi graffe. Ad esempio, se il percorso dell'associazione di output del BLOB è `%Environment%/newblob.txt` e il valore dell'impostazione dell'app Environment è Development, verrà creato un BLOB nel contenitore Development.

## <a name="summary"></a>Riepilogo

Le associazioni di input consentono di connettere la funzione a un'origine dati. È possibile connettersi a diversi tipi di origini dati e i parametri per ognuna sono diversi. Per risolvere i valori delle diverse origini, è possibile usare espressioni di associazione nel file *function.json*, nei parametri della funzione o nel codice.