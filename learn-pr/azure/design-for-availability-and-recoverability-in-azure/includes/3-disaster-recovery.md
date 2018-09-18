La progettazione per la disponibilità elevata consente di mantenere un'applicazione o un processo in esecuzione nonostante condizioni avverse ed eventi sfavorevoli. Tuttavia, come è necessario procedere quando si verifica un evento talmente significativo da determinare la perdita dei dati e rendere impossibile mantenere disponibili le app e i processi? In caso di emergenza, è necessario avere un piano. È necessario conoscere gli obiettivi e le aspettative per il ripristino, essere consapevoli dei costi e dei limiti del piano e sapere come metterlo in atto.

## <a name="what-is-disaster-recovery"></a>Che cos'è il ripristino di emergenza?

Il ripristino di emergenza riguarda il *ripristino da eventi a impatto elevato*, che comportano tempi di inattività e perdite di dati. Una situazione di emergenza è un evento singolo importante, con un impatto molto più esteso e persistente di quello attenuabile dalla sezione a disponibilità elevata della struttura dell'applicazione.

Il termine "emergenza" spesso evoca scenari relativi a calamità *naturali* ed eventi esterni (terremoti, alluvioni, tempeste tropicali e così via), ma esistono molti altri tipi di emergenze. Un problema grave nel corso di una distribuzione o un aggiornamento può lasciare un'app in uno stato non riconoscibile. Pirati informatici possono crittografare o eliminare i dati e causare danni di altro tipo, che portano offline un'app o eliminano una parte delle relative funzionalità.

Indipendentemente dalla causa, una volta che si è verificata una situazione di emergenza gli unici rimedi sono un piano di ripristino di emergenza ben definito e accuratamente testato e un'applicazione che supporta attivamente le attività di ripristino di emergenza a livello di progettazione.

## <a name="how-to-create-a-disaster-recovery-plan"></a>Come creare un piano di ripristino di emergenza

Un piano di ripristino di emergenza è un singolo documento, che descrive in dettaglio le procedure necessarie per il recupero dalla perdita di dati e dai tempi di inattività causati da una situazione di emergenza e identifica il responsabile della gestione di tali procedure. Gli operatori devono saper usare il piano come un manuale per ripristinare la connettività dell'applicazione e i dati quando si verifica un'emergenza. Un piano dettagliato, messo per iscritto e dedicato al ripristino di emergenza è essenziale per garantire un risultato favorevole. Il processo di creazione del piano consente anche di definire un quadro completo dell'applicazione. I passaggi scritti risultanti favoriscono la presa di decisioni ottimali e l'implementazione di azioni opportune nei momenti potenzialmente caotici che seguono un evento grave.

La creazione di un piano di ripristino di emergenza richiede una conoscenza approfondita dei flussi di lavoro, dei dati, dell'infrastruttura e delle dipendenze dell'applicazione.

### <a name="risk-assessment-and-process-inventory"></a>Valutazione dei rischi e inventario dei processi

Il primo passaggio per la creazione di un piano di ripristino di emergenza è l'esecuzione di un'analisi dei rischi per determinare l'impatto dei diversi tipi di emergenze sull'applicazione. Per l'analisi dei rischi, l'esatta natura di un'emergenza è meno importante del potenziale impatto in termini di perdita di dati e tempo di inattività dell'applicazione. Esplorare i vari tipi di emergenze ipotetiche e analizzare in modo specifico gli effetti corrispondenti. Ad esempio, un attacco dannoso mirato potrebbe modificare il codice o i dati, generando un impatto di tipo diverso rispetto a quello di un terremoto, che interrompe la connettività di rete e rende non disponibile un data center.

La valutazione dei rischi deve prendere in considerazione *tutti* i processi che non possono tollerare tempi di inattività illimitati e tutte le categorie di dati che non possono tollerare una perdita illimitata. Quando si verifica un'emergenza che interessa più componenti dell'applicazione, è fondamentale che i proprietari del piano possano usarlo per eseguire un inventario completo degli elementi che richiedono attenzione e definire le priorità di ogni elemento.

Alcune app possono costituire un singolo processo o una singola classificazione dei dati. È comunque importante tenere presente questo aspetto, dal momento che probabilmente l'applicazione sarà un componente di un piano di ripristino di emergenza più ampio che include più applicazioni dell'organizzazione.

### <a name="recovery-objectives"></a>Obiettivi di ripristino

