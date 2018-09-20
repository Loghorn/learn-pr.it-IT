Alcune applicazioni richiedono una quantità maggiore di risorse di archiviazione dati rispetto ad altre. Le app come Dynamics CRM, Exchange Server, SAP Business Suite, SQL Server, Oracle e SharePoint richiedono alte prestazioni costanti e una bassa latenza per un'esecuzione ottimale.

Durante la creazione delle macchine virtuali o l'aggiunta di nuovi dischi, si dispone di alcune opzioni che avranno un impatto significativo sulle prestazioni del disco, a partire dal _tipo_ di archiviazione scelto.

## <a name="types-of-disks"></a>Tipi di dischi 
I dischi di Azure sono stati progettati per il 99,999% di disponibilità. 

Durante la creazione di dischi è possibile scegliere tra tre livelli di prestazioni per l'archiviazione: dischi SSD Premium, HDD Standard e SSD Standard. A seconda delle dimensioni di macchina virtuale, è possibile combinare e associare questi tipi di dischi.

### <a name="premium-ssd-disks"></a>Dischi SSD Premium 

I dischi SSD Premium sono basati su unità SSD (Solid-State Drive)e offrono prestazioni elevate e supporto per dischi a bassa latenza per le macchine virtuali che eseguono carichi di lavoro con un elevato numero di operazioni di I/O. Queste unità tendono a essere più affidabili perché non hanno componenti mobili. Una testina di lettura o scrittura non deve spostarsi nella posizione appropriata su un disco per trovare i dati richiesti. 

È possibile usare i dischi SSD Premium con dimensioni di macchine virtuali che includono una "s" nel nome della serie. Sono ad esempio disponibili la **serie Dv3** e la serie **Dsv3**. La **serie Dsv3** può essere usata con i dischi SSD Premium.

### <a name="standard-hdd-storage"></a>Archiviazione su dischi HDD Standard

I dischi HDD Standard sono supportati da unità disco rigido (HDD) tradizionali. I dischi HDD Standard vengono fatturati a una tariffa inferiore rispetto ai dischi Premium. I dischi HDD Standard possono essere usati con qualsiasi dimensione di macchina virtuale.

### <a name="standard-ssd"></a>SSD Standard

L'archiviazione Premium è limitata a dimensioni specifiche di macchine virtuali, quindi il tipo di macchina virtuale avrà un impatto sulle funzionalità di archiviazione: dimensioni, capacità massima e tipo di archiviazione. Si supponga di avere una macchina virtuale di fascia bassa, ma che siano necessari supporti di archiviazione SSD per le prestazioni di I/O? Questa è la situazione per cui sono ideali i dischi SSD Standard. I dischi SSD Standard offrono un livello intermedio di costi e prestazioni tra i dischi HDD Standard e quelli SSD Premium.

È possibile usare i dischi SSD Standard con qualsiasi dimensione di macchina virtuale, incluse le dimensioni che non supportano l'archiviazione Premium. I dischi SSD Standard rappresentano infatti l'unico modo per usare unità SSD con tali macchine virtuali. Questo tipo di disco è disponibile solo in aree specifiche e solo con _dischi gestiti_.

### <a name="unmanaged-vs-managed-disks"></a>Dischi non gestiti e dischi gestiti

Quando si creano macchine virtuali o dischi rigidi virtuali, è possibile scegliere di usare dischi **non gestiti** oppure **gestiti**.

Con i dischi non gestiti si è responsabili degli account di archiviazione usati per i dischi rigidi virtuali corrispondenti ai dischi delle macchine virtuali. Si pagano le tariffe dell'account di archiviazione per la quantità di spazio usato. Un singolo account di archiviazione prevede un limite fisso di velocità di 20.000 operazioni di I/O al secondo e questo significa che un singolo account di archiviazione è in grado di supportare 40 dischi rigidi virtuali standard a pieno ritmo. Se è necessario aumentare, servirà più di un account di archiviazione e ciò può risultare complicato.

I dischi gestiti sono il **modello di archiviazione su disco consigliato** e più recente. Consentono infatti di risolvere queste complicazioni delegando ad Azure la gestione degli account di archiviazione. È necessario specificare il tipo di disco e le dimensioni del disco ed Azure crea e gestisce sia il disco _che_ lo spazio di archiviazione usati. Non è necessario preoccuparsi dei limiti per l'account di archiviazione e aumentare la disponibilità diventa quindi più semplice. Ecco alcuni dei vantaggi disponibili rispetto ai dischi non gestiti precedenti:

- **Maggiore affidabilità**: Azure garantisce che i dischi rigidi virtuali associati a macchine virtuali a elevata affidabilità verranno posizionati in parti diverse dell'archiviazione di Azure per fornire livelli simili di resilienza.
- **Migliore sicurezza**: i dischi gestiti sono risorse gestite nel gruppo di risorse. Questo significa che possono sfruttare il controllo degli accessi in base al ruolo per limitare chi può usare i dati dei dischi rigidi virtuali.
- **Supporto degli snapshot**: è possibile usare gli snapshot per creare una copia di sola lettura di un disco rigido virtuale. È necessario arrestare la macchina virtuale proprietaria, ma la creazione dello snapshot richiede solo pochi secondi. Al termine, è possibile accendere la macchina virtuale e usare lo snapshot per creare una macchina virtuale duplicata per risolvere un problema di produzione o ripristinare la macchina virtuale al punto in cui è stato creato lo snapshot.
- **Supporto del backup**: è possibile eseguire automaticamente il backup dei dischi gestiti in aree diverse per il ripristino di emergenza con Backup di Azure senza effetti sul servizio della macchina virtuale.

