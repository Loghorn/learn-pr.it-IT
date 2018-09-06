Capita di rado di riuscire a prevedere esattamente il carico sul sistema: le applicazioni pubbliche possono crescere rapidamente oppure può presentarsi la necessità che con la crescita dell'azienda un'applicazione interna supporti una base utenti più ampia. Anche quando si può prevedere il carico, è raro che questo sia sempre costante: i rivenditori hanno una domanda maggiore durante le vacanze e i siti Web di articoli sportivi registrano la massima affluenza durante i playoff. In questo articolo vengono definiti i concetti di _aumento/riduzione delle prestazioni_ e _aumento/riduzione del numero di istanze_ e viene illustrato come Azure possa migliorare le funzionalità di scalabilità e come le tecnologie senza server e per i contenitori possano migliorare le capacità di scalabilità dell'architettura.

## <a name="what-is-scaling"></a>Cos'è la scalabilità?

La _scalabilità_ è il processo di gestione delle risorse per consentire a un'applicazione di soddisfare un set di requisiti relativi alle prestazioni.  Quando ci sono troppe risorse che servono gli utenti, queste non verranno usate in modo efficiente, con un conseguente spreco di denaro. Un numero troppo limitato di risorse disponibili produrrà invece un impatto sulle prestazioni dell'applicazione. L'obiettivo è soddisfare i requisiti di prestazioni definiti ottimizzando i costi. 

Il termine "_risorse_" può fare riferimento a tutto ciò che serve per riuscire a eseguire le applicazioni. Memoria e CPU per le macchine virtuali sono le risorse più ovvie, ma per alcuni servizi di Azure può essere necessario prendere in considerazione larghezza di banda o astrazioni come le unità richieste di Cosmos DB.

In un mondo in cui le esigenze delle applicazioni sono costanti è facile prevedere il livello di risorse necessario. Nel mondo reale, le esigenze delle applicazioni cambiano nel tempo e possono quindi essere più difficili da prevedere. Nel migliore dei casi, questi cambiamenti saranno prevedibili o stagionali, ma ciò non accade per tutte le applicazioni. Idealmente, si dovrebbe effettuare il provisioning della giusta quantità di risorse per soddisfare la domanda e adattarle in base alle fluttuazioni della domanda.

La scalabilità è difficile in uno scenario locale in cui si acquistano e gestiscono i propri server. L'aggiunta di risorse può risultare costosa e spesso passa troppo tempo prima che queste siano disponibili online, a volte addirittura più tempo rispetto all'effettiva necessità di una maggiore capacità. Ridurre la capacità durante i periodi di scarsa domanda sul sistema può essere ugualmente difficile e si continuerà quindi a incorrere in costi più elevati.

La facile scalabilità è un vantaggio chiave di Azure. La maggior parte delle risorse di Azure consente di aggiungere o rimuovere facilmente le risorse e molti servizi includono opzioni automatiche che monitorano la domanda e si adattano automaticamente. Questa funzionalità di scalabilità automatica consente di impostare soglie per il livello minimo e massimo di istanze che devono essere disponibili e le aggiunge o le rimuove in base a una metrica delle prestazioni, ad esempio l'uso della CPU.

## <a name="what-is-scaling-up-and-down"></a>Cos'è l'aumento e la riduzione delle prestazioni?

L'aumento delle prestazioni è il processo in cui la capacità di una determinata istanza viene aumentata. Una macchina virtuale può passare da 1 vCPU e 3,5 GB di RAM a 2 vCPU e 7 GB di RAM per offrire una maggiore capacità di elaborazione. La riduzione delle prestazioni è invece il processo in cui la capacità di una determinata istanza viene ridotta. La riduzione della capacità di una macchina virtuale da 2 vCPU e 7 GB di RAM a 1 vCPU e 3,5 GB di RAM, ad esempio, ne ridurrà la capacità e il costo.

![aumento-riduzione-prestazioni](../media/ScaleUpDown.png)

Di seguito vengono illustrati i concetti di aumento e riduzione delle prestazioni nel contesto delle risorse di Azure:

