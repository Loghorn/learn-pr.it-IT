La protezione della rete da attacchi e da accessi non autorizzati è un elemento importante di qualsiasi architettura. Prima che il proprio ambiente diventasse troppo grande, Lamna Healthcare ha investito del tempo per pianificare l'infrastruttura di rete. Ora vedremo che cosa si intende per sicurezza di rete, come integrare un approccio a più livelli nell'architettura e come Azure consente di garantire la sicurezza di rete per l'ambiente.

## <a name="what-is-network-security"></a>Che cos'è la sicurezza di rete

La sicurezza di rete consiste nel proteggere la comunicazione delle risorse all'interno e all'esterno della rete.  L'obiettivo è quello di limitare l'esposizione dei servizi e dei sistemi a livello di rete. Limitando l'esposizione, si riduce la probabilità che le risorse possano essere attaccate. Nell'ottica della sicurezza di rete, gli sforzi vanno concentrati negli ambiti seguenti:

- Proteggere il flusso del traffico tra le applicazioni e Internet
- Proteggere il flusso del traffico tra un'applicazione e l'altra
- Proteggere il flusso del traffico tra gli utenti e l'applicazione

Proteggere il flusso del traffico tra le applicazioni e Internet significa limitare l'esposizione all'esterno della rete. Gli attacchi alla rete iniziano, principalmente, al di fuori della rete, pertanto limitando l'esposizione su Internet e proteggendo il perimetro, è possibile ridurre il rischio di attacchi.

Proteggere il flusso del traffico tra un'applicazione e l'altra significa tutelare i dati trasmessi tra le applicazioni, tra diversi livelli e altri servizi all'interno della rete. Limitando l'esposizione tra queste risorse, si riduce l'effetto che può avere una risorsa compromessa. Questo contribuisce a contenere un'ulteriore propagazione in rete.

Proteggere il flusso del traffico tra utenti e l'applicazione significa proteggere il flusso di rete per gli utenti finali. Ciò limita l'esposizione delle risorse agli attacchi esterni e rappresenta un meccanismo che consente agli utenti di usare le risorse in sicurezza. 

## <a name="a-layered-approach-to-network-security"></a>Sicurezza di rete basata su un approccio a più livelli

Il filo conduttore di questo modulo è l'impiego di un approccio alla sicurezza strutturato su più livelli e questo approccio vale anche per il livello di rete. Non è sufficiente concentrarsi solo sulla protezione del perimetro della rete oppure limitarsi a considerare la sicurezza tra servizi all'interno di una rete. Un approccio a più livelli significa predisporre vari livelli di protezione, in modo che, se un utente malintenzionato riesce ad accedere a un livello, sono presenti ulteriori protezioni per contenere l'attacco.

Diamo un'occhiata agli strumenti che fornisce Azure per creare un approccio a più livelli in grado di proteggere la superficie della rete.

### <a name="internet-protection"></a>Protezione di Internet

Se si parte dal perimetro della rete, si lavora per limitare ed eliminare gli attacchi provenienti da Internet. Un ottimo punto di partenza consiste nel valutare le risorse che sono connesse a Internet e consentire solo le comunicazioni in ingresso e in uscita necessarie. Identificare tutte le risorse che consentono qualsiasi tipo di traffico di rete in ingresso, assicurarsi che siano necessarie e che utilizzino unicamente le porte e i protocolli richiesti. Centro sicurezza di Azure è un ottimo punto in cui ricercare queste informazioni, perché indica le risorse connesse a Internet che non sono associate a gruppi di sicurezza di rete (NSG), nonché le risorse non protette da un firewall.

Esistono un paio di modi per garantire la protezione in ingresso sul perimetro. Il gateway applicazione è un servizio di bilanciamento del carico di livello 7 che include anche un web application firewall (WAF) per garantire la sicurezza avanzata per i servizi basati su HTTP. Il WAF si basa su regole dei set di regole core OWASP 3.0 o 2.2.9 e assicura la protezione da vulnerabilità note comuni, ad esempio gli attacchi cross-site scripting e SQL injection.

![Gateway applicazione con WAF](../media-draft/appgw-waf.png)

Per la protezione dei servizi non basati su HTTP o per una maggiore personalizzazione, le risorse di rete possono essere protette mediante appliance virtuali di rete. Le appliance virtuali di rete sono simili alle appliance firewall che si possono trovare in reti locali e sono disponibili presso molti dei fornitori di sicurezza di rete più diffusi. Le appliance virtuali di rete consentono una maggiore personalizzazione delle impostazioni di sicurezza per le applicazioni che la richiedono, ma possono comportare una maggiore complessità, pertanto è consigliabile valutare attentamente i requisiti.

Per qualsiasi risorsa esposta a Internet, esiste il rischio di subire attacchi di tipo Denial of Service. Questi tipi di attacchi tentano di sovraccaricare una risorsa di rete inviando talmente tante richieste da rallentare o addirittura bloccare la risorsa. Per mitigare questi attacchi, DDoS di Azure fornisce una protezione di base per tutti i servizi di Azure e una protezione avanzata per un'ulteriore personalizzazione. La protezione DDoS blocca il traffico degli attacchi e inoltra il traffico rimanente verso la destinazione prevista. Entro pochi minuti dal rilevamento degli attacchi, l'utente riceve una notifica in base alle metriche di Monitoraggio di Azure.

![DDoS](../media-draft/ddos.png)

### <a name="virtual-network-security"></a>Sicurezza della rete virtuale

