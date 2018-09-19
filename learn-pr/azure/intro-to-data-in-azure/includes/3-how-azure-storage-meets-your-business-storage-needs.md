Analizzando i vantaggi dell'archiviazione dei dati di Azure, emerge che offre le migliori opzioni per archiviare il portale di formazione. Si analizzeranno ora in dettaglio i vantaggi e le opzioni disponibili per vedere come possono adattarsi alle esigenze aziendali.

## <a name="how-azure-data-storage-can-meet-your-business-storage-needs"></a>Come l'archiviazione dei dati di Azure può soddisfare le esigenze di archiviazione aziendali

Azure offre diverse opzioni adatte a soddisfare specifiche esigenze di archiviazione dei dati. Esaminiamone brevemente alcune.

:::row:::
  :::column:::
    ![Database SQL di Azure](../media/3-azure-sql-db.png)
  :::column-end:::
    :::column span="3"::: **Database SQL di Azure**

Il **database SQL di Azure** è un database cloud relazionale affidabile e completamente gestito. È possibile usare questa funzionalità per archiviare i dati che si aggiornano e a cui si accede di frequente, ad esempio i dati personali e le informazioni relative alla formazione del personale. È anche possibile eseguire la migrazione dei database SQL Server esistenti senza apportare modifiche alle applicazioni. La figura seguente illustra i tipi di dati dello scenario del portale di formazione online che verrebbero archiviati in un database SQL di Azure.

![Figura che illustra il database SQL di Azure usato per archiviare le informazioni degli studenti, ad esempio trascrizioni, certificazioni e materiale didattico.](../media/3-Azure_SQL.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Cosmos DB](../media/3-cosmos-db.png)
  :::column-end:::
    :::column span="3"::: **Azure Cosmos DB**

Azure Cosmos DB è un servizio di database distribuito a livello globale. Supporta dati senza schema e consente di creare applicazioni **Always On** altamente reattive con le quali poter gestire i dati in continuo cambiamento. È possibile usare questa funzionalità per archiviare i dati che vengono aggiornati e gestiti da utenti in tutto il mondo. La figura seguente illustra un esempio di database di Azure Cosmos DB usato per archiviare i dati a cui accedono gli utenti di tutto il mondo.

![Figura che illustra l'uso di Azure Cosmos DB nello scenario di formazione online per archiviare il catalogo dei corsi. Azure Cosmos DB è un'ottima scelta in questo caso perché il catalogo viene aggiornato dagli amministratori e usato dagli studenti di tutto il mondo.](../media/3-Azure_cosmos_db.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Archiviazione BLOB di Azure](../media/3-azure-blob-storage.png)
  :::column-end:::
    :::column span="3"::: **Archiviazione BLOB di Azure**

Archiviazione BLOB di Azure consente di trasmettere file video o audio di grandi dimensioni direttamente nel browser dell'utente da qualsiasi parte del mondo. Archiviazione BLOB consente anche di memorizzare i dati a scopo di backup e ripristino, ripristino di emergenza e archiviazione. Offre la possibilità di archiviare fino a 8 TB di dati per le macchine virtuali. La figura seguente illustra un esempio d'uso di Archiviazione BLOB di Azure.

![Figura che illustra Archiviazione BLOB di Azure usato per archiviare e trasmettere file video o audio.](../media/3-Azure_blob.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Data Lake Storage Gen2](../media/3-azure-data-lake.png)
  :::column-end:::
    :::column span="3"::: **Azure Data Lake Storage Gen2**

La funzionalità Data Lake consente di eseguire analisi relative all'uso dei dati e preparare i report. Data Lake è un repository di grandi dimensioni che archivia dati strutturati e non strutturati.

**Azure Data Lake Storage Gen2** combina i vantaggi in termini di scalabilità e costi offerti dall'archiviazione di oggetti con l'affidabilità e le prestazioni delle funzionalità di file system per Big Data. La figura seguente illustra come Azure Data Lake consenta di archiviare tutti i dati aziendali rendendoli disponibili per l'analisi.