Un piano completo deve specificare due requisiti aziendali di importanza critica per ogni processo implementato dall'applicazione:

* **Obiettivo del punto di ripristino (RPO)**: la durata massima di perdita dei dati accettabile. L'obiettivo RPO viene misurato in unità di tempo, non di volume: "30 minuti di dati", "quattro ore di dati" e così via. L'obiettivo RPO riguarda la limitazione e il ripristino in seguito a una *perdita* di dati, non a un *furto* di dati.
* **Obiettivo del tempo di ripristino (RTO)**: la durata massima di tempo di inattività accettabile, dove "tempo di inattività" deve essere definito dalle proprie specifiche.

![RTO e RPO](../media-draft/rto-rpo.png)

Ogni processo o carico di lavoro principale implementato da un'app deve avere valori RPO e RTO separati. Anche se si arriva agli stessi valori per processi diversi, ognuno di essi deve essere generato con un'analisi distinta, in cui vengono analizzati i rischi di uno scenario di emergenza e le potenziali strategie di ripristino per ogni processo corrispondente.

Il processo di specificazione dei valori RPO e RTO rappresenta di fatto la creazione dei requisiti di ripristino di emergenza per l'applicazione. Richiede di stabilire la priorità di ogni carico di lavoro e categoria di dati e di eseguire un'analisi costi/benefici. L'analisi include problematiche quali costi di implementazione e manutenzione, spese operative, overhead del processo, impatto sulle prestazioni e impatto del tempo di inattività e della perdita di dati. È necessario definire esattamente il significato di "tempo di inattività" per l'applicazione e in alcuni casi è possibile stabilire valori RPO e RTO distinti per diversi livelli di funzionalità. Specificare gli obiettivi RPO e RTO non significa semplicemente scegliere valori arbitrari. Gran parte del valore di un piano di ripristino di emergenza deriva dalle ricerche e dalle analisi per l'individuazione del potenziale impatto di un'emergenza e del costo dell'attenuazione dei rischi.

### <a name="detailing-recovery-steps"></a>Definizione dei dettagli delle procedure di ripristino

Il piano finale deve descrivere in dettaglio le esatte procedure da eseguire per il ripristino dei dati persi e della connettività dell'applicazione. Le procedure spesso includono informazioni relative a:

* **Backup**: frequenza di creazione dei backup, posizione in cui si trovano e modalità per il ripristino dei dati.
* **Repliche dei dati**: numero e posizioni delle repliche, natura e caratteristiche di coerenza dei dati replicati e modalità per il passaggio a una replica diversa.
* **Distribuzioni**: modalità di esecuzione delle distribuzioni, modalità di esecuzione del rollback e scenari di errore per le distribuzioni.
* **Infrastruttura**: risorse locali e cloud, infrastruttura di rete e inventario hardware.
* **Dipendenze**: servizi esterni usati dall'applicazione, inclusi contratti di servizio e informazioni di contatto.
* **Configurazione e notifica**: flag o opzioni che possono essere impostati per ridurre in modo graduale le prestazioni dell'applicazione e servizi usati per notificare agli utenti l'impatto sull'applicazione.

I passaggi esatti necessari dipenderanno principalmente dai dettagli di implementazione dell'app, quindi è importante mantenere aggiornato il piano. L'esecuzione di test periodici del piano consentirà di identificare le lacune e le sezioni non aggiornate.

## <a name="designing-for-disaster-recovery"></a>Progettazione per il ripristino di emergenza

Il ripristino di emergenza non è una funzionalità automatica. Deve essere progettato, realizzato e testato. Un'app che deve supportare un'efficace strategia di ripristino di emergenza deve essere completamente sviluppata per il ripristino di emergenza. Azure offre servizi, funzionalità e indicazioni che aiutano a creare app che supportano il ripristino di emergenza, ma è possibile scegliere se includerli o meno nella progettazione.

La progettazione per il ripristino di emergenza presenta due problemi principali:

* **Ripristino dei dati**: usare i backup e la replica per ripristinare i dati persi.
* **Processo di ripristino**: ripristinare i servizi e distribuire il codice per il ripristino in seguito a interruzioni.

### <a name="data-recovery-and-replication"></a>Replica e ripristino dei dati