Una volta all'interno di una rete virtuale (VNet), è importante limitare la comunicazione tra le risorse solo a quella davvero necessaria.

Per la comunicazione tra macchine virtuali, i gruppi di sicurezza di rete (NSG) sono un elemento fondamentale per limitare la comunicazione non necessaria. I gruppi di sicurezza di rete operano ai livelli 3 e 4 e comprendono un elenco delle subnet e delle interfacce di rete da e verso le quali la comunicazione non è consentita. I gruppi di sicurezza di rete sono completamente personalizzabili e offrono la possibilità di bloccare completamente la comunicazione di rete da e verso le macchine virtuali. Tramite l'uso dei gruppi di sicurezza di rete, è possibile isolare le applicazioni in ambienti, i livelli e servizi.

![Gruppi di sicurezza di rete di Azure](../media-draft/azure-network-security.png)

Per isolare i servizi di Azure e consentire unicamente la comunicazione tra reti virtuali, usare gli endpoint di servizio di rete virtuale. Con gli endpoint di servizio è possibile associare le risorse dei servizi di Azure alla rete virtuale. In questo modo si ottiene una maggiore sicurezza perché si rimuove completamente l'accesso Internet pubblico alle risorse, consentendo solo il traffico dalla rete virtuale. Ciò riduce la superficie di attacco per il proprio ambiente, le attività di amministrazione per limitare la comunicazione tra la rete virtuale e i servizi di Azure e rappresenta il routing ottimale per questa comunicazione.

### <a name="network-integration"></a>Integrazione di rete

È uno scenario comune disporre di un'infrastruttura di rete esistente che deve essere integrata per consentire la comunicazione da reti locali o per consentire una comunicazione migliore tra servizi in Azure. Esistono vari modi per gestire questa integrazione e migliorare la sicurezza della rete.

Le connessioni di rete privata virtuale (VPN) sono un modo comune per stabilire canali di comunicazione sicura tra le reti e questo vale anche quando si lavora con una rete virtuale in Azure. La connessione tra reti virtuali di Azure e un dispositivo VPN locale è un ottimo modo per garantire comunicazioni protette tra la rete e le macchine virtuali in Azure.

Per creare una connessione privata dedicata tra la rete e Azure, è possibile usare ExpressRoute. ExpressRoute consente di estendere le reti locali nel cloud Microsoft tramite una connessione privata fornita da un provider di connettività. Con ExpressRoute è possibile stabilire connessioni ai servizi cloud Microsoft, come Microsoft Azure, Office 365 e Dynamics 365. Ciò migliora la sicurezza delle comunicazioni locali perché questo tipo di traffico viene inviato tramite il circuito privato anziché tramite Internet. Non è necessario consentire l'accesso a questi servizi agli utenti finali tramite Internet e si può inviare il traffico tramite appliance per un ulteriore controllo.

![ExpressRoute](../media-draft/expressroute-connection-overview.png)

Per integrare facilmente più reti virtuali in Azure, il peering di reti virtuali consente di stabilire una connessione diretta tra reti virtuali designate. Una volta stabilita la connessione, è possibile usare gli NSG per garantire l'isolamento tra le risorse nello stesso modo con cui si proteggono le risorse all'interno di una rete virtuale. Questa integrazione offre la possibilità di fornire lo stesso livello di sicurezza di base in tutte le reti virtuali con peering. La comunicazione è consentita solo tra reti virtuali connesse direttamente.

## <a name="network-security-at-lamna-healthcare"></a>Sicurezza di rete presso Lamna Healthcare

Lamna Healthcare ha sfruttato molti di questi servizi per costruire un'infrastruttura di rete protetta. La comunicazione tra le risorse è negata per impostazione predefinita e consentita solo quando necessario. La connettività in ingresso da Internet è abilitata solo per i servizi che la richiedono e i protocolli RDP e SSH non sono consentiti dagli endpoint di Internet ma solo da risorse interne attendibili.

Per proteggere i servizi Web con connessione Internet, questi vengono posizionati dietro gateway applicazione con WAF abilitato. Questo vale sia per i servizi in esecuzione in macchine virtuali che nel servizio app. I gateway applicazione consentono di mettersi al riparo da molte vulnerabilità comuni.

La protezione DDoS standard è abilitata per garantire la protezione degli endpoint con connessione Internet da attacchi Denial of Service.

Tramite l'uso di gruppi di risorse, è possibile isolare completamente la comunicazione tra servizi applicazione e tra gli ambienti. Consentono esclusivamente le comunicazioni necessarie tra i servizi di un ambiente e non consentono l'accesso tra ambienti di produzione e ambienti non di produzione.

Per creare connettività dedicata tra gli utenti finali e le applicazioni in Azure, è stato effettuato il provisioning di un circuito ExpressRoute con connettività alla rete locale. In questo modo il traffico verso Azure non transita da Internet e una connessione privata per i servizi in Azure consente di comunicare con i sistemi rimanendo in locale.

Con questo approccio, Lamna Healthcare ha sfruttato al meglio i servizi di Azure per garantire la sicurezza a più livelli della sua infrastruttura di rete.

## <a name="summary"></a>Riepilogo

Un approccio alla sicurezza di rete strutturato su più livelli consente di ridurre il rischio di esposizione agli attacchi basati sulla rete. Azure mette a disposizione diversi servizi e funzionalità per proteggere le risorse con connessione Internet, le risorse interne e le comunicazioni tra le reti locali. Queste funzionalità rendono possibile la creazione di soluzioni sicure in Azure.