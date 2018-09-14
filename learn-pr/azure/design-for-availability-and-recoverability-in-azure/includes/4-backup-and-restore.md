Il backup è la linea di difesa finale e più potente contro una perdita di dati permanente. Un'efficace strategia di backup richiede più semplicemente creare copie dei dati. È necessario prendere in considerazione architettura di data e l'infrastruttura dell'applicazione. L'app consente di gestire numerosi tipi di dati di importanza diversa, ampiamente diffusi tra i file System, database e altri servizi di archiviazione sia nel cloud e locali. Con i servizi e i prodotti appropriati, è possibile semplificare il processo di backup e migliorare il tempo di ripristino delle copie di backup.

## <a name="establish-backup-and-restoration-requirements"></a>Stabilire i requisiti di backup e ripristino

Come per una strategia di ripristino di emergenza, i requisiti di backup si basano su un'analisi costi/benefici. Analisi dei dati dell'app deve tener l'importanza relativa delle diverse categorie di dati che gestisce l'app, nonché requisiti esterni, ad esempio Leggi di conservazione dei dati.

Per stabilire i requisiti di backup per l'app, prendere nota dei dati dell'applicazione ed eseguire un'analisi per raggrupparli in base ai seguenti requisiti:

* Quantità di questo tipo di dati di cui è possibile tollerare una perdita, misurata per durata
* Tempo massimo consentito per un ripristino di questo tipo di dati
* I requisiti di conservazione di backup: quanto tempo e con quale frequenza backup necessario rimangono disponibili

Questi concetti mappa accuratamente i concetti dell'obiettivo del punto di ripristino e obiettivo del tempo di ripristino (RPO e RTO). La durata della perdita accettabile in generale si traduce direttamente negli intervalli di backup e negli RPO richiesti. Il tempo massimo richiesto per un ripristino corrisponde all'obiettivo RTO per il componente dati dell'applicazione. Entrambi i requisiti devono essere sviluppati in termini di costi di raggiungerli. Ogni organizzazione vuole dire che realmente non può permettersi di perdere *qualsiasi* dati, ma spesso che non si verifica quando viene considerato il costo di raggiungimento di tale requisito.

Il backup ha un ruolo assolutamente centrale per il ripristino di emergenza, ma i backup, i ripristini e gli scenari associati vanno al di là dell'ambito del ripristino di emergenza. Backup potrebbero dover essere ripristinati in situazioni di emergenza non, inclusi quelli in cui non sono gli obiettivi RTO e RPO di grande interesse. Ad esempio, se una piccola quantità di dati meno recenti rispetto all'intervallo di backup è danneggiata o eliminata, ma l'applicazione non verificarsi tempi di inattività, l'applicazione non potrebbe essere mai il rischio che manca il contratto di servizio e un ripristino corretto comporterà alcuna perdita di dati. Il piano di ripristino di emergenza può includere o meno indicazioni sull'esecuzione di ripristini in situazioni non di emergenza.

> [!TIP]
> È importante non confondere *archiviazione*, *replica* e *backup*. Dell'archivio è l'archiviazione dei dati per l'accesso in lettura e conservazione a lungo termine. La replica è la copia quasi in tempo reale dei dati tra le repliche per supportare la disponibilità elevata e alcuni scenari di ripristino di emergenza. Alcuni requisiti, ad esempio Leggi di conservazione dei dati, possono influenzare le strategie per tutte e tre queste problematiche. Dell'archivio, replica e backup tutte richiedono analisi distinta e implementazione.

## <a name="azure-backup-and-restore-capabilities"></a>Funzionalità di backup e ripristino di Azure

Azure offre diversi servizi correlati al backup e funzionalità per vari scenari, tra cui i dati in Azure e i dati locali. La maggior parte dei servizi di Azure offre qualche tipo di funzionalità di backup. Di seguito sono descritte alcune delle principali offerte di Azure correlate al backup.

### <a name="azure-backup"></a>Backup di Azure

Backup di Azure è una famiglia di prodotti che consente di eseguire il backup dei dati negli insiemi di credenziali di Servizi di ripristino di Azure per l'archiviazione e il ripristino. Gli insiemi di credenziali di Servizi di ripristino sono risorse di archiviazione dedicate di Azure, che contengono backup dei dati e della configurazione per macchine virtuali, server, singole workstation e carichi di lavoro.

