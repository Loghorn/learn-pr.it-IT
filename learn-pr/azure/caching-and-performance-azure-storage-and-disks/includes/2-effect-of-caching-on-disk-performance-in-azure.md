Dobbiamo esaminare i fattori che influenzano le prestazioni del disco in Azure, e su come la memorizzazione nella cache per ottimizzare le prestazioni.

### <a name="disk-caching"></a>La cache del disco

Una cache è un componente specifico in un computer che archivia i dati in modo che sia accessibile più rapidamente, in genere in memoria. I dati in una cache sono spesso i dati sono stati letti in precedenza o dati ottenuti da un calcolo del precedente. L'obiettivo è accedere ai dati più velocemente rispetto a ottenerlo dal disco.

La memorizzazione nella cache utilizza specializzato e talvolta costosa archiviazione temporanea che è più veloce di lettura e/o scrittura delle prestazioni rispetto a un archivio permanente. Poiché questo tipo di archiviazione della cache è spesso limitato, le decisioni necessario decidere quali operazioni sui dati verranno ottenere il massimo vantaggio dalla memorizzazione nella cache. Ma anche in cui la cache può essere resi ampiamente disponibile, ad esempio in Azure, è comunque importante conoscere i modelli di carico di lavoro di ogni disco prima di decidere in quale tipo di memorizzazione nella cache da usare.

**Caching di lettura** tenta di velocizzare il recupero dei dati. Invece di lettura da un archivio permanente, i dati vengono letti dalla cache più veloce. Letture dati trovate nella cache nelle condizioni seguenti:

- I dati sono state lette prima e presente nella cache.
- E la cache è sufficientemente grande da contenere tutti i dati.

È importante notare che la cache di lettura interviene nel caso alcune _prevedibilità_ alla coda di lettura, ad esempio un set di letture sequenziali. Per i/o casuali, in cui si accede a dati sono sparsi tra archiviazione, la memorizzazione nella cache sarà di non offre alcun vantaggio e possono anche ridurre le prestazioni del disco.

**La cache in scrittura** tenta di accelerare la scrittura dei dati all'archiviazione. Usando una cache in scrittura, l'app può prendere in considerazione i salvataggio dei dati. In realtà, i dati sono in coda in una cache, in attesa di essere scritti in un archivio permanente. Come è facile immaginare, questo meccanismo può essere un potenziale punto di guasto, come se il sistema è in esecuzione viene scaricato verso il basso prima che questo dati memorizzati nella cache su disco. Alcuni sistemi, ad esempio SQL Server, gestiscono la scrittura di dati memorizzati nella cache per l'archiviazione su disco persistente autonomamente.

### <a name="azure-disk-caching"></a>La memorizzazione nella cache di dischi di Azure

Esistono due tipi di disco di archiviazione su disco tale problema di memorizzazione nella cache:

- La memorizzazione nella cache di archiviazione di Azure
- La cache del disco di macchina virtuale di Azure (VM)

La memorizzazione nella cache di archiviazione di Azure fornisce servizi di cache per l'archiviazione Blob di Azure, file di Azure e altri contenuti in Azure. Configurazione di questi tipi di cache esula dall'ambito di questo modulo.

La cache del disco di macchina virtuale di Azure è come ottimizzare l'accesso in lettura e scrittura ai file di disco rigido virtuale (VHD) collegati alle macchine virtuali di Azure. Ci concentreremo sul caching su disco in questo modulo.

### <a name="azure-virtual-machine-disk-types"></a>Tipi di disco di macchina virtuale di Azure

Esistono tre tipi di dischi usati con macchine virtuali di Azure:

- **Disco del sistema operativo**: quando si crea una VM di Azure, Azure associa automaticamente un disco rigido virtuale per il sistema operativo (OS). Il disco rigido virtuale verrà archiviato come blob di pagine in archiviazione di Azure.
- **Disco temporaneo**: quando si crea una VM di Azure, Azure aggiunge automaticamente un disco temporaneo. Questo disco viene usato per i dati, ad esempio i file di pagina e lo scambio. I dati sul disco vadano persi durante la manutenzione o ridistribuire una macchina virtuale. Pertanto, non usarlo per l'archiviazione permanente dei dati, ad esempio i file di database o log delle transazioni.
- **I dischi dati**: un disco dati è un disco rigido virtuale collegato a una macchina virtuale per archiviare i dati dell'applicazione o altri dati da mantenere.

