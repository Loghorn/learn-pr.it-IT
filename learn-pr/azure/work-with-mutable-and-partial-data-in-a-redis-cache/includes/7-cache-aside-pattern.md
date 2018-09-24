Quando si sviluppa un'applicazione è importante offrire un'esperienza utente di alto livello e parte di questa esperienza è il recupero rapido dei dati. Il recupero dei dati da un database è in genere un processo lento e, se i dati vengono letti spesso, ciò può causare un'esperienza utente di scarsa qualità. Il modello cache-aside descrive come è possibile implementare una cache in combinazione con un database per restituire i dati usati più di frequente nel minor tempo possibile.

Si vedrà come è possibile usare il modello cache-aside per assicurarsi che i dati importanti siano rapidamente accessibili.

## <a name="what-is-the-cache-aside-pattern"></a>Che cos'è il modello cache-aside?

Il modello cache-aside stabilisce che, quando è necessario recuperare dati da un'origine dati, ad esempio un database relazionale, è prima necessario verificare se i dati sono disponibili nella cache. In caso affermativo, vengono usati i dati nella cache. Se i dati non sono nella cache, viene eseguita una query sul database e quando i dati vengono restituiti all'utente, vengono aggiunti alla cache. In questo modo i dati saranno accessibili dalla cache la volta successiva che sono necessari.

## <a name="when-to-implement-the-cache-aside-pattern"></a>Quando implementare il modello cache-aside?

La lettura dei dati da un database è in genere un processo lento. Questo processo implica la compilazione di una query complessa, la preparazione di un piano di esecuzione di query, l'esecuzione della query e quindi la preparazione del risultato. In alcuni casi, questo processo può anche richiedere la lettura dei dati dal disco. Sono disponibili ottimizzazioni che possono essere eseguite a livello di database, come la precompilazione delle query e il caricamento di alcuni dati in memoria. Tuttavia, l'esecuzione della query e la preparazione del risultato saranno sempre operazioni inevitabili durante il recupero dei dati da un database.

È possibile risolvere questo problema usando il modello cache-aside. Con il modello cache-aside entrano in gioco ancora l'applicazione e il database, ma ora è presente anche una cache. Una cache archivia i dati in memoria, in modo da non dover interagire con il file system. Le cache consentono inoltre di archiviare i dati in strutture di dati molto semplici, come coppie chiave-valore, in modo che non sia necessario eseguire query complesse per raccogliere i dati o gestire gli indici durante la scrittura dei dati. Per questo motivo, una cache offre in genere prestazioni migliori rispetto a un database. Quando si usa un'applicazione, questa tenterà di leggere i dati dalla cache prima di tutto. Se i dati richiesti non sono presenti nella cache, l'applicazione li recupererà dal database, come sempre. Questi dati vengono tuttavia archiviati nella cache per richieste successive. La volta successiva che un utente richiede i dati, verranno restituiti direttamente dalla cache.

![Diagramma del caricamento dei dati nella cache](../media/8-cache-aside-set-cache.png)

### <a name="how-to-manage-updating-data"></a>Come gestire l'aggiornamento dei dati

Con l'implementazione del modello cache-aside si introduce un piccolo problema. Dato che i dati sono ora archiviati in una cache e in un archivio dati, possono verificarsi problemi quando si tenta di eseguire un aggiornamento. Per aggiornare i dati, ad esempio, è necessario aggiornare sia la cache che l'archivio dati.

La soluzione per questo problema nel modello cache-aside consiste nell'invalidare i dati nella cache. Quando si aggiornano i dati nell'applicazione, è prima necessario eliminare i dati nella cache e quindi apportare le modifiche all'origine dati direttamente. In questo modo, la volta successiva che vengono richiesti, i dati non saranno presenti nella cache e il processo verrà ripetuto. 

![Diagramma dell'invalidamento dei dati memorizzati nella cache](../media/8-cache-aside-invalidate.png)

## <a name="considerations-for-using-the-cache-aside-pattern"></a>Considerazioni per l'uso del modello cache-aside

Valutare attentamente quali dati è opportuno inserire nella cache. Non tutti i dati sono adatti alla memorizzazione nella cache.

- **Durata**: per rendere efficace la strategia di cache-aside, assicurarsi che i criteri di scadenza corrispondano alla frequenza di accesso dei dati. Se si imposta un periodo di scadenza troppo breve, le applicazioni potrebbero recuperare continuamente i dati dall'archivio dati e aggiungerli alla cache.

- **Rimozione**: le dimensioni della maggior parte delle cache è limitata rispetto agli archivi dati standard, quindi i dati verranno rimossi all'occorrenza. Assicurarsi di scegliere criteri di rimozione appropriati per i dati.

- **Priming**: per rendere efficace il modello cache-aside, molte soluzioni eseguono il prepopolamento della cache con i dati che si presume saranno soggetti ad accessi frequenti.

- **Coerenza**: l'implementazione del modello cache-aside non garantisce la coerenza tra l'archivio dati e la cache. I dati in un archivio dati possono essere modificati senza notifica alla cache. Questo può causare problemi gravi di sincronizzazione.

Il modello cache-aside è utile quando è necessario accedere ai dati frequentemente da un'origine dati che usa un disco. L'uso del modello cache-aside consentirà di archiviare i dati importanti in una cache per aumentare la velocità di recupero. 