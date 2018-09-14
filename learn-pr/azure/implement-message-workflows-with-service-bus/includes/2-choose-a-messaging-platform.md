Esistono molte piattaforme per le comunicazioni che consentono di migliorare l'affidabilità di un'applicazione distribuita, inclusi diversi all'interno di Azure. Ognuno di questi strumenti svolge una funzione diversa. Verranno ora analizzati i singoli strumenti di Azure per aiutare a scegliere quello più appropriato.

L'architettura del nostro pizza ordinamento e applicazione di registrazione richiede alcuni componenti: un sito Web, archiviazione dei dati, servizio back-end, e così via. È possibile associare i componenti dell'applicazione tra loro in molti modi diversi e una singola applicazione può sfruttare i vantaggi di più tecniche. 

È necessario stabilire quali tecniche usare nell'applicazione Contoso Slices. Il primo passaggio consiste nel valutare ogni posizione in cui avviene una comunicazione tra più parti. Alcuni componenti _devono_ venire eseguiti in modo tempestivo per il corretto funzionamento dell'applicazione. Alcuni siano importanti, ma non critico. Infine, altri componenti, ad esempio le notifiche dell'app per dispositivi mobili, possono essere facoltativi.

In questo modulo si apprenderanno informazioni sulle piattaforme di comunicazione disponibili in Azure, in modo da poter scegliere quella più adatta per ogni requisito dell'applicazione.

## <a name="decide-between-messages-and-events"></a>Scegliere tra messaggi ed eventi

Messaggi ed eventi sono entrambi **datagrammi**: pacchetti di dati inviati da un componente a un altro. Le differenze tra di essi a prima vista possono sembrare minime, ma possono determinare differenze significative nella progettazione dell'applicazione. 

### <a name="messages"></a>Messaggi

Nella terminologia delle applicazioni distribuite, la caratteristica distintiva di un messaggio è il fatto che l'integrità complessiva dell'applicazione può basarsi sulla ricezione dei messaggi. È possibile pensare all'invio di un messaggio come un componente che passa il testimone di un flusso di lavoro a un altro componente. L'intero flusso di lavoro può essere un processo di business fondamentale e il messaggio è il cemento che tiene uniti i componenti.

Un messaggio in genere contiene i dati stessi, non solo un riferimento (ad esempio, un URL o ID) ai dati. L'invio dei dati come parte del datagramma è meno fragile rispetto all'invio di un riferimento. L'architettura di messaggistica garantisce la consegna del messaggio e poiché non ricerche aggiuntive sono necessarie, il messaggio viene gestito in modo affidabile. Tuttavia, l'applicazione di invio deve sapere esattamente quali dati includere, per evitare l'invio di troppi dati, che richiede il componente ricevente per svolgere il lavoro non necessario. Da questo punto di vista, il mittente e il destinatario di un messaggio sono spesso collegati da un rigoroso contratto di dati.

Nella nuova architettura di Contoso sezioni, quando viene immesso un ordine di pizza, sarebbe probabilmente usano messaggi. Il front-end web o app per dispositivi mobili invierebbe un messaggio per i componenti di elaborazione back-end. Nel back-end, si otterrebbe passaggi, ad esempio il routing a un archivio di quasi il cliente e ad addebitare la carta di credito avverrebbero.

### <a name="events"></a>Eventi

Un evento attiva notifica che si è verificato qualcosa. Gli eventi sono "leggeri" rispetto ai messaggi e vengono spesso usati per le comunicazioni broadcast.

Gli eventi hanno le caratteristiche seguenti:
* L'evento può essere inviato a più destinatari o a nessuno
* Gli eventi sono spesso destinati a una distribuzione estesa, ovvero hanno un numero elevato di sottoscrittori per ogni origine di pubblicazione
* Il componente di pubblicazione dell'evento non ha aspettative sull'azione eseguita da un componente destinatario

La catena di pizzerie userebbe probabilmente gli eventi per inviare notifiche agli utenti relative alle modifiche di stato. È stato possibile inviare gli eventi di modifica dello stato per griglia di eventi di Azure, quindi a funzioni di Azure e hub di notifica di Azure per un completamente _senza server_ soluzione.