Dischi del sistema operativo e i dischi dati sfruttano i vantaggi della memorizzazione nella cache del disco di macchina virtuale di Azure. Le dimensioni della cache per un disco di macchina virtuale dipende la dimensione di istanza di macchina virtuale e sul numero di dischi montati nella macchina virtuale. La memorizzazione nella cache può essere abilitata per i dischi dati solo fino a quattro.

## <a name="cache-options-for-azure-vms"></a>Opzioni della cache per macchine virtuali di Azure

Sono disponibili tre opzioni comuni per la cache del disco della macchina virtuale:

- **Lettura/scrittura** – cache Write-back. Usare questa opzione solo se l'applicazione gestisce correttamente la scrittura dei dati memorizzati nella cache in dischi persistenti, quando necessario.
- **Sola lettura** -operazioni di lettura vengono eseguite dalla cache.
- **Nessuno** -Nessuna cache. Selezionare questa opzione per i dischi in sola lettura e scrittura con intensa attività. File di log sono un candidato valido dal momento che le operazioni di scrittura pesanti.

Non tutte le opzioni di memorizzazione nella cache sono disponibile per ogni tipo di disco. Nella tabella seguente illustra le opzioni di memorizzazione nella cache per ogni tipo di disco:

| |**sola lettura**  |**Lettura/scrittura**  |**Nessuno**  |
|---------|---------|---------|---------|
|Disco del sistema operativo     |   Sì      |   Sì (impostazione predefinita)     |   Sì      |
|Disco dati     |   Sì (impostazione predefinita)      |  Sì       |  Sì       |
|Disco temporaneo     |  no       |   no      |   no      |

> [!NOTE]
> Impossibile modificare le opzioni di memorizzazione nella cache di disco per L-Series e macchine virtuali serie B.

## <a name="performance-considerations-for-azure-vm-disk-caching"></a>Considerazioni sulle prestazioni per la cache del disco di macchina virtuale di Azure

Pertanto, come le impostazioni della cache possono influenzare le prestazioni dei carichi di lavoro in esecuzione in macchine virtuali di Azure?

### <a name="os-disk"></a>Disco del sistema operativo

Per un disco del sistema operativo della macchina virtuale, il comportamento predefinito consiste nell'utilizzare la cache in modalità lettura/scrittura. Se si hanno applicazioni che archiviano file di dati sul disco del sistema operativo e le applicazioni consentono di eseguire numerose operazioni di lettura/scrittura casuale ai file di dati, si consiglia di spostare tali file in un disco dati con la memorizzazione nella cache disattivata. Perché? Anche se la coda di lettura non contiene letture sequenziali, la memorizzazione nella cache sarà di non offre alcun vantaggio. Il sovraccarico della gestione della cache, come se i dati non sequenziali, può ridurre le prestazioni del disco.

### <a name="data-disks"></a>Dischi dati

Per le applicazioni sensibili alle prestazioni, è consigliabile usare dischi dati anziché il disco del sistema operativo. Utilizzo dischi separati consente di configurare le impostazioni della cache appropriate per ogni disco.

Ad esempio, nelle macchine virtuali di Azure che esegue SQL Server, l'abilitazione **Read-only** miglioramenti significativi delle prestazioni può causare la memorizzazione nella cache sui dischi dati (per normali e di dati TempDB). File di log, d'altra parte, sono buoni candidati per i dischi dati con nessuna memorizzazione nella cache.

> [!WARNING]
> La modifica l'impostazione della cache di un disco di Azure, scollega e quindi ricollega il disco di destinazione. Se il disco del sistema operativo, la VM viene riavviata. Arrestare tutte le applicazioni/i servizi che potrebbero essere interessati da questa interruzione prima di modificare l'impostazione della cache del disco.
