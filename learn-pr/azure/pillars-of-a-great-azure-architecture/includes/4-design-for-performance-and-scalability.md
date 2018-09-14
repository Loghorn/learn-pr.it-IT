Immaginiamo che sia appena stata pubblicata una notizia sulla rivoluzionaria cura contro il cancro scoperta dalla propria organizzazione. Si tratta di una tappa fondamentale nella storia della ricerca medica, destinata senza dubbio a portare un grande afflusso di traffico nel sito Web aziendale. Il sito Web sarà in grado di gestire questo aumento di traffico, oppure a causa del carico elevato rallenterà le prestazioni o smetterà di rispondere?

In questo articolo esamineremo alcuni dei principi di base che assicurano prestazioni dell'applicazione eccezionali tramite l'uso del ridimensionamento e dell'ottimizzazione.

## <a name="what-is-scaling-and-performance-optimization"></a>Che cosa sono il ridimensionamento e l'ottimizzazione delle prestazioni?

Per ridimensionamento e ottimizzazione delle prestazioni si intende la capacità di adattare le risorse a disposizione di un'applicazione in base alle richieste che riceve. L'ottimizzazione delle prestazioni include il ridimensionamento delle risorse, l'identificazione e l'ottimizzazione di potenziali colli di bottiglia e l'ottimizzazione del codice dell'applicazione per le prestazioni massime.

### <a name="scaling"></a>Ridimensionamento

Le risorse di calcolo possono essere ridimensionate, o scalate, in due direzioni:

* La *scalabilità verticale* consiste nell'aggiunta di altre risorse a una singola istanza.
* La *scalabilità orizzontale* consiste nell'aggiunta di istanze.

![Scalabilità verticale e orizzontale](../media-draft/scale-up-scale-out.png)

La scalabilità verticale riguarda l'aggiunta di più risorse, come CPU o memoria, a una singola istanza. Questa istanza potrebbe essere una macchina virtuale o un servizio PaaS. L'aggiunta di ulteriore capacità all'istanza aumenta le risorse disponibili per l'applicazione, ma non senza un limite. La capacità delle macchine virtuali è infatti limitata a quella dell'host in cui vengono eseguite e gli host stessi hanno limitazioni fisiche. Quando si scala verticalmente un'istanza, prima o poi si incorre in questi limiti, che impediscono di aggiungere ulteriori risorse all'istanza.

La scalabilità orizzontale riguarda l'aggiunta di ulteriori istanze a un servizio. Possono essere macchine virtuali o servizi PaaS, ma in questo caso, invece di aggiungere capacità rendendo una singola istanza più potente, si aggiunge capacità aumentando il numero totale di istanze. Il vantaggio della scalabilità orizzontale è che può essere applicata all'infinito se si hanno più computer da aggiungere all'architettura. La scalabilità orizzontale richiede un qualche tipo di distribuzione del carico. Può essere un servizio di bilanciamento del carico che distribuisce le richieste tra i server disponibili oppure un meccanismo di individuazione dei servizi che identifica i server attivi a cui inviare le richieste.

In entrambi i casi le risorse possono essere ridotte, riducendo di conseguenza anche i costi dell'ottimizzazione.

### <a name="performance-optimization"></a>Ottimizzazione delle prestazioni

Nell'ambito dell'ottimizzazione delle prestazioni, i due elementi fondamentali che assicurano che le prestazioni siano accettabili sono la rete e l'archiviazione. Entrambe possono avere un impatto sul tempo di risposta dell'applicazione. La scelta delle tecnologie di rete e di archiviazione più appropriate per la propria architettura contribuirà a garantire la migliore esperienza utente.

Per ottimizzare le prestazioni è importante anche conoscere le prestazioni delle applicazioni stesse. Errori, codice con prestazioni insufficienti e colli di bottiglia in sistemi dipendenti possono essere individuati con uno strumento di gestione delle prestazioni delle applicazioni. Spesso questi problemi possono essere nascosti ad utenti finali, sviluppatori e amministratori, ma possono avere un impatto negativo sulle prestazioni complessive dell'applicazione.

## <a name="scalability-and-performance-patterns-and-practices"></a>Procedure consigliate e modelli di scalabilità e prestazioni

Diamo un'occhiata alcuni modelli e procedure consigliate che possono essere sfruttate per migliorare la scalabilità e prestazioni dell'applicazione.

### <a name="data-partitioning"></a>Partizionamento dei dati

In molte soluzioni su larga scala, i dati vengono suddivisi in partizioni separate per poter essere gestiti e per poter accedervi separatamente. La strategia di partizionamento deve essere scelta attentamente per ottimizzare le prestazioni riducendo al minimo gli effetti negativi. Il partizionamento può aiutare a migliorare la scalabilità, ridurre i conflitti e ottimizzare le prestazioni.

