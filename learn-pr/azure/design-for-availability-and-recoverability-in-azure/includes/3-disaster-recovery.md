La progettazione per la disponibilità elevata consente di mantenere un'applicazione o un processo in esecuzione nonostante condizioni avverse ed eventi sfavorevoli. Tuttavia, come è necessario procedere quando si verifica un evento talmente significativo da determinare la perdita dei dati e rendere impossibile mantenere disponibili le app e i processi? In caso di emergenza, è necessario avere un piano. È necessario sapere quali obiettivi e le aspettative sono per il ripristino, quali sono i costi e i limiti del piano di e come eseguire su di esso.

## <a name="what-is-disaster-recovery"></a>Che cos'è il ripristino di emergenza?

Ripristino di emergenza riguarda *il ripristino da eventi ad alto impatto* che comportare tempi di inattività e perdita o dati. Una situazione di emergenza è un singolo evento importante con un impatto molto più grande e di lunga durata rispetto alle capacità di attenuare il problema offerte dalla parte relativa alla disponibilità elevata della progettazione dell'applicazione.

"Emergenza" suscita spesso opinioni dei *naturale* emergenze e gli eventi esterni (terremoti, alluvioni, tempeste tropical e così via) ma molti altri tipi di situazioni di emergenza esistono anche. Un problema estremamente grave nel corso di una distribuzione o un aggiornamento può lasciare un'app in uno stato non riconoscibile. Alcuni pirati informatici possono crittografare o eliminare i dati e causare gravi di altri tipi di danni che accettano un'app in modalità offline o eliminano alcune delle proprie funzionalità.

Indipendentemente dalla causa, una volta che si è verificata una situazione di emergenza, l'unico rimedio sono un piano di ripristino di emergenza ben definito e accuratamente testato e un'applicazione che supporta attivamente le attività di ripristino di emergenza a livello di progettazione.

## <a name="how-to-create-a-disaster-recovery-plan"></a>Come creare un piano di ripristino di emergenza

Un piano di ripristino di emergenza è un singolo documento che descrive in dettaglio le procedure necessarie per il ripristino da perdita di dati e i tempi di inattività causato da una situazione di emergenza e identifica chi è responsabile dell'indirizzamento tali procedure. Gli operatori devono saper usare il piano come un manuale per ripristinare la connettività dell'applicazione e i dati quando si verifica un'emergenza. Un piano dettagliato, scritto dedicato al ripristino di emergenza è essenziale per garantire un risultato favorevole. Il processo di creazione del piano consente di assemblare un quadro completo dell'applicazione. I passaggi scritti risultanti alzerà di prendere una decisione ottima e follow-through del Costa Rica caotico, in panico di un evento di emergenza.

La creazione di un piano di ripristino di emergenza richiede un'approfondita conoscenza dei flussi di lavoro, dei dati, dell'infrastruttura e delle dipendenze dell'applicazione.

### <a name="risk-assessment-and-process-inventory"></a>Valutazione dei rischi e inventario dei processi

Il primo passaggio per la creazione di un piano di ripristino di emergenza è l'esecuzione di un'analisi dei rischi per determinare l'impatto dei diversi tipi di emergenze sull'applicazione. Per l'analisi dei rischi non è importante l'esatta natura di un'emergenza, ma piuttosto il relativo impatto potenziale in termini di perdita di dati e tempo di inattività dell'applicazione. Esplorare i vari tipi di emergenze ipotetici e provare a essere specifiche quando si pensa i relativi effetti. Ad esempio, un attacco dannoso destinazione può modificare codice o dati risultante in un altro tipo di impatto rispetto a un terremoto che interrompe la disponibilità della connettività e Data Center di rete.

La valutazione dei rischi deve prendere in considerazione *tutti* i processi che non possono tollerare tempi di inattività illimitati e tutte le categorie di dati che non possono tollerare una perdita illimitata. Quando si verifica un'emergenza che interessa più componenti dell'applicazione, è fondamentale che i proprietari del piano possano usarlo per eseguire un inventario completo degli elementi che richiedono attenzione e definire le priorità di ogni elemento.

Alcune app possono costituire un singolo processo o una singola classificazione dei dati. È comunque importante tenere presente questo aspetto, dal momento che probabilmente l'applicazione sarà un componente di un piano di ripristino di emergenza più ampio che include più applicazioni dell'organizzazione.

### <a name="recovery-objectives"></a>Obiettivi di ripristino

Un piano completo deve specificare due requisiti aziendali di importanza critica per ogni processo implementato dall'applicazione:

* **Obiettivo del punto di ripristino (RPO)**: la durata massima di perdita dei dati accettabile. RPO viene misurata in unità di tempo, non volume: "30 minuti di dati", "quattro ore di dati", e così via. L'obiettivo RPO riguarda la limitazione e il ripristino in seguito a una *perdita* di dati, non a un *furto* di dati.
* **Obiettivo del tempo di ripristino (RTO)**: la durata massima di tempo di inattività accettabile, dove "tempo di inattività" deve essere definito dalle proprie specifiche. Ad esempio, se la durata di tempo di inattività accettabile è otto ore in caso di emergenza, l'obiettivo RPO è otto ore.

