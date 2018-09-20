Come per i computer locali, le prestazioni delle macchine virtuali spesso possono essere direttamente collegate alla velocità di lettura e scrittura dei dati. Per capire come migliorare queste prestazioni, è necessario innanzitutto identificare il modo in cui vengono misurate e le impostazioni e le scelte che vi influiscono.

È necessario esaminare in particolare i dischi e l'archiviazione sottostanti usati per le macchine virtuali. Nell'analizzare le prestazioni, tenere presente che è necessario considerare anche il livello dell'applicazione. Ad esempio, se si esegue un database in una macchina virtuale, sarà utile esaminare le impostazioni per le prestazioni specifiche del database per verificare che sia ottimizzato per la macchina virtuale e l'archiviazione eseguite nel database stesso.

Per iniziare, verranno definiti alcuni termini e le garanzie offerte da Azure al riguardo.

## <a name="io-operations-per-second"></a>Operazioni di I/O al secondo

Il tipo di archiviazione selezionato (Standard o Premium) definisce la velocità dei dischi. Queste prestazioni vengono misurate in termini di operazioni di I/O al secondo, o IOPS.

Le operazioni di I/O al secondo indicano il numero di richieste che possono essere elaborate dal disco in un secondo. Una singola richiesta è un'operazione di lettura o scrittura. Questa misurazione viene applicata direttamente all'archiviazione. Ad esempio, un disco in grado di eseguire **5000 operazioni di I/O al secondo** è teoricamente in grado di elaborare 5.000 operazioni di lettura e/o scrittura al secondo.

Le operazioni di I/O al secondo influiscono direttamente sulle prestazioni dell'applicazione. Alcune applicazioni, come i siti Web per la vendita al dettaglio, hanno bisogno di operazioni di I/O al secondo elevate per poter gestire tutte le richieste di I/O minime e casuali che devono essere elaborare rapidamente per mantenere il sito reattivo.

### <a name="iops-in-azure"></a>Operazioni di I/O al secondo in Azure

Quando si collega un disco di Archiviazione Premium a una macchina virtuale a scalabilità elevata, Azure effettua automaticamente il provisioning di un numero garantito di operazioni di I/O al secondo, in base alla specifica del disco. Ad esempio, un disco **P50** effettua il provisioning di **7500 operazioni di I/O al secondo**. Ogni dimensione di macchina virtuale a scalabilità elevata prevede anche un limite di operazioni di I/O al secondo specifico sostenibile. Ad esempio, una macchina virtuale **GS5 Standard** ha un limite di **80.000 operazioni di I/O al secondo**.

Le operazioni di I/O al secondo sono una misurazione dei dischi di archiviazione, ma hanno un limite _teorico_, ovvero altri due fattori possono influire sulle effettive prestazioni dell'applicazione: la **velocità effettiva** e la **latenza**.

### <a name="what-is-throughput"></a>Che cos'è la velocità effettiva?
La velocità effettiva, chiamata anche "larghezza di banda" è la quantità di dati che l'applicazione invia ai dischi di archiviazione in un intervallo specificato (in genere al secondo). Se l'applicazione esegue operazioni di I/O con blocchi di dati di grandi dimensioni, richiede una velocità effettiva elevata.

Azure effettua il provisioning della velocità effettiva in dischi di archiviazione Premium in base alla specifica di ogni disco. Ad esempio, un disco **P50** effettua il provisioning di una velocità effettiva del disco di **250 MB al secondo**. Ogni dimensione di macchina virtuale a scalabilità elevata prevede anche un _limite di velocità effettiva_ specifico sostenibile. Ad esempio, una macchina virtuale **GS5 Standard** ha una velocità effettiva massima di **2.000 MB al secondo**.

#### <a name="iops-vs-throughput"></a>Operazioni di I/O al secondo e velocità effettiva

Le operazioni di I/O al secondo e la velocità effettiva hanno una relazione diretta e modificando le prime si influirà sulla seconda. Per ottenere un limite teorico di velocità effettiva, è possibile usare questa formula: `IOPS x I/O size = throughput`. È importante tenere presenti entrambi i valori quando si pianifica l'applicazione.

### <a name="what-is-latency"></a>Che cos'è la latenza?

La lettura e la scrittura di dati richiedono tempo. È qui che entra in gioco la _latenza_. La latenza è il tempo necessario all'app per inviare una richiesta al disco e ottenere la risposta. In pratica, la latenza indica il tempo necessario per _elaborare_ una singola richiesta di I/O di lettura o scrittura.

La latenza limita le operazioni di I/O al secondo. Ad esempio, se il disco può gestire 5000 operazioni di I/O al secondo, ma l'elaborazione di ogni operazione dura 10 ms, l'app verrà limitata a 100 operazioni al secondo a causa del tempo di elaborazione. Questo è solo un esempio e la maggior parte delle volte la latenza è molto inferiore. In definitiva, la latenza e la velocità effettiva determineranno la velocità con cui l'app può elaborare i dati dall'archiviazione. 

Archiviazione Premium offre latenze coerentemente ridotte ed è possibile ottenere una latenza ancora migliore in caso di necessità tramite la _memorizzazione nella cache_. 

## <a name="testing-your-disk-performance"></a>Test delle prestazioni dei dischi

È possibile modificare e bilanciare le operazioni di I/O al secondo, la velocità effettiva e la latenza dei dischi delle macchine Virtuali selezionando il tipo di archiviazione e le dimensioni di macchina virtuale più appropriati. In genere, le dimensioni di macchina virtuale maggiori e più costose offriranno migliori garanzie di livelli massimi di operazioni di I/O al secondo e velocità effettiva. Aggiungendo a questa equazione anche la scelta tra Archiviazione Standard e Premium e tra unità HDD e SSD, sarà possibile valutare le prestazioni provando diversi parametri.

Per selezionare la combinazione più appropriata, è necessario identificare i requisiti dell'applicazione. Le applicazioni con I/O elevato come i server di database o i sistemi di elaborazione delle transazioni online richiedono un numero maggiore di operazioni di I/O al secondo, mentre le applicazioni basate prevalentemente sul calcolo possono accettare requisiti molto inferiori. Inoltre, i _tipi_ delle operazioni eseguite dalle applicazioni influiscono sulla velocità effettiva. L'I/O ad accesso casuale elevato tende a essere più lento rispetto alle letture sequenziali prolungate.

Dopo aver selezionato la configurazione, è possibile usare strumenti come [Iometer](http://iometer.org/) per testare le prestazioni del disco nelle macchine virtuali Windows e Linux. Questo strumento può aiutare a farsi un'idea più concreta delle prestazioni previste. Può anche permettere di identificare come migliorare l'utilizzo dell'archiviazione dell'app. Ad esempio, un'applicazione che esegue operazioni di I/O a thread singolo riscontrerà probabilmente prestazioni di I/O ridotte a causa della latenza.

Si esamineranno ora altre scelte che è possibile adottare per migliorare le prestazioni dei dischi.

