A questo punto è disponibile un sito Web esistente in esecuzione in un server Ubuntu Linux locale. L'obiettivo è creare una macchina virtuale di Azure usando l'immagine Ubuntu più recente e quindi eseguire la migrazione del sito nel cloud. In questa unità verranno presentate le opzioni che occorrerà valutare durante la creazione di una macchina virtuale in Azure.

## <a name="introduction-to-azure-virtual-machines"></a>Introduzione alle macchine virtuali di Azure

Le macchine virtuali di Azure sono risorse di cloud computing scalabili su richiesta. Includono processore, memoria, spazio di archiviazione e risorse di rete. È possibile avviare e arrestare le macchine virtuali in base alle esigenze e gestirle dal portale o mediante l'interfaccia della riga di comando di Azure. È anche possibile usare SSH (Secure Shell) in remoto per connettersi direttamente alla macchina virtuale in esecuzione ed eseguire comandi come da un computer locale.

### <a name="running-linux-in-azure"></a>Esecuzione di Linux in Azure

La creazione di macchine virtuali basate su Linux in Azure è facile. Microsoft ha collaborato con i principali fornitori di Linux per assicurarsi che le distribuzioni siano ottimizzate per la piattaforma Azure. È possibile creare macchine virtuali da immagini predefinite per un'ampia gamma delle distribuzioni Linux più diffuse, ad esempio SUSE, Red Hat e Ubuntu o creare una propria distribuzione di Linux per l'esecuzione nel cloud.

## <a name="creating-an-azure-vm"></a>Creazione di una macchina virtuale di Azure

