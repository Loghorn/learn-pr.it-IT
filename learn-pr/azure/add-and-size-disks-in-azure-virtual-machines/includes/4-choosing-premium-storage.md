Alcune applicazioni impegnative sull'archiviazione dei dati rispetto ad altri. Ad esempio, Microsoft Exchange Server e SharePoint Server necessari dischi ad alte prestazioni per l'esecuzione al meglio.

Rivediamo lo scenario di migrazione del database le cronologie di case per l'ufficio legale in Azure. Si desidera assicurarsi che i avvocati e altri collaboratori possano accedere dettagli del caso nel minor tempo. L'azienda cresce e si desidera che il database per gestire un aumento della domanda.

Uno è possibile ottimizzare le prestazioni delle macchine virtuali (VM) in Azure consiste nell'usare l'archiviazione premium per dischi rigidi virtuali (VHD). Archiviazione Premium offre più veloci input e output (i/o) rispetto all'archiviazione standard. I/o disco più veloce comportare prestazioni migliori per le applicazioni a elevato utilizzo di dischi.

È possibile confrontare i livelli di prestazioni per l'archiviazione e altre informazioni sugli account di archiviazione premium.

## <a name="how-premium-storage-differs-from-standard-storage"></a>Modo in cui archiviazione premium è diverso dall'archiviazione standard

In Azure, archiviazione premium è implementato in unità SSD (Solid State Drive). Unità SSD offrono prestazioni dei / o superiore rispetto a dischi rigidi (HDD) e può essere più affidabili perché non hanno componenti mobili. Per spostare la posizione corretta in un disco per trovare i dati richiesti non ha una testina di lettura o scrittura. 

Archiviazione standard è implementata nelle unità disco rigido. Archiviazione standard un fatturato con una frequenza inferiore. Scegliere l'archiviazione standard per controllare i costi e scegliere l'archiviazione premium quando sono richieste prestazioni i/o veloce.

È anche possibile usare una combinazione di spazio di archiviazione standard e premium per dischi in una singola macchina virtuale.

> [!NOTE]
> Se si usa dischi gestiti, hanno una terza opzione: SSDs standard. Unità SSD standard sono intermedio di prezzi tra premium unità SSD e HDD standard sia le prestazioni ma non sono disponibili per i dischi non gestiti. Verranno fornite ulteriori informazioni su unità SSD standard più avanti in questo modulo.

## <a name="premium-storage-accounts"></a>Account di archiviazione premium

I VHD sono archiviati negli account di archiviazione per utilizzo generico di Azure come BLOB di pagine. Per usare archiviazione premium per i dischi rigidi virtuali, è necessario archiviare i dischi rigidi virtuali in un **account di archiviazione premium**. Si sceglie il livello di prestazioni di un account di archiviazione quando si crea e non è possibile modificare questa impostazione in un secondo momento.

> [!IMPORTANT]
> Archiviazione Premium potrebbe non essere disponibile in tutte le aree di Azure. Per verificare la disponibilità nell'area dell'utente, vedere [prodotti disponibili in base all'area](https://azure.microsoft.com/en-us/global-infrastructure/services/).

### <a name="data-replication-and-premium-storage-accounts"></a>Account di archiviazione premium e la replica dei dati

I dati nell'account di archiviazione di Microsoft Azure vengono sempre replicati per assicurarne la durabilità e la disponibilità elevata. Azure copie di replica di archiviazione dei dati in modo che siano protetti da pianificati e gli eventi non pianificati, ad esempio errori hardware temporanei, rete o interruzioni dell'alimentazione, forti calamità naturali e così via. È possibile scegliere di replicare i dati nello stesso data center, tra data center di zona nella stessa area e persino tra aree. Esistono quattro tipi di replica:

- **Archiviazione con ridondanza locale (LRS)** -Azure replica i dati all'interno di stesso data center di Azure. I dati rimangono disponibili se un nodo ha esito negativo. Tuttavia, se un intero data center non riesce, i dati siano disponibili.
- **Archiviazione con ridondanza geografica (GRS)** -Azure replica i dati in un'area secondaria a centinaia di miglia dall'area primaria. Se l'account di archiviazione sono archiviazione con ridondanza geografica abilitata, quindi i dati sono assicurati anche se è presente un'interruzione completa a livello di area o un'emergenza in cui l'area primaria non è recuperabile.
- **Archiviazione con ridondanza geografica e accesso in lettura (RA-GRS)** -Azure offre accesso in lettura ai dati nella posizione secondaria e la replica geografica tra due aree. Se un data center non riesce, i dati rimangono leggibili ma non possono essere modificati.
- **Archiviazione con ridondanza della zona (ZRS)** -Azure replica i dati in modo sincrono in tre cluster di archiviazione in una singola area. Ogni cluster di archiviazione è separato fisicamente dagli altri e si trova nella propria zona di disponibilità (AZ). Con questo tipo di replica, è possibile comunque accedere e gestire i dati nel caso in cui una zona non è più disponibile.

Gli account di archiviazione standard supportano tutti i tipi di replica, ma gli account di archiviazione premium supportano solo l'archiviazione con ridondanza locale (LRS). Poiché macchine virtuali eseguite in una singola area geografica, questa restrizione non è in genere un problema per l'archiviazione dei dischi rigidi Virtuali.

## <a name="migrating-from-standard-storage-to-premium-storage"></a>Migrazione dall'archiviazione standard ad archiviazione premium

È consigliabile creare in primo luogo i dischi rigidi virtuali nel tipo corretto di account di archiviazione. Tuttavia, a volte requisiti cambiano, le richieste aumentano o si può notare che si è scelto il tipo non corretto. Prendere in considerazione un'operazione di spostamento nell'archiviazione premium in queste situazioni.

Prima di avviare una migrazione, prendere in considerazione che comporta un tempo di inattività durante i quali sarà disponibile per gli utenti della macchina virtuale.

> [!NOTE]
> Per informazioni dettagliate della migrazione da standard ad archiviazione premium, vedere [la migrazione ad archiviazione Premium di Azure (dischi non gestiti)](https://docs.microsoft.com/azure/storage/common/storage-migration-to-premium-storage).

Quando si crea una macchina virtuale in Azure, è necessario trovare il giusto equilibrio tra costi e prestazioni. Archiviazione Premium è una buona scelta per le applicazioni a elevato utilizzo di dischi, ad esempio database, perché supporta IO più veloce rispetto all'archiviazione standard.
