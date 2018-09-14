Quando si compila un'applicazione, si desidera fornire un'esperienza utente eccezionale e una parte di che è il recupero rapido dei dati. Il recupero dei dati da un database è in genere un processo lenta e se questi dati viene letto spesso, è possibile ottenere un'esperienza utente di scarsa qualità. Il modello cache-aside descrive come è possibile implementare una cache in combinazione con un database, per restituire più di frequente i dati nel minor tempo.

In questo caso, si apprenderà come utilizzare il modello cache-aside per assicurarsi che i dati importanti sono rapidamente accessibili.

## <a name="what-is-the-cache-aside-pattern"></a>Che cos'è il modello cache-aside?

Il modello cache-aside determina, quando è necessario recuperare i dati da un'origine dati, ad esempio un database relazionale, è necessario prima di tutto controllare per i dati nella cache. Se i dati sono presenti nella cache, è necessario usarlo. Se i dati sono non nella cache e quindi eseguono query sul database e quando i dati restituiti all'utente, aggiungerlo alla cache. Ciò consentirà quindi di accedere ai dati dalla cache la volta successiva che è necessaria.

## <a name="when-to-implement-the-cache-aside-pattern"></a>Quando implementare il modello cache-aside?

Lettura di dati da un database è in genere un processo lenta. Questa operazione implica la compilazione di una query complessa, la preparazione di un piano di esecuzione di query, l'esecuzione della query, quindi la preparazione del risultato. In alcuni casi, questo processo può leggere i dati dal disco anche. Sono disponibili ottimizzazioni che possono essere eseguite a livello di database, come la pre-compilazione di query e il caricamento di alcuni dei dati in memoria. Tuttavia, l'esecuzione della query e la preparazione del risultato verrà sempre eseguito durante il recupero dei dati da un database.

È possibile risolvere il problema usando il modello cache-aside. Nel modello cache-aside, c'è ancora l'applicazione e il database, ma ora è presente anche una cache. Una cache archivia i dati in memoria, in modo da non dover interagire con il file system. Le cache di archiviano i dati in strutture di dati molto semplice, come coppie chiave-valore, in modo che gli utenti non devono eseguire query complesse per raccogliere i dati o gestione degli indici durante la scrittura dei dati. Per questo motivo, una cache è in genere più efficiente rispetto a un database. Quando si usa un'applicazione, verrà effettuato un tentativo di leggere prima di tutto i dati dalla cache. Se i dati richiesti non sono presente nella cache, l'applicazione verrà recuperarlo dal database, come viene sempre completata. Tuttavia, quindi Archivia i dati nella cache per le richieste successive. La volta successiva quando un utente richiede i dati, verrà restituito viene dalla cache direttamente.

![Diagramma di caricamento dei dati da memorizzare nella cache](../media-draft/cache-aside-set-cache.png)

### <a name="how-to-manage-updating-data"></a>Come gestire l'aggiornamento dei dati

Quando si implementa il modello cache-aside, si introduce un piccolo problema. Poiché i dati viene ora archiviati in una cache e un archivio dati, si verificano problemi quando si prova a eseguire un aggiornamento. Ad esempio, per aggiornare i dati, è necessario aggiornare la cache e l'archivio dati.

La soluzione al problema del modello cache-aside è per invalidare i dati nella cache. Quando si aggiornano i dati nell'applicazione, è necessario innanzitutto eliminare i dati nella cache e quindi apportare le modifiche all'origine dati direttamente. In questo modo, la volta successiva che vengono richiesti i dati, non sarà presente nella cache e il processo viene ripetuto. 

![Diagramma di invalidamento dati memorizzati nella cache](../media-draft/cache-aside-invalidate.png)

## <a name="considerations-for-using-the-cache-aside-pattern"></a>Considerazioni sull'uso del modello cache-aside

Valutare con attenzione i dati da inserire nella cache. Non tutti i dati sono adatti da memorizzare nella cache.

- **Durata**: per la cache-aside essere efficace, assicurarsi che i criteri di scadenza corrispondano la frequenza di accesso dei dati. Effettua il periodo di scadenza troppo breve possono causa applicazioni recuperino continuamente i dati dai dati di archiviare e aggiungerlo alla cache.

- **Rimozione**: le cache hanno dimensioni limitate rispetto agli archivi dati utilizzati in genere, e i dati verranno rimossi se necessario. Assicurarsi di che scegliere un criterio di rimozione appropriato per i dati.

- **Inizializzazione**: per rendere effettivo il modello cache-aside, molte soluzioni verranno la cache viene prepopolata con dati che pensano sarà possibile accedere spesso.

- **Coerenza**: implementazione del modello cache-aside non garantisce la coerenza tra l'archivio dati e la cache. Dati in un archivio dati possono essere modificati senza inviare notifica cache. Questo può causare problemi gravi di sincronizzazione.

Il modello cache-aside è utile quando si è necessaria per accedere ai dati frequentemente da un'origine dati che usa un disco. Usa il modello cache-aside, si archivierà i dati importanti in una cache per contribuire ad aumentare la velocità di recuperarli. 