Alcune applicazioni richiedono una quantità maggiore di risorse di archiviazione dati rispetto ad altre. I server Microsoft Exchange e SharePoint, ad esempio, richiedono dischi ad alte prestazioni per l'esecuzione ottimale.

Si riesaminerà ora lo scenario di migrazione del database dei casi archiviati dello studio legale in Azure. Si vuole assicurarsi che avvocati e altri membri del personale possano accedere ai dettagli sui casi il più rapidamente possibile. Lo studio è in crescita e si vuole che il database gestisca l'aumento della domanda.

Un modo per ottimizzare le prestazioni delle macchine virtuali (VM) in Azure consiste nell'usare Archiviazione Premium per i dischi rigidi virtuali (VHD). Archiviazione Premium offre maggiore velocità di input e output (I/O) rispetto ad Archiviazione Standard. Una maggiore velocità di I/O del disco garantisce prestazioni migliori per le applicazioni a elevato utilizzo di dischi.

Verranno ora confrontati i livelli di prestazioni per l'archiviazione e verranno fornite altre informazioni sugli account di archiviazione Premium.

## <a name="how-premium-storage-differs-from-standard-storage"></a>Differenze tra Archiviazione Premium e Archiviazione Standard

In Azure l'implementazione di Archiviazione Premium viene effettuata nelle unità SSD. Le unità SSD offrono prestazioni di I/O superiori rispetto alle unità disco rigido (HDD) e possono essere più affidabili perché non hanno componenti mobili. Una testina di lettura o scrittura non deve spostarsi nella posizione appropriata su un disco per trovare i dati richiesti. 

L'implementazione di Archiviazione Standard viene effettuata nelle unità disco rigido. Archiviazione Standard ha costi in fattura inferiori. Scegliere Archiviazione Standard per controllare i costi e Archiviazione Premium quando sono richieste prestazioni di I/O veloci.

È anche possibile scegliere di usare una combinazione di Archiviazione Standard e Premium per ogni disco in una singola macchina virtuale.

> [!NOTE]
> Se si usano i dischi gestiti, è disponibile una terza opzione: unità SDD Standard. Le unità SSD Standard offrono un livello intermedio sia di prestazioni che di prezzo tra le opzioni HDD Standard e SSD Premium, ma non sono disponibili per i dischi non gestiti. Verranno fornite altre informazioni sulle unità SSD Standard più avanti in questo modulo.

## <a name="premium-storage-accounts"></a>Account di archiviazione Premium

I dischi rigidi virtuali vengono archiviati in account di archiviazione di Azure per utilizzo generico come BLOB di pagine. Per usare Archiviazione Premium per i dischi rigidi virtuali, è necessario archiviare tali dischi in un **account di archiviazione Premium**. Il livello di prestazioni di un account di archiviazione viene scelto al momento della creazione e non è possibile modificare questa impostazione in un secondo momento.

> [!IMPORTANT]
> Archiviazione Premium potrebbe non essere disponibile in tutte le aree di Azure. Per controllare la disponibilità per un'area, vedere [Prodotti disponibili in base all'area](https://azure.microsoft.com/en-us/global-infrastructure/services/).

### <a name="data-replication-and-premium-storage-accounts"></a>Replica dei dati e account di archiviazione Premium

I dati nell'account di archiviazione di Microsoft Azure vengono sempre replicati per assicurarne la durabilità e la disponibilità elevata. La replica di Archiviazione di Azure copia i dati in modo che siano protetti da eventi pianificati e non pianificati, ad esempio errori hardware temporanei, interruzioni della rete o dell'alimentazione, gravi calamità naturali e così via. È possibile scegliere di replicare i dati nello stesso data center, tra data center di zona nella stessa area e persino tra aree. Ci sono quattro tipi di replica:

- **Archiviazione con ridondanza locale (LRS)** - Azure replica i dati nello stesso data center di Azure. I dati rimangono disponibili in caso di errore di un nodo. In caso di errore dell'intero data center, tuttavia, i dati potrebbero non essere disponibili.
- **Archiviazione con ridondanza geografica (GRS)** - Azure replica i dati in un'area secondaria a centinaia di chilometri di distanza dall'area primaria. Se per l'account di archiviazione è stata abilitata l'archiviazione con ridondanza geografica, la durabilità dei dati è assicurata anche in caso di un'interruzione completa a livello di area o in situazioni di emergenza in cui l'area primaria non è recuperabile.
- **Archiviazione con ridondanza geografica e accesso in lettura (RA-GRS)** - Azure fornisce accesso in sola lettura ai dati nell'area secondaria e la replica geografica tra due aree. In caso di errore di un data center, i dati rimangono leggibili ma non possono essere modificati.
- **Archiviazione con ridondanza della zona (ZRS)** - Azure replica i dati in modo sincrono in tre cluster di archiviazione in una singola area. Ogni cluster di archiviazione è separato fisicamente dagli altri e si trova nella propria zona di disponibilità (AZ). Con questo tipo di replica, è comunque possibile accedere ai dati e gestirli nel caso in cui una zona smetta di essere disponibile.

Gli account di archiviazione Standard supportano tutti i tipi di replica, mentre gli account di archiviazione Premium supportano solo l'archiviazione con ridondanza locale. Poiché le macchine virtuali vengono eseguite in una singola area, questa limitazione non rappresenta in genere un problema per l'archiviazione dei dischi rigidi virtuali.

## <a name="migrating-from-standard-storage-to-premium-storage"></a>Migrazione da Archiviazione Standard ad Archiviazione Premium

È consigliabile creare i dischi rigidi virtuali nel tipo corretto di account di archiviazione. Talvolta, tuttavia, i requisiti cambiano, le richieste aumentano o ci si rende conto di aver scelto un tipo non appropriato. In questi casi, prendere in considerazione il passaggio ad Archiviazione Premium.

Prima di avviare una migrazione, tenere presente che comporterà tempo di inattività durante il quale le macchine virtuali non saranno disponibili per gli utenti.

> [!NOTE]
> Per informazioni dettagliate complete sulla migrazione da Archiviazione Standard ad Archiviazione Premium, vedere [Migrazione in Archiviazione Premium di Azure (dischi non gestiti)](https://docs.microsoft.com/azure/storage/common/storage-migration-to-premium-storage).

Quando si crea una macchina virtuale in Azure, è necessario trovare il giusto equilibrio tra costi e prestazioni. Archiviazione Premium è una buona scelta per le applicazioni a elevato utilizzo di dischi, ad esempio i database, perché offre una maggiore velocità di I/O rispetto ad Archiviazione Standard.
