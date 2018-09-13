La scelta della soluzione di archiviazione appropriata può aiutare a migliorare le prestazioni. In questa unità si applicheranno le nozioni apprese sui dati dello scenario di vendita online e si individuerà il servizio di Azure migliore per ogni set di dati. 

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

**Classificazione dei dati:** semistrutturati

**Operazioni:**

- I clienti richiedono numerose operazioni di lettura, con la possibilità di eseguire query su numerosi campi nel database.
- L'azienda richiede un numero elevato di operazioni di scrittura per tenere traccia dell'inventario che cambia continuamente.

**Latenza e velocità effettiva:** velocità effettiva elevata e bassa latenza

**Supporto delle transazioni:** necessario

### <a name="recommended-service-azure-cosmos-db"></a>Servizio consigliato: Azure Cosmos DB

Azure Cosmos DB supporta i dati semistrutturati, o dati NoSQL, per impostazione predefinita. Con Azure Cosmos DB è quindi disponibile il supporto dei nuovi campi, ad esempio il campo relativo alla funzionalità Bluetooth o qualsiasi nuovo campo necessario in futuro.

Per quanto riguarda le operazioni, Azure Cosmos DB supporta SQL per le query e tutte le proprietà sono indicizzate per impostazione predefinita. La creazione di query per filtrare in base a qualsiasi campo è supportata.

In merito alla latenza e alla velocità effettiva, Azure Cosmos DB consente di configurare la velocità effettiva in autonomia. È possibile aumentare le prestazioni per gestire l'aumento della domanda dei clienti durante i periodi di picco degli acquisti e ridurle durante i periodi di calma per limitare i costi. E poiché Azure Cosmos DB indicizza tutte le proprietà per impostazione predefinita, è possibile consentire ai clienti di eseguire query su qualsiasi campo.

Azure Cosmos DB è anche conforme ad ACID, quindi le transazioni vengono completate in base a tali requisiti rigorosi.

Azure Cosmos DB consente inoltre di replicare i dati ovunque nel mondo con un semplice clic. Se quindi gli utenti del sito di e-commerce sono concentrati negli Stati Uniti, in Francia e in Inghilterra, è possibile replicare i dati in tali data center per ridurre la latenza grazie a una maggiore vicinanza fisica dei dati agli utenti. Anche con i dati replicati in tutto il mondo, è possibile scegliere uno di cinque livelli di coerenza e scegliere i compromessi da accettare tra coerenza, disponibilità, latenza e velocità effettiva.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Anche altri servizi di Azure, come Archiviazione tabelle di Azure, Azure HBase in HDInsight e Cache Redis di Azure, consentono di archiviare dati NoSQL. In questo scenario, dal momento che gli utenti vogliono eseguire query su più campi, Azure Cosmos DB è la scelta più adatta. Questo perché Azure Cosmos DB indicizza tutti i campi per impostazione predefinita, mentre gli altri servizi si limitano ai dati che indicizzano e offrono funzionalità limitate per eseguire query su tutti i campi del database.

## <a name="photos-and-videos"></a>Foto e video

**Classificazione dei dati:** non strutturati

**Operazioni:**

- Foto e video devono essere recuperati solo in base all'ID.
- Le operazioni di creazione e aggiornamento saranno abbastanza rare e possono avere una latenza più elevata rispetto alle operazioni di lettura.

**Latenza e velocità effettiva:** le operazioni di recupero in base all'ID devono supportare bassa latenza e velocità effettiva elevata. Le operazioni di creazione e aggiornamento possono avere una latenza più elevata rispetto alle operazioni di lettura.

**Supporto delle transazioni:** non necessario

### <a name="recommended-service-azure-blob-storage"></a>Servizio consigliato: Archiviazione BLOB di Azure

Archiviazione BLOB di Azure supporta l'archiviazione di file come foto e video. Si integra anche con la Rete per la distribuzione di contenuti di Azure (rete CDN) per la memorizzazione nella cache dei contenuti più usati e la loro archiviazione nei server perimetrali, al fine di ridurre la latenza nel recupero delle immagini per gli utenti.

Con Archiviazione BLOB di Azure, è anche possibile spostare le immagini dal livello di archiviazione ad accesso frequente al livello di archiviazione archivio o ad accesso sporadico per ridurre i costi e ottimizzare la velocità effettiva per le immagini e i video più visualizzati.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Si potrebbero caricare le immagini nel Servizio app di Azure, così che lo stesso server che esegue l'app fornisca anche le immagini, ma questo funzionerebbe solo se il numero di immagini è ridotto. Ma in presenza di numerosi file per un pubblico globale si otterranno risultati migliori usando Archiviazione BLOB di Azure con la Rete CDN di Azure.

## <a name="business-data"></a>Dati aziendali

**Classificazione dei dati:** strutturati

**Operazioni:** query analitiche complesse di sola lettura su più database

**Latenza e velocità effettiva:** è prevista una certa latenza nei risultati a causa della natura complessa delle query.

**Supporto delle transazioni:** necessario

### <a name="recommended-service-azure-sql-database"></a>Servizio consigliato: database SQL di Azure

Le query sui dati aziendali vengono in genere eseguite dai business analyst, che conoscono più probabilmente SQL rispetto a qualsiasi altro linguaggio di query. Il database SQL di Azure può essere considerato la soluzione ideale anche da solo, ma se associato ad Azure Analysis Services consente ai data analyst di creare un modello semantico sui dati nel database SQL. Questo modello potrà quindi essere condiviso con gli utenti aziendali, connettendosi al modello da qualsiasi strumento di business intelligence, per esplorare immediatamente i dati e ottenere informazioni dettagliate. 

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Azure SQL Data Warehouse supporta le soluzioni OLAP e le query SQL. Ma i business analyst dovranno eseguire query su più database e queste non sono supportate da SQL Data Warehouse.

Analysis Services potrebbe essere usato in aggiunta al database SQL. Ma i business analyst sono più abituati a usare SQL che non Power BI e quindi preferiscono usare un database che supporti le query SQL, che invece non sono supportate da Analysis Services. Inoltre, i dati finanziari archiviati nel set di dati aziendali presentano una natura relazionale e multidimensionale. Analysis Services supporta i dati tabulari archiviati nel servizio stesso, ma non i dati multidimensionali. Per analizzare i dati multidimensionali con Analysis Services, è possibile usare DirectQuery nel database SQL.

Analisi di flusso di Azure è un'ottima soluzione per analizzare i dati e trasformarli in informazioni dettagliate di utilità pratica, ma è un servizio incentrato sui dati di streaming in tempo reale, mentre in questo caso i business analyst esaminano solo dati cronologici.

## <a name="summary"></a>Riepilogo

Ogni categoria di dati ha requisiti di archiviazione diversi ed è necessario capire qual è la soluzione più appropriata. È sempre necessario considerare la categoria dei dati, le operazioni necessarie, la latenza e la necessità del supporto transazionale.