> [!NOTE]
> Sia Backup di Azure che Azure Site Recovery usano gli insiemi di credenziali di Servizi di ripristino di Azure per l'archiviazione. Backup di Azure è una soluzione di backup per utilizzo generico. Azure Site Recovery consente di coordinare la replica e failover e il supporto a operazioni di ripristino di emergenza RPO e RTO basso.

Backup di Azure rappresenta una soluzione di backup per utilizzo generico per flussi di lavoro cloud e locali in esecuzione su macchine virtuali o server fisici. È progettato come sostituto delle soluzioni di backup tradizionali e archivia i dati in Azure anziché su nastri o altri supporti fisici locali.

Quattro diversi prodotti e servizi possono usare Backup di Azure per creare i backup:

* **Azure Backup Agent** è una piccola applicazione Windows che esegue il backup di file, cartelle e dello stato del sistema dalla macchina virtuale Windows o server in cui è installato. Funziona in modo che è simile a molti consumer basati sul cloud le soluzioni di backup, ma richiede la configurazione di un insieme di credenziali di ripristino di Azure. Dopo aver scaricato e installato l'applicazione in un server Windows o una macchina virtuale, è possibile configurarla modo da creare backup fino a tre volte al giorno.
* **System Center Data Protection Manager** è un sistema di backup e ripristino affidabile, completo e di livello aziendale. Data Protection Manager è un'applicazione di Windows Server che può eseguire il backup di file System e le macchine virtuali (Windows e Linux), creare backup bare metal di server fisici, eseguire il backup basato sulle applicazioni di molti prodotti server Microsoft, ad esempio SQL Server ed Exchange. Data Protection Manager e fa parte della famiglia di prodotti System Center viene concesso in licenza e venduto con System Center, ma è considerata parte della famiglia di prodotti di Backup di Azure perché consente di archiviare i backup in un insieme di credenziali di ripristino di Azure.
* **Server di Backup di Azure** è simile a Data Protection Manager, ma è concesso in licenza come parte di una sottoscrizione di Azure e non richiede una licenza di System Center. Server di Backup di Azure supporta le stesse funzionalità, come Data Protection Manager, ad eccezione di backup su nastro locale e l'integrazione con altri prodotti System Center.
* Il **backup delle macchine virtuali IaaS di Azure** è una funzionalità chiavi in mano di backup e ripristino per le macchine virtuali di Azure. Backup della macchina virtuale supporta i backup di una volta al giorno per le macchine virtuali Windows e Linux. Supporta il ripristino di singoli file, disco è pieno e VM intera e può anche eseguire il backup coerenti con l'applicazione. Le singole applicazioni possono essere a conoscenza di operazioni di backup e ottenere le risorse del file System in uno stato coerente prima viene creato lo snapshot.

![Backup di Azure](../media/azure-backup.png)

Backup di Azure può aggiungere valore e contribuire alla strategia di backup e ripristino per le applicazioni IaaS e locali praticamente di qualsiasi tipo e dimensione.

### <a name="azure-blob-storage"></a>Archiviazione BLOB di Azure

Archiviazione di Azure non include una funzionalità di backup automatizzata, ma i BLOB vengono comunemente usati per eseguire il backup di tutti i tipi di dati da diverse origini. Molti servizi che forniscono funzionalità di backup usano i BLOB per archiviare i dati, una destinazione comune per strumenti e script in qualsiasi tipo di scenario di backup.

Gli account di archiviazione per utilizzo generico v2 supportano tre livelli di archiviazione BLOB con prestazioni e costi differenti. **Ad accesso sporadico** archiviazione offre il miglior rapporto costo-prestazioni per la maggior parte dei backup, anziché **hot** archiviazione, che offre costi di archiviazione superiori ma costi di accesso inferiore. **Archivio**-archiviazione a livelli può essere appropriata per i backup secondari o i backup dei dati con bassa aspettative per il tempo di recupero. È basso costo, ma richiede fino a 15 ore lead time per accedere.

