Analogamente a qualsiasi altro computer, le macchine virtuali in Azure utilizzano i dischi come posizioni per archiviare un sistema operativo, le applicazioni e i dati. Questi dischi vengono chiamati i dischi rigidi virtuali (VHD).

Supponiamo di che avere creato una macchina virtuale (VM) in Azure che ospiterà il database di cronologie di case da cui dipende l'ufficio legale. Una configurazione del disco ben progettata è fondamentale per buone prestazioni e resilienza per SQL Server.

In questa unità, si apprenderà come scegliere i valori di configurazione corretto per i dischi e come collegare i dischi a una macchina virtuale.

## <a name="how-disks-are-used-by-vms"></a>Come i dischi vengono usati dalle macchine virtuali

Le macchine virtuali usano dischi per tre scopi diversi:

- **Archiviazione del sistema operativo**. Tutte le macchine Virtuali includono un disco che archivia il sistema operativo. Questa unità viene registrata come unità SATA ed etichettata come unità c: in Windows e montata in "/" nei sistemi operativi simili a Unix. Ha una capacità massima di 2048 gigabyte (GB) e il relativo contenuto è tratto dall'immagine di macchina virtuale usata per creare la macchina virtuale.
- **Archiviazione temporanea**. Tutte le macchine Virtuali includono un disco rigido virtuale temporaneo che viene usato per i file di pagina e lo scambio. Dati in questa unità possono essere persi durante un evento di manutenzione o la ridistribuzione. L'unità viene etichettato come unità d: in una macchina virtuale Windows per impostazione predefinita. Non utilizzare questo disco per archiviare dati importanti di cui non si desidera perdere.
- **Archiviazione dei dati**. Un disco dati è qualsiasi altro disco collegato a una macchina virtuale. Si usano i dischi dati per archiviare i file, database e altri dati che è necessario rendere persistenti i riavvii. Alcune immagini di macchina virtuale includono dischi dati per impostazione predefinita. È anche possibile aggiungere i propri dischi di dati fino al numero massimo specificato per la dimensione della macchina virtuale. Ogni disco dati viene registrato come unità SCSI e ha una capacità massima di 4095 GB. È possibile scegliere le lettere di unità o punti di montaggio per unità dati.

## <a name="storing-vhd-files"></a>L'archiviazione dei file di disco rigido virtuale

In Azure, i dischi rigidi virtuali vengono archiviati in un account di archiviazione di Azure come **BLOB di pagine**.

Questa tabella mostra i diversi tipi di account di archiviazione e gli oggetti che possono essere usati con ogni tipo.

|**Tipo di account di archiviazione**|**Standard per utilizzo generico**|**Utilizzo generico premium**|**Archiviazione BLOB, livelli ad accesso frequente e ad accesso sporadico**|
|-----|-----|-----|-----|
|**Servizi supportati**| Archiviazione code di Azure Blob di archiviazione, file di Azure, Azure | Archiviazione BLOB | Archiviazione BLOB|
|**Tipi di BLOB supportati**|BLOB in blocchi, BLOB di pagine e BLOB di aggiunta | BLOB di pagine | BLOB in blocchi e BLOB di aggiunta|

Entrambi i BLOB di pagine supporto di archiviazione per utilizzo generico standard e premium. Scegliere un account di archiviazione standard se il costo è un problema primari. Archiviazione Premium comporta un costo in più, ma garantirà anche molto maggiore i/o operazioni al secondo (IOPS). Se le prestazioni dei dati sono un requisito per la macchina virtuale, preferisce archiviazione premium.

## <a name="attach-data-disks-to-vms"></a>Collegare dischi dati per le macchine virtuali

È possibile aggiungere dischi dati a una macchina virtuale in qualsiasi momento da ricollegamento alla macchina virtuale. Per collegare un disco associa il file di disco rigido virtuale associato alla macchina virtuale. 

Il disco rigido virtuale non può essere eliminato dalla memoria mentre è collegato.

### <a name="attach-an-existing-data-disk-to-an-azure-vm"></a>Collegare un disco dati esistente a una VM di Azure

Si disponga già di un disco rigido virtuale che archivia i dati da usare nella macchina virtuale di Azure. In questo scenario ferma legge, ad esempio, magari aver già convertito i dischi fisici ai dischi rigidi virtuali in locale. In questo caso, è possibile usare il comando di PowerShell `Add-AzureRmVhd` cmdlet per caricarlo nell'account di archiviazione. Questo cmdlet è ottimizzato per il trasferimento dei file di disco rigido virtuale e può completare il caricamento più velocemente rispetto agli altri metodi. Il trasferimento viene completato utilizzando più thread per ottenere prestazioni ottimali. Dopo che è stato caricato il disco rigido virtuale, è necessario collegarlo a una VM esistente come disco dati. Questo approccio una modalità eccellente per distribuire i dati di tutti i tipi di macchine virtuali di Azure. I dati sono automaticamente presenti nella macchina virtuale e non è necessario per partizionare e formattare il nuovo disco.

### <a name="attach-a-new-data-disk-to-an-azure-vm"></a>Collegare un nuovo disco dati a una VM di Azure

È possibile usare il portale di Azure per aggiungere un disco dati nuovo e vuoto a una macchina virtuale. 

Questo processo verrà creato un **VHD** file come blob di pagine nell'account di archiviazione specificare e allegare il file con estensione vhd come disco dati alla macchina virtuale.

Prima di poter utilizzare il nuovo disco rigido virtuale per archiviare i dati, è necessario inizializzare, partizioni e formattare il nuovo disco. È possibile mettere in pratica questi passaggi nell'esercizio successivo.

In server fisici locali, i dati vengono archiviati su dischi rigidi fisici. Si archiviano dati in una macchina virtuale di Azure (VM) in dischi rigidi virtuali (VHD). Questi VHD sono archiviati come BLOB di pagine negli account di archiviazione di Azure. Ad esempio, quando si esegue la migrazione di database dell'azienda le forze di cronologie dei case in Azure, è necessario creare dischi rigidi virtuali in cui verranno salvati i file di database.
