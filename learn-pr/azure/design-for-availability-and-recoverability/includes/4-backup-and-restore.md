Il backup è la linea di difesa finale e più potente contro una perdita di dati permanente. Un'efficace strategia di backup richiede più della semplice creazione di copie dei dati: è necessario prendere in considerazione l'architettura dei dati e l'infrastruttura dell'applicazione. Un'app può gestire numerosi tipi di dati di importanza diversa, ampiamente distribuiti tra file system, database e altri servizi di archiviazione, sia nel cloud che in locale. Con i servizi e i prodotti appropriati, è possibile semplificare il processo di backup e migliorare il tempo di ripristino delle copie di backup.

## <a name="establish-backup-and-restoration-requirements"></a>Stabilire i requisiti di backup e ripristino

Come per una strategia di ripristino di emergenza, i requisiti di backup si basano su un'analisi costi/benefici. L'analisi dei dati dell'app deve essere orientata dall'importanza relativa delle diverse categorie di dati gestiti dall'app, nonché da requisiti esterni, come la legislazione relativa alla conservazione dei dati.

Per stabilire i requisiti di backup per l'app, prendere nota dei dati dell'applicazione ed eseguire un'analisi per raggrupparli in base ai seguenti requisiti:

* Quantità di questo tipo di dati di cui è possibile tollerare una perdita, misurata per durata
* Tempo massimo consentito per un ripristino di questo tipo di dati
* Requisiti di conservazione dei backup, periodo di tempo e frequenza con cui devono rimanere disponibili i backup

Questi concetti sono perfettamente in linea con quelli di obiettivo del punto di ripristino e obiettivo del tempo di ripristino (RPO e RTO). La durata della perdita accettabile in generale si traduce direttamente negli intervalli di backup e negli RPO richiesti. Il tempo massimo richiesto per un ripristino corrisponde all'obiettivo RTO per il componente dati dell'applicazione. Entrambi i requisiti devono essere analizzati rispetto al costo necessario per soddisfarli: tutte le organizzazioni in genere ritengono di non potersi permettere *alcuna* perdita di dati, ma spesso non è così quando si considera il costo necessario per soddisfare tale requisito.

Il backup ha un ruolo assolutamente centrale per il ripristino di emergenza, ma i backup, i ripristini e gli scenari associati vanno al di là dell'ambito del ripristino di emergenza. Potrebbe essere necessario eseguire il ripristino dei backup in situazioni non di emergenza, in cui gli obiettivi RTO e RPO non sono particolarmente importanti. Ad esempio, se una quantità limitata di dati meno recenti rispetto all'intervallo di backup è danneggiata o viene eliminata, ma l'applicazione non subisce tempi di inattività, non vi sono rischi di violazioni del contratto di servizio e l'esecuzione di un ripristino consentirà di evitare qualsiasi perdita di dati. Il piano di ripristino di emergenza può includere o meno indicazioni sull'esecuzione di ripristini in situazioni non di emergenza.

> [!TIP]
> È importante non confondere *archiviazione*, *replica* e *backup*. L'archiviazione è la memorizzazione dei dati per l'accesso in lettura e la conservazione a lungo termine, mentre la replica è la copia quasi in tempo reale dei dati tra le repliche per supportare la disponibilità elevata e alcuni scenari di ripristino di emergenza. Alcuni requisiti, ad esempio la legislazione relativa alla conservazione dei dati, possono influenzare le strategie per tutti e tre questi elementi, ma l'archiviazione, la replica e il backup richiedono analisi e implementazioni distinte.

## <a name="azure-backup-and-restore-capabilities"></a>Funzionalità di backup e ripristino di Azure

Azure offre diversi servizi correlati al backup e funzionalità per vari scenari, tra cui i dati in Azure e i dati locali. La maggior parte dei servizi di Azure offre qualche tipo di funzionalità di backup. Di seguito sono descritte alcune delle principali offerte di Azure correlate al backup.

### <a name="azure-backup"></a>Backup di Azure

Backup di Azure è una famiglia di prodotti che consente di eseguire il backup dei dati negli insiemi di credenziali di Servizi di ripristino di Azure per l'archiviazione e il ripristino. Gli insiemi di credenziali di Servizi di ripristino sono risorse di archiviazione dedicate di Azure, che contengono backup dei dati e della configurazione per macchine virtuali, server, singole workstation e carichi di lavoro.

