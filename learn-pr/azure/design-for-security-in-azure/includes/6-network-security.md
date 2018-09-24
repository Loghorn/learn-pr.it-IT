La protezione della rete da attacchi e da accessi non autorizzati è una parte importante di qualsiasi architettura. Nell'ambito della pianificazione della migrazione nel cloud, Lamna Healthcare si è impegnata a progettare la propria infrastruttura di rete per garantire la presenza di controlli di sicurezza di rete appropriati e proteggere l'infrastruttura di rete da attacchi. Si osserverà ora che cosa si intende per sicurezza di rete, come integrare un approccio a più livelli nell'architettura e in che modo Azure garantisce la sicurezza di rete per l'ambiente.

## <a name="what-is-network-security"></a>Che cos'è la sicurezza di rete

La sicurezza di rete consiste nel proteggere le comunicazioni delle risorse all'interno e all'esterno della rete. L'obiettivo è quello di limitare l'esposizione dei servizi e dei sistemi a livello di rete. Limitando l'esposizione, si riduce la probabilità che le risorse possano essere attaccate. Nell'ottica della sicurezza di rete, gli sforzi possono concentrarsi sugli ambiti seguenti:

- Proteggere il flusso del traffico tra le applicazioni e Internet
- Proteggere il flusso del traffico tra un'applicazione e l'altra
- Proteggere il flusso del traffico tra gli utenti e l'applicazione

Proteggere il flusso del traffico tra le applicazioni e Internet significa limitare l'esposizione all'esterno della rete. Poiché gli attacchi di rete iniziano prevalentemente al di fuori della rete, limitando l'esposizione su Internet e proteggendo il perimetro, è possibile ridurre il rischio di attacchi.

La protezione del traffico tra applicazioni deve essere incentrata sui dati tra le applicazioni e i rispettivi livelli, tra diversi ambienti e in altri servizi all'interno della rete. Limitando l'esposizione tra queste risorse, si riduce il possibile effetto di una risorsa compromessa. Questo contribuisce a contenere l'ulteriore propagazione nella rete.

Proteggere il flusso del traffico tra utenti e l'applicazione significa proteggere il flusso di rete per gli utenti finali. Ciò limita l'esposizione delle risorse agli attacchi esterni e rappresenta un meccanismo che consente agli utenti di usare le risorse in sicurezza. 

## <a name="a-layered-approach-to-network-security"></a>Sicurezza di rete basata su un approccio a più livelli

Il filo conduttore di questo modulo è l'impiego di un approccio alla sicurezza strutturato su più livelli e questo approccio vale anche per il livello di rete. Non è sufficiente concentrarsi sulla protezione del perimetro della rete oppure limitarsi a considerare la sicurezza tra servizi all'interno di una rete. Un approccio a più livelli significa predisporre vari livelli di protezione, in modo che, se un utente malintenzionato riesce a superare un livello, sono presenti ulteriori protezioni per contenere l'attacco.

Verranno ora presentati gli strumenti forniti da Azure per creare un approccio a più livelli in grado di proteggere la superficie della rete.

### <a name="internet-protection"></a>Protezione di Internet

Iniziando dal perimetro della rete, l'obiettivo è limitare ed eliminare gli attacchi provenienti da Internet. Un ottimo punto di partenza consiste nel valutare le risorse con connessione Internet e consentire solo la comunicazione in ingresso e in uscita necessaria. Identificare tutte le risorse che consentono qualsiasi tipo di traffico di rete in ingresso, assicurarsi che siano necessarie e che usino unicamente le porte e i protocolli richiesti. Centro sicurezza di Azure offre tutte queste informazioni, perché individua le risorse con connessione Internet che non sono associate a gruppi di sicurezza di rete, nonché le risorse non protette da un firewall.

Esistono un paio di modi per garantire la protezione in ingresso nel perimetro. Il gateway applicazione è un servizio di bilanciamento del carico di livello 7 che include anche un web application firewall (WAF) per garantire la sicurezza avanzata per i servizi basati su HTTP. WAF usa regole dei set di regole di base OWASP 3.0 o 2.2.9 e assicura la protezione dalle vulnerabilità più comunemente note, ad esempio gli attacchi cross-site scripting e SQL injection.

