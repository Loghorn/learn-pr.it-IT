Il successo di una società di servizi spesso dipende direttamente dai contratti di servizio che la società stabilisce con i clienti. I clienti si aspettano che i servizi forniti siano sempre disponibili e che i dati siano protetti. Microsoft prende molto seriamente questo aspetto. Azure fornisce strumenti che è possibile usare per gestire disponibilità, sicurezza dei dati e monitoraggio ed essere quindi certi che i servizi siano sempre disponibili per i clienti.

L'amministrazione di una VM di Azure non si limita alla gestione del sistema operativo o del software in esecuzione nella VM. Consente di sapere quali sono i servizi forniti da Azure, assicurando la disponibilità dei servizi e supportando l'automazione. Questi servizi consentono di pianificare la strategia di continuità aziendale e ripristino di emergenza dell'organizzazione.

Verrà qui illustrato un servizio di Azure che consente di migliorare la disponibilità della VM, semplifica le attività di gestione della VM e conserva i dati della VM di cui è stato eseguire un backup, tenendoli al sicuro. Si inizierà definendo la disponibilità.

## <a name="what-is-availability"></a>Che cos'è la disponibilità?

La disponibilità è la percentuale di tempo in cui un servizio è disponibile per l'uso.

Si supponga di avere un sito Web e di voler fare in modo che i clienti possano accedere alle informazioni in qualsiasi momento. Per l'accesso al sito Web è prevista una disponibilità del 100%.

### <a name="why-do-i-need-to-think-about-availability-when-using-azure"></a>Perché preoccuparsi della disponibilità quando si usa Azure?

Le VM di Azure vengono eseguite in server fisici ospitati nel data center di Azure. Come per la maggior parte dei dispositivi fisici, potrebbe verificarsi un guasto. Se si verifica un guasto nel server fisico, si verifica un errore anche nelle macchine virtuali ospitate in tale server. In questo caso Azure sposterà automaticamente la VM in un server host integro. Questa migrazione automatica potrebbe tuttavia richiedere alcuni minuti, durante i quali le applicazioni ospitate in tale VM non saranno disponibili.

Le VM potrebbero anche essere interessate da aggiornamenti periodici avviati da Azure. Questi eventi di manutenzione comprendono aggiornamenti software e hardware e sono necessari per migliorare l'affidabilità e le prestazioni della piattaforma. Questi eventi vengono in genere eseguiti senza conseguenze per le VM guest, ma in alcuni casi le macchine virtuali verranno riavviate per completare un aggiornamento.

> [!NOTE]
> Microsoft non aggiorna automaticamente il sistema operativo o il software delle VM. È l'utente ad avere il controllo completo e la responsabilità di questa operazione. All'host del software e all'hardware sottostanti, tuttavia, vengono periodicamente applicate patch per garantire affidabilità e prestazioni elevate in qualsiasi momento.

Per assicurarsi che i servizi non vengano interrotti ed evitare un singolo punto di guasto, è consigliabile distribuire almeno due istanze di ogni VM. Questa funzionalità è chiamata _set di disponibilità_.

### <a name="what-is-an-availability-set"></a>Che cos'è un set di disponibilità?

Un **set di disponibilità** è una funzionalità logica usata per assicurarsi che venga distribuito un gruppo di VM correlate in modo che non siano tutte soggette a un singolo punto di guasto e non vengano tutte aggiornate contemporaneamente durante un aggiornamento del sistema operativo host nel data center. Le VM inserite in un set di disponibilità eseguiranno un set identico di funzionalità e avranno lo stesso software installato.

> [!TIP]
> Microsoft offre un contratto di servizio di connettività esterna al 99,95% per le VM a più istanze distribuite in un set di disponibilità. Perché il contratto di servizio sia valido, devono quindi essere distribuite almeno due istanze della VM in un set di disponibilità. 

È possibile creare i set di disponibilità nella sezione del portale di Azure relativa al ripristino di emergenza. È anche possibile crearli usando i modelli di Resource Manager oppure uno strumento di scripting o API. Quando si inseriscono le VM in un set di disponibilità, Azure garantisce che vengano distribuite tra **domini di errore** e **domini di aggiornamento**.