![Figura che illustra il ruolo di Azure Data Lake nella preparazione e nell'archiviazione dei dati per l'uso da parte di strumenti di analisi. Azure Data Lake può gestire un'ampia gamma di tipi di input, ad esempio dati relazionali, dati video o dati dei sensori.](../media/3-Data_lake_store_concept.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![File di Azure](../media/3-azure-files.png)
  :::column-end:::
    :::column span="3"::: **File di Azure**

Il servizio File di Azure offre condivisioni file completamente gestite nel cloud. Le applicazioni in esecuzione in Azure possono condividere facilmente i file tra le macchine virtuali. È possibile usare le condivisioni file di Azure simultaneamente per le distribuzioni cloud o locali di Windows, Linux e macOS. La figura seguente illustra l'uso di File di Azure per la condivisione di dati tra due aree geografiche. File di Azure usa il protocollo SMB (Server Message Block) che assicura la crittografia dei dati inattivi e in transito.

![Figura che illustra le funzionalità di condivisione file del servizio File di Azure. ](../media/3-Azure_Files.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Code di Azure](../media/3-azure-queue.png)
  :::column-end:::
    :::column span="3"::: **Code di Azure**

Il servizio Archiviazione code di Azure consente di archiviare grandi quantità di messaggi a cui è possibile accedere da qualsiasi parte del mondo. Per avere un'idea chiara, si consideri che la dimensione massima di un messaggio nella coda è di 64 KB e una coda può contenere milioni di messaggi.

Generalmente sono presenti uno o più componenti mittente e uno o più componenti destinatario. I componenti mittente aggiungono messaggi alla coda, mentre i componenti destinatario recuperano i messaggi dall'inizio della coda per l'elaborazione. La figura seguente illustra più applicazioni mittente che aggiungono messaggi alla coda di Azure e un'applicazione destinataria che recupera i messaggi.

![Figura di un'architettura completa di Archiviazione code di Azure](../media/3-Azure_Queue.png)

È possibile usare l'archiviazione code per eseguire le operazioni seguenti:

- Creare un backlog di lavoro e passare messaggi tra i diversi server Web di Azure.
- Gestire il carico tra vari server Web o infrastrutture e gestire i picchi di traffico.
- Creare resilienza in caso di errore dei componenti quando più utenti accedono contemporaneamente ai dati.

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Archiviazione Standard di Azure](../media/3-azure-standard-storage.png)
  :::column-end:::
    :::column span="3"::: **Archiviazione Standard di Azure**

Le macchine virtuali in Azure archiviano sistemi operativi, applicazioni e dati usando dischi. Archiviazione Standard di Azure offre un supporto per dischi affidabile e a basso costo per le macchine virtuali in cui vengono eseguiti carichi di lavoro che non hanno importanza cruciale. Con Archiviazione Standard, i dati vengono archiviati in unità disco rigido (HDD).

Per le macchine virtuali è possibile usare dischi SSD e HDD Standard per carichi di lavoro meno critici e dischi SSD Premium per applicazioni di produzione cruciali. I dischi di Azure offrono costantemente una durabilità di livello aziendale, con una percentuale di frequenza di errori annualizzata pari a ZERO, ovvero la migliore del settore. La figura seguente illustra una macchina virtuale di Azure che usa dischi separati per archiviare dati diversi.

![Figura che illustra due dischi all'interno di una macchina virtuale: uno che contiene il sistema operativo e l'altro che contiene i dati.](../media/3-Azure_disks.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Livelli di archiviazione](../media/3-storage-tiers.png)
  :::column-end:::
    :::column span="3"::: **Livelli di archiviazione**

Per l'archiviazione di oggetti BLOB, in Azure sono disponibili tre livelli:

1. **Livello di archiviazione ad accesso frequente**: ottimizzato per l'archiviazione di dati a cui si accede di frequente.

1. **Livello di archiviazione ad accesso sporadico**: ottimizzato per l'archiviazione di dati a cui si accede poco frequentemente e che rimangono archiviati per almeno 30 giorni.

1. **Livello di archiviazione archivio**: ottimizzato per l'archiviazione dei dati a cui si accede raramente e che rimangono archiviati per almeno 180 giorni con requisiti di latenza flessibili.

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Crittografia e replica](../media/3-azure-storage-encryption.png)
  :::column-end:::
    :::column span="3"::: **Crittografia e replica**

Azure offre protezione e disponibilità elevata dei dati grazie a funzionalità di crittografia e replica.

#### <a name="encryption-for-storage-services"></a>Crittografia per i servizi di archiviazione

Per le risorse sono disponibili i tipi di crittografia seguenti:

1. **Crittografia del servizio di archiviazione di Azure** per dati inattivi consente di proteggere i dati in base ai criteri di sicurezza e ai requisiti di conformità dell'organizzazione. Questa funzionalità esegue la crittografia dei dati prima di archiviarli e ne esegue la decrittografia prima di recuperarli. La crittografia e la decrittografia sono trasparenti all'utente.

1. **Crittografia lato client**, in cui i dati sono già crittografati dalle librerie client. Azure archivia i dati in stato crittografato quando sono inattivi e ne esegue la decrittografia in fase di recupero.

#### <a name="replication-for-storage-availability"></a>Replica per la disponibilità delle risorse di archiviazione

Quando si crea un account di archiviazione viene impostato un tipo di replica. La funzionalità di replica garantisce che i dati siano durevoli e sempre disponibili. Azure offre repliche geografiche e a livello di area per proteggere i dati da calamità naturali e altre situazioni di emergenza locali, come incendi o inondazioni.

  :::column-end:::
:::row-end:::