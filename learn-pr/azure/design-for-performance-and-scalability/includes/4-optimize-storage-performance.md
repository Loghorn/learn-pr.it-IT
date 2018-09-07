È importante fare alcune considerazioni sulle prestazioni di archiviazione nell'architettura. Proprio come la latenza di rete, una riduzione delle prestazioni a livello di archiviazione può compromettere l'esperienza degli utenti finali. In che modo si potrebbe ottimizzare l'archiviazione dei dati? Quali aspetti occorre prendere in considerazione per assicurarsi di non introdurre colli di bottiglia in termini di archiviazione nell'architettura? Qui di seguito viene spiegato come ottimizzare le prestazioni dell'archiviazione nell'architettura.

## <a name="optimize-virtual-machine-storage-performance"></a>Ottimizzare le prestazioni di archiviazione della macchina virtuale

Innanzitutto verrà spiegato come ottimizzare l'archiviazione per le macchine virtuali. L'archiviazione su disco svolge un ruolo fondamentale nelle prestazioni delle macchine virtuali. La scelta del tipo di disco più giusto per l'applicazione è una decisione importante.

Applicazioni diverse hanno diversi requisiti di archiviazione. L'applicazione potrebbe essere sensibile alla latenza delle letture e scritture del disco, oppure potrebbe richiedere di gestire un numero elevato di operazioni di input/output al secondo (IOPS) o una maggiore velocità effettiva complessiva del disco.

Quando si compila un carico di lavoro IaaS, quale tipo di disco è opportuno usare? Vi sono quattro opzioni:

* **Archiviazione SSD locale**: ciascuna macchina virtuale ha un disco temporaneo supportato da un'archiviazione SSD locale. Le sue dimensioni variano a seconda delle dimensioni della macchina virtuale. Poiché si tratta di un SSD locale, le prestazioni sono elevate, ma i dati potrebbero venire persi durante un evento di manutenzione o una ridistribuzione della macchina virtuale. Questo disco è adatto solo per l'archiviazione temporanea dei dati che non sono necessari in modo permanente. Questo disco è molto utile per il file di paging o di scambio e per elementi quali tempdb in SQL Server. Non è previsto alcun addebito per questa risorsa di archiviazione, perché è inclusa nel costo della macchina virtuale.

* **Archiviazione HDD standard**: si tratta di una risorsa di archiviazione su disco con spindle e potrebbe essere adatta anche quando l'applicazione non presenta una velocità effettiva minore o una latenza incoerente. Un carico di lavoro di sviluppo/test in cui non sono necessarie prestazioni garantite è un'applicazione ideale per questo tipo di disco.

* **Archiviazione SSD standard**: si tratta di una risorsa di archiviazione basata su SSD con la bassa latenza delle unità SSD, ma livelli inferiori di velocità effettiva. Un server Web non di produzione rappresenta un buon caso di uso per questo tipo di disco.

* **Archiviazione SSD Premium**: questa risorsa di archiviazione basata su SSD è particolarmente adatta per i carichi di lavoro dell'ambiente di produzione che richiedono la massima affidabilità e una bassa latenza costante, oppure livelli elevati di IOPS e velocità effettiva. Poiché possono vantare elevati livelli di prestazioni e affidabilità, sono consigliati per tutti i carichi di lavoro della produzione.

L'archiviazione Premium può essere collegata solo a macchine virtuali di determinate dimensioni. Tali macchine virtuali sono quelle che hanno una S nel nome, ad esempio D2s_v3 o Standard_F2s_v2. Qualsiasi tipo di macchina virtuale (con o senza una s nel nome) può essere collegata a unità SSD o HDD di archiviazione standard.

È possibile effettuare lo striping dei dischi tramite una tecnologia di striping (come Storage Spaces Direct in Windows o mdadm in Linux) per aumentare la velocità effettiva e gli IOPS distribuendo attività tra più dischi. Lo striping del disco consente di superare i limiti delle prestazioni dei dischi e viene spesso impiegato in sistemi database ad alte prestazioni e altri sistemi con requisiti di archiviazione intensivi.

Quando ci si affida ai carichi di lavoro della macchina virtuale, è necessario valutare i requisiti delle prestazioni dell'applicazione per determinare lo spazio di archiviazione sottostante di cui verrà eseguito il provisioning per le macchine virtuali.

## <a name="optimize-storage-performance-for-your-application"></a>Ottimizzare le prestazioni di archiviazione dell'applicazione

