Per iniziare, si esamineranno rapidamente i servizi, gli stili di dati e gli account di archiviazione di Azure. 

Archiviazione di Microsoft Azure è un servizio gestito che offre risorse di archiviazione sicure, scalabili e durevoli. Esaminiamo in dettaglio questi singoli termini.

| | |
|-|-|
| **Durevole** | La ridondanza garantisce che i dati siano al sicuro in caso di errori hardware temporanei. È anche possibile replicare i dati tra data center o aree geografiche per una protezione aggiuntiva da catastrofi locali o calamità naturali. Con questo tipo di replica, i dati mantengono disponibilità elevata in caso di interruzioni impreviste. |
| **Sicuro** | Tutti i dati scritti in Archiviazione di Azure vengono crittografati dal servizio. Archiviazione di Azure offre un controllo dettagliato su chi potrà accedere ai dati. |
| **Scalabile** | La soluzione Archiviazione di Azure è progettata per offrire scalabilità elevata in modo da soddisfare le esigenze di archiviazione dati e di prestazioni delle attuali applicazioni. |
| **Gestito** | Microsoft Azure gestisce la manutenzione e i problemi critici per conto dell'utente. |

Una singola sottoscrizione di Azure può ospitare fino a 200 account di archiviazione, ognuno dei quali può contenere 500 TB di dati. Per particolari esigenze aziendali, è possibile rivolgersi al team di Archiviazione di Azure e ottenere l'approvazione per arrivare a un massimo di 250 account di archiviazione in una singola sottoscrizione, ottenendo così fino a 125 petabyte di spazio di archiviazione.

## <a name="azure-data-services"></a>Servizi dati di Azure

Archiviazione di Azure include quattro tipi di dati.

- **BLOB**: archivio oggetti a scalabilità elevata per dati di testo e binari.
- **File**: condivisioni file gestite per distribuzioni cloud o locali.
- **Code** : archivio di messaggistica per una messaggistica affidabile tra i componenti delle applicazioni.
- **Tabelle**: archivio NoSQL per l'archiviazione senza schema di dati strutturati. Questo servizio è stato sostituito da Cosmos DB e non verrà illustrato in questa sede.

Tutti questi tipi di dati in Archiviazione di Azure sono accessibili da ogni parte del mondo tramite HTTP o HTTPS. Microsoft fornisce SDK per Archiviazione di Azure in un'ampia gamma di linguaggi, nonché un'API REST. È anche possibile esplorare visivamente i dati direttamente nel portale di Azure.

### <a name="blob-storage"></a>Archiviazione BLOB
Archiviazione BLOB di Azure è una soluzione di archiviazione di oggetti ottimizzata per l'archiviazione di enormi quantità di dati non strutturati, come dati di testo o binari. È ideale per:

- Invio di immagini o documenti direttamente in un browser, inclusi siti Web statici completi.
- Archiviazione di file per l'accesso distribuito.
- Flussi audio e video.
- Archiviazione di dati per backup e ripristino, ripristino di emergenza e archiviazione.
- Archiviazione di dati a scopo di analisi da parte di un servizio locale o ospitato in Azure.

Archiviazione di Azure supporta tre tipi di BLOB:

| Tipo di BLOB | Descrizione |
|-----------|-------------|
| **BLOB in blocchi** | I BLOB in blocchi vengono usati per contenere file di testo o binari con dimensioni fino a 5 TB (50.000 blocchi da 100 MB). Il caso d'uso principale dei BLOB in blocchi è l'archiviazione di file che vengono letti dall'inizio alla fine, ad esempio i file multimediali o di immagine per i siti Web. Sono denominati BLOB in blocchi perché i file di dimensioni superiori a 100 MB devono essere caricati come blocchi di piccole dimensioni, che vengono quindi consolidati (ne viene eseguito il commit) nell'oggetto BLOB finale. |
| **BLOB di pagine** | I BLOB di pagine vengono usati per contenere file ad accesso casuale con dimensioni fino a 8 TB. I BLOB di pagine vengono usati principalmente per l'archiviazione di backup dei dischi rigidi virtuali che fungono da dischi permanenti di Macchine virtuali di Microsoft Azure. Si chiamano BLOB di pagine perché consentono l'accesso in lettura/scrittura casuale a pagine da 512 byte. |
| **BLOB di accodamento** | I BLOB di accodamento sono costituiti da blocchi, analogamente ai BLOB in blocchi, ma sono ottimizzati per le operazioni di accodamento. Vengono spesso usati per la registrazione di informazioni da una o più origini nello stesso BLOB. È ad esempio possibile scrivere nello stesso BLOB di accodamento tutte le informazioni di registrazione traccia per un'applicazione in esecuzione in più macchine virtuali. Le dimensioni di un BLOB di accodamento singolo possono arrivare fino a un massimo di 195 GB. |