![RTO e RPO](../media/rto-rpo.png)

Ogni processo principale o il carico di lavoro che viene implementato da un'app è necessario separare i valori RPO e RTO. Anche se si arriva agli stessi valori per processi diversi, ognuno di essi deve essere generato con un'analisi distinta, in cui vengono analizzati i rischi di uno scenario di emergenza e le potenziali strategie di ripristino per ogni processo corrispondente.

Il processo di specificazione dei valori RPO e RTO di fatto rappresenta la creazione dei requisiti di ripristino di emergenza per l'applicazione. È necessario stabilire la priorità di ogni carico di lavoro e la categoria di dati e l'esecuzione di un'analisi costi / benefici. L'analisi include problematiche, quali costi di implementazione e la manutenzione, spese operative, processo di sovraccarico, impatto sulle prestazioni e l'impatto del tempo di inattività e perdita di dati. È necessario definire esattamente quali "tempo di inattività" si intende per l'applicazione e in alcuni casi, si possono stabilire i valori RPO e RTO separati per diversi livelli di funzionalità. Specificare gli obiettivi RPO e RTO non significa semplicemente scegliere valori arbitrari. Gran parte del valore di un piano di ripristino di emergenza deriva dalle ricerche e dalle analisi necessarie per l'individuazione del potenziale impatto di un'emergenza e del costo per l'attenuazione dei rischi.

### <a name="detailing-recovery-steps"></a>Definizione dei dettagli delle procedure di ripristino

Il piano finale deve descrivere in dettaglio le esatte procedure da eseguire per il ripristino dei dati persi e della connettività dell'applicazione. Le procedure spesso includono informazioni relative a:

* **I backup**: con quale frequenza vengono create, in cui si trovano e come ripristinare i dati da essi.
* **Le repliche di dati**: il numero e le posizioni delle repliche, le caratteristiche di natura e la coerenza dei dati replicati e come passare a una replica diversa.
* **Le distribuzioni**: modalità di esecuzione delle distribuzioni, come i ripristini si verificano e scenari di errore per le distribuzioni.
* **Infrastruttura**: in locale e le risorse cloud, infrastruttura di rete e l'inventario hardware.
* **Dipendenze**: servizi esterni che vengono usati dall'applicazione, inclusi i contratti di servizio e le informazioni di contatto.
* **Configurazione e notifica**: flag o le opzioni che è possibile impostare in modo che l'applicazione e servizi che vengono usati per notificare agli utenti di impatto dell'applicazione.

I passaggi esatti necessari dipenderà principalmente i dettagli di implementazione dell'app, pertanto è importante mantenere il piano di aggiornamento. L'esecuzione di test periodici del piano consentirà di identificare i gap e le sezioni non aggiornate.

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

