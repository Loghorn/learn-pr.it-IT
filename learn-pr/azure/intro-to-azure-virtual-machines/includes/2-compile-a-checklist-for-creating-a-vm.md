La migrazione di server locali ad Azure richiede pianificazione e attenzione. Si possono spostare tutti contemporaneamente oppure, con maggiore probabilità, in piccoli batch o anche singolarmente. Prima di creare una singola VM, è consigliabile esaminare il modello di infrastruttura attuale e definirne il mapping nel cloud.

Di seguito verrà presentato un elenco di controllo degli aspetti da considerare.

- [Iniziare dalla rete](#Network)
- [Assegnare un nome alla VM](#Name_VM)
- [Scegliere la località della VM](#VM_Location)
- [Determinare la dimensione della VM](#VM_Size)
- [Informazioni sul modello di determinazione prezzi](#VM_Cost)
- [Spazio di archiviazione per la VM](#VM_Storage)
- [Selezionare un sistema operativo](#VM_OS)

<a name="Network" />

## <a name="start-with-the-network"></a>Iniziare dalla rete

Il primo elemento da considerare non è la macchina virtuale, ma la rete.

Le reti virtuali vengono usate in Azure per offrire connettività privata tra Macchine virtuali di Azure e altri servizi di Azure. Le VM e i servizi che fanno parte della stessa rete virtuale possono accedere l'uno all'altro. Per impostazione predefinita, i servizi all'esterno della rete virtuale non possono connettersi ai servizi all'interno. È tuttavia possibile configurare la rete in modo da consentire l'accesso al servizio esterno, includendo i server locali.

Questo ultimo punto è il motivo per cui è consigliabile dedicare tempo a valutare la propria configurazione di rete. Modificare le subnet e gli indirizzi di rete dopo che sono stati configurati non è semplice. Se si prevede di connettere la rete aziendale privata ai servizi di Azure, sarà opportuno considerare la topologia prima di implementare qualsiasi VM.

Quando si configura una rete virtuale, si specificano gli spazi indirizzi disponibili, le subnet e la sicurezza. Se la rete virtuale verrà connessa ad altre reti virtuali, è necessario selezionare intervalli di indirizzi che non si sovrappongano. Si tratta dell'intervallo di indirizzi privati che può essere usato dalle VM e dai servizi nella rete. È possibile usare indirizzi IP non instradabili come 10.0.0.0/8, 172.16.0.0/12 o 192.168.0.0/16 oppure definire un proprio intervallo. Qualsiasi intervallo di indirizzi raggiungibile solo all'interno della rete virtuale, nelle reti virtuali interconnesse e dal percorso locale verrà considerato da Azure come parte dello spazio indirizzi IP privato della rete virtuale. Se un altro utente è responsabile delle reti interne, è necessario contattare questa persona prima di selezionare lo spazio indirizzi per assicurarsi che non si verifichino sovrapposizioni e per comunicare lo spazio che si intende specificare, in modo da non usare lo stesso intervallo di indirizzi IP.

### <a name="segregate-your-network"></a>Separare la rete

Dopo aver scelto gli spazi indirizzi della rete virtuale, è possibile creare una o più subnet per la rete virtuale. Questa operazione viene eseguita per suddividere la rete in sezioni più gestibili. Ad esempio, è possibile assegnare 10.1.0.0 alle VM, 10.2.0.0 ai servizi back-end e 10.3.0.0 alle VM di SQL Server.

> [!NOTE]
> Azure riserva i primi quattro indirizzi e l'ultimo di ogni subnet per uso interno.

### <a name="secure-the-network"></a>Proteggere la rete

Per impostazione predefinita non esistono limiti di sicurezza tra le subnet e i servizi in ognuna di esse possono quindi comunicare tra loro. È tuttavia possibile configurare gruppi di sicurezza di rete (NSG) che consentono di controllare il flusso di traffico da e verso le subnet e da e verso le VM. I gruppi di sicurezza di rete fungono da firewall software, applicando regole personalizzate a ogni richiesta in ingresso o in uscita a livello di interfaccia di rete e di subnet. In questo modo è possibile ottenere il controllo completo su ogni richiesta di rete in ingresso e in uscita nella VM.

## <a name="plan-each-vm-deployment"></a>Pianificare ogni distribuzione di VM

Al termine del mapping dei requisiti di comunicazione e di rete, si può iniziare considerare le VM da creare. Un buon piano consiste nel selezionare un server e completare un inventario:

- Con quali risorse comunica il server?
- Quali porte sono aperte?
- Quale sistema operativo viene usato?
- Quanto spazio su disco viene usato?
- Che tipo di dati viene usato? Sono previste restrizioni, legali o di altro tipo, se il server non è in locale?
- Qual è il carico di CPU, memoria e I/O dei dischi del server? È necessario tenere conto di picchi di traffico?

Rispondendo a queste domande si potranno fornire alcune delle informazioni che verranno richieste da Azure per una nuova macchina virtuale.

<a name="Name_VM" />

### <a name="name-the-vm"></a>Assegnare un nome alla VM

Un'informazione a cui di solito non viene prestata molta attenzione è il **nome** della VM. Il nome della VM viene usato come nome computer, configurato nell'ambito del sistema operativo. Il nome specificato può contenere fino a 15 caratteri per una VM Windows e 64 caratteri per una VM Linux.

Questo nome definisce anche una **risorsa di Azure** gestibile e modificarlo in un secondo momento non è semplice. È quindi consigliabile scegliere nomi significativi e coerenti per poter identificare facilmente la funzione svolta dalla VM. È buona pratica includere nel nome le informazioni seguenti:

| Elemento | Esempio | Note |
| --- | --- | --- |
| Ambiente |dev, prod, QA |Identifica l'ambiente per la risorsa |
| Località |usOcc (Stati Uniti occidentali), usOr (Stati Uniti orientali) |Identifica l'area in cui la risorsa viene distribuita. |
| Istanza |01, 02 |Per le risorse con più istanze denominate (server Web e così via) |
| Prodotto o servizio |service |Identifica il prodotto, l'applicazione o il servizio supportato dalla risorsa |
| Ruolo |sql, web, messaggistica |Identifica il ruolo della risorsa associata | 

Ad esempio, `devusc-webvm01` potrebbe rappresentare il primo server Web di sviluppo ospitato nella località Stati Uniti centro-meridionali. 

#### <a name="what-is-an-azure-resource"></a>Informazioni sulle risorse di Azure

Una **risorsa di Azure** è un elemento gestibile in Azure. Così come per un computer fisico in un data center, affinché le VM svolgano la propria funzione sono necessari diversi elementi:

- VM
- Account di archiviazione per i dischi
- Rete virtuale, condivisa con altri servizi e VM
- Interfaccia di rete per la comunicazione sulla rete
- Gruppi di sicurezza di rete per proteggere il traffico di rete
- Indirizzo Internet pubblico (facoltativo)

Azure creerà tutte queste risorse in base alle esigenze. In alternativa, è possibile specificare risorse esistenti nell'ambito del processo di distribuzione. Ogni risorsa deve avere un nome che verrà usato per identificarla. Se Azure crea la risorsa, userà il nome della VM per generare un nome di risorsa. Questo è un altro motivo per cui è opportuno mantenere la massima coerenza nei nomi di VM.

<a name="VM_Location" />

### <a name="decide-the-location-for-the-vm"></a>Scegliere la località della VM

Azure ha data center con server e dischi in tutto il mondo. Questi data center sono raggruppati in _aree_ geografiche, come "Stati Uniti occidentali", "Europa settentrionale", "Asia sud-orientale" e così via, per garantire ridondanza e disponibilità.

Quando si crea e si distribuisce una macchina virtuale, è necessario selezionare un'area in cui si vogliono allocare le risorse (CPU, spazio di archiviazione e così via). Ciò consente di posizionare le VM il più vicino possibile agli utenti per migliorare le prestazioni e rispettare qualsiasi requisito legale, di conformità o fiscale.

<a name="VM_Size" />

### <a name="determine-the-size-of-the-vm"></a>Determinare la dimensione della VM

Dopo aver impostato il nome e la località, è necessario scegliere la dimensione della VM. Invece di specificare potenza di elaborazione, memoria e capacità di archiviazione in modo indipendente, Azure offre diverse _dimensioni di VM_, che forniscono varianti di questi elementi in diverse dimensioni. Azure offre una vasta gamma di opzioni in termini di dimensioni di VM e consente di selezionare la combinazione di calcolo, memoria e spazio di archiviazione appropriata in base alle specifiche esigenze.

Il modo migliore per determinare la dimensione di VM appropriata consiste nel considerare il tipo di carico di lavoro che la VM dovrà eseguire. In base al carico di lavoro, si può scegliere tra un subset delle dimensioni di VM disponibili. In Azure, le opzioni di carico di lavoro sono classificate come segue:

| Opzione              | Descrizione |
|---------------------|-------------|
| **Utilizzo generico** | Le VM per utilizzo generico sono progettate per offrire un rapporto CPU-memoria equilibrato. Sono ideali per test e sviluppo, database medio-piccoli e server Web con traffico da medio a ridotto. |
| **Con ottimizzazione per il calcolo** | Le VM con ottimizzazione per il calcolo sono progettate per offrire un rapporto CPU-memoria elevato. Sono idonee per server Web con traffico medio, appliance di rete, processi batch e server applicazioni. |
| **Ottimizzato per la memoria** | Le VM con ottimizzazione per la memoria sono progettate per offrire un rapporto memoria-CPU elevato. Sono eccellenti per server di database relazionali, cache medio-grandi e analisi in memoria. |
| **Con ottimizzazione per l'archiviazione** | Le VM con ottimizzazione per l'archiviazione sono progettate per offrire livelli elevati di I/O e velocità effettiva dei dischi. Sono ideali per le VM che eseguono database. |
| **GPU** | Le VM GPU sono macchine virtuali specializzate destinate a carichi intensivi di rendering della grafica e modifica di video. Queste VM sono opzioni ideali per l'inferenza e il training di modelli con l'apprendimento profondo. |
| **High Performance Computing** | Le VM con High Performance Computing sono le macchine virtuali con le CPU più veloci e potenti e con interfacce di rete facoltative a velocità effettiva elevata. |

È possibile filtrare il tipo di carico di lavoro quando si configura la dimensione della VM in Azure. La dimensione scelta influisce direttamente sul costo del servizio. Maggiore è la capacità di memoria, CPU e GPU necessaria, maggiore sarà il prezzo.

<a name="VM_Cost" />

### <a name="understanding-the-pricing-model"></a>Informazioni sul modello di determinazione prezzi

Per ogni VM, alla sottoscrizione verranno addebitati due costi separati: calcolo e archiviazione.

**Costi di calcolo**: il prezzo viene determinato su base oraria, ma le spese di calcolo vengono fatturate al minuto. Se la VM viene distribuita per 55 minuti, ad esempio, vengono addebitati solo 55 minuti di utilizzo. Non vengono addebitati costi per la capacità di calcolo in caso di arresto e deallocazione della VM, perché in questo modo viene rilasciato l'hardware. La tariffa oraria varia in base alla dimensione di VM e al sistema operativo selezionati. Il costo per una VM include l'addebito per il sistema operativo Windows. Le istanze basate su Linux sono più economiche perché non sono previsti costi di licenza per il sistema operativo.

> [!TIP]
> È possibile risparmiare riusando licenze esistenti per Windows con il **Vantaggio Azure Hybrid**.

**Costi di archiviazione**: il costo per lo spazio di archiviazione usato dalla VM viene addebitato separatamente. Lo stato della VM non influisce sulle spese di archiviazione sostenute. Anche se la VM viene arrestata/deallocata e non vengono fatturati costi per la VM in esecuzione, verrà addebitato il costo per lo spazio di archiviazione usato dai dischi.

Per i costi di calcolo è possibile scegliere tra due opzioni di pagamento.

| Opzione | Descrizione |
|--------|-------------|
| **Pagamento in base al consumo** | Con l'opzione del **pagamento in base al consumo**, si paga la capacità di calcolo al secondo, senza alcun impegno a lungo termine o pagamento anticipato. È possibile aumentare o ridurre la capacità di calcolo su richiesta, nonché eseguire l'avvio o l'arresto in qualsiasi momento. Preferire questa opzione se si eseguono applicazioni con carichi di lavoro a breve termine o imprevedibili che non possono essere interrotti. È l'opzione appropriata, ad esempio, per eseguire un rapido test o sviluppare un'app in una VM. |
| **Istanze di macchina virtuale riservate** | L'opzione delle istanze di macchina virtuale riservate consiste nell'acquisto anticipato di una macchina virtuale per uno o tre anni in un'area specificata. Si fissa un impegno anticipato e in cambio si ottiene un risparmio del 72% sul prezzo con pagamento in base al consumo. Le **istanze riservate** sono flessibili e possono essere facilmente scambiate o restituite pagando un corrispettivo per la risoluzione anticipata. Preferire questa opzione se la VM deve essere eseguita in modo continuo o si desidera un budget prevedibile **ed** è possibile impegnarsi a usare la VM per almeno un anno. |

<a name="VM_Storage" />

### <a name="storage-for-the-vm"></a>Spazio di archiviazione per la VM

Tutte le macchine virtuali di Azure hanno almeno due dischi rigidi virtuali. Il primo disco contiene il sistema operativo, mentre il secondo viene usato come risorsa di archiviazione temporanea. È possibile aggiungere altri dischi per archiviare i dati delle applicazioni. Il numero massimo è determinato dalla dimensione di VM selezionata e corrisponde in genere a due per CPU. Vengono comunemente creati uno o più dischi dati, in particolare perché il disco del sistema operativo tende a essere di dimensioni ridotte. La separazione dei dati su dischi rigidi virtuali diversi, inoltre, consente di gestire in modo indipendente la sicurezza, l'affidabilità e le prestazioni del disco.

I dati per ogni disco rigido virtuale sono contenuti in **Archiviazione di Azure** come BLOB di pagine, in modo da consentire ad Azure di allocare solo lo spazio effettivamente usato. Il costo di archiviazione viene infatti misurato in questo modo, ossia in base allo spazio di archiviazione utilizzato.

#### <a name="what-is-azure-storage"></a>Informazioni su Archiviazione di Azure

Archiviazione di Azure è la soluzione di archiviazione dati basata sul cloud di Microsoft. Supporta quasi tutti i tipi di dati e offre sicurezza, ridondanza e accesso scalabile ai dati archiviati. Un account di archiviazione consente di accedere agli oggetti in Archiviazione di Azure per una sottoscrizione specifica. Le VM hanno sempre uno o più account di archiviazione che includono ogni disco virtuale collegato.

I dischi virtuali possono essere basati su account di archiviazione **Standard** o **Premium**. Archiviazione Premium di Azure sfrutta unità SSD per supportare prestazioni elevate e bassa latenza nelle VM che eseguono carichi di lavoro con I/O elevato. Usare Archiviazione Premium di Azure per carichi di lavoro di produzione, soprattutto se sensibili alle variazioni nelle prestazioni o con I/O elevato. L'archiviazione Standard è appropriata per lo sviluppo o i test.

Quando si creano i dischi, sono disponibili due opzioni per gestire la relazione tra l'account di archiviazione e ogni disco rigido virtuale. È possibile scegliere **dischi non gestiti** oppure **dischi gestiti**.

| Opzione | Descrizione |
|--------|-------------|
| **Dischi non gestiti** | Con i dischi non gestiti si è responsabili degli account di archiviazione usati per i dischi rigidi virtuali corrispondenti ai dischi delle VM. Vengono addebitate le tariffe degli account di archiviazione per la quantità di spazio usata. Un singolo account di archiviazione prevede un limite di velocità fissa di 20.000 operazioni di I/O al secondo, ossia può supportare 40 dischi rigidi virtuali standard con utilizzo massimo. Se si deve aumentare il numero di istanze con altri dischi, saranno necessari più account di archiviazione e ciò può risultare complicato. |
| **Dischi gestiti** | I dischi gestiti sono il **modello di archiviazione su disco più recente e consigliato**. Consentono infatti di risolvere queste complicazioni delegando ad Azure la gestione degli account di archiviazione. Si specificano le dimensioni del disco, fino a 4 TB, e Azure crea e gestisce sia il disco _che_ lo spazio di archiviazione. Non è necessario preoccuparsi dei limiti dell'account di archiviazione ed è così possibile aumentare più facilmente il numero di istanze di dischi gestiti. |

<a name="VM_OS" />

### <a name="select-an-operating-system"></a>Selezionare un sistema operativo

Azure offre un'ampia gamma di immagini del sistema operativo installabili nella VM, tra cui diverse versioni di Windows e diversi tipi di Linux. Come indicato in precedenza, la scelta del sistema operativo influirà sulle tariffe orarie di calcolo perché Azure include nel prezzo il costo della licenza del sistema operativo.

Se si preferisce non limitarsi a immagini del sistema operativo di base, è possibile cercare in Azure Marketplace immagini di installazione più sofisticate che includono il sistema operativo e gli strumenti software più diffusi installati per scenari specifici. Se è necessario un nuovo un sito WordPress, ad esempio, lo stack di tecnologie standard è costituito da un server Linux, un server Web Apache, un database MySQL e PHP. Invece di installare e configurare ogni singolo componente, è possibile sfruttare un'immagine del Marketplace e installare l'intero stack in una sola operazione.

Se non si riesce a trovare un'immagine del sistema operativo adeguata, infine, è possibile creare un'immagine del disco personalizzata inserendo gli elementi necessari, caricarla nell'archivio di Azure e usarla per creare una VM di Azure. Tenere presente che Azure supporta solo sistemi operativi a 64 bit.