Questa differenza tra eventi e messaggi è fondamentale perché le piattaforme di comunicazione sono in genere progettate per gestire uno o l'altro elemento. Il bus di servizio è progettato per gestire i messaggi. Se si vuole inviare eventi, scegliere Griglia di eventi. 

Azure offre anche l'hub eventi di Azure, ma viene usato più spesso per un tipo specifico di flusso elevata per il flusso di comunicazioni usate per analitica. Ad esempio, se si fosse stato eseguito in rete sensori nel nostro forni pizza, potremmo utilizziamo hub eventi insieme a Azure Stream Analitica per cercare i modelli nei cambiamenti di temperatura che potrebbero indicare un incendio indesiderato o usura del componente.

## <a name="service-bus-topics-queues-and-relays"></a>Gli inoltri, code e argomenti del Bus di servizio

Il Bus di servizio di Azure per poter scambiare messaggi in tre modi diversi: code, argomenti e inoltri.

### <a name="what-is-a-queue"></a>Che cos'è una coda?

Una **coda** è una semplice posizione di archiviazione temporanea per i messaggi. Un componente mittente aggiunge un messaggio alla coda. Un componente di destinazione preleva il messaggio all'inizio della coda. In circostanze normali, ogni messaggio viene ricevuto da un solo ricevitore.

![Coda del bus di servizio di Azure](../media/2-service-bus-queue.png)

Le code disaccoppiano i componenti di origine e di destinazione per isolare i componenti di destinazione in caso di domanda elevata. 

Durante i periodi di picco, i messaggi possono rivelarsi più velocemente di quanto i componenti di destinazione possono gestirle. Poiché i componenti di origine non devono di alcuna connessione diretta alla destinazione, l'origine non è interessato e aumenterà la coda. I componenti di destinazione rimuoveranno i messaggi dalla coda man mano che sono in grado di gestirli. Quando la domanda diminuisce, i componenti di destinazione possono recuperare e la lunghezza della coda si riduce. 

Una coda risponde a una situazione di domanda elevata di questo tipo senza che sia necessario aggiungere risorse al sistema. Tuttavia, per i messaggi che devono essere gestiti in modo relativamente rapido, l'aggiunta di altre istanze del componente di destinazione può consentire la condivisione del carico. Ogni messaggio viene gestito da una sola istanza. Si tratta di un metodo efficace per ridimensionare l'intera applicazione aggiungendo solo le risorse per i componenti che ne hanno effettivamente bisogno.

### <a name="what-is-a-topic"></a>Che cos'è un argomento?

Un **argomento** è simile a una coda ma può avere più sottoscrizioni. Ciò significa che più componenti di destinazione possono sottoscrivere un singolo argomento, quindi ogni messaggio viene recapitato a più destinatari. Le sottoscrizioni consentono anche di filtrare i messaggi nell'argomento per ricevere solo i messaggi rilevanti. Le sottoscrizioni offrono le stesse comunicazioni disaccoppiate delle code e rispondono nello stesso modo in caso di domanda elevata. Usare un argomento se si vuole che ogni messaggio venga recapitato a più di un componente di destinazione.

Gli argomenti non sono supportati nel piano tariffario Basic.

![Argomento del bus di servizio di Azure](../media/2-service-bus-topic.png)

### <a name="what-is-a-relay"></a>Che cos'è un endpoint di inoltro?

Un **inoltro** è un oggetto che esegue una comunicazione sincrona bidirezionale tra applicazioni. Non si tratta di una posizione di archiviazione temporanea per i messaggi come nel caso di code e argomenti. Offre invece bidirezionale, le connessioni senza buffer attraverso i limiti di rete, ad esempio i firewall. Quando si desidera che le comunicazioni dirette tra componenti come se sono stati che si trova nello stesso segmento di rete ma separati da dispositivi di sicurezza di rete, usare un endpoint di inoltro.

> [!NOTE]
> Anche se gli inoltri fanno parte del Bus di servizio, essi non implementano flussi di lavoro di messaggistica loosely-coupled e non viene discussa più avanti in questo modulo.

## <a name="service-bus-queues-and-storage-queues"></a>Le code del Bus di servizio e le code di archiviazione

