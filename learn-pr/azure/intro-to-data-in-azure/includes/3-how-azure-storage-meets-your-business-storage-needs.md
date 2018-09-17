Da un primo esame dei vantaggi offerti si comprende che Archiviazione di Azure è in grado di offrire opzioni ottimali per l'archiviazione del portale di formazione. Si analizzeranno ora in dettaglio i vantaggi e le opzioni disponibili in Archiviazione di Azure per vedere come possono adattarsi alle esigenze aziendali.

## <a name="how-azure-storage-can-meet-your-business-storage-needs"></a>In che modo Archiviazione di Azure può soddisfare le esigenze di archiviazione aziendali

Archiviazione di Azure offre diverse opzioni adatte a specifiche esigenze di archiviazione dei dati.

### <a name="azure-sql-database"></a>Database SQL di Azure

Il **database SQL di Azure** è un solido database cloud relazionale completamente gestito che archivia tutti i dati. È possibile usare questa funzionalità per archiviare i dati che si aggiornano e a cui si accede di frequente, ad esempio i dati personali e le informazioni relative alla formazione del personale. È anche possibile eseguire la migrazione dei database SQL Server esistenti senza apportare modifiche alle applicazioni. La figura seguente illustra i tipi di dati dello scenario del portale di formazione online che verrebbero archiviati in un database SQL di Azure.

![Figura che illustra il database SQL di Azure usato per archiviare le informazioni degli studenti, ad esempio trascrizioni, certificazioni e materiale didattico.](../media/3-Azure_SQL.png)

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB è un database distribuito a livello globale. Supporta dati senza schema offrendo la possibilità di creare applicazioni *Always On* altamente reattive in grado di gestire dati soggetti a costanti cambiamenti. È possibile usare questa funzionalità per archiviare i dati che vengono aggiornati e gestiti in base agli input di utenti in tutto il mondo. La figura seguente illustra un database di Azure Cosmos DB di esempio usato per archiviare i dati a cui accedono numerosi utenti dislocati in tutto il mondo.

![Figura che illustra l'uso di Azure Cosmos DB nello scenario di formazione online per archiviare il catalogo dei corsi. Azure Cosmos DB è un'ottima scelta in questo caso perché il catalogo viene aggiornato dagli amministratori e usato dagli studenti di tutto il mondo.](../media/3-Azure_cosmos_db.png)

### <a name="azure-blob-storage"></a>Archiviazione BLOB di Azure

Archiviazione BLOB di Azure offre la possibilità di trasmettere file video o audio di grandi dimensioni direttamente nel browser dell'utente da qualsiasi parte del mondo. Archiviazione BLOB consente anche di memorizzare i dati a scopo di backup e ripristino, ripristino di emergenza e archiviazione. Archiviazione BLOB di Azure può supportare fino a 8 TB di dati per l'archiviazione dei file per le macchine virtuali. La figura seguente illustra un esempio d'uso di Archiviazione BLOB di Azure.

![Figura che illustra Archiviazione BLOB di Azure usato per archiviare e trasmettere file video o audio.](../media/3-Azure_blob.png)

### <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

La funzionalità Data Lake di Archiviazione di Azure consente di eseguire analisi relative all'utilizzo dei dati e preparare report di conseguenza. Data Lake è un repository di grandi dimensioni che archivia dati strutturati e non.

**Azure Data Lake Storage Gen2** combina i vantaggi in termini di scalabilità e costi offerti dall'archiviazione di oggetti con l'affidabilità e le prestazioni delle funzionalità di file system per Big Data. La figura seguente illustra come Azure Data Lake consenta di archiviare tutti i dati aziendali rendendoli disponibili per l'analisi.