La replica consente di duplicare i dati archiviati tra diverse repliche di un archivio dati. A differenza del *backup*, che crea snapshot dei dati di sola lettura e di lunga durata da usare per il ripristino, la replica crea copie dei dati in tempo reale o quasi in tempo reale. L'obiettivo della replica è mantenere sincronizzate le repliche con la minima latenza possibile, garantendo al tempo stesso la velocità di risposta dell'applicazione. La replica è un componente fondamentale della progettazione per la disponibilità elevata e il ripristino di emergenza e rappresenta una caratteristica comune delle applicazioni a livello di produzione.

La replica viene usata per attenuare i problemi causati da un archivio dati non raggiungibile o in cui si è verificato un errore con l'esecuzione di un *failover*: la modifica della configurazione dell'applicazione per instradare le richieste di dati a una replica funzionante. Il failover è spesso automatizzato, attivato dal rilevamento degli errori incorporato in un prodotto di archiviazione dati o dalle funzionalità di rilevamento implementate con la propria soluzione di monitoraggio. A seconda dell'implementazione e dello scenario, il failover potrebbe dover essere eseguito manualmente dagli operatori del sistema.

La replica non è un elemento che è possibile implementare da zero. La maggior parte dei sistemi di database completi e altri prodotti e servizi di archiviazione dati includono un qualche tipo di replica come una funzionalità strettamente integrata, a causa dei relativi requisiti funzionali e di prestazioni. È tuttavia lo sviluppatore a decidere se includere queste funzionalità nella progettazione dell'applicazione e a doverle usarle in modo appropriato.

Diversi servizi di Azure supportano vari livelli e concetti di replica. Ad esempio:

