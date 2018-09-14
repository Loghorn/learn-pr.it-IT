Scelta della soluzione di archiviazione corretto può causare prestazioni migliori, riduzione dei costi e gestibilità migliorate. In questo caso, applicherai ciò che si sono appresi i dati nel proprio scenario di vendita online e trova il miglior servizio di Azure per ogni set di dati. 

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

**Classificazione dei dati:** semi-strutturati a causa della necessità di estendere o modificare lo schema per i nuovi prodotti

**Operazioni:**

- I clienti richiedono un numero elevato di operazioni di lettura, con la possibilità di eseguire query su molti campi all'interno del database.
- L'azienda richiede un numero elevato di operazioni di scrittura per tenere traccia dell'inventario costantemente.

**Latenza e velocità effettiva:** velocità effettiva elevata e bassa latenza

**Supporto delle transazioni:** necessario

### <a name="recommended-service-azure-cosmos-db"></a>Servizio consigliato: Azure Cosmos DB

Azure Cosmos DB supporta i dati semistrutturati, o dati NoSQL, per impostazione predefinita. Di conseguenza, il supporto di nuovi campi, ad esempio il campo "Bluetooth-abilitato" o eventuali nuovi campi, è necessario in futuro, è un dato con Azure Cosmos DB.

Azure Cosmos DB supporta SQL per le query e tutte le proprietà vengono indicizzate per impostazione predefinita. È possibile creare query in modo che i clienti possono filtrare su qualsiasi proprietà nel catalogo.

Azure Cosmos DB è anche conforme ad ACID, quindi le transazioni vengono completate in base a tali requisiti rigorosi.

Azure Cosmos DB consente inoltre di replicare i dati ovunque nel mondo con un semplice clic. Pertanto, se il sito di e-commerce con utenti concentrati in Inghilterra, degli Stati Uniti e Francia, è possibile replicare i dati per tali data Center per ridurre la latenza, come si spostano fisicamente i dati più vicini agli utenti. 

Anche con i dati replicati in tutto il mondo, è possibile scegliere uno dei cinque livelli di coerenza. Se si sceglie il livello di coerenza giusto, è possibile determinare i compromessi da stabilire tra coerenza, disponibilità, latenza e velocità effettiva. È possibile aumentare le prestazioni per gestire l'aumento della domanda dei clienti durante i periodi di picco degli acquisti e ridurle durante i periodi di calma per limitare i costi.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Azure SQL Database sarebbe una scelta eccellente per questo set di dati non fosse per la necessità di estendere il schema ad hoc per i nuovi prodotti. Nel Database SQL di Azure, deve essere conforme a uno schema di tutti i dati. Database SQL di Azure può fornire molte degli stessi vantaggi di Azure Cosmos DB, ma non può gestire dati eterogenei. 

Anche altri servizi di Azure, come Archiviazione tabelle di Azure, Azure HBase in HDInsight e Cache Redis di Azure, consentono di archiviare dati NoSQL. In questo scenario, gli utenti preferiscono eseguire una query su più campi, in modo che Azure Cosmos DB è una soluzione migliore. Azure Cosmos DB indicizza tutti i campi per impostazione predefinita, mentre gli altri servizi sono limitati i dati che vengono indicizzati e l'esecuzione di query sui risultati dei campi non indicizzata in una riduzione delle prestazioni.

## <a name="photos-and-videos"></a>Foto e video

**Classificazione dei dati:** non strutturati

**Operazioni:**

- Devono solo essere recuperati tramite ID.
- I clienti richiedono un numero elevato di operazioni di lettura con latenza bassa.
- Le operazioni di creazione e aggiornamento saranno abbastanza rare e possono avere una latenza più elevata rispetto alle operazioni di lettura.

**Latenza e velocità effettiva:** le operazioni di recupero in base all'ID devono supportare bassa latenza e velocità effettiva elevata. Le operazioni di creazione e aggiornamento possono avere una latenza più elevata rispetto alle operazioni di lettura.

**Supporto delle transazioni:** non necessario

### <a name="recommended-service-azure-blob-storage"></a>Servizio consigliato: Archiviazione BLOB di Azure

Archiviazione BLOB di Azure supporta l'archiviazione di file come foto e video. Funziona anche con contenuto Delivery Network (rete CDN di Azure) per la memorizzazione nella cache del contenuto usato più di frequente e archiviare i dati sui server perimetrali. Rete CDN di Azure riduce la latenza di mettere a disposizione le tali immagini per gli utenti.

Usando l'archiviazione Blob di Azure, è inoltre possibile spostare immagini dal livello di archiviazione ad accesso frequente per il livello di archiviazione ad accesso sporadico o archivio, in modo da ridurre i costi e la velocità effettiva dello stato attivo su più di frequente visualizzati immagini e video.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Si è stato possibile caricare le immagini nel servizio App di Azure, in modo che il server stesso in cui è in esecuzione l'app gestisce le immagini. Questa soluzione funzionerà se non si dispone di molti file. Ma in presenza di numerosi file per un pubblico globale si otterranno risultati migliori usando Archiviazione BLOB di Azure con la Rete CDN di Azure.

## <a name="business-data"></a>Dati aziendali

**Classificazione dei dati:** strutturati

**Operazioni:** query analitiche complesse di sola lettura su più database

**Latenza e velocità effettiva:** è prevista una certa latenza nei risultati a causa della natura complessa delle query.

**Supporto delle transazioni:** necessario

### <a name="recommended-service-azure-sql-database"></a>Servizio consigliato: database SQL di Azure

Le query sui dati aziendali vengono in genere eseguite dai business analyst, che conoscono più probabilmente SQL rispetto a qualsiasi altro linguaggio di query. Il database SQL di Azure può essere considerato la soluzione ideale anche da solo, ma se associato ad Azure Analysis Services consente ai data analyst di creare un modello semantico sui dati nel database SQL. Analisti dei dati possono quindi condividerlo con gli utenti aziendali, in modo che devono solo per la connessione al modello da qualsiasi strumento di business intelligence (BI) per esplorare i dati e individuare nuove prospettive immediatamente. 

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Azure SQL Data Warehouse supporta le soluzioni OLAP e le query SQL. Ma i business analyst dovranno eseguire query su più database e queste non sono supportate da SQL Data Warehouse.

Oltre a Database SQL di Azure è possibile utilizzare Azure Analysis Services. Ma sono più che mostrano delle doti SQL anziché i business analyst nell'uso di Power BI. Quindi, desiderano ricevere un database che supporta le query SQL, che Azure Analysis Services non esiste. Inoltre, i dati finanziari archiviati nel set di dati aziendali presentano una natura relazionale e multidimensionale. Azure Analysis Services supporta dati tabulari archiviati nel servizio stesso, ma non multidimensionale dei dati. Per analizzare dati multidimensionali con Azure Analysis Services, è possibile usare una query diretta al Database SQL.

Analisi di flusso di Azure è un'ottima soluzione per analizzare i dati e trasformarli in informazioni dettagliate di utilità pratica, ma è un servizio incentrato sui dati di streaming in tempo reale, In questo scenario, il business analyst sta esaminando solo i dati cronologici.

## <a name="summary"></a>Riepilogo

Ogni tipo di dati ha requisiti di archiviazione diverso, e ha il compito di determinare quale soluzione è ottimale. Valutare sempre il tipo di dati, le operazioni necessarie, prevista latenza e la necessità per il supporto delle transazioni.