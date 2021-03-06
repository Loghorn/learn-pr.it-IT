Sono state analizzate le impostazioni e le proprietà selezionabili per prevedere le prestazioni del disco. Ora si analizzeranno le modalità per migliorare tali prestazioni mediante la _memorizzazione nella cache_.

## <a name="disk-caching"></a>Memorizzazione nella cache del disco

Una cache è un componente specializzato che archivia i dati, in genere nella memoria, in modo che siano accessibili più rapidamente. I dati in una cache sono spesso dati letti in precedenza o dati ottenuti da un calcolo precedente. L'obiettivo è accedere ai dati più velocemente rispetto a quando si recuperano dal disco.

La memorizzazione nella cache usa un'archiviazione temporanea specializzata e talvolta dispendiosa, che offre prestazioni di lettura e scrittura più veloci rispetto all'archiviazione permanente. Poiché questo tipo di archiviazione nella cache è spesso limitato, è opportuno decidere quali operazioni sui dati traggono maggiori vantaggi dalla memorizzazione nella cache. Anche nei casi in cui la cache può essere resa ampiamente disponibile, ad esempio in Azure, è comunque importante conoscere i modelli di carico di lavoro di ogni disco prima di decidere quale tipo di memorizzazione nella cache usare.

La **memorizzazione nella cache di lettura** prova a velocizzare il _recupero_ dei dati. Invece che da un archivio permanente, i dati vengono letti dalla cache più veloce. Le letture dei dati raggiungono la cache alle condizioni seguenti:

- I dati sono stati letti in precedenza ed esistono nella cache.
- La cache è sufficientemente grande da contenere tutti i dati.

È importante notare che la memorizzazione nella cache di lettura è utile in caso di _prevedibilità_ della coda di lettura, ad esempio nel caso di una serie di letture sequenziali. Per operazioni di I/O casuali, nel caso in cui i dati a cui si accede siano sparsi tra più risorse di archiviazione, la memorizzazione nella cache offre un vantaggio minimo se non nullo e può addirittura ridurre le prestazioni del disco.

La **memorizzazione nella cache di scrittura** prova ad accelerare la _scrittura dei dati_ nella risorsa di archiviazione. Usando una cache in scrittura, l'app può prendere in considerazione i dati da salvare. In realtà i dati vengono accodati in una cache, in attesa di essere scritti su disco. Come è facile immaginare, questo meccanismo può rappresentare un potenziale punto di guasto, ad esempio se il sistema viene arrestato prima che i dati memorizzati nella cache vengano scritti. Alcuni sistemi, come SQL Server, gestiscono la scrittura dei dati memorizzati nella cache nell'archivio su disco persistente in modo autonomo.

### <a name="azure-disk-caching"></a>Memorizzazione nella cache del disco di Azure

Esistono due tipi di memorizzazione nella cache del disco che interessano l'archiviazione su disco:

- Memorizzazione nella cache di Archiviazione di Azure
- Memorizzazione nella cache del disco della macchina virtuale di Azure (VM)

La memorizzazione nella cache di Archiviazione di Azure fornisce servizi cache per Archiviazione BLOB di Azure, File di Azure e altri contenuti in Azure. La configurazione di questi tipi di cache non rientra nell'ambito di questo modulo.

La memorizzazione nella cache del disco della macchina virtuale di Azure riguarda l'ottimizzazione dell'accesso in lettura e scrittura ai file del disco rigido virtuale collegati alle macchine virtuali di Azure. In questo modulo verrà presa in considerazione la memorizzazione nella cache del disco.

### <a name="azure-virtual-machine-disk-types"></a>Tipi di dischi della macchina virtuale di Azure

Esistono tre tipi di dischi che vengono usati con le macchine virtuali di Azure:

- **Disco del sistema operativo**: quando si crea una macchina virtuale di Azure, Azure collega automaticamente un disco rigido virtuale per il sistema operativo.

- **Disco temporaneo**: quando si crea una macchina virtuale di Azure, Azure aggiunge automaticamente anche un disco temporaneo. Questo disco viene usato per dati come i file di paging e di scambio. I dati in questo disco potrebbero andare perduti durante la manutenzione o la ridistribuzione di una macchina virtuale. Pertanto evitare di usarlo per l'archiviazione di dati permanenti, come file di database o log delle transazioni.

- **Dischi dati**: un disco dati è un disco rigido virtuale collegato a una macchina virtuale che archivia i dati delle applicazioni o altri dati che è necessario mantenere.

I dischi del sistema operativo e i dischi dati sfruttano i vantaggi della memorizzazione nella cache del disco della macchina virtuale di Azure. Le dimensioni della cache per un disco della macchina virtuale dipendono dalle dimensioni dell'istanza della macchina virtuale e dal numero di dischi montati nella macchina virtuale. La memorizzazione nella cache può essere abilitata per un massimo di quattro dischi dati.