Considerati tutti i vantaggi aggiuntivi, incluse le caratteristiche di prestazioni garantite, è consigliabile scegliere sempre dischi gestiti per le nuove macchine virtuali.

### <a name="disk-comparison"></a>Confronto dei dischi
La tabella seguente mette a confronto dischi HDD Standard, SSD Standard e SSD Premium per aiutare a scegliere quali usare.

|    | Disco Premium di Azure |Disco SSD Standard di Azure (anteprima)| Disco HDD Standard di Azure 
|--- | ------------------ | ------------------------------- | ----------------------- 
| **Tipo di disco** | Unità SSD | Unità SSD | Unità HDD  
| **Panoramica**  | Supporto per dischi SSD a prestazioni elevate e bassa latenza per le macchine virtuali che eseguono carichi di lavoro con elevato numero di operazioni di I/O o ambienti di produzione host cruciali |Affidabilità e prestazioni più coerenti rispetto ai dischi HDD. Ottimizzato per carichi di lavoro con un basso numero di operazioni di I/O al secondo| Disco conveniente basato su HDD per l'accesso non frequente
| **Scenario**  | Carichi di lavoro di produzione con requisiti particolari di prestazioni |Server Web, applicazioni aziendali usate poco di frequente e sviluppo/test| Backup, carichi di lavoro non critici, accesso poco frequente 
| **Dimensioni disco** | 32-4095 GiB | 128-4095 GiB | 32-4095 GiB 
| **Velocità effettiva massima per disco** | 250 MiB/s | Fino a 60 MiB/s | Fino a 60 MiB/s 
| **Numero massimo di operazioni di I/O al secondo per disco** | 7500 IOPS | Fino a 500 IOPS | Fino a 500 IOPS 

Di seguito sono disponibili altri dettagli sulle prestazioni dei dischi.

## <a name="data-replication"></a>Replica dei dati

I dati nell'account di archiviazione di Microsoft Azure vengono sempre replicati per assicurarne la durabilità e la disponibilità elevata. La replica di Archiviazione di Azure copia i dati in modo che siano protetti da eventi pianificati e non pianificati, ad esempio errori hardware temporanei, interruzioni della rete o dell'alimentazione, gravi calamità naturali e così via. È possibile scegliere di replicare i dati nello stesso data center, tra data center di zona nella stessa area e persino tra aree. Ci sono quattro tipi di replica:

- **Archiviazione con ridondanza locale (LRS)** - Azure replica i dati nello stesso data center di Azure. I dati rimangono disponibili in caso di errore di un nodo. In caso di errore dell'intero data center, tuttavia, i dati potrebbero non essere disponibili.
- **Archiviazione con ridondanza geografica (GRS)** - Azure replica i dati in un'area secondaria a centinaia di chilometri di distanza dall'area primaria. Se per l'account di archiviazione è stata abilitata l'archiviazione con ridondanza geografica, la durabilità dei dati è assicurata anche in caso di un'interruzione completa a livello di area o in situazioni di emergenza in cui l'area primaria non è recuperabile.
- **Archiviazione con ridondanza geografica e accesso in lettura (RA-GRS)** - Azure fornisce accesso in sola lettura ai dati nell'area secondaria e la replica geografica tra due aree. In caso di errore di un data center, i dati rimangono leggibili ma non possono essere modificati.
- **Archiviazione con ridondanza della zona (ZRS)** - Azure replica i dati in modo sincrono in tre cluster di archiviazione in una singola area. Ogni cluster di archiviazione è separato fisicamente dagli altri e si trova nella propria zona di disponibilità (AZ). Con questo tipo di replica, è comunque possibile accedere ai dati e gestirli nel caso in cui una zona smetta di essere disponibile.

Gli account di archiviazione Standard supportano tutti i tipi di replica, mentre gli account di archiviazione Premium supportano solo l'archiviazione con ridondanza locale. Poiché le macchine virtuali vengono eseguite in una singola area, questa limitazione non rappresenta in genere un problema per l'archiviazione dei dischi rigidi virtuali.

## <a name="disk-performance"></a>Prestazioni dei dischi

Le prestazioni dei dischi dipendono dal tipo di disco scelto. Ogni disco è classificato per un numero specifico di operazioni di I/O al secondo, o IOPS. Inoltre, ogni unità ha una classificazione di velocità effettiva, che determina la quantità di dati che è possibile leggere o scrivere in un secondo. La combinazione di questi due elementi determina la velocità del disco.

Con l'archiviazione Standard, è possibile ottenere una velocità effettiva massima di **500 IOPS e 60 MB/secondo** per ogni disco (anche su unità SSD). Con l'archiviazione Premium, il numero di IOPS dipende dai dischi Premium scelti e dalle dimensioni della macchina virtuale.

|  | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
|--|----|----|-----|-----|-----|-----|-----|
| **Dimensioni disco** | 32 GiB | 64 GiB | 128 GiB | 512 GiB | 1 TiB | 2 TiB | 4 TiB |
| **Numero massimo di operazioni di I/O al secondo per disco** | 120 | 240 | 500 | 2300 | 5000 | 7500 | 7500 |
| **Velocità effettiva massima per disco** | 25 MB/sec | 50 MB/sec | 100 MB/sec | 200 MB/sec | 250 MB/sec | 250 MB/sec |

Come si può vedere, l'intervallo disponibile va da **25 MB/sec** e **120 IOPS** a **250 MB/sec** e **7500 IOPS**.