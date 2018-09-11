Abbiamo installato software personalizzato, configurato un server FTP e configurato la macchina virtuale per la ricezione di file video. Tuttavia, se si prova a connettersi all'indirizzo IP pubblico con il protocollo FTP, si scoprirà che è bloccato. 

Apportare modifiche alla configurazione del server è un'operazione eseguita comunemente con dispositivi nell'ambiente locale. In questo senso, è possibile considerare le macchine virtuali di Azure un'estensione di tale ambiente. È possibile modificare la configurazione, gestire le reti, aprire o bloccare il traffico e così via, tramite il portale di Azure, l'interfaccia della riga di comando di Azure o strumenti di Azure PowerShell.

Abbiamo già visto alcune delle informazioni base e opzioni di gestione nel pannello **Panoramica** della macchina virtuale. Ora si approfondirà la configurazione di rete.

## <a name="opening-ports-in-azure-vms"></a>Apertura delle porte nelle macchine virtuali di Azure

<!-- TODO: Azure portal is inconsistent here in applying the NSG.
By default, new VMs are locked down. 

Apps can make outgoing requests, but the only inbound traffic allowed is from the virtual network (e.g. other resources on the same local network), and from Azure's Load Balancer (probe checks). -->

Esistono due passaggi per modificare la configurazione per supportare FTP. Quando si crea una nuova macchina virtuale, è possibile aprire alcune porte comuni (RDP, HTTP, HTTPS e SSH). Tuttavia, se sono necessarie altre modifiche al firewall, sarà necessario intervenire personalmente.

La procedura prevede due passaggi:

1. Creare un gruppo di sicurezza di rete.
2. Creare una regola in ingresso che consenta il transito del traffico sulle porte 20 e 21 per il supporto FTP attivo.

### <a name="what-is-a-network-security-group"></a>Che cos'è un gruppo di sicurezza di rete?

Le reti virtuali sono alla base del modello di rete di Azure e offrono isolamento e protezione. I gruppi di sicurezza di rete (NSG) sono lo strumento principale che consente di applicare e controllare le regole del traffico di rete a livello di rete. I gruppi di sicurezza di rete sono un livello di sicurezza facoltativo che fornisce un firewall software mediante il filtraggio del traffico in ingresso e in uscita sulla rete virtuale. 

I gruppi di sicurezza possono essere associati a un'interfaccia di rete (per le regole per ogni host), a una subnet nella rete virtuale (per l'applicazione a più risorse) o a entrambi i livelli. 

#### <a name="security-group-rules"></a>Regole dei gruppi di sicurezza

I gruppi di sicurezza di rete usano _regole_ per consentire o negare il transito del traffico nella rete. Ogni regola identifica l'indirizzo di origine e destinazione (o un intervallo), il protocollo, la porta (o intervallo di porte), la direzione (in ingresso o in uscita), una priorità numerica e se consentire o negare il traffico che corrisponde alla regola.

![Regole dei gruppi di sicurezza di rete](../media/7-nsg-rules.png)

Ogni gruppo di sicurezza ha un set di regole di sicurezza predefinite per applicare le regole di rete predefinite descritte in precedenza. Queste regole predefinite non possono essere modificate, ma _possono_ essere sottoposto a override.

#### <a name="how-azure-uses-network-rules"></a>Come vengono usate le regole di rete in Azure

Per il traffico in ingresso, Azure elabora il gruppo di sicurezza associato alla subnet e quindi il gruppo di sicurezza applicato all'interfaccia di rete. Il traffico in uscita viene elaborato nell'ordine inverso, vale a dire prima l'interfaccia di rete seguita dalla subnet.

> [!WARNING]
> Tenere presente che i gruppi di sicurezza sono facoltativi a entrambi i livelli. Se non viene applicato alcun gruppo di sicurezza, **tutto il traffico è consentito** da Azure. Se la macchina virtuale ha un indirizzo IP pubblico, questo potrebbe essere un grave rischio in particolare se il sistema operativo non include un qualche tipo di firewall.

Le regole vengono valutate nell'_ordine di priorità_, a partire dalla regola con la **priorità più bassa**. Le regole di negazione **arrestano** sempre la valutazione. Ad esempio, se una richiesta in uscita viene bloccata da regola di interfaccia di rete, tutte le regole applicate alla subnet non verranno controllate. Affinché possa attraversare il gruppo di sicurezza, il traffico deve passare attraverso _tutti_ i gruppi applicati.

L'ultima regola è sempre una regola **Nega tutto**. Si tratta di una regola predefinita aggiunta a ogni gruppo di sicurezza per il traffico in ingresso e in uscita con una priorità di 65500. Questo significa che affinché il traffico possa passare attraverso il gruppo di sicurezza _è necessario avere una regola per consentirlo_, in assenza della quale verrà bloccato dalla regola finale.

> [!NOTE]
> SMTP (porta 25) è un caso speciale. A seconda del livello di sottoscrizione e di quando è stato creato l'account, il traffico SMTP in uscita potrebbe essere bloccato. È possibile richiedere di rimuovere questa restrizione con una giustificazione aziendale.

Dato che non è stato creato un gruppo di sicurezza per questa macchina virtuale, è possibile farlo e applicarlo.

## <a name="creating-network-security-groups"></a>Creazione di gruppi di sicurezza di rete

I gruppi di sicurezza sono risorse gestite, come la maggior parte degli elementi in Azure. È possibile crearli nel portale di Azure o tramite gli strumenti di scripting da riga di comando. L'aspetto più complicato è la definizione delle regole. Vediamo come definire una nuova regola per consentire l'accesso FTP.