* Le funzionalità di replica di **Archiviazione di Azure** dipendono dal tipo di replica selezionato per l'account di archiviazione. Questa replica può essere locale (all'interno di un data center), a livello di zona (tra data center in un'area geografica) o a livello di area (tra aree geografiche). Né l'applicazione né gli operatori interagiscono direttamente con questa caratteristica. I failover sono automatici e trasparenti ed è sufficiente selezionare un livello di replica che consente di bilanciare i costi e i rischi.
* La replica del **database SQL di Azure** è automatica su scala ridotta, ma il ripristino in seguito a un'interruzione di un intero data center di Azure o a livello regionale richiede la replica geografica. L'impostazione della replica geografica è manuale, ma si tratta di una funzionalità di alto livello del servizio ed è ampiamente supportata dalla documentazione.
* **Cosmos DB** è un sistema di database distribuito a livello globale e la replica è fondamentale per la sua implementazione. Con Azure Cosmos DB anziché configurare direttamente la replica si configurano le opzioni correlate al partizionamento e alla coerenza dei dati.

Esistono molte progettazioni di replica differenti, che presentano diverse priorità in termini di costi, prestazioni e coerenza dei dati. La replica *attiva* richiede l'esecuzione di aggiornamenti in più repliche contemporaneamente, assicurando la coerenza a spese della velocità effettiva. Al contrario la replica *passiva* esegue la sincronizzazione in background, rimuovendo la replica come vincolo per le prestazioni dell'applicazione ma incrementando il valore RPO. La replica *attiva-attiva* o *multimaster* rende possibile l'uso di più repliche contemporaneamente, consentendo il bilanciamento del carico ma complicando la coerenza dei dati, mentre la replica *attiva-passiva* mantiene repliche di riserva da usare esclusivamente durante il failover.

![Replica geografica del database SQL di Azure](../media-draft/geo-replication.png)

> [!IMPORTANT]
> **Né la replica né il backup da soli offrono una soluzione completa per il ripristino di emergenza**. Il ripristino dei dati è un solo componente del ripristino di emergenza e la replica non soddisfa completamente molti tipi di scenari di ripristino di emergenza. Ad esempio, in uno scenario di danneggiamento dei dati, la natura del danno potrebbe consentirne la diffusione dall'archivio dati primario alle repliche, rendendo inutili tutte le repliche e richiedendo un backup per il ripristino.

### <a name="process-recovery"></a>Ripristino dei processi

Dopo un'emergenza, i dati aziendali non sono l'unica risorsa di cui è necessario il ripristino. Gli scenari di emergenza spesso determinano tempi di inattività, che si tratti di problemi di connettività di rete, interruzioni del data center o istanze di macchine virtuali o distribuzioni software danneggiate. La progettazione dell'applicazione deve consentire di eseguire il ripristino a uno stato funzionante.

Nella maggior parte dei casi il ripristino dei processi comporta il failover a una distribuzione funzionante distinta. A seconda dello scenario, la posizione geografica può essere un aspetto critico. Ad esempio, una calamità naturale su larga scala che porta offline un'intera area di Azure richiederà il ripristino del servizio in un'altra area. I requisiti di ripristino di emergenza dell'applicazione, in particolare l'obiettivo RTO, dovrebbero guidare la progettazione e aiutare a decidere quanti ambienti replicati sono necessari, dove devono essere posizionati e se devono essere mantenuti in uno stato pronto per l'esecuzione o devono essere pronti ad accettare una distribuzione in caso di emergenza.

A seconda della struttura dell'applicazione, esistono diverse strategie, servizi e funzionalità di Azure che è possibile sfruttare per migliorare il supporto dell'app per il ripristino dei processi dopo un'emergenza.

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery è un servizio dedicato per la gestione del ripristino dei processi per carichi di lavoro in esecuzione in macchine virtuali distribuite in Azure, macchine virtuali in esecuzione in server fisici e carichi di lavoro in esecuzione direttamente nei server fisici. Site Recovery replica i carichi di lavoro in posizioni alternative, consente di eseguire il failover quando si verifica un'interruzione del servizio e supporta il test di un piano di ripristino di emergenza.

![Azure Site Recovery](../media-draft/asr.png)

Site Recovery supporta la replica di intere macchine virtuali e immagini di server fisici, nonché di singoli *carichi di lavoro*, dove un carico di lavoro può essere una singola applicazione, un'intera macchina virtuale o un sistema operativo con le relative applicazioni. È possibile replicare qualsiasi carico di lavoro dell'applicazione, ma Site Recovery offre un supporto integrato di alto livello per numerose applicazioni server Microsoft, come SQL Server e SharePoint, nonché per un numero limitato di applicazioni di terze parti come SAP.

Per tutte le app eseguite su macchine virtuali o server fisici, è consigliabile quantomeno investigare la possibilità di usare Azure Site Recovery. Questo approccio è ideale per scoprire e valutare scenari e possibilità per il ripristino dei processi.

#### <a name="service-specific-features"></a>Funzionalità specifiche dei servizi

Per le app eseguite nelle offerte PaaS di Azure come Servizio app, la maggior parte di questi servizi offre funzionalità e indicazioni per il supporto del ripristino di emergenza. In alcuni scenari è possibile usare funzionalità specifiche dei servizi per supportare il ripristino rapido. Ad esempio, il server SQL Azure supporta la replica geografica per il ripristino rapido del servizio in un'altra area. Il servizio app di Azure offre una funzionalità di backup e ripristino e la documentazione include indicazioni per l'uso di Gestione traffico di Azure per il supporto del routing del traffico a un'area secondaria.

![Coppie di aree](../media-draft/AzRegionPairs.png)

## <a name="testing-a-disaster-recovery-plan"></a>Test di un piano di ripristino di emergenza

La pianificazione del ripristino di emergenza non termina con il completamento del piano. Testare il piano è un aspetto fondamentale del ripristino di emergenza, per garantire che le indicazioni e le spiegazioni siano chiare e aggiornate.

Scegliere gli intervalli per l'esecuzione dei diversi tipi e ambiti di test, ad esempio test dei backup e dei meccanismi di failover ogni mese e simulazione di ripristino di emergenza su larga scala ogni sei mesi. Seguire sempre le procedure e i dettagli esattamente come sono documentati nel piano. Eventualmente, è possibile richiedere a qualcuno che non ha familiarità con il piano di comunicare il proprio punto di vista su tutto ciò che potrebbe essere reso più chiaro. Quando si esegue il test, identificare le lacune, le aree di miglioramento e le operazioni da automatizzare e aggiungere tutti questi miglioramenti al piano.

Assicurarsi di includere nei test anche il sistema di monitoraggio. Ad esempio, se l'applicazione supporta il failover automatizzato, introdurre errori in una dipendenza o un altro componente di importanza critica per assicurarsi che l'applicazione funzioni correttamente end-to-end, con rilevamento dell'errore e attivazione del failover automatizzato.

 Identificando attentamente i requisiti e articolando un piano, sarà possibile determinare i tipi di servizi da usare per soddisfare gli obiettivi di ripristino. Azure offre diversi servizi e funzionalità che consentono di soddisfare questi obiettivi.