![Figura che mostra un singolo gateway applicazione che filtra tutte le richieste esterne effettuate alle macchine virtuali che si trovano in due siti diversi. La funzionalità web application firewall del gateway applicazione protegge il sistema da attacchi dannosi, mentre il servizio di bilanciamento del carico distribuisce le richieste legittime tra macchine virtuali.](../media/appgw-waf.png)

Per la protezione dei servizi non basati su HTTP o per una maggiore personalizzazione, è possibile proteggere le risorse di rete mediante appliance di rete virtuali. Le appliance virtuali di rete sono simili alle appliance firewall che si possono trovare in reti locali e sono disponibili presso molti dei più noti fornitori di sicurezza di rete. Le appliance virtuali di rete consentono una maggiore personalizzazione delle impostazioni di sicurezza per le applicazioni che la richiedono, ma possono comportare una maggiore complessità, pertanto è consigliabile valutare attentamente i requisiti.

Per qualsiasi risorsa esposta a Internet, esiste il rischio di subire attacchi di tipo Denial of Service. Questi tipi di attacchi tentano di sovraccaricare una risorsa di rete inviando talmente tante richieste da rallentare o addirittura bloccare la risorsa. Per mitigare questi attacchi, DDoS di Azure fornisce una protezione di base per tutti i servizi di Azure e una protezione avanzata ulteriormente personalizzabile per le proprie risorse. La protezione DDoS blocca il traffico degli attacchi e inoltra il traffico rimanente alla destinazione prevista. Entro pochi minuti dal rilevamento degli attacchi, si riceve una notifica in base alle metriche di Monitoraggio di Azure.

![Figura che mostra la protezione DDoS di Azure installata tra la rete virtuale e le richieste degli utenti esterni. La protezione DDoS di Azure blocca attacchi dannosi contro il traffico, ma inoltra il traffico legittimo alla destinazione prevista.](../media/ddos.png)

### <a name="virtual-network-security"></a>Sicurezza della rete virtuale

All'interno di una rete virtuale (VNet) è importante limitare la comunicazione tra le risorse solo a quella davvero necessaria.

Per la comunicazione tra macchine virtuali, i gruppi di sicurezza di rete (NSG) sono un elemento fondamentale per limitare la comunicazione non necessaria. I gruppi di sicurezza di rete operano ai livelli 3 e 4 e comprendono un elenco dei tipi di comunicazione consentiti e negati da e verso interfacce di rete e subnet. I gruppi di sicurezza di rete sono completamente personalizzabili e offrono la possibilità di bloccare completamente la comunicazione di rete da e verso le macchine virtuali. Usando gruppi di sicurezza di rete, è possibile isolare le applicazioni tra ambienti, livelli e servizi.