L'archivio BLOB non modificabile è configurabile come non cancellabile e non modificabile per un intervallo specificato dall'utente. Archivio blob non modificabile è stato progettato principalmente per soddisfare requisiti rigorosi per determinati tipi di dati, ad esempio dati finanziari. È un'ottima opzione per garantire che i backup sono protetti da eliminazioni accidentali o la modifica.

### <a name="azure-sql-database"></a>Database SQL di Azure

Il database SQL di Azure include funzionalità di backup complete e automatiche senza costi aggiuntivi. Vengono creati backup completi settimanali e backup differenziali ogni 12 ore, mentre i backup dei log vengono eseguiti ogni cinque minuti. I backup creati dal servizio sono utilizzabili per ripristinare un database a un momento specifico, anche se è stato eliminato. I ripristini possono essere eseguiti usando il portale di Azure, PowerShell o l'API REST. Anche i backup per i database crittografati con Transparent Data Encryption (funzionalità abilitata per impostazione predefinita) sono crittografati.

Il backup del database SQL è di livello aziendale, pronto per la produzione e abilitato per impostazione predefinita. Se si sta valutando le opzioni di database diverso per un'app, devono essere incluso come parte dell'analisi costi / benefici, perché è un vantaggio significativo del servizio. Ogni app che usa il database SQL di Azure dovrebbe sfruttarne i vantaggi, includendolo nel piano di ripristino di emergenza e nelle procedure di backup e ripristino.

### <a name="azure-app-service"></a>Servizio app di Azure

Le applicazioni Web ospitate nei livelli Premium e Standard di servizio App di Azure backup pianificati e manuali chiavi in mano il supporto. I backup includono la configurazione e il contenuto dei file, nonché il contenuto dei database usati dall'app. Supportano anche semplici filtri per l'esclusione dei file. Ripristinare le operazioni possono essere destinati a diverse istanze del servizio App, rendendo di servizio App di eseguire il backup di un modo semplice per spostare i contenuti dell'app a un altro.

I backup del servizio app sono limitati a 10 GB in totale, incluso il contenuto dell'app e del database. Si tratta di un'ottima soluzione per le nuove app in fase di sviluppo e le app su scala ridotta. Le applicazioni più avanzate non saranno in genere usare backup del servizio App. Verrà invece dipendono distribuzione affidabile e le procedure di rollback, strategie di archiviazione che non usano l'archiviazione su disco dell'applicazione e strategie di backup dedicate per i database e un archivio permanente.

## <a name="verify-backups-and-test-restore-procedures"></a>Verificare i backup e testare le procedure di ripristino

Nessun sistema di backup è completo senza una strategia per la verifica dei backup e il test delle procedure di ripristino. Anche se si usa un servizio di backup dedicato o un prodotto, è necessario comunque di documento e le pratiche per procedure di ripristino per assicurarsi che si è ben noti e riportare il sistema allo stato previsto.

Le strategie per la verifica dei backup possono variare e dipendono dalla natura dell'infrastruttura. È possibile prendere in considerazione tecniche, ad esempio la creazione di una nuova distribuzione dell'applicazione, il ripristino del backup ad esso e confrontare lo stato delle due istanze. In molti casi, questa tecnica simula strettamente la procedure di ripristino di emergenza effettiva. È sufficiente eseguire un confronto di un subset dei dati di backup con i dati in tempo reale subito dopo la creazione di un backup. Un componente comune di verifica del backup viene effettuato un tentativo di ripristinare i backup precedenti per assicurarsi che siano ancora disponibile e operativa e che il sistema di backup non è stato modificato in modo che ne esegue il rendering non compatibile.

Qualsiasi strategia è preferibile rispetto al fatto di scoprire che i backup sono danneggiati o incompleti durante un tentativo di ripristino di emergenza.

Una strategia di backup e ripristino è un elemento importante per garantire la possibilità di ripristinare l'architettura in seguito alla perdita o al danneggiamento dei dati. Esaminare l'architettura per definire i requisiti di backup e ripristino. Azure offre diversi servizi e funzionalità in grado di fornire capacità di backup e ripristino per qualsiasi architettura.
