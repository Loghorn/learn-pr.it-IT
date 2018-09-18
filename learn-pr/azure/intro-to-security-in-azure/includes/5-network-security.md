La protezione della rete da attacchi e da accessi non autorizzati è una parte importante di qualsiasi architettura. Ora vedremo che cosa si intende per sicurezza di rete, come integrare un approccio a più livelli nell'architettura e come Azure consente di garantire la sicurezza di rete per l'ambiente.

## <a name="a-layered-approach-to-network-security"></a>Sicurezza di rete basata su un approccio a più livelli

Il filo conduttore di questo modulo è l'impiego di un approccio alla sicurezza strutturato su più livelli e questo approccio vale anche per il livello di rete. Non è sufficiente concentrarsi sulla protezione del perimetro della rete oppure limitarsi a considerare la sicurezza tra servizi all'interno di una rete. Un approccio a più livelli significa predisporre vari livelli di protezione, in modo che, se un utente malintenzionato riesce a superare un livello, sono presenti ulteriori protezioni per contenere l'attacco.

Diamo un'occhiata agli strumenti forniti da Azure per creare un approccio a più livelli in grado di proteggere la superficie della rete.

### <a name="internet-protection"></a>Protezione di Internet

Se si parte dal perimetro della rete, si intende limitare ed eliminare gli attacchi provenienti da Internet. Un ottimo punto di partenza consiste nel valutare le risorse che sono connesse a Internet e consentire solo le comunicazioni in ingresso e in uscita necessarie. Identificare tutte le risorse che consentono qualsiasi tipo di traffico di rete in ingresso, assicurarsi che siano necessarie e che utilizzino unicamente le porte e i protocolli richiesti. Queste informazioni si possono trovare agevolmente nel Centro sicurezza di Azure, perché individua le risorse connesse a Internet che non sono associate a gruppi di sicurezza di rete (NSG), nonché le risorse non protette da un firewall.

Esistono un paio di modi per garantire la protezione in ingresso sul perimetro:

* Il gateway applicazione è un servizio di bilanciamento del carico con un web application firewall che fornisce protezione dalla vulnerabilità note più comuni.

* Per i servizi non HTTP o per configurazioni avanzate, è possibile usare appliance virtuali di rete. Le appliance virtuali di rete sono simili alle appliance firewall hardware.


Per qualsiasi risorsa esposta a Internet, esiste il rischio di subire attacchi di tipo Denial of Service. Questi tipi di attacchi tentano di sovraccaricare una risorsa di rete inviando talmente tante richieste da rallentare o addirittura bloccare la risorsa. Per mitigare questi attacchi, DDoS di Azure fornisce una protezione di base per tutti i servizi di Azure e una protezione avanzata ulteriormente personalizzabile per le proprie risorse. La protezione DDoS blocca il traffico degli attacchi e inoltra il traffico rimanente alla destinazione prevista. Entro pochi minuti dal rilevamento degli attacchi, si riceve una notifica in base alle metriche di Monitoraggio di Azure.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![DDoS](../media-COPIED-FROM-DESIGNFORSECURITY/ddos.png)

### <a name="virtual-network-security"></a>Sicurezza della rete virtuale

All'interno di una rete virtuale (VNet) è importante limitare la comunicazione tra le risorse solo a quella davvero necessaria.

Per la comunicazione tra macchine virtuali, i gruppi di sicurezza di rete (NSG) sono un elemento fondamentale per limitare la comunicazione non necessaria. Comprendono un elenco dei tipi di comunicazione consentiti e negati da e verso interfacce di rete e subnet e sono completamente personalizzabili.

È possibile escludere completamente l'accesso alla rete Internet pubblica per i servizi limitando l'accesso agli endpoint di servizio. Con gli endpoint di servizio è possibile limitare l'accesso dei servizi di Azure alla propria rete virtuale.

### <a name="network-integration"></a>Integrazione di rete

È una situazione comune quella in cui si dispone di un'infrastruttura di rete esistente che deve essere integrata per consentire la comunicazione da reti locali o per consentire una comunicazione migliore tra servizi in Azure. Esistono vari modi per gestire questa integrazione e migliorare la sicurezza della rete.

Le connessioni di rete privata virtuale (VPN) sono un modo comune per stabilire canali di comunicazione sicura tra le reti. La connessione tra reti virtuali di Azure e un dispositivo VPN locale è un ottimo modo per garantire comunicazioni protette tra la propria rete e la rete virtuale in Azure.

Per creare una connessione privata dedicata tra la rete e Azure, è possibile usare ExpressRoute. ExpressRoute consente di estendere le reti locali nel cloud Microsoft tramite una connessione privata fornita da un provider di connettività. Con ExpressRoute è possibile stabilire connessioni ai servizi cloud Microsoft, come Microsoft Azure, Office 365 e Dynamics 365. Ciò migliora la sicurezza delle comunicazioni locali perché questo tipo di traffico viene inviato tramite il circuito privato anziché tramite Internet. Non è necessario consentire l'accesso a questi servizi agli utenti finali tramite Internet e si può inviare il traffico tramite appliance per un ulteriore controllo.

## <a name="summary"></a>Riepilogo

Un approccio alla sicurezza di rete strutturato su più livelli consente di ridurre il rischio di esposizione agli attacchi basati sulla rete. Azure mette a disposizione diversi servizi e funzionalità per proteggere le risorse con connessione Internet, le risorse interne e le comunicazioni tra le reti locali. Queste funzionalità rendono possibile la creazione di soluzioni sicure in Azure.