* Nelle macchine virtuali di Azure la scalabilità viene implementata in base alle dimensioni di una macchina virtuale. Alle dimensioni è associata una determinata quantità di vCPU, RAM e spazio di archiviazione locale. È possibile, ad esempio, aumentare le prestazioni passando da una macchina virtuale Standard_DS1_v2 (1 vCPU e 3,5 GB di RAM) a una macchina virtuale Standard_DS2_v2 (2 vCPU e 7 GB di RAM).
* Il database SQL di Azure è un'implementazione di piattaforma distribuita come servizio (PaaS, Platform as a Service) di Microsoft SQL Server.  È possibile aumentare le prestazioni di un database in base al numero di unità di transazione di database (DTU) o vCPU. Le DTU sono un'astrazione delle risorse sottostanti e una combinazione di CPU, IO e memoria. È possibile scalare un'istanza di database da 500 DTU a 250 DTU.
* Il Servizio app di Azure è un servizio di hosting di siti Web PaaS in Azure. I siti Web vengono eseguiti in una server farm virtuale, anche nota come piano di servizio app. Nel piano di servizio app è possibile passare a un piano superiore o inferiore. Ogni livello include opzioni di capacità specifiche. Il piano di servizio app S1 include ad esempio 1 vCPU e 1,75 GB di RAM per istanza. È possibile passare al piano di servizio app S2 che include 2 vCPU e 3 GB di RAM per istanza.

Per avere queste funzionalità in un ambiente locale occorre in genere attendere l'approvvigionamento dell'hardware necessario e l'installazione prima di poter iniziare a usare il nuovo livello. In Azure le risorse fisiche sono già distribuite e disponibili ed è sufficiente selezionare il livello alternativo che si intende usare.

A seconda dei servizi cloud scelti, può essere necessario valutare l'impatto dell'aumento delle prestazioni nella soluzione.

Ad esempio, se si sceglie di aumentare le prestazioni del database SQL di Azure, il servizio gestisce l'aumento delle prestazioni di singoli nodi e mantiene la sua operatività. La modifica del livello di servizio e/o del livello di prestazioni di un database crea una replica del database originale al nuovo livello di prestazioni e quindi passa le connessioni alla replica. Durante questo processo non si verifica alcuna perdita di dati e ci sarà solo una breve interruzione (in genere meno di quattro secondi) quando il servizio passa alla replica.

In alternativa, se si sceglie di aumentare o ridurre le prestazioni di una macchina virtuale, è possibile farlo selezionando una dimensione di istanza diversa. Nella maggior parte dei casi questa operazione richiede un riavvio della macchina virtuale ed è quindi consigliabile tenere presente questo aspetto quando si esegue questa attività.

È consigliabile, infine, cercare sempre soluzioni che offrano un'opzione di riduzione delle prestazioni. Se l'applicazione è in grado di assicurare prestazioni adeguate a un piano tariffario inferiore, la fattura di Azure può essere ridotta in modo significativo.

## <a name="what-is-scaling-out-and-in"></a>Cos'è l'aumento e la riduzione del numero di istanze?

Mentre l'aumento e la riduzione delle prestazioni regola la quantità di risorse a disposizione di una singola istanza, l'aumento e la riduzione del numero di istanze regola il numero totale di istanze.

L'_aumento del numero di istanze_ è il processo di aggiunta di altre istanze per supportare il carico della soluzione. Ad esempio, se il front-end di un sito Web è ospitato in macchine virtuali, si può aumentare il numero delle macchine virtuali qualora il livello di carico aumentasse.

La _riduzione del numero di istanze_ è il processo di rimozione delle istanze non più necessarie per supportare il carico della soluzione. Se i front-end del sito Web sono poco usati, si può decidere di ridurre il numero di istanze per risparmiare sui costi.


![aumento-riduzione-numero-di-istanze](../media/ScaleInOut.png)

Di seguito sono riportati alcuni esempi di aumento e riduzione del numero di istanze nel contesto delle risorse di Azure:

