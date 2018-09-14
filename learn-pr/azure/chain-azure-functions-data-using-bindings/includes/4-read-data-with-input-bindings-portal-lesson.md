Per connettersi a un'origine dati è necessario configurare un *associazione di input*. Questa associazione verrà rendono possibile scrivere codice minimo per creare un messaggio. Non devi scrivere codice per attività quali l'apertura di una connessione di archiviazione. Il runtime di funzioni di Azure e l'associazione richiedere automaticamente di tali attività.

## <a name="input-binding-types"></a>Tipi di associazione di input

Sono disponibili più tipi di input, tuttavia, non tutti i tipi supportano sia di input e output. Sarà possibile usarli ogni volta che si desidera inserire i dati di quel tipo. In questo caso, esamineremo i tipi che supportano le associazioni di input e quando utilizzarle.

- **Archivio BLOB** le associazioni di archiviazione blob consentono di leggere da un blob.

- **COSMOS DB** The Azure Cosmos DB input binding Usa l'API SQL per recuperare uno o più Azure Cosmos DB documenti e li passa al parametro di input della funzione. L'ID documento o i parametri di query possono essere determinati in base al trigger che richiama la funzione.

- **Microsoft Graph** le associazioni di input di Microsoft Graph consentono di leggere i file da OneDrive, leggere i dati da Excel e ottenere i token di autenticazione in modo che sia possibile interagire con qualsiasi API Microsoft Graph.
- **App per dispositivi mobili** associazione di input per l'App per dispositivi mobili carica un record da un endpoint tabella per dispositivi mobili e lo passa alla funzione.

- **Archivio tabelle** è possibile leggere i dati e usare l'archiviazione tabelle di Azure.

## <a name="how-to-create-an-input-binding"></a>Come creare un'associazione di input?

Per definire un'associazione di input, è necessario definire il `direction` come `in`.
I parametri per ogni tipo di associazione possono differire; sono quelle descritte nella [documentazione Microsoft](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings?azure-portal=true)

## <a name="what-is-a-binding-expression"></a>Che cos'è un'espressione di associazione?

Un'espressione di associazione è di testo specializzata in Function. JSON, i parametri di funzione o il codice che viene valutato quando viene richiamata la funzione per produrre un valore. Ad esempio, è possibile usare un'espressione di associazione per ottenere l'ora corrente o recuperare un valore dalle impostazioni dell'app.

### <a name="types-of-binding-expressions"></a>Tipi di espressioni di associazione

- Impostazioni app
- Nome file del trigger
- Metadati del trigger
- Payload JSON
- Nuovo GUID
- Data e ora corrente
- Espressioni di associazione

Quasi tutte le espressioni sono identificate tramite la disposizione del testo tra parentesi graffe. Tuttavia, le espressioni di associazione di impostazione di app vengono identificate in modo diverso da altre espressioni di associazione: tali tipi vengono incapsulati in segni di percentuale anziché in parentesi graffe. Se, ad esempio il percorso di associazione di output di blob è `%Environment%/newblob.txt` e il valore dell'impostazione app ambiente è lo sviluppo, verrà creato un blob nel contenitore di sviluppo.

Le associazioni di input consentono di connettere la funzione a un'origine dati. Esistono diversi tipi di origini dati a che è possibile connettersi e i parametri per ogni variabile. È possibile usare espressioni di associazione nel file Function. JSON, i parametri di funzione o il codice, per risolvere i valori da varie origini.