![Figura che mostra l'utilizzo di un gruppo di sicurezza di rete per limitare la comunicazione diretta con Internet del back-end e del livello intermedio. Le richieste Internet vengono ricevute dal front-end, che quindi le passa al livello intermedio. Il livello intermedio comunica con il back-end. ](../media/azure-network-security.png)

Per isolare i servizi di Azure e consentire unicamente la comunicazione da reti virtuali, usare endpoint di servizio di rete virtuale. Con gli endpoint di servizio è possibile associare le risorse dei servizi di Azure alla propria rete virtuale. In questo modo si ottiene una maggiore sicurezza perché si rimuove completamente l'accesso Internet pubblico alle risorse, consentendo il traffico solo dalla rete virtuale. Ciò riduce la superficie di attacco per il proprio ambiente e le attività di amministrazione per limitare la comunicazione tra la rete virtuale e i servizi di Azure e assicura il routing ottimale per questa comunicazione.

### <a name="network-integration"></a>Integrazione di rete

È una situazione comune quella in cui si dispone di un'infrastruttura di rete esistente che deve essere integrata per consentire la comunicazione da reti locali o per consentire una comunicazione migliore tra servizi in Azure. Esistono vari modi per gestire questa integrazione e migliorare la sicurezza della rete.

Le connessioni di rete privata virtuale (VPN) sono un modo comune per stabilire canali di comunicazione sicura tra le reti e questo vale anche quando si lavora con una rete virtuale in Azure. La connessione tra reti virtuali di Azure e un dispositivo VPN locale è un ottimo modo per garantire comunicazioni protette tra la propria rete e le macchine virtuali in Azure.

Per creare una connessione privata dedicata tra la rete e Azure, è possibile usare ExpressRoute. ExpressRoute consente di estendere le reti locali nel cloud Microsoft tramite una connessione privata fornita da un provider di connettività. Con ExpressRoute è possibile stabilire connessioni ai servizi cloud Microsoft, come Microsoft Azure, Office 365 e Dynamics 365. Ciò migliora la sicurezza delle comunicazioni locali perché questo tipo di traffico viene inviato tramite il circuito privato anziché tramite Internet. Non è necessario consentire agli utenti finali l'accesso a questi servizi tramite Internet ed è possibile inviare il traffico tramite appliance per un ulteriore controllo.

![Diagramma dell'architettura che illustra un circuito ExpressRoute che connette la rete del cliente con le risorse di Azure.](../media/expressroute-connection-overview.png)

Per integrare facilmente più reti virtuali in Azure, il peering reti virtuali consente di stabilire una connessione diretta tra reti virtuali designate. Una volta stabilita la connessione, è possibile usare i gruppi di sicurezza di rete per garantire l'isolamento tra le risorse nello stesso modo in cui si proteggono le risorse all'interno di una rete virtuale. Questa integrazione offre la possibilità di fornire lo stesso livello di sicurezza di base in tutte le reti virtuali con peering. La comunicazione è consentita solo tra reti virtuali connesse direttamente.

## <a name="network-security-at-lamna-healthcare"></a>Sicurezza di rete presso Lamna Healthcare

Lamna Healthcare ha sfruttato molti di questi servizi per costruire un'infrastruttura di rete protetta. La comunicazione tra le risorse è negata per impostazione predefinita e consentita solo quando necessario. La connettività in ingresso da Internet è abilitata solo per i servizi che la richiedono. I protocolli RDP e SSH non sono consentiti dagli endpoint Internet, ma solo da risorse interne attendibili.

Per proteggere i servizi Web con connessione Internet, questi vengono posizionati dietro gateway applicazione con WAF abilitato. Questo vale per i servizi in esecuzione sia in macchine virtuali che nel servizio app. I gateway applicazione consentono all'azienda di mettersi al riparo da molte vulnerabilità comuni.

È abilitata la protezione DDoS standard per garantire la protezione degli endpoint con connessione Internet da attacchi Denial of Service.

Tramite l'uso di gruppi di risorse di rete, è stato possibile isolare completamente la comunicazione tra i servizi dell'applicazione e tra gli ambienti. Sono consentite esclusivamente le comunicazioni necessarie tra i servizi di un ambiente e non è consentito l'accesso tra ambienti di produzione e ambienti non di produzione.

Per creare connettività dedicata tra gli utenti finali e le applicazioni in Azure, è stato effettuato il provisioning di un circuito ExpressRoute con connettività alla rete locale. In questo modo il traffico verso Azure non transita da Internet e una connessione privata consente ai servizi in Azure di comunicare con i sistemi rimasti in locale.

Con questo approccio, Lamna Healthcare ha sfruttato al meglio i servizi di Azure per garantire la sicurezza a più livelli della sua infrastruttura di rete.

## <a name="summary"></a>Riepilogo

Un approccio alla sicurezza di rete strutturato su più livelli consente di ridurre il rischio di esposizione agli attacchi basati sulla rete. Azure offre diversi servizi e funzionalità per proteggere le risorse con connessione Internet, le risorse interne e la comunicazione tra reti locali. Queste funzionalità rendono possibile la creazione di soluzioni sicure in Azure.