* Per il livello infrastruttura, si useranno probabilmente set di scalabilità di macchine virtuali per automatizzare l'aggiunta e la rimozione di istanze aggiuntive.
  * I set di scalabilità di macchine virtuali consentono di creare e gestire un gruppo di macchine virtuali identiche con bilanciamento del carico.
  * Il numero di istanze di macchine virtuali può aumentare o diminuire automaticamente in risposta alla domanda o a una pianificazione definita.
* In un'implementazione del database SQL di Azure è possibile condividere il carico tra le istanze di database tramite il partizionamento orizzontale. Il _partizionamento orizzontale_ è una tecnica per distribuire grandi quantità di dati strutturati in modo identico tra più database indipendenti.
* In Servizio app di Azure il piano di servizio app è la server farm Web virtuale che ospita il contenuto. L'aumento del numero di istanze in questo modo comporta l'aumento del numero di macchine virtuali nella farm. In modo analogo ai set di scalabilità di macchine virtuali, il numero di istanze può essere aumentato o diminuito automaticamente in risposta a determinate metriche o a una pianificazione.

L'aumento del numero di istanze viene in genere eseguito facilmente nel portale di Azure, tramite strumenti da riga di comando o modelli di Resource Manager e nella maggior parte dei casi risulta trasparente per l'utente finale.

### <a name="autoscale"></a>Scalabilità automatica

È possibile configurare alcuni di questi servizi per l'uso di una funzionalità denominata scalabilità automatica. Grazie alla scalabilità automatica, le operazioni manuali di scalabilità dei servizi non sono più necessarie. È invece possibile impostare una soglia minima e massima di istanze e applicare la scalabilità in base a metriche (lunghezza della coda, uso della CPU) o pianificazioni (giorni feriali tra le 17.00 e le 19.00) specifiche.

![scalabilità automatica](../media/autoscale.png)

### <a name="considerations-when-scaling-in-and-out"></a>Considerazioni relative alla riduzione e all'aumento del numero di istanze

Quando si aumenta il numero di istanze, i tempi di avvio dell'applicazione possono influire sulla velocità di scalabilità dell'applicazione. Se l'app Web richiede due minuti per l'avvio e per essere disponibile per gli utenti, ognuna delle istanze richiederà due minuti prima di essere disponibile per gli utenti. È consigliabile prendere in considerazione i tempi di avvio al momento di determinare la velocità di scalabilità.

È anche necessario riflettere sulle modalità di gestione dello stato da parte dell'applicazione. Quando si riduce il numero di istanze dell'applicazione, qualsiasi stato archiviato nel computer non sarà più disponibile. Se un utente si connette a un'istanza il cui stato non è disponibile, può essere necessario eseguire l'accesso o selezionare di nuovo i dati, determinando un'esperienza utente poco soddisfacente. Un modello comune consiste nell'esternalizzare lo stato a un altro servizio, ad esempio Cache Redis o un database SQL, rendendo i server Web senza stato. Con i front-end Web senza stato non occorre sapere quali singole istanze siano disponibili. Eseguono tutte lo stesso processo e sono distribuite allo stesso modo.

## <a name="throttling"></a>Limitazione

È stato appurato che il carico posto su un'applicazione varia nel tempo. Ciò può essere dovuto al numero di utenti attivi o simultanei e alle attività in esecuzione. Sebbene sia possibile usare la scalabilità automatica per aggiungere capacità, è anche possibile usare un meccanismo di limitazione per limitare il numero di richieste provenienti da un'origine. È possibile proteggere i limiti delle prestazioni configurando limiti noti a livello di applicazione, evitando così che questa venga compromessa. La limitazione viene usata più di frequente nelle applicazioni che espongono endpoint API.

Dopo che l'applicazione ha identificato la possibile violazione di un limite, può essere avviata la limitazione per verificare che il contratto di servizio del sistema complessivo non venga violato. Ad esempio, se si espone un'API che consente ai clienti di ottenere i dati, è possibile limitare il numero di richieste a 100 al minuto. Se un cliente qualsiasi supera questo limite si può rispondere con un codice di stato 429 HTTP includendo il tempo di attesa prima di poter inviare correttamente un'altra richiesta.

## <a name="serverless"></a>Elaborazione serverless

