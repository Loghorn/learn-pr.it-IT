La protezione della rete da attacchi e da accessi non autorizzati è una parte importante di qualsiasi architettura. Ora vedremo che cosa si intende per sicurezza di rete, come integrare un approccio a più livelli nell'architettura e come Azure consente di garantire la sicurezza di rete per l'ambiente.

## <a name="a-layered-approach-to-network-security"></a>Sicurezza di rete basata su un approccio a più livelli

Si sarà notato che il tema conduttore di questo modulo è l'importanza di un approccio alla sicurezza strutturato su più livelli e questo vale anche per il livello di rete. Non è sufficiente concentrarsi sulla protezione del perimetro della rete oppure limitarsi a considerare la sicurezza tra servizi all'interno di una rete. Un approccio a più livelli significa predisporre vari livelli di protezione, in modo che, se un utente malintenzionato riesce a superare un livello, sono presenti ulteriori protezioni per contenere l'attacco.

Verranno ora presentati gli strumenti forniti da Azure per creare un approccio a più livelli in grado di proteggere la superficie della rete.

:::row:::
  :::column:::
    ![Immagine che rappresenta una connessione Internet sicura](../media/5-internet-protection.png)
  :::column-end:::
    :::column span="3":::: **Protezione di Internet**

Iniziando dal perimetro della rete, l'obiettivo è limitare ed eliminare gli attacchi provenienti da Internet. È consigliabile valutare prima le risorse con connessione Internet e consentire solo la comunicazione in ingresso e in uscita necessaria. Assicurarsi di identificare tutte le risorse che consentono qualsiasi tipo di traffico di rete in ingresso e quindi verificare che usino unicamente le porte e i protocolli richiesti. Centro sicurezza di Azure offre tutte queste informazioni, perché individua le risorse con connessione Internet che non sono associate a gruppi di sicurezza di rete, nonché le risorse non protette da un firewall.

Esistono un paio di modi per garantire la protezione in ingresso sul perimetro:

* Il gateway applicazione di Azure è un servizio di bilanciamento del carico con un Web application firewall che fornisce protezione dalla vulnerabilità note più comuni.

* Le appliance virtuali di rete sono la scelta ideale per le configurazioni avanzate o i servizi non HTTP e sono simili alle appliance firewall hardware.

Per qualsiasi risorsa esposta a Internet, esiste il rischio di subire attacchi Denial of Service. Questi tipi di attacchi provano a sovraccaricare una risorsa di rete inviando talmente tante richieste da rallentare o addirittura bloccare la risorsa. Per mitigare questi attacchi, Protezione DDoS di Azure fornisce una protezione di base per tutti i servizi di Azure e una protezione avanzata ulteriormente personalizzabile per le proprie risorse. Protezione DDoS di Azure blocca il traffico degli attacchi e inoltra il traffico rimanente alla destinazione prevista. Entro pochi minuti dal rilevamento degli attacchi, si riceve una notifica in base alle metriche di Monitoraggio di Azure.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![DDoS](../media/ddos.png)

 :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Immagine che rappresenta una rete virtuale Internet](../media/5-vnet-security.png)
  :::column-end:::
    :::column span="3":::: **Sicurezza della rete virtuale**

In una rete virtuale è essenziale limitare la comunicazione tra le risorse solo a quella davvero necessaria.

Per la comunicazione tra macchine virtuali, i gruppi di sicurezza di rete sono un elemento fondamentale per limitare la comunicazione non necessaria. Comprendono un elenco dei tipi di comunicazione consentiti e negati da e verso interfacce di rete e subnet e sono completamente personalizzabili.

È possibile escludere completamente l'accesso alla rete Internet pubblica per i servizi limitando l'accesso agli endpoint di servizio. Con gli endpoint di servizio è possibile limitare l'accesso dei servizi di Azure alla propria rete virtuale.
 :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Immagine che rappresenta una rete protetta](../media/5-network-integration.png)
  :::column-end:::
    :::column span="3":::: **Integrazione della rete**

È una situazione comune quella in cui si ha un'infrastruttura di rete esistente che deve essere integrata per consentire la comunicazione da reti locali o per consentire una comunicazione migliore tra servizi in Azure. Esistono vari modi per gestire questa integrazione e migliorare la sicurezza della rete.

Le connessioni di rete privata virtuale (VPN) sono un modo comune per stabilire canali di comunicazione sicura tra le reti. La connessione tra reti virtuali di Azure e un dispositivo VPN locale è un ottimo modo per garantire comunicazioni protette tra la propria rete e la rete virtuale in Azure.

Per creare una connessione privata dedicata tra la rete e Azure, è possibile usare Azure ExpressRoute. ExpressRoute consente di estendere le reti locali nel cloud Microsoft tramite una connessione privata fornita da un provider di connettività. Con ExpressRoute è possibile stabilire connessioni ai servizi cloud Microsoft, come Microsoft Azure, Office 365 e Dynamics 365. Ciò migliora la sicurezza delle comunicazioni locali perché questo tipo di traffico viene inviato tramite il circuito privato anziché tramite Internet. Non è necessario consentire agli utenti finali l'accesso a questi servizi tramite Internet ed è possibile inviare il traffico tramite appliance per un ulteriore controllo.
 :::column-end:::
:::row-end:::

## <a name="summary"></a>Riepilogo

Un approccio alla sicurezza di rete strutturato su più livelli consente di ridurre il rischio di esposizione agli attacchi basati sulla rete. Azure offre diversi servizi e funzionalità per proteggere le risorse con connessione Internet, le risorse interne e la comunicazione tra reti locali. Queste funzionalità rendono possibile la creazione di soluzioni sicure in Azure.