* **Archiviazione di Azure** funzionalità di replica dipendono dal tipo di replica selezionato per l'account di archiviazione. Questa replica può essere locale (all'interno di un Data Center), di zona (tra data center in un'area geografica) o a livello di area (tra le aree). Né l'applicazione né gli operatori interagiscono direttamente con questa caratteristica. I failover sono automatici e trasparenti ed è sufficiente selezionare un livello di replica che consente di bilanciare i costi e i rischi.
* La replica del **database SQL di Azure** è automatica su scala ridotta, ma il ripristino in seguito a un'interruzione di un intero data center di Azure o a livello regionale richiede la replica geografica. La configurazione di replica geografica è manuale, ma è una funzionalità di alto livello del servizio e ben supportata dalla documentazione.
* **Cosmos DB** è un sistema di database distribuito a livello globale e la replica è fondamentale per la relativa implementazione. Con Azure Cosmos DB, invece di configurare la replica direttamente, si configura opzioni correlate alla coerenza dei dati e partizionamento.

Esistono molte progettazioni di replica differenti, che presentano diverse priorità in termini di costi, prestazioni e coerenza dei dati. La replica *attiva* richiede l'esecuzione di aggiornamenti in più repliche contemporaneamente, assicurando la coerenza al costo della velocità effettiva. Al contrario, *passivo* replica esegue la sincronizzazione in background, la rimozione di replica come vincolo sulle prestazioni dell'applicazione, ma si corrono RPO. La replica *attiva-attiva* o *multimaster* rende possibile l'uso di più repliche contemporaneamente, consentendo il bilanciamento del carico al costo di complicare la coerenza dei dati, mentre la replica *attiva-passiva* mantiene repliche di riserva da usare esclusivamente durante il failover.

![Replica geografica del database SQL di Azure](../media/geo-replication.png)

> [!IMPORTANT]
> **Né la replica né il backup da soli offrono una soluzione completa per il ripristino di emergenza**. Il ripristino dei dati è un solo componente del ripristino di emergenza e la replica non soddisfa completamente molti tipi di scenari di ripristino di emergenza. Ad esempio, in uno scenario di danneggiamento dei dati, la natura del danno potrebbe consentirne la diffusione dall'archivio dati primario alle repliche, rendendo inutili tutte le repliche e richiedendo un backup per il ripristino.

### <a name="process-recovery"></a>Ripristino dei processi

Dopo un'emergenza, i dati aziendali non sono l'unica risorsa di cui è necessario il ripristino. Gli scenari di emergenza spesso determinano tempi di inattività, che si tratti di problemi di connettività di rete, interruzioni del data center o istanze di macchine virtuali o distribuzioni software danneggiate. La progettazione dell'applicazione deve consentire di eseguire il ripristino a uno stato funzionante.

Nella maggior parte dei casi, il ripristino dei processi comporta il failover in una distribuzione funzionante distinta. A seconda dello scenario, area geografica può essere un aspetto critico. Ad esempio una calamità naturale su larga scala che offre un'intera area di Azure in modalità offline richiederà il ripristino del servizio in un'altra area. Requisiti di ripristino di emergenza dell'applicazione, in particolare RTO, devono determinare la struttura e consentono di che decidere quanti ambienti replicati è necessario, in cui devono essere disponibile e se deve essere mantenuti in uno stato pronto per l'esecuzione o deve sia pronto ad accettare una distribuzione in caso di emergenza.

A seconda della progettazione dell'applicazione, esistono alcune strategie differenti e servizi di Azure e le funzionalità che è possibile sfruttare per migliorare il supporto dell'app per il ripristino di processo dopo un'emergenza.

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery è un servizio dedicato alla gestione del processo di ripristino per i carichi di lavoro in esecuzione in macchine virtuali distribuite in Azure, macchine virtuali in esecuzione in server fisici e i carichi di lavoro in esecuzione direttamente nel server fisici. Site Recovery replica i carichi di lavoro in posizioni alternative, consente di eseguire il failover quando si verifica un'interruzione del servizio e supporta il test di un piano di ripristino di emergenza.

![Azure Site Recovery](../media/asr.png)

Site Recovery supporta la replica di intere macchine virtuali e immagini di server fisici, nonché di singoli *carichi di lavoro*, dove un carico di lavoro può essere una singola applicazione, un'intera macchina virtuale o un sistema operativo con le relative applicazioni. È possibile replicare qualsiasi carico di lavoro dell'applicazione, ma Site Recovery offre un supporto integrato di alto livello per numerose applicazioni server Microsoft, come SQL Server e SharePoint, nonché per un numero limitato di applicazioni di terze parti come SAP.

Tutte le app che viene eseguito su macchine virtuali o server fisici ad almeno deve analizzare l'uso di Azure Site Recovery. È un ottimo modo per scoprire ed esplorare gli scenari e le possibilità per il ripristino di processo.

#### <a name="service-specific-features"></a>Funzionalità specifiche dei servizi

Per le app eseguite nelle offerte PaaS di Azure come il servizio app, la maggior parte di questi servizi offre funzionalità e indicazioni per il supporto del ripristino di emergenza. Per alcuni scenari, è possibile usare funzionalità specifiche dei servizi per supportare il ripristino rapido. Ad esempio, il server SQL Azure supporta la replica geografica per il ripristino rapido del servizio in un'altra area. Il servizio app di Azure offre una funzionalità di backup e ripristino e la documentazione include indicazioni per l'uso di Gestione traffico di Azure per il supporto del routing del traffico a un'area secondaria.

![Coppie di aree](../media/AzRegionPairs.png)

## <a name="testing-a-disaster-recovery-plan"></a>Test di un piano di ripristino di emergenza

La pianificazione del ripristino di emergenza non termina con il completamento del piano. Testare il piano è un aspetto fondamentale del ripristino di emergenza, per garantire che le indicazioni e le spiegazioni siano chiare e aggiornate.

Scegliere gli intervalli per l'esecuzione dei diversi tipi e ambiti di test, ad esempio test dei backup e dei meccanismi di failover ogni mese e l'esecuzione di una simulazione di ripristino di emergenza su larga scala ogni sei mesi. Sempre seguire i passaggi e i dettagli esattamente come si è documentati nel piano di, quindi è consigliabile che un utente ha familiarità con il piano di inviare un punto di vista su tutto ciò che è stato possibile stabilire più chiara. Quando si esegue il test, identificare i gap, aree di miglioramento e posizioni per automatizzare e aggiungere questi miglioramenti al piano.

Assicurarsi di includere il sistema di monitoraggio nell'esecuzione dei test nonché. Ad esempio, se l'applicazione supporta il failover automatizzato, introdurre errori in una dipendenza o un altro componente di importanza critica per assicurarsi che l'applicazione funzioni correttamente end-to-end, inclusi il rilevamento dell'errore e l'attivazione del failover automatizzato.

 Identificando attentamente i requisiti e articolando un piano, sarà possibile determinare i tipi di servizi da usare per soddisfare gli obiettivi di ripristino. Azure offre diversi servizi e funzionalità che consentono di soddisfare questi obiettivi.