### <a name="files"></a>File
File di Azure consente di configurare condivisioni file di rete a disponibilità elevata a cui è possibile accedere usando il protocollo Server Message Block (SMB) standard. Più macchine virtuali possono quindi condividere gli stessi file con accesso sia in lettura che in scrittura. È possibile leggere i file usando l'interfaccia REST o le librerie client di archiviazione. È anche possibile associare un URL univoco a ogni file, per implementare il controllo di accesso con granularità fine a un file privato per un determinato periodo di tempo. Le condivisioni file possono essere usate per molti scenari comuni:

- Archiviazione dei file di configurazione condivisi per macchine virtuali, strumenti o utilità, in modo che tutti gli utenti usino la stessa versione.
- File di log, ad esempio diagnostica, metriche e dump di arresto anomalo del sistema.
- Dati condivisi tra applicazioni locali e macchine virtuali di Azure, per consentire la migrazione delle app nel cloud per un periodo di tempo.

### <a name="queues"></a>Code
Il Servizio di accodamento di Azure viene usato per archiviare e recuperare i messaggi. La dimensione massima dei messaggi nella coda può essere di 64 KB e una coda può contenere milioni di messaggi. Le code vengono in genere usate per archiviare elenchi di messaggi da elaborare in modo asincrono.

È possibile usare le code per connettere tra loro in modo efficiente parti diverse dell'applicazione. Ad esempio, è possibile eseguire l'elaborazione delle immagini sulle foto caricate dagli utenti. Può anche essere utile fornire funzionalità di rilevamento del viso o aggiunta di tag, in modo che gli utenti possano effettuare ricerche tra tutte le immagini archiviate nel servizio. Si possono usare le code per passare messaggi al servizio di elaborazione delle immagini e segnalare che nuove immagini sono state caricate e sono pronte per l'elaborazione. Un'architettura di questo tipo consente di sviluppare e aggiornare in modo indipendente ogni parte del servizio.

## <a name="azure-storage-accounts"></a>Account di archiviazione di Azure

Per accedere a uno di questi servizi da un'applicazione, è necessario creare un _account di archiviazione_. L'account di archiviazione offre uno spazio dei nomi univoco in Azure per archiviare gli oggetti dati e accedervi. Un account di archiviazione contiene tutti i BLOB, i file, le code, le tabelle e i dischi di macchine virtuali creati in tale account.

### <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

È possibile creare un account di archiviazione di Azure usando il portale di Azure, Azure PowerShell o l'interfaccia della riga di comando di Azure. Archiviazione di Azure offre tre diverse opzioni di account, con differenze nei prezzi e nelle funzionalità supportate.

> [!div class="mx-tableFixed"]
> | Tipo di account | Descrizione |
> |--------------|-------------|
> | **Utilizzo generico v2(GPv2)** | Gli account per utilizzo generico v2 sono account di archiviazione che supportano tutte le più recenti funzionalità relative a BLOB, file, code e tabelle. I prezzi degli account per utilizzo generico v2 sono stati definiti in modo da offrire i costi più bassi per gigabyte. |
> | **Utilizzo generico v1(GPv1)** | Gli account per utilizzo generico v1 consentono di accedere a tutti i servizi di Archiviazione di Azure, ma potrebbero non offrire le funzionalità più recenti o i prezzi più bassi per gigabyte. In questi account, ad esempio, non sono supportate l'archiviazione ad accesso sporadico e l'archiviazione archivio. Dal momento che i prezzi per le transazioni degli account per utilizzo generico v1 sono inferiori, questo tipo di account potrebbe essere vantaggioso per carichi di lavoro con varianza o frequenze di lettura elevate. |
> | **Account di archiviazione BLOB** | Gli account di archiviazione BLOB, un tipo di account legacy, supportano tutte le funzionalità per i BLOB in blocchi degli account per utilizzo generico v2, ma sono limitati al supporto dei soli BLOB in blocchi e di accodamento. I prezzi sono in linea di massima simili a quelli degli account per utilizzo generico v2. |
    
Per altre informazioni sulla creazione di account di archiviazione, vedere il modulo **Creare un account di archiviazione di Azure** nel portale di formazione.