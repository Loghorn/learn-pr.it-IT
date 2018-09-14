Iniziamo prendendo un esame rapido dei servizi di archiviazione di Azure, gli stili di dati e account. 

Archiviazione di Microsoft Azure è un servizio gestito che offre sicuro, durevole e un'archiviazione scalabile nel cloud. Scomponendo le presenti condizioni.

| | |
|-|-|
| **Durevole** | La ridondanza garantisce che i dati siano al sicuro in caso di errori hardware temporanei. È anche possibile replicare dati tra Data Center o aree geografiche per una protezione aggiuntiva da catastrofi locali o calamità naturali. Con questo tipo di replica, i dati mantengono disponibilità elevata in caso di interruzioni impreviste. |
| **Proteggere** | Tutti i dati scritti in Archiviazione di Azure vengono crittografati dal servizio. Archiviazione di Azure offre un controllo dettagliato su chi potrà accedere ai dati. |
| **Scalabile** | La soluzione Archiviazione di Azure è progettata per offrire scalabilità elevata in modo da soddisfare le esigenze di archiviazione dati e di prestazioni delle attuali applicazioni. |
| **Gestito** | Microsoft Azure gestisce la manutenzione e i problemi critici per conto dell'utente. |

Una singola sottoscrizione di Azure può ospitare fino a 200 account di archiviazione, ognuno dei quali può contenere 500 TB di dati. Se si dispone di un business case, è possibile comunicare con il team di archiviazione di Azure e ottenere l'approvazione per un massimo di 250 account di archiviazione in una sottoscrizione, che esegue il push di spazio di archiviazione massimo fino a 125 petabyte!

## <a name="azure-data-services"></a>Servizi dati di Azure

Archiviazione di Azure include quattro tipi di dati.

- **I BLOB**: un archivio di oggetti a scalabilità elevatissima per i dati di testo e binari.
- **File**: file gestite condivide per il cloud o le distribuzioni locali.
- **Le code**: un archivio di messaggistica per messaggistica affidabile tra i componenti dell'applicazione.
- **Tabelle**: archivio NoSQL per l'archiviazione senza schema dei dati strutturati. Questo servizio è stato sostituito da Cosmos DB e non verrà qui descritte.

Tutti questi tipi di dati in archiviazione di Azure sono accessibili ovunque nel mondo tramite HTTP o HTTPS. Microsoft fornisce SDK per archiviazione di Azure in un'ampia gamma di linguaggi, nonché un'API REST. Consente anche di esplorare il direttamente dati nel portale di Azure.

### <a name="blob-storage"></a>Archiviazione BLOB

Archiviazione Blob di Azure è una soluzione di archiviazione di oggetti ottimizzata per l'archiviazione di enormi quantità di dati non strutturati, ad esempio i dati di testo o binario. L'archivio BLOB è ideale per le operazioni seguenti:

- Invio di immagini o documenti direttamente in un browser, inclusi i siti Web statico completo.
- Archiviazione di file per l'accesso distribuito.
- Streaming di audio e video.
- L'archiviazione dei dati per il backup e ripristino, ripristino di emergenza e archiviazione.
- Archiviazione di dati a scopo di analisi da parte di un servizio locale o ospitato in Azure.

Archiviazione di Azure supporta tre tipi di BLOB:

| Tipo di BLOB | Descrizione |
|-----------|-------------|
| **BLOB in blocchi** | I BLOB in blocchi vengono usati per contenere il testo o binario file con dimensioni fino a 5 TB (50.000 blocchi di 100 MB). Il caso d'uso principale dei BLOB in blocchi è l'archiviazione di file che vengono letti dall'inizio alla fine, ad esempio i file multimediali o di immagine per i siti Web. Si chiamano BLOB in blocchi perché i file di dimensioni superiori a 100 MB devono essere caricati come blocchi di piccole dimensioni, che vengono quindi consolidati (o stato eseguito il commit) nell'oggetto blob finale. |
| **BLOB di pagine** | I BLOB di pagine vengono usati per contenere ad accesso casuale file con dimensioni fino a 8 TB. I BLOB di pagine vengono usati principalmente come spazio di archiviazione sottostante per i dischi rigidi virtuali usati per fornire dischi durevoli di macchine virtuali di Azure (macchine virtuali di Azure). Si chiamano BLOB di pagine perché consentono l'accesso in lettura/scrittura casuale a pagine da 512 byte. |
| **I BLOB di Accodamento** | Aggiungere i BLOB sono costituiti da blocchi come i BLOB in blocchi, ma sono ottimizzati per operazioni di Accodamento. Questi vengono spesso usati per la registrazione di informazioni da una o più origini nell'oggetto blob stesso. Ad esempio, è possibile scrivere tutti la traccia di registrazione per lo stesso blob di Accodamento per un'applicazione in esecuzione su più macchine virtuali. Le dimensioni di un BLOB di accodamento singolo possono arrivare fino a un massimo di 195 GB. |