> [!NOTE]
> Sia Backup di Azure che Azure Site Recovery usano gli insiemi di credenziali di Servizi di ripristino di Azure per l'archiviazione. Backup di Azure è una soluzione di backup per utilizzo generico, mentre Azure Site Recovery può coordinare la replica e il failover e supportare operazioni di ripristino di emergenza con obiettivi RPO e RTO limitati.

Backup di Azure rappresenta una soluzione di backup per utilizzo generico per flussi di lavoro cloud e locali in esecuzione su macchine virtuali o server fisici. È progettato come sostituto delle soluzioni di backup tradizionali e archivia i dati in Azure anziché su nastri o altri supporti fisici locali.

Quattro diversi prodotti e servizi possono usare Backup di Azure per creare i backup:

* **Agente di Backup di Azure** è una piccola applicazione Windows che esegue il backup di file, cartelle e dello stato del sistema dalla macchina virtuale o dal server Windows in cui è installata. Funziona in modo simile a molte soluzioni di backup basate sul cloud di tipo consumer, ma richiede la configurazione di un insieme di credenziali di Servizi di ripristino di Azure. Dopo aver scaricato e installato l'applicazione in un server Windows o una macchina virtuale, è possibile configurarla modo da creare backup fino a tre volte al giorno.
* **System Center Data Protection Manager** è un sistema di backup e ripristino affidabile, completo e di livello aziendale. DPM è un'applicazione Windows Server in grado di eseguire il backup di file system e macchine virtuali (Windows e Linux), creare backup bare metal dei server fisici ed eseguire il backup basato sulle applicazioni di diversi prodotti server Microsoft, come SQL Server ed Exchange. DPM fa parte della famiglia di prodotti System Center ed è concesso in licenza e venduto con System Center, ma viene incluso nella famiglia di prodotti Backup di Azure perché consente di archiviare i backup in un insieme di credenziali di Servizi di ripristino di Azure.
* **Server di Backup di Azure** è simile a System Center DPM, ma è concesso in licenza come parte di una sottoscrizione di Azure e non richiede una licenza di System Center. Server di Backup di Azure supporta le stesse funzionalità di DPM, ad eccezione dei backup su nastro locali e dell'integrazione con altri prodotti System Center.
* Il **backup delle macchine virtuali IaaS di Azure** è una funzionalità chiavi in mano di backup e ripristino per le macchine virtuali di Azure. Il backup delle macchine virtuali supporta backup giornalieri delle macchine virtuali Windows e Linux. Supporta il ripristino di singoli file, interi dischi e intere macchine virtuali e può anche eseguire backup coerenti con l'applicazione. Le singole applicazioni possono riconoscere le operazioni di backup e impostare le risorse del file system in uno stato coerente prima che venga creato lo snapshot.

![Backup di Azure](../media-draft/azure-backup.png)

Backup di Azure può aggiungere valore e contribuire alla strategia di backup e ripristino per le applicazioni IaaS e locali praticamente di qualsiasi tipo e dimensione.

### <a name="azure-blob-storage"></a>Archiviazione BLOB di Azure

Archiviazione di Azure non include una funzionalità di backup automatizzata, ma i BLOB vengono comunemente usati per eseguire il backup di dati di ogni tipo da diverse origini. Molti servizi che forniscono funzionalità di backup usano i BLOB per archiviare i dati e i BLOB sono una destinazione comune per strumenti e script in qualsiasi tipo di scenario di backup.

Gli account di archiviazione per utilizzo generico v2 supportano tre livelli di archiviazione BLOB con prestazioni e costi differenti. L'archiviazione **ad accesso sporadico** offre il migliore rapporto costo/prestazioni per la maggior parte dei backup, a differenza dell'archiviazione **ad accesso frequente**, che comporta costi di accesso inferiori, ma costi di archiviazione superiori. L'archiviazione a livello di **archivio** può essere appropriata per i backup secondari o i backup di dati con basse aspettative per il tempo di ripristino, perché ha un costo ridotto, ma richiede fino a 15 ore di lead time per l'accesso.