### <a name="caching"></a>Memorizzazione nella cache

Usare la memorizzazione nella cache nell'architettura può contribuire al miglioramento delle prestazioni. La memorizzazione nella cache è un meccanismo tramite il quale i dati o gli asset (pagine Web, immagini) usati di frequente vengono archiviati per poter essere recuperati più rapidamente. La memorizzazione nella cache può essere usata a diversi livelli dell'applicazione. È possibile usarla tra i server applicazioni e un database, per ridurre i tempi di recupero dati, oppure tra gli utenti finali e i server Web, posizionando il contenuto statico più vicino all'utente e riducendo il tempo necessario per restituire le pagine Web all'utente finale. La memorizzazione nella cache ha anche l'effetto secondario di eseguire l'offload delle richieste dal database o dai server Web, aumentando le prestazioni per altre richieste.

### <a name="autoscaling"></a>Scalabilità automatica

La scalabilità automatica è il processo di allocazione dinamica delle risorse in base ai requisiti di corrispondenza delle prestazioni. Man mano che aumenta il volume di lavoro, un'applicazione potrebbe richiedere altre risorse per gestire i livelli di prestazioni desiderate e soddisfare i contratti di servizio. Quando la richiesta diminuisce e le risorse aggiuntive non sono più necessarie, è possibile deallocarle per ridurre al minimo i costi.

La scalabilità automatica sfrutta la flessibilità degli ambienti ospitati nel cloud, attenuando contemporaneamente il sovraccarico di gestione. Riduce la necessità che un operatore monitori continuamente le prestazioni di un sistema e prenda decisioni sull'aggiunta o la rimozione di risorse.

### <a name="decouple-resource-intensive-tasks-as-background-jobs"></a>Separare le attività a elevato utilizzo di risorse come processi in background

Molti tipi di applicazioni richiedono attività in background che vengono eseguite in modo indipendente dell'interfaccia utente (UI). Alcuni esempi sono i processi batch, le attività con uso intensivo della capacità di elaborazione e i processi a esecuzione prolungata come i flussi di lavoro. I processi in background possono essere eseguiti senza l'interazione dell'utente: l'applicazione può avviare il processo e quindi continuare a elaborare le richieste interattive degli utenti. Questo consente di ridurre al minimo il carico sull'interfaccia utente dell'applicazione, consentendo di migliorare la disponibilità e di abbreviare i tempi di risposta interattiva.

### <a name="use-a-messaging-layer-between-services"></a>Usare un livello di messaggistica tra servizi

Aggiunta di un livello di messaggistica tra servizi può avere dei vantaggi di prestazioni e scalabilità. L'aggiunta di un livello di messaggistica crea un buffer per le richieste tra i servizi, che assicura il flusso costante delle richieste stesse senza errori, in caso l'applicazione non sia in grado di soddisfarle. L'applicazione gestisce quindi le richieste rispondendo nell'ordine in cui sono state ricevute.

### <a name="implement-scale-units"></a>Implementare l'unità di scala

Scalare come unità. Per ogni risorsa, determinare l'impatto che può avere un'attività di ridimensionamento nei sistemi dipendenti. In questo modo applicare operazioni di scalabilità più semplice e meno soggetto a impatto negativo sull'applicazione. Ad esempio, se si aggiunge un numero x di ruoli Web e di lavoro, potrebbero essere necessari y code aggiuntive e z account di archiviazione per gestire il carico di lavoro aggiuntivo generato dai ruoli. In modo che un'unità di scala può essere costituito da x ruoli web e di lavoro, code y e z account di archiviazione. Progettare l'applicazione in modo che possa essere ridimensionata facilmente aggiungendo una o più unità di scala.

### <a name="performance-monitoring"></a>Monitoraggio delle prestazioni

Le applicazioni distribuite e i servizi in esecuzione nel cloud sono, per loro natura, componenti software complessi che comprendono molte parti mobili. In un ambiente di produzione è importante essere in grado di rilevare il modo in cui gli utenti usano il sistema, di tracciare l'utilizzo delle risorse e, in generale, di monitorare l'integrità e le prestazioni del sistema stesso. Queste informazioni possono essere usate come strumento diagnostico per rilevare e correggere i problemi, nonché per individuare potenziali problemi e impedire che si verifichino.

Esaminare tutti i livelli dell'applicazione per identificare e risolvere gli eventuali colli di bottiglia delle prestazioni. Questi colli di bottiglia potrebbero essere dovuti a una gestione insufficiente della memoria o persino al processo di aggiunta di indici nel database. Potrebbe essere un processo iterativo poiché più necessario un collo di bottiglia e quindi individuare un altro che erano consapevoli di.

Con un approccio completo per il monitoraggio delle prestazioni, sarà in grado di determinare quali tipi di Microsoft patterns and practices che potranno usufruire dell'architettura.