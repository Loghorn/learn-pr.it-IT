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

## <a name="scalability-and-performance-best-practices"></a>Procedure consigliate per la scalabilità e le prestazioni

Esaminare tutti i livelli dell'applicazione per identificare e risolvere gli eventuali colli di bottiglia delle prestazioni. Questi colli di bottiglia potrebbero essere dovuti a una gestione insufficiente della memoria o persino al processo di aggiunta di indici nel database. Può essere un processo iterativo, in quanto si può individuare un collo di bottiglia e in seguito scoprirne un altro di cui non si era a conoscenza.

L'uso della memorizzazione nella cache nell'architettura può contribuire a migliorare le prestazioni. La memorizzazione nella cache è un meccanismo tramite il quale i dati o gli asset (pagine Web, immagini) usati di frequente vengono archiviati per poter essere recuperati più rapidamente. La memorizzazione nella cache può essere usata a diversi livelli dell'applicazione. È possibile usarla tra i server applicazioni e un database, per ridurre i tempi di recupero dati, oppure tra gli utenti finali e i server Web, posizionando il contenuto statico più vicino all'utente e riducendo il tempo necessario per restituire le pagine Web all'utente finale. La memorizzazione nella cache ha anche l'effetto secondario di eseguire l'offload delle richieste dal database o dai server Web, aumentando le prestazioni per altre richieste.

Non è insolito trovare applicazioni legacy compilate come un'unica grande applicazione che esegue più attività. Queste architetture sono spesso limitate alla scalabilità verticale e non supportano la scalabilità orizzontale. In questi tipi di architetture è opportuno cercare di modernizzare l'applicazione e separare i servizi l'uno dall'altro. La separazione consiste nel dividere le principali funzioni di un'applicazione in applicazioni distinte. Dopo la separazione, ogni servizio può quindi essere scalato orizzontalmente e ridimensionato in base alle esigenze, indipendentemente dagli altri.

Insieme alla separazione, l'aggiunta di un livello di messaggistica tra i servizi può avere un impatto positivo sulle prestazioni. L'aggiunta di un livello di messaggistica crea un buffer per le richieste tra i servizi, che assicura il flusso costante delle richieste stesse senza errori, in caso l'applicazione non sia in grado di soddisfarle. L'applicazione gestisce quindi le richieste rispondendo nell'ordine in cui sono state ricevute.

A seconda dell'architettura in uso, è possibile ottimizzare le prestazioni dell'applicazione in un'ampia varietà di modi. È necessario valutare le prestazioni dell'applicazione e individuare le aree le cui prestazioni potrebbero non essere ottimali.