L'elaborazione serverless offre un ambiente di esecuzione ospitato nel cloud che esegue le app ma che astrae completamente l'ambiente sottostante. È sufficiente creare un'istanza del servizio e aggiungere il codice; non è necessaria, né tantomeno consentita, la gestione o la manutenzione dell'infrastruttura.

Le app senza server vengono configurate per rispondere ad eventi. Potrebbe trattarsi di un endpoint REST, un timer o un messaggio ricevuto da un altro servizio di Azure. L'app senza server viene eseguita solo quando viene attivata da un evento.

L'infrastruttura non è una responsabilità dell'utente. La scalabilità e le prestazioni vengono gestite automaticamente e vengono addebitate solo le risorse esatte usate. Non è necessario prenotare la capacità. Funzioni di Azure, Istanze di contenitore di Azure e App per la logica sono esempi di elaborazione serverless disponibili in Azure.

Di seguito viene ripreso l'esempio Lamna Healthcare. Potrebbe presentare del potenziale per un risparmio sui costi e una gestione più semplice. Si consideri un endpoint API. Invece di ospitare l'API nel Servizio app di Azure dove l'azienda deve pagare la capacità riservata, il cliente può usare un'app di Funzioni di Azure attivata da una richiesta HTTP. Funzioni di Azure consente al team di pagare solo le risorse necessarie per elaborare ogni transazione. Il costo e la scalabilità saranno direttamente in linea con il numero di transazioni nel sistema.

## <a name="containers"></a>Contenitori

Un contenitore è un metodo che esegue le applicazioni in un ambiente virtualizzato. Una macchina virtuale è virtualizzata a livello hardware, dove un hypervisor rende possibile l'esecuzione di più sistemi operativi virtualizzati in un singolo server fisico. Con i contenitori, la virtualizzazione passa a un livello superiore. La virtualizzazione avviene a livello di sistema operativo, consentendo l'esecuzione di più istanze dell'applicazione identiche all'interno dello stesso sistema operativo.

I contenitori si adattano bene agli scenari in cui viene aumentato il numero di istanze. Sono concepiti per essere leggeri e sono stati progettati per essere creati, scalati e arrestati in modo dinamico man mano che l'ambiente e la domanda cambiano.

Un vantaggio dell'uso dei contenitori è la possibilità di eseguire più applicazioni isolate in ogni macchina virtuale. Poiché i contenitori stessi sono protetti e isolati a livello di kernel, non si deve necessariamente usare macchine virtuali separate per carichi di lavoro separati.

Anche se è possibile eseguire i contenitori nelle macchine virtuali, sono disponibile due servizi di Azure che semplificano le operazioni di gestione e scalabilità dei contenitori:

* **Servizio Kubernetes di Azure (AKS)**

  Il servizio Kubernetes di Azure consente di configurare macchine virtuali che fungono da nodi. Azure ospita il piano di gestione di Kubernetes e prevede la fatturazione solo dei nodi del ruolo di lavoro in esecuzione che ospitano i contenitori.

  Per incrementare il numero dei nodi del ruolo di lavoro in Azure si può usare l'interfaccia della riga di comando di Azure per incrementarli manualmente. Al momento della redazione di questo documento, è disponibile una versione di anteprima di Cluster Autoscaler su AKS che consente la scalabilità automatica dei nodi del ruolo di lavoro. Nel cluster Kubernetes è possibile usare HPA (Horizontal Pod Autoscaler) per aumentare il numero di istanze del contenitore da distribuire.

  La scalabilità nel servizio Kubernetes di Azure è possibile anche con il componente Virtual Kubelet descritto di seguito.

* **Istanze di contenitore di Azure (ACI)**
  
  Istanze di contenitore di Azure è un approccio senza server che consente di creare ed eseguire contenitori su richiesta. Viene addebitato solo il tempo di esecuzione per secondo.

  È possibile usare Virtual Kubelet per connettere Istanze di contenitore di Azure all'ambiente Kubernetes, incluso il servizio Kubernetes di Azure. Con Virtual Kubelet, quando il cluster Kubernetes necessita di istanze di contenitore aggiuntive, la domanda può essere soddisfatta da Istanze di contenitore di Azure. Poiché Istanze di contenitore di Azure è senza server, non è necessario avere a disposizione capacità riservata. È quindi possibile usufruire del controllo e della flessibilità offerti dalla scalabilità Kubernetes con la fatturazione per secondo dell'approccio senza server. Al momento della redazione di questo documento, Virtual Kubelet è descritto come software sperimentale e non deve essere usato in scenari di produzione.

