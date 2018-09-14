L'esecuzione di una migrazione di server locali in Azure richiede pianificazione e attenzione. È possibile spostarli in una sola volta, o con maggiore probabilità, in piccoli batch o persino singolarmente. Prima di creare una singola macchina virtuale, l'utente dovrebbe esaminare l'attuale modello di infrastruttura per vedere come eseguirne il mapping nel cloud.

Di seguito viene illustrato nel dettaglio un elenco di controllo di aspetti da considerare.

- [Iniziare dalla rete](#Network)
- [Assegnare un nome alla macchina virtuale](#Name_VM)
- [Decidere la posizione della macchina virtuale](#VM_Location)
- [Determinare le dimensioni della macchina virtuale](#VM_Size)
- [Informazioni sul modello di prezzi](#VM_Cost)
- [Archiviazione della macchina virtuale](#VM_Storage)
- [Selezionare un sistema operativo](#VM_OS)

<a name="Network" />

## <a name="start-with-the-network"></a>Iniziare dalla rete

La prima cosa da considerare non è la macchina, ma la rete.

Le reti virtuali vengono usate in Azure per garantire la connettività privata tra Macchine virtuali e altri servizi di Azure. Le macchine virtuali e i servizi che fanno parte della stessa rete virtuale possono accedere l'uno all'altro. Per impostazione predefinita, i servizi esterni non possono connettersi ai servizi interni alla rete virtuale. Tuttavia, è possibile configurare la rete per consentire l'accesso al servizio esterno, inclusi i server in locale.

Questo ultimo punto è il motivo per cui è consigliabile dedicare alcuni minuti all'analisi della propria configurazione di rete. Le subnet e gli indirizzi di rete non sono semplici da modificare in seguito alla configurazione, e se si prevede di connettere la rete privata aziendale ai servizi di Azure, si dovrà tenere in considerazione la topologia prima di attivare qualsiasi macchina virtuale.

Quando si configura una rete virtuale si specificano gli spazi indirizzi, le subnet e la sicurezza disponibili. Se la rete virtuale deve essere connessa ad altre reti virtuali, è necessario selezionare intervalli di indirizzi che non si sovrappongano. Si tratta dell'intervallo di indirizzi privati che le macchine virtuali e i servizi nella rete possono usare. È possibile usare indirizzi IP non instradabili quali 10.0.0.0/8, 172.16.0.0/12 o 192.168.0.0/16 o definire il proprio intervallo. Azure considererà qualunque intervallo di indirizzi come parte dello spazio indirizzi IP privato della rete virtuale se raggiungibile solo all'interno della rete virtuale, delle reti virtuali interconnesse e dal percorso locale. Se un altro utente è responsabile per le reti interne, è necessario contattare questa persona prima di selezionare lo spazio indirizzi per assicurarsi che non si verifichino sovrapposizioni e per comunicare lo spazio che si desidera usare, in modo da non usare lo stesso intervallo di indirizzi IP.

### <a name="segregate-your-network"></a>Separare la rete

Dopo avere deciso gli spazi indirizzi della rete virtuale, è possibile creare una o più subnet per la rete virtuale. Questa operazione viene eseguita per suddividere la rete in sezioni più gestibili. Ad esempio, è possibile assegnare 10.1.0.0 alle macchine virtuali, 10.2.0.0 ai servizi back-end e 10.3.0.0 alle VM di SQL Server.

> [!NOTE]
> Azure riserva i primi quattro e l'ultimo indirizzo in ogni subnet per uso interno.

### <a name="secure-the-network"></a>Proteggere la rete

Per impostazione predefinita non esiste alcun limite di sicurezza tra subnet, quindi i servizi in ognuna di queste subnet possono comunicare tra loro. È tuttavia possibile configurare gruppi di sicurezza di rete (NSG) che consentono di controllare il flusso di traffico da e verso le subnet e da e verso le VM. Gli NSG fungono da firewall software, applicando regole personalizzate a ogni richiesta in ingresso o in uscita a livello di interfaccia di rete e di subnet. In questo modo è possibile controllare completamente ogni richiesta di rete in ingresso e in uscita della macchina virtuale.

## <a name="plan-each-vm-deployment"></a>Pianificare la distribuzione di ogni macchina virtuale

Dopo aver eseguito il mapping dei requisiti per la comunicazione e di rete, è possibile considerare le macchine virtuali da creare. Un buon piano consiste nel selezionare un server e creare un inventario:

- Con quali dispositivi comunica il server?
- Quali porte sono aperte?
- Quale sistema operativo viene usato?
- Quanto spazio su disco viene usato?
- Che tipo di dati vengono usati? Sono previste restrizioni (legali o di altro tipo) non sono disponibili, in locale?
- In che ordine sono organizzati i carichi di I/O per CPU, memoria e disco nel server? È necessario prendere in considerazione il traffico continuo?

È quindi possibile iniziare a rispondere ad alcune delle domande di Azure relative a una nuova macchina virtuale.

<a name="Name_VM" />

### <a name="name-the-vm"></a>Assegnare un nome alla macchina virtuale

Un'informazione a cui di solito la gente non pensa molto è il **nome** della macchina virtuale. Nome della macchina virtuale viene usato come nome del computer, viene configurato come parte del sistema operativo. È possibile specificare un nome di un massimo di 15 caratteri in una macchina virtuale Windows e 64 caratteri in una VM Linux.

Questo nome definisce anche una **risorsa di Azure** gestibile e non è semplice modificare in un secondo momento. Ciò significa che è necessario scegliere nomi che sono significativi e coerenti, in modo che sia possibile identificare facilmente del funzionamento della macchina virtuale. È buona pratica includere le seguenti informazioni nel nome:

| Elemento | Esempio | Note |
| --- | --- | --- |
| Ambiente |dev, prod, QA |Identifica l'ambiente per la risorsa |
| Posizione |uw (Stati Uniti occidentali), ue (Stati Uniti orientali) |Identifica l'area in cui la risorsa viene distribuita |
| Istanza |01, 02 |Per le risorse che hanno più di un'istanza denominata (server web, e così via). |
| Prodotto o servizio |servizio |Identifica il prodotto, l'applicazione o il servizio supportato dalla risorsa |
| Ruolo |SQL, Web, messaggistica |Identifica il ruolo della risorsa associata | 

Ad esempio, `devusc-webvm01` potrebbe rappresentare il primo server di sviluppo Web ospitato nella posizione Stati Uniti centro-meridionali. 

#### <a name="what-is-an-azure-resource"></a>Informazioni sulla risorsa in Azure

Un' **risorse di Azure** è un elemento gestibile in Azure. Proprio come un computer fisico nel data center, le macchine virtuali hanno diversi elementi necessari a svolgere le loro funzioni:

- Macchina virtuale
- Account di archiviazione per i dischi
- Rete virtuale condivisa con altri servizi e macchine virtuali
- Interfaccia di rete per comunicare sulla rete
- Gruppi di sicurezza di rete per proteggere il traffico di rete
- Indirizzo Internet pubblico (facoltativo)

Azure creerà tutte le risorse, se necessario, oppure l'utente può specificare quelle esistenti come parte del processo di distribuzione. Ogni risorsa richiede un nome che verrà utilizzato per identificarlo. Se Azure crea la risorsa, userà il nome della macchina virtuale per generare un nome di risorsa. Questo è un altro motivo per cui è necessario essere estremamente coerenti con i nomi della macchina virtuale.

<a name="VM_Location" />

### <a name="decide-the-location-for-the-vm"></a>Decidere la posizione della macchina virtuale

Azure dispone di data center in tutto il mondo che contengono server e dischi. Questi Data Center sono raggruppati in geografico _aree_ ("Stati Uniti occidentali", "Europa settentrionale", "Asia sud-orientale", e così via) per fornire ridondanza e disponibilità.

Quando si crea e si distribuisce una macchina virtuale, è necessario selezionare un'area in cui si desidera allocare le risorse, CPU, archiviazione e così via. Ciò consente di posizionare le macchine virtuali il più vicino possibile agli utenti per migliorare le prestazioni e rispettare i requisiti legali, di conformità o fiscali.

<a name="VM_Size" />

### <a name="determine-the-size-of-the-vm"></a>Determinare le dimensioni della macchina virtuale

Dopo aver impostato il nome e il percorso impostato, è necessario scegliere le dimensioni della macchina virtuale. Invece di specificare potenza di elaborazione, memoria e capacità di archiviazione in modo indipendente, Azure offre _macchine Virtuali di dimensioni diverse_ che implicano variazioni di questi elementi per le diverse dimensioni. Azure offre un intervallo a livello di VM opzioni relative alle dimensioni consente di selezionare la combinazione appropriata di calcolo, memoria e spazio di archiviazione per l'operazione che si desidera eseguire.

Il modo migliore per determinare le dimensioni appropriate della macchina virtuale consiste nel considerare il tipo di carico di lavoro che la macchina virtuale deve eseguire. In base al carico di lavoro, l'utente può scegliere da un subset di dimensioni della macchina virtuale disponibili. Le diverse opzioni del carico di lavoro sono classificate come segue in Azure:

| Opzione              | Descrizione |
|---------------------|-------------|
| **Utilizzo generico** | Le macchine virtuali per utilizzo generico sono progettate per avere un rapporto CPU-memoria equilibrato. Soluzione ideale per test e sviluppo, database medio-piccoli e server Web con traffico da medio a ridotto. |
| **Ottimizzate per il calcolo** | Le macchine virtuali ottimizzate per il calcolo sono progettate per avere un rapporto CPU-memoria elevato. Soluzione idonea per server Web con livelli medi di traffico, appliance di rete, processi batch e server applicazioni. |
| **Ottimizzate per la memoria** | Le macchine virtuali ottimizzate per la memoria sono progettate per avere un rapporto memoria-CPU elevato. Soluzione ideale per server di database relazionali, cache medio-grandi e analisi in memoria. |
| **Ottimizzate per l'archiviazione** | Le macchine virtuali ottimizzate per l'archiviazione sono progettate per avere I/O e velocità effettiva del disco elevati. Ideale per database in esecuzione su macchine virtuali. |
| **GPU** | Le macchine virtuali con GPU sono macchine virtuali specializzate e destinate a livelli intensivi di rendering della grafica e modifica di video. Queste macchine virtuali sono opzioni ideali per training del modello e inferenza con l'apprendimento avanzato. |
| **Macchine virtuali con High Performance Computing** | Le macchine virtuali con High Performance Computing sono le macchine virtuali con le CPU più veloci e potenti, con interfacce di rete ad alta velocità effettiva facoltative. |

È possibile filtrare il tipo di carico di lavoro quando si configurano le dimensioni della macchina virtuale in Azure. Le dimensioni scelte influiscono direttamente sul costo del servizio. Più CPU, memoria e GPU è necessario, maggiore sarà il prezzo punto.

<a name="VM_Cost" />

### <a name="understanding-the-pricing-model"></a>Informazioni sul modello di prezzi

Per ogni macchina virtuale, alla sottoscrizione possono essere addebitati due diversi costi: calcolo e archiviazione.

**Costi di calcolo**: le spese di calcolo vengono addebitate su base oraria ma vengono fatturate a minuto. Ad esempio, vengono addebitati solo 55 minuti di uso se la macchina virtuale viene distribuita per 55 minuti. Non viene addebitata la capacità di calcolo se si interrompe l'operazione e si esegue la deallocazione della macchina virtuale, poiché si rilascia l'hardware. La tariffa oraria varia in base alle dimensioni della macchina virtuale e al sistema operativo scelto. Il costo per una macchina virtuale include l'addebito per il sistema operativo Windows. Le istanze basate su Linux sono più economiche perché non sono previsti costi di licenza del sistema operativo.

> [!TIP]
> È possibile risparmiare riutilizzando le licenze esistenti per Windows con **Vantaggio Azure Hybrid**.

**Costi di archiviazione**: a parte viene addebitato lo spazio di archiviazione che la macchina virtuale usa. Lo stato della macchina virtuale non influisce in alcun modo sulle spese di archiviazione che verranno addebitate; anche se la macchina virtuale viene arrestata/deallocata e non viene addebitato il costo per la macchina virtuale in esecuzione, verrà addebitato il costo per lo spazio di archiviazione usato dai dischi.

È possibile scegliere tra due diverse opzioni di pagamento per i costi di calcolo.

| Opzione | Descrizione |
|--------|-------------|
| **Pagamento in base al consumo** | Con il **pagamento a consumo** opzione, si paga la capacità di calcolo per il secondo, senza alcun impegno a lungo termine o pagamenti iniziali. È possibile aumentare o diminuire la capacità di calcolo su richiesta, nonché avviarla o arrestarla in qualsiasi momento. Preferire questa opzione se si eseguono applicazioni con carichi di lavoro a breve termine o imprevedibili che non può essere interrotta. Ad esempio, se si esegue un rapido test, o si sviluppa un'app in una macchina virtuale, questa è l'opzione appropriata. |
| **Istanze di macchina virtuale riservate** | L'opzione Istanze di macchina virtuale riservate consiste in un acquisto anticipato di una macchina virtuale per uno o tre anni in un'area specificata. L'utente si impegna in anticipo e in cambio risparmia il 72% sul prezzo rispetto alle tariffe con pagamento a consumo. Le **istanze riservate** sono flessibili e possono facilmente essere scambiate o restituite pagando un supplemento per la risoluzione anticipata. Preferire questa opzione se la macchina virtuale deve essere eseguita in modo continuativo o se si desidera un budget prevedibile **e** se l'utente può impegnarsi a usare la macchina virtuale per almeno un anno. |

<a name="VM_Storage" />

### <a name="storage-for-the-vm"></a>Archiviazione della macchina virtuale

Tutte le macchine virtuali di Azure hanno almeno due dischi rigidi virtuali. Il primo disco archivia il sistema operativo e il secondo viene utilizzato come archiviazione temporanea. È possibile aggiungere altri dischi per archiviare i dati dell'applicazione; il numero massimo è determinato dalla scelta delle dimensioni della macchina virtuale, in genere due per CPU. È comune creare uno o più dischi dati, in particolare perché il disco del sistema operativo tende a essere molto piccolo. Inoltre, separando i dati da diversi dischi rigidi virtuali consente di gestire la sicurezza, affidabilità e prestazioni del disco in modo indipendente.

I dati per ogni disco rigido virtuale vengono archiviati nei **archiviazione di Azure** come BLOB di pagine, che consente di allocare spazio solo per lo spazio di archiviazione Usa Azure. di misurare il costo di archiviazione: l'utente paga per lo spazio di archiviazione usato.

#### <a name="what-is-azure-storage"></a>Che cos'è Archiviazione di Azure?

Archiviazione di Azure è la soluzione di archiviazione dati basata su cloud di Microsoft. Supporta quasi tutti i tipi di dati e offre sicurezza, ridondanza e accesso scalabile ai dati archiviati. Un account di archiviazione offre accesso agli oggetti in Archiviazione di Azure per una sottoscrizione specifica. Le macchine virtuali hanno sempre uno o più account di archiviazione per contenere ciascun disco virtuale collegato.

I dischi virtuali si possono basare sugli account di archiviazione **Standard** oppure **Premium**. L'Archiviazione Premium di Azure sfrutta le unità SSD per consentire prestazioni elevate e bassa latenza alle macchine virtuali che eseguono carichi di lavoro di I/O intensivi. Usare l'Archiviazione Premium di Azure per carichi di lavoro di produzione, specialmente per carichi di lavoro sensibili a variazioni delle prestazioni o con I/O intensivi. L'archiviazione Standard è ottimale per lo sviluppo o il test.

Quando si creano i dischi, sono disponibili due opzioni per gestire la relazione tra l'account di archiviazione e ogni disco rigido virtuale. È possibile scegliere tra i **dischi non gestiti** o i **dischi gestiti**.

| Opzione | Descrizione |
|--------|-------------|
| **Dischi non gestiti** | Con i dischi non gestiti si è responsabili degli account di archiviazione usati per archiviare i dischi rigidi virtuali corrispondenti ai dischi delle macchine virtuali. Si pagano le tariffe dell'account di archiviazione per la quantità di spazio usato. Un singolo account di archiviazione ha un limite di frequenza fissa di 20.000 operazioni dei / o/sec. Ciò significa che un account di archiviazione è in grado di supportare 40 standard i dischi rigidi virtuali a utilizzo completo. Se è necessario aumentare i dischi, serviranno più account di archiviazione e ciò può risultare complicato. |
| **Dischi gestiti** | I dischi gestiti rappresentano il **modello di archiviazione su disco consigliato più recente**. Risolvono in modo elegante queste complicazioni delegando ad Azure il carico di gestione degli account di archiviazione. È necessario specificare le dimensioni del disco, fino a 4 TB, ed Azure crea e gestisce sia il disco _che_ lo spazio di archiviazione. Non è necessario preoccuparsi dei limiti per l'account di archiviazione e aumentare la disponibilità dei dischi gestiti diventa quindi più semplice. |

<a name="VM_OS" />

### <a name="select-an-operating-system"></a>Selezionare un sistema operativo

Azure offre un'ampia gamma di immagini del sistema operativo che è possibile installare nella macchina virtuale, tra cui diverse versioni di Windows e versioni di Linux. Come accennato in precedenza, la scelta del sistema operativo influisce sulle tariffe orarie di calcolo perché Azure include il costo della licenza del sistema operativo nel prezzo.

Se si desidera avere più di una semplice immagine del sistema operativo, è possibile cercare nel Marketplace di Azure immagini di installazione più sofisticate che includono il sistema operativo e gli strumenti più diffusi software installati per scenari specifici. Ad esempio, se è necessario un nuovo sito WordPress, lo stack di tecnologie standard costituita da un server Linux, server web Apache, un database MySQL e PHP. Invece di impostare e configurare ogni singolo componente, è possibile usare un'immagine del Marketplace e installare l'intero stack in una sola operazione.

Infine, se non è possibile trovare un'immagine del sistema operativo adeguata, è possibile creare l'immagine del disco con ciò che è necessario caricarlo in archiviazione di Azure e usarlo per creare una VM di Azure. Tenere presente che Azure supporta solo sistemi operativi a 64 bit.