## <a name="cache-options-for-azure-vms"></a>Opzioni della cache per le macchine virtuali di Azure

Sono disponibili tre opzioni comuni per la memorizzazione nella cache del disco della macchina virtuale.

- **Lettura/scrittura**: cache writeback. Usare questa opzione solo se l'applicazione gestisce correttamente la scrittura di dati memorizzati nella cache in dischi persistenti, quando necessario.
- **Sola lettura**: le operazioni di lettura vengono eseguite dalla cache.
- **Nessuna**: nessuna cache. Selezionare questa opzione per dischi in sola scrittura e dischi con attività di scrittura intensive. I file di log sono un candidato ideale perché sono operazioni con attività di scrittura intensive.

Non tutte le opzioni di memorizzazione nella cache sono disponibili per ogni tipo di disco. La tabella seguente illustra le opzioni di memorizzazione nella cache per ogni tipo di disco:

|               | **Sola lettura**  | **Lettura/scrittura** | **Nessuna** |
|---------------|----------------|----------------|----------|
| Disco del sistema operativo       | Sì            | Sì (impostazione predefinita)  | Sì      |
| Disco dati     | Sì (impostazione predefinita)  | Sì            | Sì      |
| Disco temporaneo     | No             | No             | No       |

> [!NOTE]
> Le opzioni di memorizzazione nella cache del disco non possono essere modificate per le macchine virtuali **serie L** e **serie B**.

## <a name="performance-considerations-for-azure-vm-disk-caching"></a>Considerazioni sulle prestazioni per la memorizzazione nella cache del disco della macchina virtuale di Azure

In che modo le impostazioni della cache influiscono sulle prestazioni dei carichi di lavoro in esecuzione nelle macchine virtuali di Azure?

### <a name="os-disk"></a>Disco del sistema operativo

Per un disco del sistema operativo della macchina virtuale, il comportamento predefinito consiste nell'usare la cache in modalità lettura/scrittura. Se si hanno applicazioni che archiviano file di dati nel disco del sistema operativo e le applicazioni eseguono numerose operazioni di lettura/scrittura casuali sui file di dati, è consigliabile spostare tali file su un disco dati in cui la memorizzazione nella cache è disattivata. Perché? Se la coda di lettura non contiene letture sequenziali, la memorizzazione nella cache offrirà un vantaggio minimo se non nullo. Il sovraccarico rappresentato dalla gestione della cache, come se i dati fossero sequenziali, può ridurre le prestazioni del disco.

### <a name="data-disks"></a>Dischi dati

Per applicazioni sensibili alle prestazioni è consigliabile usare dischi dati anziché il disco del sistema operativo. L'uso di dischi separati consente di configurare le impostazioni della cache appropriate per ogni disco.

Ad esempio, nelle macchine virtuali di Azure che eseguono SQL Server l'abilitazione della memorizzazione nella cache di **Sola lettura** nei dischi dati (per dati normali e TempDB) può comportare miglioramenti significativi delle prestazioni. I file di log, d'altra parte, sono candidati ideali per i dischi dati senza memorizzazione nella cache.

> [!WARNING]
> La modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione. Se si tratta del disco del sistema operativo, la macchina virtuale viene riavviata. Arrestare tutte le applicazioni e i servizi che potrebbero essere interessati da questa interruzione prima di modificare l'impostazione della cache del disco.

È possibile configurare le impostazioni della cache del disco della macchina virtuale con uno qualsiasi degli strumenti seguenti:

- Portale di Azure
- Interfaccia della riga di comando di Azure
- Azure PowerShell
- Modelli di Gestione risorse

## <a name="using-the-azure-portal-to-configure-caching"></a>Uso del portale di Azure per configurare la memorizzazione nella cache

Quando si effettua il provisioning di una nuova macchina virtuale tramite il portale di Azure, non è possibile modificare la configurazione di memorizzazione nella cache predefinita per il disco del sistema operativo dalle operazioni di lettura/scrittura finché la macchina virtuale non viene distribuita.

Quando si aggiunge un disco dati a una macchina virtuale esistente, è possibile configurare l'opzione di memorizzazione nella cache prima che il disco venga distribuito alla macchina virtuale.

La modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione. Se si tratta del disco del sistema operativo, la macchina virtuale viene riavviata. Arrestare tutte le applicazioni e i servizi che potrebbero essere interessati da questa interruzione prima di modificare l'impostazione della cache del disco.

Verrà ora creata una macchina virtuale e si modificheranno le impostazioni della cache tramite il portale di Azure.