![Figura che illustra il ruolo di Azure Data Lake nella preparazione e nell'archiviazione dei dati per l'uso da parte di strumenti di analisi. Azure Data Lake è in grado di gestire un'ampia gamma di tipi di input, ad esempio dati relazionali, video o di sensori.](../media/3-Data_lake_store_concept.png)

### <a name="azure-files"></a>File di Azure

Il servizio File di Azure offre condivisioni file completamente gestite nel cloud. Le applicazioni in esecuzione in Azure possono condividere facilmente i file tra le macchine virtuali. È possibile usare le condivisioni file di Azure simultaneamente per le distribuzioni cloud o locali di Windows, Linux e macOS. La figura seguente illustra l'uso di File di Azure per la condivisione di dati tra due aree geografiche. File di Azure usa il protocollo SMB (Server Message Block) che assicura la crittografia dei dati inattivi e in transito.

![Figura che illustra le funzionalità di condivisione file del servizio File di Azure. ](../media/3-Azure_Files.png)

### <a name="azure-queue"></a>Archiviazione code di Azure

Archiviazione code di Azure è un servizio che consente di archiviare grandi quantità di messaggi a cui è possibile accedere da qualsiasi parte del mondo. La dimensione massima di un messaggio nella coda è di 64 KB e una coda può contenere milioni di messaggi.

Generalmente sono presenti uno o più componenti mittente e uno o più componenti destinatario. I componenti mittente aggiungono messaggi alla coda. I componenti destinatario recuperano i messaggi dall'inizio della coda per l'elaborazione. L'illustrazione seguente mostra più applicazioni mittente che aggiungono messaggi alla coda di Azure e un'applicazione destinataria che li recupera.

![Illustrazione di un'architettura generale di Archiviazione code di Azure](../media/3-Azure_Queue.png)

Archiviazione code viene usato principalmente per le operazioni seguenti:

- Creare un backlog di lavoro e passare messaggi tra i diversi server Web di Azure.
- Gestire il bilanciamento del carico tra i vari server Web o infrastrutture e gestire i picchi di traffico.
- Creare resilienza in caso di errore dei componenti quando più utenti accedono simultaneamente ai dati.

### <a name="azure-standard-storage"></a>Archiviazione Standard di Azure

Le macchine virtuali in Azure archiviano sistemi operativi, applicazioni e dati usando dischi. Archiviazione Standard di Azure offre un supporto per dischi affidabile e a basso costo per le macchine virtuali in cui vengono eseguiti carichi di lavoro che non hanno importanza cruciale. Con Archiviazione Standard, i dati vengono archiviati in unità disco rigido (HDD).

Per le macchine virtuali è possibile usare dischi SSD e HDD Standard per carichi di lavoro meno critici e dischi SSD Premium per applicazioni di produzione cruciali. I dischi di Azure offrono costantemente una durabilità di livello aziendale, con una percentuale di frequenza di errori annualizzata pari a ZERO, ovvero la migliore del settore. La figura seguente illustra una macchina virtuale di Azure che usa dischi separati per archiviare dati diversi.

![Figura che illustra due dischi all'interno di una macchina virtuale: uno che contiene il sistema operativo e l'altro che contiene i dati.](../media/3-Azure_disks.png)

### <a name="storage-tiers"></a>Livelli di archiviazione

Per gli oggetti BLOB in Azure sono disponibili tre livelli di archiviazione:

1. **Livello di archiviazione ad accesso frequente**: ottimizzato per l'archiviazione dei dati a cui si accede di frequente. 

1. **Livello di archiviazione ad accesso sporadico**: ottimizzato per l'archiviazione dei dati a cui si accede poco frequentemente e che rimangono archiviati per almeno 30 giorni.

1. **Livello di archiviazione archivio**: ottimizzato per l'archiviazione dei dati a cui si accede raramente e che rimangono archiviati per almeno 180 giorni con requisiti di latenza flessibili. Questo livello è ideale per archiviare le versioni meno recenti dei dati in modo da recuperarli quando necessario per il controllo o per altre attività occasionali.

La figura seguente illustra i livelli di Archiviazione BLOB di Azure.

![Figura che illustra i tre diversi livelli di archiviazione di Archiviazione BLOB di Azure: accesso frequente, accesso sporadico e archivio.](../media/3-Archive_Storage_Tier.png)

### <a name="azure-storage-encryptionreplication"></a>Crittografia/replica di Archiviazione di Azure

Archiviazione di Azure offre protezione e disponibilità elevata dei dati grazie a funzionalità di crittografia e replica.

#### <a name="encryption-for-storage-services"></a>Crittografia per i servizi di archiviazione

Per le risorse sono disponibili i tipi di crittografia seguenti:

1. **Crittografia del servizio di archiviazione di Azure** per dati inattivi consente di proteggere i dati in base ai criteri di sicurezza e ai requisiti di conformità dell'organizzazione. Questa funzionalità esegue la crittografia dei dati prima di archiviarli e ne esegue la decrittografia prima di recuperarli. La crittografia e la decrittografia sono trasparenti all'utente.
1. **Crittografia lato client**, in cui i dati sono già crittografati dalle librerie client. Azure archivia i dati in stato crittografato quando sono inattivi e quindi ne esegue la decrittografia in fase di recupero.

    Questa funzionalità di crittografia garantisce la conformità dei dati agli standard di protezione globale. È adatta per archiviare le informazioni sensibili, ad esempio i dati personali e finanziari.

#### <a name="replication-for-storage-availability"></a>Replica per la disponibilità delle risorse di archiviazione

Quando si crea un account di archiviazione viene impostato un tipo di replica. La funzionalità di replica garantisce che i dati siano durevoli e sempre disponibili. Archiviazione di Azure abilita le repliche geografiche e a livello di area per proteggere i dati da calamità naturali e altre situazioni di emergenza locali, come incendi o inondazioni.