#### <a name="what-is-a-fault-domain"></a>Che cos'è un dominio di errore?

Un dominio di errore è un gruppo logico di hardware in Azure che condividono un'unità di alimentazione o un commutatore di rete comune. Può essere considerato come un rack in un data center locale. Il provisioning delle prime due VM di un set di disponibilità verrà effettuato in due diversi rack in modo che, se dovesse verificarsi un guasto a livello di rete o di alimentazione in un rack, ne sarebbe interessata solo una VM. I domini di errore vengono definiti anche per i dischi gestiti collegati alle VM.

![Un'illustrazione che mostra due domini di errore ciascuno con due macchine virtuali. Le due macchine virtuali superiori di ciascun dominio di errore ospitano servizi Internet di informazione e fanno parte di un set di disponibilità comune. Le due macchine virtuali successive in dominio ospitano database SQL e fanno parte di un altro set di disponibilità.](../media/5-fault-domains.png)

#### <a name="what-is-an-update-domain"></a>Che cos'è un dominio di aggiornamento?

Un dominio di aggiornamento è un gruppo logico di hardware che può essere sottoposto a manutenzione oppure riavviato nello stesso momento. Azure inserirà automaticamente i set di disponibilità nei domini di aggiornamento per ridurre al minimo l'impatto quando la piattaforma Azure introduce modifiche nel sistema operativo host. Azure elabora quindi ogni dominio di aggiornamento singolarmente.

I set di disponibilità sono una funzionalità avanzata che assicura che i servizi in esecuzione nelle VM siano sempre disponibili per i clienti. Non sono tuttavia infallibili. Cosa accade se si verifica un problema relativo ai dati o al software in esecuzione nella VM? In questo caso sarà necessario ricorrere ad altre tecniche di ripristino di emergenza e backup.

## <a name="failover-across-locations"></a>Failover in diverse località

È anche possibile replicare l'infrastruttura tra siti per gestire il failover a livello di area. **Azure Site Recovery** replica i carichi di lavoro da un sito primario a una località secondaria. Se si verifica un'interruzione nel sito primario, è possibile effettuare il failover in una località secondaria. Questo failover consente agli utenti di continuare ad accedere alle applicazioni senza interruzioni. Sarà quindi possibile effettuare il failback nella località primaria quando sarà di nuovo operativa. Azure Site Recovery consente la replica delle macchine virtuali o dei computer fisici, garantendo la disponibilità dei carichi di lavoro in caso di interruzione.

Oltre alle numerose utili funzionalità di Site Recovery, sono almeno due i vantaggi importanti per le aziende:

1. Site Recovery consente l'uso di Azure come destinazione per il ripristino, eliminando i costi e la complessità derivanti dalla gestione di un data center fisico secondario.

2. Site Recovery semplifica considerevolmente il test dei failover con esercitazioni sul ripristino senza alcun impatto sugli ambienti di produzione. In questo modo risulta più facile testare i failover pianificati o non pianificati. Non si può infatti considerare valido un piano di ripristino di emergenza se non si è mai provato a effettuare il failover.

I piani di ripristino creati con Site Recovery possono essere semplici o complessi a seconda dello scenario. Possono includere script di PowerShell personalizzati, runbook di Automazione di Azure o procedure di intervento manuale. È possibile sfruttare i piani di ripristino per replicare i carichi di lavoro in Azure, creando facilmente nuove opportunità per migrazione, burst temporanei durante i periodi di sovratensione o sviluppo e test di nuove applicazioni.

Azure Site Recovery usa le risorse di Azure o i server Hyper-V, VMware e fisici nell'infrastruttura locale e può diventare un elemento chiave della strategia di continuità aziendale e ripristino di emergenza (BCDR) dell'organizzazione orchestrando la replica, il failover e il ripristino di carichi di lavoro e applicazioni se si verifica un guasto nella località primaria.
