Congratulazioni, il modulo sulle prestazioni e la scalabilità è stato completato. Qui di seguito sono indicati gli argomenti trattati:

## <a name="scaling-up-and-scaling-out"></a>Aumento delle prestazioni e aumento del numero di istanze

- Differenza tra scalabilità verticale e orizzontale (il livello di risorsa su cui è stato eseguito il provisioning su un'istanza) e aumento/diminuzione del numero di istanze disponibili. Sono inoltre stati fatti alcuni esempi in relazione ad Azure.
- Considerazioni da fare durante l'aumento/diminuzione del numero di istanze disponibili in relazione ai tempi di avvio delle applicazioni e alla gestione dello stato.
- Scalabilità automatica e possibilità di ridimensionare il numero di istanze in base a una pianificazione o alla richiesta di un'applicazione.
- Tecnologie alternative per il supporto della scalabilità, inclusi l'elaborazione serverless e i contenitori.

## <a name="optimize-network-performance"></a>Ottimizzare le prestazioni di rete

- La latenza di rete misura il ritardo dei dati inviati da un mittente a un destinatario.
- In un ambiente cloud, le applicazioni con interazioni frequenti potrebbero subire una riduzione delle prestazioni rispetto alle applicazioni locali, poiché le risorse non vengono più condivise immediatamente.
- La condivisione delle API nelle vicinanze di un endpoint di scrittura database può migliorare le prestazioni, poiché la latenza di rete tra le due risorse viene ridotta.
- Gestione traffico di Azure può essere usato per instradare gli utenti all'istanza distribuita con una latenza di rete minima.
- Le reti di distribuzione dei contenuti di Azure (CDN) possono essere usate ai fini del calcolo offload dall'applicazione principale e dell'accelerazione dei tempi di caricamento delle applicazioni tramite memorizzazione dei contenuti nella cache su un nodo perimetrale CDN nelle vicinanze di un utente.

## <a name="optimize-storage-performance"></a>Ottimizzare le prestazioni di archiviazione

- Sono disponibili tre tipi principali di dischi per le distribuzioni IaaS. Dischi HDD Standard (latenza incoerente e velocità effettiva inferiore), dischi SSD Standard (latenza coerente e velocità effettiva inferiore) e dischi SSD Premium (latenza coerente e velocità effettiva elevata).
- La memorizzazione nella cache può essere usata a livello dell'applicazione per migliorare i tempi di caricamento di un'applicazione. Le informazioni richieste di frequente possono essere archiviate in una cache davanti al database, che può quindi ottimizzare i tempi di caricamento delle informazioni più richieste.
- Considerare l'uso di un archivio dati di back-end appropriato per il processo (persistenza poliglotta) durante lo compilazione della soluzione.

## <a name="identify-performance-bottlenecks"></a>Identificare i colli di bottiglia in termini di prestazioni

- Importanza di comprendere le aspettative dell'applicazione prima di architettare o compilare un'operazione.
- Informazioni sulle strategie DevOps e sulla loro efficacia in termini di compilazione di applicazioni più robuste e prestanti.
- Riepilogo delle opzioni di monitoraggio disponibili in Azure, tra cui Monitoraggio di Azure (pannello di controllo per il monitoraggio in Azure), Log Analytics di Azure (inserimento di log e monitoraggio IaaS) e Application Insights (monitoraggio delle prestazioni dell'applicazione, incluse disponibilità, prestazioni e informazioni di eccezione).