Le macchine virtuali possono essere definite e distribuite in Azure in diversi modi: il portale di Azure, uno script (tramite l'interfaccia della riga di comando di Azure o Azure PowerShell) oppure tramite un modello di Azure Resource Manager. In tutti i casi sarà necessario specificare varie informazioni, che verranno trattate a breve.

Azure Marketplace offre anche immagini preconfigurate che includono sia un sistema operativo che gli strumenti software preferiti installati per scenari specifici.

![Screenshot del portale di Azure con diverse opzioni di macchina virtuale in Azure Marketplace.](../media/2-marketplace-vm-choices.png)

## <a name="resources-used-in-a-linux-vm"></a>Risorse usate in una macchina virtuale Linux

Quando si crea una macchina virtuale Linux in Azure, è necessario creare anche le risorse per ospitarla. Queste risorse interagiscono per virtualizzare un computer ed eseguire il sistema operativo Linux. Le risorse devono essere già esistenti (e quindi selezionate durante la creazione della macchina virtuale) oppure verranno create con la macchina virtuale:

- Una macchina virtuale che fornisce le risorse CPU e memoria
- Un account di Archiviazione di Azure per contenere i dischi rigidi virtuali
- Dischi virtuali per contenere il sistema operativo, le applicazioni e i dati
- Una rete virtuale (VNet) per connettere la macchina virtuale ad altri servizi di Azure o all'hardware in locale
- Un'interfaccia di rete per comunicare con la rete virtuale
- Un indirizzo IP pubblico facoltativo in modo da poter accedere alla macchina virtuale

Come per altri servizi di Azure, sarà necessario un **gruppo di risorse** per contenere la macchina virtuale (e, facoltativamente, raggruppare queste risorse per l'amministrazione). Quando si crea una nuova macchina virtuale, è possibile usare un gruppo di risorse esistente o crearne uno nuovo.

## <a name="choose-the-vm-image"></a>Scegliere l'immagine della VM

La scelta di un'immagine è una delle decisioni principali e più importanti da prendere in fase di creazione di una VM. Un'immagine è un modello usato per creare una VM. Questi modelli includono un sistema operativo e, spesso, anche altro software, ad esempio strumenti di sviluppo o ambienti di hosting Web.

Tutto ciò che può essere installato in un computer può essere incluso in un'immagine. È possibile creare una macchina virtuale da un'immagine preconfigurata esattamente per le attività necessarie, ad esempio l'hosting di un'app Web nel server HTTP Apache.

> [!TIP]
> È anche possibile creare e caricare immagini disco personalizzate.

## <a name="sizing-your-vm"></a>Dimensionamento di una macchina virtuale

Proprio come un computer fisico dispone di una determinata quantità di memoria e potenza della CPU, lo stesso vale per una macchina virtuale. Azure offre una serie di macchine virtuali con dimensioni e prezzi diversi. Le dimensioni scelte determineranno la potenza di elaborazione, la memoria e la capacità di archiviazione massima della macchina virtuale.

> [!WARNING]
> Sono previsti limiti di quota per ogni sottoscrizione, che possono influire sulla creazione della macchina virtuale. Se si incorre in questi limiti di quota è possibile [aprire una richiesta di assistenza clienti online](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) per aumentare i limiti.

Le dimensioni delle macchine virtuali vengono raggruppate in categorie, a partire dalla serie B per attività di testing di base fino alla serie H per estese attività di elaborazione. È consigliabile selezionare le dimensioni della macchina virtuale in base al carico di lavoro da eseguire. È possibile modificare le dimensioni di una macchina virtuale dopo averla creata, ma è prima necessario arrestarla. Pertanto, è consigliabile selezionare le dimensioni appropriate sin dall'inizio se possibile.

#### <a name="here-are-some-guidelines-based-on-the-scenario-you-are-targeting"></a>Di seguito sono riportate alcune linee guida in base allo scenario di destinazione

| Scenario | Dimensioni da considerare
|-------|------------------|
| **Attività di elaborazione/Web per utilizzo generico**: test e sviluppo, database medio-piccoli e server Web con traffico da medio a ridotto. | B, Dsv3, Dv3, DSv2, Dv2 |
| **Attività di calcolo intensive**: server Web con livelli medi di traffico, appliance di rete, processi batch e server applicazioni. | Fsv2, Fs, F |
| **Utilizzo elevato della memoria**: server di database relazionali, cache medio-grandi e analisi in memoria. | Esv3, Ev3, M, GS, G, DSv2, Dv2 |
| **Elaborazione ed archiviazione dati**: Big Data, SQL e database NoSQL con esigenze elevate per velocità effettiva del disco e I/O. | Ls |
| **Livelli intensivi di rendering della grafica** o modifica di video, nonché training e inferenza dei modelli (ND) con deep learning. | NV, NC, NCv2, NCv3, ND |
| **HPC (High-Performance Computing)**: per il carico di lavoro occorrono le macchine virtuali con le CPU più veloci e potenti, con interfacce di rete ad alta velocità effettiva facoltative. | H |

## <a name="choosing-storage-options"></a>Scelta delle opzioni di archiviazione

Il set successivo di decisioni riguarda l'archiviazione. Prima di tutto è possibile scegliere la tecnologia dei dischi. Le opzioni includono un'unità disco rigido tradizionale basata su platter o un'unità SSD più moderna. Proprio come per l'hardware acquistato, l'archiviazione SSD ha un costo maggiore, ma offre prestazioni migliori.

> [!TIP]
> Sono disponibili due livelli di archiviazione su unità SSD: Standard e Premium. Per carichi di lavoro normali, ma se si vogliono prestazioni migliori, scegliere dischi SSD Standard. Scegliere dischi SSD Premium per carichi di lavoro con I/O intensivo oppure sistemi strategici che devono elaborare i dati molto rapidamente.

### <a name="mapping-storage-to-disks"></a>Mapping di archiviazione e dischi

Azure usa dischi rigidi virtuali (VHD) per rappresentare i dischi fisici per la macchina virtuale. I dischi rigidi virtuali replicano il formato logico e i dati di un'unità disco, ma vengono archiviati come BLOB di pagine in un account di Archiviazione di Azure. È possibile scegliere per ogni disco il tipo di archiviazione da usare (SSD o HDD). Ciò consente di controllare le prestazioni di ogni disco, probabilmente in base alle attività di I/O che si prevede di eseguire.

Per impostazione predefinita, per la macchina virtuale Linux verranno creati due dischi rigidi virtuali (VHD):

1. Il **disco di sistema operativo**: questa è l'unità principale e ha una capacità massima di 2048 GB. Sarà denominata `/dev/sda` per impostazione predefinita.

1. Un **disco temporaneo**: fornisce la risorsa di archiviazione temporanea per il sistema operativo o eventuali app. Nelle macchine virtuali Linux il disco è `/dev/sdb` e viene formattato e montato in `/mnt` dall'agente Linux di Azure. Le dimensioni vengono stabilite in base alla dimensione della macchina virtuale e viene usato per archiviare il file di scambio.

> [!WARNING]
> Il disco temporaneo non è persistente. Su questo disco è consigliabile scrivere solo dati non fondamentali per il sistema.

#### <a name="what-about-data"></a>Come gestire i dati

È possibile archiviare i dati nell'unità primaria insieme al sistema operativo, ma un approccio migliore consiste nel creare _dischi dati_ dedicati. È possibile creare e collegare dischi aggiuntivi alla macchina virtuale. Ogni disco può contenere fino a 4095 GB di dati e la quantità massima di spazio di archiviazione viene determinata dalla dimensione della macchina virtuale selezionata.

> [!NOTE]
> La caratteristica interessante è la possibilità di creare un'immagine di disco rigido virtuale da un disco reale. Ciò consente di eseguire facilmente la migrazione di informazioni _esistenti_ da un computer locale al cloud.

### <a name="unmanaged-vs-managed-disks"></a>Dischi gestiti e non gestiti

La scelta finale per l'archiviazione consiste nel decidere se usare dischi **non gestiti** oppure **gestiti**.

Con i dischi non gestiti si è responsabili degli account di archiviazione usati per archiviare i dischi rigidi virtuali corrispondenti ai dischi delle macchine virtuali. Si pagano le tariffe dell'account di archiviazione per la quantità di spazio usato. Un singolo account di archiviazione prevede un limite fisso di velocità di 20.000 operazioni di I/O al secondo e questo significa che un singolo account di archiviazione è in grado di supportare 40 dischi rigidi virtuali standard a pieno ritmo. Se è necessario aumentare, servirà più di un account di archiviazione e ciò può risultare complicato.

I dischi gestiti rappresentano il modello di archiviazione su disco consigliato più recente. Risolvono in modo elegante queste complicazioni delegando ad Azure il carico di gestione degli account di archiviazione. È necessario specificare il tipo di disco, Premium o Standard, e le dimensioni del disco. Azure creerà e gestirà il disco _e_ lo spazio di archiviazione usati. Non è necessario preoccuparsi dei limiti per l'account di archiviazione e aumentare la disponibilità diventa quindi più semplice. I dischi gestiti offrono anche diversi altri vantaggi:

- **Maggiore affidabilità**: Azure garantisce che i dischi rigidi virtuali associati a macchine virtuali a elevata affidabilità verranno posizionati in parti diverse di Archiviazione di Azure per fornire livelli simili di resilienza.
- **Migliore sicurezza**: i dischi gestiti sono risorse gestite reali nel gruppo di risorse. Questo significa che possono sfruttare il controllo degli accessi in base al ruolo per limitare chi può usare i dati dei dischi rigidi virtuali.
- **Supporto degli snapshot**: è possibile usare gli snapshot per creare una copia di sola lettura di un disco rigido virtuale. È necessario arrestare la macchina virtuale proprietaria, ma la creazione dello snapshot richiede solo pochi secondi. Al termine, è possibile accendere la macchina virtuale e usare lo snapshot per creare una macchina virtuale duplicata per risolvere un problema di produzione o eseguire il rollback della macchina virtuale al punto in cui è stato creato lo snapshot.
- **Supporto del backup**: è possibile eseguire automaticamente il backup dei dischi gestiti in aree diverse per il ripristino di emergenza con Backup di Azure senza effetti sul servizio della macchina virtuale.

## <a name="network-communication"></a>Comunicazione di rete

Le macchine virtuali comunicano con le risorse esterne tramite una rete virtuale (VNet). La rete virtuale rappresenta una rete privata in un'unica area, usata dalle risorse per le comunicazioni. Una rete virtuale è esattamente come le reti gestite in locale. È possibile suddividerla tramite subnet per isolare le risorse, connetterla ad altre (incluse le reti locali) e applicare le regole del traffico per regolare le connessioni in ingresso e in uscita.

### <a name="planning-your-network"></a>Pianificazione della rete

Quando si crea una nuova macchina virtuale, si avrà la possibilità di creare una nuova rete virtuale o di usare una rete virtuale esistente nella propria area.

Scegliere di delegare ad Azure la creazione della rete insieme alla macchina virtuale è semplice, ma è probabile che non sia la soluzione ideale per la maggior parte degli scenari. È consigliabile pianificare _anticipatamente_ i requisiti di rete per tutti i componenti nell'architettura e creare separatamente la struttura della rete virtuale, quindi creare le macchine virtuali e inserirle in reti virtuali create in precedenza. Il tema delle reti virtuali verrà approfondito più avanti in questo modulo.

Prima di creare una macchina virtuale, è necessario decidere come si desidera amministrare la macchina virtuale. Esaminiamo le opzioni.