### <a name="files"></a>File

File di Azure consente di configurare condivisioni file di rete a disponibilità elevata che sono accessibili tramite il protocollo Server Message Block (SMB) standard. Ciò significa che più macchine virtuali possono condividere gli stessi file con accesso in lettura e scrittura. È possibile leggere i file usando l'interfaccia REST o le librerie dei client di archiviazione. È inoltre possibile associare un URL univoco per tutti i file per consentire l'accesso con granularità fine in un file privato per un determinato periodo di tempo. Le condivisioni file possono essere usate per molti scenari comuni:

- L'archiviazione dei file di configurazione condivisi per le macchine virtuali, strumenti o utilità in modo che tutti gli utenti Usa la stessa versione.
- Ad esempio la diagnostica, metriche, file di log e dump di arresto anomalo.
- Dati condivisi tra le applicazioni locali e macchine virtuali di Azure per consentire la migrazione delle app nel cloud in un periodo di tempo.

### <a name="queues"></a>Queues

Il servizio di accodamento di Azure viene usato per archiviare e recuperare i messaggi. La dimensione massima dei messaggi nella coda può essere di 64 KB e una coda può contenere milioni di messaggi. Le code vengono in genere usate per archiviare elenchi di messaggi da elaborare in modo asincrono.

È possibile usare le code a regime di controllo libero connettere tra loro diverse parti dell'applicazione. Ad esempio, è possibile eseguire l'elaborazione di immagini in foto caricate dagli utenti. Ad esempio si vogliono fornire una sorta di rilevamento del viso o funzionalità, l'assegnazione di tag in modo che le persone possono cercare tutte le immagini sono archiviate nel servizio. È possibile utilizzare le code per inviare i messaggi al nostro servizio per indicare che le immagini nuove sono state caricate e sono pronte per l'elaborazione di elaborazione di immagini. Questa sorta di architettura consentirebbe di sviluppare e aggiornare in modo indipendente ogni parte del servizio.

## <a name="azure-storage-accounts"></a>Account di archiviazione di Azure

Per accedere a uno di questi servizi da un'applicazione, è necessario creare un _account di archiviazione_. L'account di archiviazione offre uno spazio dei nomi univoco in Azure per archiviare e accedere agli oggetti dati. Un account di archiviazione contiene qualsiasi BLOB, file, code, tabelle e dischi delle macchine Virtuali creati con tale account.

### <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

È possibile creare un account di archiviazione di Azure usando il portale di Azure, Azure PowerShell o CLI di Azure. Archiviazione di Azure offre tre diverse opzioni di account con differenze nei prezzi e funzionalità supportate.

> [!div class="mx-tableFixed"]
> | Tipo di account | Descrizione |
> |--------------|-------------|
> | **Per utilizzo generico v2 (GPv2)** | Gli account per utilizzo generico v2 sono account di archiviazione che supportano tutte le più recenti funzionalità relative a BLOB, file, code e tabelle. Prezzi per gli account per utilizzo generico v2 è stato progettato per offrire i più bassi per gigabyte prezzi. |
> | **Utilizzo generico v1 (GPv1)** | Utilizzo generico v1 account (per utilizzo generico v1) forniscono l'accesso a tutti i servizi di archiviazione di Azure, ma potrebbero non offrire le funzionalità più recenti o i più bassi per gigabyte prezzi. In questi account, ad esempio, non sono supportate l'archiviazione ad accesso sporadico e l'archiviazione archivio. Dal momento che i prezzi per le transazioni degli account per utilizzo generico v1 sono inferiori, questo tipo di account potrebbe essere vantaggioso per carichi di lavoro con varianza o frequenze di lettura elevate. |
> | **Account di archiviazione BLOB** | Un tipo di account legacy, account di archiviazione blob supportano tutte le funzionalità dei blob di blocco per utilizzo generico v2, ma sono in grado di supportare solo blocchi e BLOB di Accodamento. I prezzi sono in linea di massima simili a quelli degli account per utilizzo generico v2. |

Se è interessati a ulteriori informazioni sulla creazione di account di archiviazione, assicurarsi di passare attraverso il **creare un account di archiviazione di Azure** modulo nel portale di formazione.