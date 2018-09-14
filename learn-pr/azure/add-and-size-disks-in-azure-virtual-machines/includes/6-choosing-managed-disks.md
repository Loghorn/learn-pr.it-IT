Le applicazioni e servizi spesso necessario scalare con la richiesta. Per le macchine virtuali, ciò significa la scalabilità orizzontale o la creazione di istanze aggiuntive. Creazione di dischi per le nuove istanze manualmente possono rendere difficile per gli amministratori di Azure. In alternativa, è possibile usare **dischi gestiti** per ridurre le attività di amministrazione per creare nuovi dischi.

Per gestire dischi handle di archiviazione "dietro le quinte". Specificare il disco di tipo e le dimensioni del disco è necessario, e Azure crea e gestisce il disco per te. 

Quando si usa dischi gestiti, un altro tipo di disco è disponibile per l'utilizzo chiamato **SSD standard**.

## <a name="standard-ssds"></a>Unità SSD Standard

Si ricorderà, con dischi non gestiti, è possibile scegliere tra due tipi di disco: standard dischi rigidi (HDD) e le unità SSD premium (SSD).

Quando si usano dischi gestiti, è possibile scegliere un terzo tipo di disco: SSDs standard. Questo tipo di disco si trova tra HDD standard e premium SSD dal punto di vista delle prestazioni e costo.

È possibile usare unità SSD standard con qualsiasi dimensione di macchina virtuale, incluse le dimensioni di macchina virtuale che non supportano archiviazione premium. In effetti, avvalendomi di SSDs standard è l'unico modo per usare unità SSD con tali macchine virtuali.

## <a name="managed-disks-in-availability-sets"></a>Dischi gestiti nei set di disponibilità

In alcuni casi si usano diverse macchine virtuali per ospitare l'applicazione di gestire carichi maggiori.

È possibile inserire le macchine virtuali in un **set di disponibilità** per ridurre le probabilità di clienti che hanno rilevato un'interruzione del servizio. Usa un set di disponibilità, le macchine virtuali vengono distribuite tra domini di errore. Un dominio di errore è un gruppo logico di hardware sottostante che condivide una comune power e commutatori di rete, simile a un rack in un data center locale. Man mano che si creano le macchine virtuali all'interno di un set di disponibilità, la piattaforma Azure le distribuisce automaticamente in questi domini di errore. Questa separazione nel data center di Azure garantisce che non vi sia alcun singolo punto di errore per tutte le macchine virtuali nel set di disponibilità.

Quando si usano dischi gestiti, Azure esegue automaticamente il assicurarsi che ogni disco rigido virtuale viene posizionato nello stesso dominio di errore come la macchina virtuale. Questa garanzia rimuove la possibilità che la configurazione di account di archiviazione può includere un singolo punto di guasto. Non è possibile garantire la separazione quando si usano dischi non gestiti.

## <a name="choosing-managed-disks"></a>Scelta di dischi gestiti

Esistono alcuni aspetti che potrebbero influire sulla decisione per creare i dischi delle macchine virtuali di dischi come gestiti invece di dischi non gestiti.

**Si desidera controllare gli account di archiviazione nella sottoscrizione?**

I dischi gestiti sono facili da amministrare, ma a volte potrebbe sembrare che è possibile ottimizzare l'archiviazione o la gestione dei costi migliori per mantenere il controllo degli account di archiviazione.

**È necessario usare unità SSD standard?** 

Se si desidera usare unità SSD con una dimensione di VM che supportano archiviazione premium, è necessario scegliere i dischi gestiti.

**Sono le macchine virtuali in un set di disponibilità?** 

Si consiglia di usare i dischi gestiti per ottimizzare la resilienza delle macchine virtuali sono membri di un set di disponibilità.

## <a name="migrating-to-managed-disks"></a>Eseguire la migrazione a managed disks

È possibile convertire i dischi non gestiti a managed disks in qualsiasi momento usando uno di questi due metodi:

- Tramite il portale di Azure. Nella pagina delle impostazioni della macchina virtuale i dischi sono disponibili i **eseguire la migrazione a managed disks** dello strumento. Questo processo verrà arrestare la macchina virtuale, eseguire la migrazione dei dischi e quindi riavviare la macchina virtuale per l'utente. Si verifichi un'interruzione del servizio dalla macchina virtuale mentre la conversione viene eseguita.
- Uso di Azure PowerShell. È possibile usare il `ConvertTo-AzureRmVMManagedDisk` cmdlet per eseguire la migrazione. Se si sceglie questo metodo, è necessario arrestare e avviare manualmente la macchina virtuale.
  
> [!Note]
> Tenere presente che la migrazione da dischi non gestiti a managed disks non è reversibile. Inoltre, l'account di archiviazione originale usato dalla macchina virtuale non viene eliminato automaticamente. È possibile eliminare il BLOB VHD originali per evitare costi indesiderati. 

Managed disks ridurre il carico di lavoro amministrativo e sono disponibili alcune funzionalità, ad esempio unità SSD standard, che non sono disponibili per i dischi non gestiti.