Esistono due funzionalità di Azure che includono le code di messaggi: account di archiviazione di Azure e Bus di servizio. Come guida generale, le code di archiviazione sono più semplici da utilizzare ma sono meno sofisticate e flessibili rispetto alle code del Bus di servizio.

I principali vantaggi delle code del bus di servizio includono:

* Sono supportati messaggi di dimensioni maggiori (256 KB per ogni messaggio rispetto a 64 KB)
* Sono supportati sia il recapito di tipo At-Least-Once che quello di tipo At-Most-Once: è possibile scegliere tra una probabilità molto piccola che un messaggio vada perso o una probabilità molto piccola che venga gestito due volte
* Garanzie **first-in-first-out (FIFO)** ordine - nello stesso ordine vengono aggiunti vengono gestiti i messaggi (anche se il normale funzionamento di una coda FIFO, non è garantito per ogni messaggio)
* È possibile raggruppare più messaggi in una transazione: se un messaggio nella transazione non viene recapitato, non vengono recapitati nemmeno gli altri messaggi nella transazione
* È supportata la sicurezza basata sui ruoli
* Non è necessario che i componenti di destinazione eseguano continuamente il polling della coda

Vantaggi di code di archiviazione:

* Supporta la dimensione della coda senza limiti (rispetto ai limiti di 80 GB per le code del Bus di servizio)
* Viene conservato un log di tutti i messaggi

## <a name="how-to-choose-a-communications-technology"></a>Come scegliere una tecnologia di comunicazione

Sono stati analizzati i diversi concetti e le implementazioni offerte da Azure. Verrà ora esaminato il processo decisionale per ognuna delle comunicazioni.

#### <a name="consider-the-following-questions"></a>Prendere in considerazione le domande seguenti:

1. La comunicazione è un evento? In questo caso, provare a usare griglia di eventi o hub eventi.

1. Un singolo messaggio deve essere recapitato a più di una destinazione? In tal caso, usare un argomento del bus di servizio. In caso contrario, usare una coda.

Se si decide che è necessaria una coda:

#### <a name="choose-service-bus-queues-if"></a>Scegliere le code del bus di servizio se:

- Necessaria una garanzia di recapito at-most-once
- È necessaria una garanzia FIFO
- È necessario raggruppare i messaggi in transazioni
- Si vuole ricevere i messaggi senza eseguire il polling della coda
- È necessario fornire l'accesso basato sui ruoli alle code
- È necessario gestire messaggi di dimensioni superiori a 64 KB ma inferiore a 256 KB
- Le dimensioni della coda non supereranno gli 80 GB
- Si vuole poter pubblicare e usare batch di messaggi

#### <a name="choose-queue-storage-if"></a>Scegliere l'archiviazione di coda se:
- È necessaria una coda semplice senza particolari requisiti aggiuntivi
- È necessario un audit trail di tutti i messaggi che passano attraverso la coda
- Si prevede che le dimensioni della coda possano superare gli 80 GB
- Si vuole tenere traccia dello stato di elaborazione di un messaggio all'interno della coda

Anche se i componenti di un'applicazione distribuita possono comunicare direttamente, è spesso possibile migliorare l'affidabilità delle comunicazioni con una piattaforma di comunicazione intermedio, ad esempio il Bus di servizio di Azure o griglia di eventi di Azure.

Griglia di eventi è progettata per gli eventi, quale inviare una notifica ai destinatari solo di un evento e non contengono i dati non elaborati associati all'evento. Hub eventi di Azure è progettato per i tipi di analitica elevata per il flusso di eventi. Le code di archiviazione e del bus di servizio di Azure sono destinate ai messaggi e possono essere usate per associare le parti principali del flusso di lavoro di un'applicazione.

Se i requisiti sono semplici, se si vuole inviare ogni messaggio alla destinazione solo una o se si vuole scrivere codice più rapidamente possibile, una coda di archiviazione potrebbe essere la scelta migliore. In caso contrario, le code del bus di servizio offrono molte più opzioni e flessibilità.

Se si vuole inviare messaggi a più sottoscrittori, usare un argomento del bus di servizio.