L'archivio BLOB non modificabile è configurabile come non cancellabile e non modificabile per un intervallo specificato dall'utente. L'archivio BLOB non modificabile è stato progettato principalmente per soddisfare rigorosi requisiti per determinati tipi di dati, ad esempio i dati finanziari, ma rappresenta un'ottima opzione per garantire che i backup siano protetti da eliminazioni o modifiche accidentali.

### <a name="azure-sql-database"></a>Database SQL di Azure

Il database SQL di Azure include funzionalità di backup complete e automatiche senza costi aggiuntivi. Vengono creati backup completi settimanali e backup differenziali ogni 12 ore, mentre i backup dei log vengono eseguiti ogni cinque minuti. I backup creati dal servizio sono utilizzabili per ripristinare un database a un momento specifico, anche se è stato eliminato. I ripristini possono essere eseguiti usando il portale di Azure, PowerShell o l'API REST. Anche i backup per i database crittografati con Transparent Data Encryption (funzionalità abilitata per impostazione predefinita) sono crittografati.

Il backup del database SQL è di livello aziendale, pronto per la produzione e abilitato per impostazione predefinita. Se si stanno valutando diverse opzioni di database per un'app, deve essere incluso nell'analisi costi/benefici, perché rappresenta un vantaggio significativo del servizio. Ogni app che usa il database SQL di Azure dovrebbe sfruttarne i vantaggi, includendolo nel piano di ripristino di emergenza e nelle procedure di backup e ripristino.

### <a name="azure-app-service"></a>Servizio app di Azure

Le applicazioni Web ospitate nei livelli Standard e Premium del servizio app di Azure offrono un supporto chiavi in mano per i backup pianificati e manuali. I backup includono la configurazione e il contenuto dei file, nonché il contenuto dei database usati dall'app. Supportano anche semplici filtri per l'esclusione dei file. Le operazioni di ripristino possono essere destinate a diverse istanze del servizio app, rendendo il backup del servizio app un modo semplice per spostare il contenuto di un'app in un'altra.

I backup del servizio app sono limitati a 10 GB in totale, incluso il contenuto dell'app e del database. Si tratta di un'ottima soluzione per le nuove app in fase di sviluppo e le app su scala ridotta. Per le applicazioni più avanzate in genere non viene usato il backup del servizio app, ma è preferibile adottare affidabili procedure di distribuzione e rollback, strategie di archiviazione che non usano l'archiviazione sul disco dell'applicazione e strategie di backup dedicate per i database e l'archiviazione permanente.

## <a name="verify-backups-and-test-restore-procedures"></a>Verificare i backup e testare le procedure di ripristino

Nessun sistema di backup è completo senza una strategia per la verifica dei backup e il test delle procedure di ripristino. Anche se si usa un servizio o un prodotto di backup dedicato, è comunque necessario documentare e sperimentare le procedure di ripristino per assicurarsi che siano ben chiare e consentano di riportare il sistema allo stato previsto.

Le strategie per la verifica dei backup possono variare e dipendono dalla natura dell'infrastruttura. È possibile prendere in considerazione tecniche come la creazione di una nuova distribuzione dell'applicazione, il ripristino del backup in tale distribuzione e il confronto dello stato delle due istanze. In molti casi, questo consente di simulare con precisione le effettive procedure di ripristino di emergenza. È sufficiente eseguire un confronto di un subset dei dati di backup con i dati in tempo reale subito dopo la creazione di un backup. Un'attività comune durante la verifica dei backup è effettuare un tentativo di ripristinare i backup meno recenti per assicurarsi che siano ancora disponibili e operativi e che il sistema di backup non sia stato modificato in modo da renderli incompatibili.

Qualsiasi strategia è preferibile rispetto al fatto di scoprire che i backup sono danneggiati o incompleti durante un tentativo di ripristino di emergenza.

Una strategia di backup e ripristino è un elemento importante per garantire la possibilità di ripristinare l'architettura in seguito alla perdita o al danneggiamento dei dati. Esaminare l'architettura per definire i requisiti di backup e ripristino. Azure offre diversi servizi e funzionalità in grado di fornire capacità di backup e ripristino per qualsiasi architettura.
