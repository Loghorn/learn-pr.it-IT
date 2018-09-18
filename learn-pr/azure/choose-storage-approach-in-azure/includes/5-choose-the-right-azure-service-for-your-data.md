La scelta della soluzione di archiviazione appropriata può aiutare a migliorare le prestazioni, a diminuire i costi e a migliorare la gestibilità. In questa unità si applicheranno le nozioni apprese sui dati dello scenario di vendita online e si individuerà il servizio di Azure migliore per ogni set di dati. 

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

**Classificazione dei dati:** semistrutturati a causa della necessità di estendere o modificare lo schema per i nuovi prodotti

**Operazioni:**

- I clienti richiedono un numero elevato di operazioni di lettura, con la possibilità di eseguire query su numerosi campi nel database.
- L'azienda richiede un numero elevato di operazioni di scrittura per tenere traccia dell'inventario che cambia continuamente.

**Latenza e velocità effettiva:** velocità effettiva elevata e bassa latenza

**Supporto delle transazioni:** necessario

### <a name="recommended-service-azure-cosmos-db"></a>Servizio consigliato: Azure Cosmos DB

Azure Cosmos DB supporta i dati semistrutturati, o dati NoSQL, per impostazione predefinita. Con Azure Cosmos DB è quindi disponibile il supporto dei nuovi campi, ad esempio il campo relativo alla funzionalità Bluetooth o qualsiasi nuovo campo necessario in futuro.

Azure Cosmos DB supporta SQL per le query e tutte le proprietà sono indicizzate per impostazione predefinita. È possibile creare query in modo che i clienti possano filtrare sulla base di qualsiasi proprietà nel catalogo.

Azure Cosmos DB è anche conforme ad ACID, quindi le transazioni vengono completate in base a tali requisiti rigorosi.

Azure Cosmos DB consente inoltre di replicare i dati ovunque nel mondo con un semplice clic. Quindi, se gli utenti del sito di e-commerce sono concentrati negli Stati Uniti, in Francia e in Inghilterra, è possibile replicare i dati in tali data center per ridurre la latenza grazie a una maggiore vicinanza fisica dei dati agli utenti. 

Anche con i dati replicati in tutto il mondo, è possibile scegliere uno di cinque livelli di coerenza. Scegliendo il livello di coerenza adatto si possono determinare i compromessi da accettare tra coerenza, disponibilità, latenza e velocità effettiva. È possibile aumentare le prestazioni per gestire l'aumento della domanda dei clienti durante i periodi di picco degli acquisti e ridurle durante i periodi di calma per limitare i costi.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Azure SQL Database sarebbe una scelta eccellente per questo set di dati, non fosse per la necessità di estendere lo schema ad hoc per i nuovi prodotti. Nel Database SQL di Azure tutti i dati devono essere conformi a uno schema. Database SQL di Azure offre gran parte dei vantaggi di Azure Cosmos DB, ma non può gestire i dati eterogenei. 

Anche altri servizi di Azure, come Archiviazione tabelle di Azure, Azure HBase in HDInsight e Cache Redis di Azure, consentono di archiviare dati NoSQL. In questo scenario, dal momento che gli utenti vogliono eseguire query su più campi, Azure Cosmos DB è la scelta più adatta. Azure Cosmos DB indicizza tutti i campi per impostazione predefinita, mentre gli altri servizi possono indicizzare una quantità limitata di dati e l'esecuzione di query su risultati di campi non indicizzati provoca una riduzione delle prestazioni.

## <a name="photos-and-videos"></a>Foto e video

**Classificazione dei dati:** non strutturati

**Operazioni:**

- Devono essere recuperati solo in base all'ID.
- I clienti richiedono un numero elevato di operazioni di lettura con latenza bassa.
- Le operazioni di creazione e aggiornamento saranno abbastanza rare e possono avere una latenza più elevata rispetto alle operazioni di lettura.

**Latenza e velocità effettiva:** le operazioni di recupero in base all'ID devono supportare bassa latenza e velocità effettiva elevata. Le operazioni di creazione e aggiornamento possono avere una latenza più elevata rispetto alle operazioni di lettura.

**Supporto delle transazioni:** non necessario

### <a name="recommended-service-azure-blob-storage"></a>Servizio consigliato: Archiviazione BLOB di Azure

Archiviazione BLOB di Azure supporta l'archiviazione di file come foto e video. Si integra anche con la Rete per la distribuzione di contenuti di Azure (rete CDN) per la memorizzazione nella cache dei contenuti usati più frequentemente e la loro archiviazione nei server perimetrali. Con Azure CDN si riduce la latenza nel recupero delle immagini per gli utenti.

Con Archiviazione BLOB di Azure, è anche possibile spostare le immagini dal livello di archiviazione ad accesso frequente al livello di archiviazione archivio o ad accesso sporadico per ridurre i costi e ottimizzare la velocità effettiva per le immagini e i video visualizzati più frequentemente.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Si potrebbero caricare le immagini nel Servizio app di Azure, così che lo stesso server che esegue l'app fornisca anche le immagini, ma questa soluzione funzionerebbe solo se il numero di file è ridotto. Ma in presenza di numerosi file per un pubblico globale si otterranno risultati migliori usando Archiviazione BLOB di Azure con la Rete CDN di Azure.

## <a name="business-data"></a>Dati aziendali

**Classificazione dei dati:** strutturati

**Operazioni:** query analitiche complesse di sola lettura su più database

**Latenza e velocità effettiva:** è prevista una certa latenza nei risultati a causa della natura complessa delle query.

**Supporto delle transazioni:** necessario

### <a name="recommended-service-azure-sql-database"></a>Servizio consigliato: database SQL di Azure

Le query sui dati aziendali vengono in genere eseguite dai business analyst, che conoscono più probabilmente SQL rispetto a qualsiasi altro linguaggio di query. Il database SQL di Azure può essere considerato la soluzione ideale anche da solo, ma se associato ad Azure Analysis Services consente ai data analyst di creare un modello semantico sui dati nel database SQL. Gli analisti dei dati potranno quindi condividerlo con gli utenti aziendali, connettendosi semplicemente al modello da qualsiasi strumento di business intelligence, per esplorare immediatamente i dati e ottenere informazioni dettagliate. 

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Azure SQL Data Warehouse supporta le soluzioni OLAP e le query SQL. Ma i business analyst dovranno eseguire query su più database e queste non sono supportate da SQL Data Warehouse.

Azure Analysis Services potrebbe essere usato in aggiunta al database SQL di Azure. Ma i business analyst sono più abituati a usare SQL che non Power BI e quindi preferiscono usare un database che supporti le query SQL, che invece non sono supportate da Azure Analysis Services. Inoltre, i dati finanziari archiviati nel set di dati aziendali presentano una natura relazionale e multidimensionale. Azure Analysis Services supporta i dati tabulari archiviati nel servizio stesso, ma non i dati multidimensionali. Per analizzare i dati multidimensionali con Azure Analysis Services, è possibile usare DirectQuery nel database SQL.

Analisi di flusso di Azure è un'ottima soluzione per analizzare i dati e trasformarli in informazioni dettagliate di utilità pratica, ma è un servizio incentrato sui dati di streaming in tempo reale, mentre in questo scenario i business analyst esaminano solo dati cronologici.

## <a name="summary"></a>Riepilogo

Ogni tipo di dati ha requisiti di archiviazione diversi ed è necessario capire qual è la soluzione più appropriata. Considerare sempre il tipo di dati, le operazioni necessarie, la latenza prevista e la necessità del supporto transazionale.