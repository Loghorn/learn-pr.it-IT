Apportare modifiche alla configurazione del server è un'operazione eseguita comunemente con dispositivi nell'ambiente locale. In questo senso, è possibile considerare le macchine virtuali di Azure un'estensione di tale ambiente. È possibile modificare la configurazione, gestire le reti, open o bloccare il traffico e così via, tramite il portale di Azure, il comando di Azure o strumenti di Azure PowerShell.

A questo punto il server è in esecuzione, Apache è installato e serve le pagine. Il team di sicurezza impone che abbiamo bloccare tutti i nostri server e non abbiamo alcuna operazione per questa macchina virtuale ancora. Per questo, Apache è in ascolto sulla porta 80. Si esaminerà ora la configurazione di rete di Azure per scoprire come usare il supporto della sicurezza predefinito per migliorare la protezione del server.

## <a name="opening-ports-in-azure-vms"></a>Apertura delle porte nelle macchine virtuali di Azure

Per impostazione predefinita, le nuove macchine virtuali siano bloccate. 

Le app possono effettuare richieste in uscita, ma solo il traffico in ingresso consentito dalla rete virtuale (ad esempio, altre risorse nella stessa rete locale) e da Azure Load Balancer (controlli di probe).

Esistono due passaggi per modificare la configurazione per supportare protocolli diversi nella rete. Quando si crea una nuova macchina virtuale, è possibile aprire alcune porte comuni (RDP, HTTP, HTTPS e SSH). Tuttavia, se sono necessarie altre modifiche al firewall, sarà necessario intervenire manualmente.

La procedura prevede due passaggi:

1. Creare un gruppo di sicurezza di rete.
2. Creare una regola in ingresso che consenta il traffico sulle porte necessarie.

### <a name="what-is-a-network-security-group"></a>Che cos'è un gruppo di sicurezza di rete?

Le reti virtuali sono alla base del modello di rete di Azure e offrono isolamento e protezione. Gruppi di sicurezza di rete (Nsg) sono lo strumento principale che consente di applicare e controllare le regole del traffico di rete a livello di rete. I gruppi di sicurezza di rete sono un livello di sicurezza facoltativo che fornisce un firewall software mediante il filtraggio del traffico in ingresso e in uscita sulla rete virtuale. 

I gruppi di sicurezza possono essere associati a un'interfaccia di rete (per in base alle regole di host), una subnet nella rete virtuale (da applicare a più risorse), o entrambi i livelli. 

#### <a name="security-group-rules"></a>Regole dei gruppi di sicurezza

I gruppi di sicurezza di rete usano _regole_ per consentire o negare il transito del traffico nella rete. Ogni regola identifica l'indirizzo di origine e destinazione (o intervallo), il protocollo, la porta (o intervallo), la direzione (in ingresso o in uscita), una priorità numerica e se consentire o negare il traffico che corrisponde alla regola.

![Illustrazione che mostra l'architettura dei gruppi di sicurezza di rete in due diverse subnet. In una subnet sono presenti due macchine virtuali, ognuna con le proprie regole di interfaccia di rete.  La subnet stessa include un set di regole applicabile a entrambe le macchine virtuali. ](../media/7-nsg-rules.png)

Ogni gruppo di sicurezza ha un set di regole di sicurezza predefinite per applicare le regole di rete predefinite descritte in precedenza. Queste regole predefinite non possono essere modificate, ma _possibile_ essere sottoposto a override.

#### <a name="how-azure-uses-network-rules"></a>Uso delle regole di rete in Azure

Per il traffico in ingresso, i processi di Azure il gruppo di sicurezza associato alla subnet e quindi il gruppo di sicurezza applicato all'interfaccia di rete. Il traffico in uscita viene gestito nell'ordine inverso, vale a dire prima l'interfaccia di rete seguita dalla subnet.

> [!WARNING]  
> Tenere presente che i gruppi di sicurezza sono facoltativi a entrambi i livelli. Se non viene applicato alcun gruppo di sicurezza, quindi **tutto il traffico è consentito** da Azure. Se la VM ha un indirizzo IP pubblico, potrebbe trattarsi di un grave rischio, in particolare se il sistema operativo non fornisce un firewall incorporato.

Le regole vengono valutate nel _ordine di priorità_, che inizia con la **priorità più bassa** regola. Le regole di negazione **arrestano** sempre la valutazione. Ad esempio, se una regola di interfaccia di rete blocca una richiesta in uscita, tutte le regole applicate alla subnet non verranno controllate. Il traffico attraverso il gruppo di sicurezza viene consentito se passa attraverso _tutti_ i gruppi applicati.

L'ultima regola è sempre una regola **Nega tutto**. Si tratta di una regola predefinita aggiunta a ogni gruppo di sicurezza per il traffico in ingresso e in uscita con una priorità di 65500. È quindi opportuno che il traffico passa attraverso il gruppo di sicurezza _è necessario disporre di una regola di assenso_, o la regola finale predefinita lo blocca.

> [!NOTE]  
> SMTP (porta 25) è un caso speciale. A seconda del livello di abbonamento e quando l'account è stato creato, il traffico SMTP in uscita può essere bloccato. È possibile richiedere di rimuovere questa restrizione con una giustificazione aziendale.

Dato che non è stato creato un gruppo di sicurezza per questa macchina virtuale, è possibile farlo e applicarlo.

## <a name="creating-network-security-groups"></a>Creazione di gruppi di sicurezza di rete

I gruppi di sicurezza sono risorse gestite, come la maggior parte degli elementi in Azure. È possibile crearli nel portale di Azure o tramite gli strumenti di scripting da riga di comando. L'aspetto più complicato è la definizione delle regole. Si vedrà come definire una nuova regola per consentire l'accesso HTTP e bloccare tutto il resto.