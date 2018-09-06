La scelta della soluzione di archiviazione appropriata può aiutare a migliorare le prestazioni. In questa unità si applicheranno le nozioni apprese sui dati dello scenario di vendita online e si troverà il servizio di Azure migliore per ogni set di dati. 

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

Classificazione dei dati: semistrutturati

Operazioni:

* I clienti richiedono numerose operazioni di lettura, con la possibilità di eseguire query su numerosi campi nel database.
* L'azienda richiede un numero elevato di operazioni di scrittura per tenere traccia dell'inventario che cambia continuamente.

Latenza e velocità effettiva: velocità effettiva elevata e bassa latenza

Supporto delle transazioni: richiesto

### <a name="recommended-service-azure-cosmos-db"></a>Servizio consigliato: Azure Cosmos DB

Azure Cosmos DB supporta i dati semistrutturati, o dati NoSQL, per impostazione predefinita. Con Azure Cosmos DB è quindi disponibile il supporto dei nuovi campi, ad esempio il campo relativo alla funzionalità Bluetooth o qualsiasi nuovo campo necessario in futuro.

Per quanto riguarda le operazioni, Azure Cosmos DB supporta SQL per le query e tutte le proprietà vengono indicizzate per impostazione predefinita, quindi è supportata la creazione di query che permettono ai clienti di applicare filtri di quasi tutti i tipi in base alle esigenze.

Per quanto riguarda latenza e velocità effettiva, Azure Cosmos DB consente di configurare la velocità effettiva, che può essere aumentata per gestire l'aumento della domanda dei clienti nei periodi di picco degli acquisti o ridotta nei periodi meno intensi per risparmiare sui costi. E poiché Azure Cosmos DB indicizza tutte le proprietà per impostazione predefinita, è possibile consentire ai clienti di eseguire query su qualsiasi campo.

Azure Cosmos DB è anche conforme ad ACID, quindi le transazioni vengono completate in base a tali requisiti rigorosi.

Azure Cosmos DB consente inoltre di replicare i dati ovunque nel mondo con un semplice clic. Se quindi il sito di e-commerce ha utenti concentrati negli Stati Uniti, in Francia e in Inghilterra, è possibile replicare i dati in tali data center per ridurre la latenza grazie a una maggiore vicinanza fisica dei dati agli utenti. E, anche con i dati replicati in tutto il mondo, è possibile scegliere uno dei cinque livelli di coerenza in modo da determinare il compromesso ideale tra coerenza, disponibilità, latenza e velocità effettiva.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Anche altri servizi di Azure, come Archiviazione tabelle di Azure, Azure HBase in HDInsight e Cache Redis di Azure, consentono di archiviare dati NoSQL. In questo scenario, poiché gli utenti devono poter eseguire una query su più campi, Azure Cosmos DB è una soluzione migliore rispetto ad Archiviazione tabelle di Azure, Cache Redis di Azure e Azure HBase in HDInsight perché Azure Cosmos DB indicizza tutti i campi per impostazione predefinita, mentre i dati indicizzati dagli altri servizi sono limitati e ciò limita la possibilità di eseguire query su qualsiasi campo del database.

## <a name="photos-and-videos"></a>Foto e video

Classificazione dei dati: non strutturati

Operazioni:

* Foto e video devono essere recuperati solo in base all'ID.
* Le operazioni di creazione e aggiornamento saranno abbastanza rare e possono avere una latenza maggiore rispetto alle operazioni di lettura.

Latenza e velocità effettiva: le operazioni di recupero in base all'ID devono supportare bassa latenza e velocità effettiva elevata. Le operazioni di creazione e aggiornamento possono avere una latenza maggiore rispetto alle operazioni di lettura.

Supporto delle transazioni: non richiesto

## <a name="recommended-service-azure-blob-storage"></a>Servizio consigliato: Archiviazione BLOB di Azure

Archiviazione BLOB di Azure supporta l'archiviazione di file come foto e video. Si integra anche con Rete di distribuzione dei contenuti di Azure (rete CDN) per la memorizzazione nella cache dei contenuti più usati e la loro archiviazione nei server perimetrali, riducendo la latenza di recupero delle immagini per gli utenti.

Con Archiviazione BLOB di Azure, è anche possibile spostare le immagini dal livello di archiviazione ad accesso frequente al livello di archiviazione archivio o ad accesso sporadico per ridurre i costi e ottimizzare la velocità effettiva per le immagini e i video più visualizzati.

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Si potrebbe caricare le immagini in Servizi app di Azure, così che lo stesso server che esegue l'app fornisca anche le immagini. Ciò potrebbe funzionare se le immagini non fossero molte, ma in presenza di numerosi file per un pubblico globale si otterranno risultati migliori usando Archiviazione BLOB di Azure con Rete di distribuzione dei contenuti di Azure.

## <a name="business-data"></a>Dati di business

Classificazione dei dati: strutturati

Operazioni: query analitiche complesse di sola lettura su più database

Latenza e velocità effettiva: è prevista una certa latenza nei risultati a causa della natura complessa delle query.

Supporto delle transazioni: richiesto

### <a name="recommended-service-azure-sql-database"></a>Servizio consigliato: database SQL di Azure

Le query sui dati di business vengono in genere eseguite dai business analyst, che conoscono più probabilmente SQL rispetto a qualsiasi altro linguaggio di query. Il database SQL di Azure può essere usato come soluzione autonoma ma, se usato in combinazione con Azure Analysis Services consente agli analisti di creare un modello semantico basato sui dati nel database SQL di Azure e quindi di condividerlo con gli utenti aziendali, che potranno semplicemente connettersi al modello da qualsiasi strumento di business intelligence per iniziare immediatamente a esplorare i dati e ottenere informazioni dettagliate. 

### <a name="why-not-other-azure-services"></a>Perché non scegliere altri servizi di Azure?

Azure SQL Data Warehouse supporta le soluzioni OLAP e le query SQL, ma i business analyst devono eseguire query su più database e questa funzionalità non è supportata da Azure SQL Data Warehouse.

È possibile usare Azure Analysis Services in aggiunta al database SQL di Azure, tuttavia i business analyst sono più esperti in SQL rispetto all'uso di Power BI, quindi preferiscono un database che supporti le query SQL, non supportate da Azure Analysis Services. I dati finanziari archiviati nel set di dati di business sono inoltre di natura relazionale e multidimensionale e Azure Analysis Services supporta i dati tabulari archiviati nel servizio, ma non i dati multidimensionali. Per analizzare i dati multidimensionali con Azure Analysis Services, è possibile usare DirectQuery nel database SQL di Azure.

Analisi di flusso di Azure è un'ottima soluzione per analizzare i dati e trasformarli in informazioni dettagliate di utilità pratica, ma è un servizio incentrato sui dati di streaming in tempo reale, mentre in questo caso i business analyst esaminano solo dati cronologici.

## <a name="summary"></a>Riepilogo

Ogni categoria di dati ha requisiti di archiviazione diversi ed è necessario capire qual è la soluzione più appropriata. È sempre necessario considerare la categoria dei dati, le operazioni richieste, la latenza e la necessità del supporto delle transazioni.