Sebbene sia possibile usare diverse tecnologie di archiviazione per migliorare le prestazioni del disco non elaborato, è anche possibile migliorare le prestazioni di accesso ai dati a livello di applicazione. Qui di seguito verranno esaminati alcuni modi per farlo.

### <a name="caching"></a>Memorizzazione nella cache

Un approccio comune per migliorare le prestazioni delle applicazioni è quello di integrare un livello di caching tra l'applicazione e l'archivio dati. In genere, la cache archivia dati nella memoria e ne consente il recupero in modo rapido. Questi dati possono essere dati usati di frequente, dati specificati da un database o dati temporanei, ad esempio lo stato utente. È possibile controllare il tipo di dati archiviati, la frequenza di aggiornamento e la relativa scadenza. Condividendo questa cache nella stessa area dell'applicazione e del database, sarà possibile ridurre la latenza complessiva tra i due. Il pull dei dati dalla cache sarà tendenzialmente più rapido rispetto al recupero degli stessi dati da un database. Di conseguenza, usando un livello di caching sarà possibile migliorare notevolmente le prestazioni complessive dell'applicazione.

![Cache](../media/cache.png)

Cache Redis di Azure è un servizio di memorizzazione nella cache di Azure. Si basa su Cache Redis open source. Cache Redis di Azure è un servizio completamente gestito offerto da Microsoft. Si seleziona il livello di prestazioni richiesto e si configura l'applicazione per usare il servizio.

### <a name="polyglot-persistence"></a>Persistenza poliglotta

La persistenza poliglotta presuppone l'uso di tecnologie di archiviazione dati diverse per gestire i requisiti di archiviazione.

Si consideri un esempio di e-commerce. È possibile archiviare gli asset dell'applicazione in un archivio BLOB, recensioni dei prodotti e raccomandazioni in un archivio NoSQL e un profilo utente/dati dell'account in un database SQL.

![polyglotPersistence](../media/polyglotpersistence.png)

È importante in quanto i diversi archivi dati sono progettati per determinati casi d'uso, o potrebbero essere più accessibili per via dei costi limitati. Ad esempio, l'archiviazione di BLOB in un database SQL può essere costosa e l'accesso potrebbe essere più lento rispetto a un archivio BLOB.

L'uso di diversi archivi di backup aumenta la complessità della soluzione. Si considerino le modalità di osservazione dei requisiti non funzionali tra gli archivi dati e l'impatto della riduzione delle prestazioni del servizio sull'intera applicazione. Si consideri inoltre come mantenere i dati coerenti tra gli archivi dati. 

**La coerenza finale** offre spesso un buon bilanciamento, ma sono disponibili diversi modelli di coerenza a seconda del servizio.

Coerenza finale significa che gli archivi dati di replica convergono se non sono presenti ulteriori operazioni di scrittura. Se viene eseguita un'operazione di scrittura su uno di questi archivi dati, le operazioni di lettura da un altro archivio possono fornire dati non aggiornati. La coerenza finale consente una scalabilità superiore poiché la latenza per le operazioni di lettura e scrittura è bassa ed elimina i tempi di attesa per verificare se le informazioni sono coerenti tra tutti gli archivi.

## <a name="lamna-healthcare-example"></a>Esempio: Lamna Healthcare

Il sistema di prenotazione pazienti di Lamna Healthcare è ospitato in due aree di Azure, l'Europa occidentale e l'Australia orientale. Usano le macchine virtuali come nodi front-end per distribuire il sito Web, mentre il database SQL di Azure viene distribuito in Europa occidentale come primario e in Australia orientale come secondario leggibile. I nodi front-end non richiedono livelli elevati di velocità effettiva del disco, ma prestazioni di latenza coerenti, affidabilità di produzione e un'archiviazione basata su SSD Premium.

Ospitano un'istanza di Cache Redis in ogni area di Azure per archiviare le richieste più comuni degli utenti e la disponibilità dei dottori. La memorizzazione nella cache è stata implementata in modo da ottimizzare le prestazioni delle attività di lettura dati più comuni osservate nell'applicazione.

## <a name="summary"></a>Riepilogo

Sono stati descritti alcuni esempi su come migliorare le prestazioni di archiviazione a livello di infrastruttura scegliendo l'architettura disco più corretta, e a livello di applicazione usando la memorizzazione nella cache e scegliendo la piattaforma più giusta per i dati. Una soluzione strutturata in modo appropriato garantisce il migliore accesso possibile ai dati. Ora verrà esaminato come è possibile identificare i problemi di prestazioni in un'architettura.