## <a name="scaling-at-lamna-healthcare"></a>Scalabilità in Lamna Healthcare

Lamna Healthcare usa un sistema di prenotazioni e gestione dei pazienti. Il sistema gestisce le prenotazioni degli appuntamenti e i record dei pazienti in decine di ospedali e strutture sanitarie. Il servizio sanitario locale funziona al massimo della sua capacità e al momento non ne è prevista la crescita. Il sistema viene eseguito in un sito Web PHP ospitato nel Servizio app di Azure.

Il modello di carico dell'applicazione è prevedibile poiché il cliente opera principalmente dal lunedì al venerdì dalle 9.00 alle 17.00.  Dal martedì al venerdì viene gestita una media di 1.200 transazioni all'ora nell'intero sistema. Durante il fine settimana, il sistema gestisce una media di 500 transazioni all'ora. Dopo la calma del fine settimana, i lunedì sono frenetici con una media di 2.000 transazioni all'ora.

L'applicazione è ospitata in un piano di servizio app S1, ma il team operativo ha notato un uso elevato della CPU (oltre il 95%) in tutte le istanze. L'uso elevato influisce sui tempi di elaborazione e caricamento dell'applicazione. In un ambiente cloud la presenza di risorse con un uso elevato non è necessariamente negativa. Denota un'ottimizzazione della spesa poiché le risorse distribuite vengono usate in maniera ottimale. 

Il team decide di _passare a un livello superiore_ del piano di servizio app per le istanze distribuite, ovvero da S1 (1 vCPU e 1,75 GB di RAM) a S2 (2 vCPU e 3 GB di RAM). Questa operazione viene eseguita facilmente tramite il portale di Azure, ma si otterrebbe lo stesso risultato usando un singolo comando nell'interfaccia della riga di comando di Azure, Azure PowerShell o i modelli di Resource Manager.

Il team decide di automatizzare il numero di istanze distribuite in base a una pianificazione, dal momento che il profilo di carico è prevedibile. Viene quindi configurata la pianificazione di scalabilità automatica del piano di servizio app. Si supponga che due istanze siano in grado di gestire sufficientemente 500 transazioni all'ora. Per soddisfare i requisiti (in base alle informazioni dettagliate e al monitoraggio dei test di carico), il team potrebbe quindi passare a sei istanze per il periodo compreso tra il martedì e il venerdì e a otto istanze per il lunedì.

La scalabilità automatica offre anche un altro vantaggio, ovvero la preparazione per scenari imprevisti. Il sito potrebbe improvvisamente dover affrontare un carico superiore al previsto nel fine settimana (più appuntamenti nella stagione invernale a causa di raffreddori e influenza). Il team può configurare la scalabilità automatica in modo da aumentare di un'istanza quando la percentuale di CPU è superiore al 90% e ridurre di un'istanza quando l'uso è inferiore al 15%.

Il team ha usato il modello di limitazione all'interno dell'API di prenotazione dei pazienti che è stata esposta dietro un'istanza di Gestione API di Azure. Ciò contribuisce a evitare prestazioni inadeguate del sistema consentendo il passaggio solo di un determinato volume di unità elaborate.

## <a name="summary"></a>Riepilogo

È stato illustrato l'aumento e la riduzione delle prestazioni e l'aumento e la riduzione del numero di istanze, nonché l'uso di queste opzioni nell'architettura. È stato anche illustrato come le tecnologie senza server e i contenitori possano contribuire a migliorare le funzionalità di scalabilità. Nel prossimo articolo verranno esaminati l'impatto delle prestazioni di rete sull'applicazione e i diversi modi disponibili per ottimizzare la rete.