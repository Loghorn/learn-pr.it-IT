La protezione della rete da attacchi e da accessi non autorizzati è un elemento importante di qualsiasi architettura. Ora vedremo che cosa si intende per sicurezza di rete, come integrare un approccio a più livelli nell'architettura e come Azure consente di garantire la sicurezza di rete per l'ambiente.

## <a name="a-layered-approach-to-network-security"></a>Sicurezza di rete basata su un approccio a più livelli

Il filo conduttore di questo modulo è l'impiego di un approccio alla sicurezza strutturato su più livelli e questo approccio vale anche per il livello di rete. Non è sufficiente concentrarsi solo sulla protezione del perimetro della rete oppure limitarsi a considerare la sicurezza tra servizi all'interno di una rete. Un approccio a più livelli significa predisporre vari livelli di protezione, in modo che, se un utente malintenzionato riesce ad accedere a un livello, sono presenti ulteriori protezioni per contenere l'attacco.

Diamo un'occhiata agli strumenti che fornisce Azure per creare un approccio a più livelli in grado di proteggere la superficie della rete.

### <a name="internet-protection"></a>Protezione di Internet

Se si parte dal perimetro della rete, si lavora per limitare ed eliminare gli attacchi provenienti da Internet. Un ottimo primo punto di partenza consiste nel valutare le risorse che sono con connessione internet e consentano la comunicazione solo in ingresso e in uscita laddove necessario. Identificare tutte le risorse che consentono qualsiasi tipo di traffico di rete in ingresso, assicurarsi che siano necessarie e che utilizzino unicamente le porte e i protocolli richiesti. Centro sicurezza di Azure è un ottimo punto di cercare queste informazioni, perché vengono identificate con connessione internet alle risorse che non dispongono di gruppi di sicurezza di rete associati, nonché alle risorse non protette dietro un firewall.

Per garantire la protezione in ingresso sul perimetro, sono disponibili due opzioni:

* Gateway applicazione di Azure è un servizio di bilanciamento del carico che include un web application firewall che fornisca protezione dai più comuni, a causa di vulnerabilità note.

* Per non HTTP servizi o configurazioni avanzate, è possono utilizzare Appliance virtuali di rete (Nva). Appliance virtuali di rete sono simili alle appliance firewall hardware.


Tutte le risorse esposte a internet sono a rischio di attacchi da un attacco denial of service. Questi tipi di attacchi tentano di sovraccaricare una risorsa di rete inviando talmente tante richieste da rallentare o addirittura bloccare la risorsa. Per mitigare questi attacchi, protezione DDoS di Azure fornisce la protezione di base in tutti i servizi di Azure e una protezione avanzata per un'ulteriore personalizzazione per le risorse. Protezione DDoS di Azure blocca il traffico degli attacchi e inoltra il traffico rimanente verso la destinazione prevista. Entro pochi minuti dal rilevamento degli attacchi, l'utente riceve una notifica in base alle metriche di Monitoraggio di Azure.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![DDoS](../media-COPIED-FROM-DESIGNFORSECURITY/ddos.png)

### <a name="virtual-network-security"></a>Sicurezza della rete virtuale

Una volta all'interno di una rete virtuale (VNet), è importante limitare la comunicazione tra le risorse solo a quella davvero necessaria.

Per la comunicazione tra macchine virtuali, i gruppi di sicurezza di rete sono una parte fondamentale per limitare la comunicazione non necessaria. Forniscono un elenco dei consentiti e negato la comunicazione da e verso le subnet e interfacce di rete e sono completamente personalizzabili.

È possibile rimuovere completamente l'accesso a internet pubblico per i servizi limitando l'accesso agli endpoint del servizio. Con gli endpoint di servizio, accesso ai servizi di Azure può essere limitata alla rete virtuale.

### <a name="network-integration"></a>Integrazione di rete

È comune disporre di infrastruttura di rete esistente che deve essere integrato per fornire la comunicazione da reti locali o per fornire maggiore comunicazione tra servizi in Azure. Esistono vari modi per gestire questa integrazione e migliorare la sicurezza della rete.

Le connessioni di rete privata virtuale (VPN) sono un modo comune per stabilire canali di comunicazione sicura tra le reti. Connessione tra reti virtuali di Azure e il dispositivo VPN locale è un ottimo modo per garantire comunicazioni protette tra la rete e la rete virtuale in Azure.

Per fornire una connessione privata dedicata tra la rete e Azure, è possibile usare Azure ExpressRoute. ExpressRoute consente di estendere le reti locali nel cloud Microsoft tramite una connessione privata fornita da un provider di connettività. Con ExpressRoute è possibile stabilire connessioni ai servizi cloud Microsoft, come Microsoft Azure, Office 365 e Dynamics 365. Ciò migliora la sicurezza delle comunicazioni locali perché questo tipo di traffico viene inviato tramite il circuito privato anziché tramite Internet. Non è necessario consentire l'accesso a questi servizi per gli utenti finali tramite internet, e possibile inviare il traffico attraverso Appliance per un'ulteriore controllo del traffico.

## <a name="summary"></a>Riepilogo

Un approccio alla sicurezza di rete strutturato su più livelli consente di ridurre il rischio di esposizione agli attacchi basati sulla rete. Azure offre diversi servizi e funzionalità per proteggere le risorse con connessione internet, risorse interne e la comunicazione tra le reti locali. Queste funzionalità rendono possibile la creazione di soluzioni sicure in Azure.