Come l'azienda Usa i dati altamente sensibili e ha grandi quantità di informazioni che verrà archiviati in Azure, esistono alcuni dubbi sulla sicurezza e affidabilità delle connessioni sulla rete Internet pubblica. La società non è disposta a migrare a livello globale in Azure, a meno che non è possibile dimostrare livelli superiori di connettività, sicurezza e affidabilità.

In questo caso, si passerà di là di connessioni che eseguono la rete Internet a linee dedicate diretto in Data Center di Azure.

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute consente alle organizzazioni di estendere le proprie reti locali nel Cloud Microsoft tramite una connessione privata implementata da un provider di connettività. Ciò significa che la connettività al Data Center di Azure non viene accettata la rete Internet ma attraverso un collegamento dedicato. ExpressRoute consente anche connessioni efficienti con altri servizi basati su Microsoft Cloud, come Office 365 e Dynamics 365.

ExpressRoute offre molti vantaggi, inclusi i seguenti:

- Velocità più elevate, da 50 Mbps a 10 Gbps, con il ridimensionamento dinamico della larghezza di banda

- Latenza più bassa

- Maggiore affidabilità tramite peering predefiniti

- Sicurezza elevata

ExpressRoute offre anche altri vantaggi, ad esempio:

- Connettività a tutti i servizi di Azure supportati

- Connettività globale a tutte le aree (richiede un componente aggiuntivo Premium)

- Routing dinamico su Border Gateway Protocol

- Contratti di servizio (SLA) per il tempo di attività di connessione

- Qualità del servizio (QoS) per Skype for Business

È inoltre disponibile il componente aggiuntivo ExpressRoute premium, che offre i vantaggi, ad esempio i limiti di route migliorati, connettività del servizio globale e collegamenti di maggiore di rete virtuale per circuito.

## <a name="expressroute-connectivity-models"></a>Modelli di connettività di ExpressRoute

Le connessioni verso ExpressRoute possono essere stabilite mediante i meccanismi seguenti:

- Rete IP VPN (Any-to-Any)

- Cross Connection virtuale tramite scambio Ethernet

- Connessione Ethernet da punto a punto

 Le caratteristiche e le funzionalità di ExpressRoute sono identiche in tutti i modelli di connettività sopra descritti.

### <a name="what-is-layer-3-connectivity"></a>Informazioni sulla connettività di livello 3

Microsoft usa uno standard di settore protocollo di routing dinamico (BGP) per lo scambio di route tra la rete locale, le istanze in Azure e Microsoft indirizzi pubblici. Vengono stabilite più sessioni BGP con la rete per profili di traffico diversi.

### <a name="any-to-any-ipvpn-networks"></a>Reti (IPVPN) Any-to-Any

I provider IPVPN forniscono in genere la connettività tra succursali e i data center aziendale su connessioni gestite di livello 3. Con ExpressRoute, il Data Center di Azure vengono visualizzati come se fossero un'altra succursale.

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>Cross connection virtuale tramite scambio Ethernet

Se l'organizzazione ha un percorso condiviso con una funzionalità di scambio cloud, richiesta cross Connection per il Cloud Microsoft tramite scambio Ethernet del provider. Queste connessioni incrociate a Microsoft Cloud possono operare a livello 2 o layer 3 connessioni, come nel modello di rete OSI gestite.

### <a name="point-to-point-ethernet-connection"></a>Connessione Ethernet da punto a punto

Collegamenti Ethernet punto a punto possono fornire le connessioni dal livello 3 al livello 2 o gestito tra i data center locali o gli uffici al Cloud Microsoft.

## <a name="how-expressroute-works"></a>Funzionamento di ExpressRoute

Azure ExpressRoute Usa una combinazione di circuiti ExpressRoute e domini di routing per fornire la connettività di larghezza di banda elevata nel cloud di Microsoft.

### <a name="what-are-expressroute-circuits"></a>Quali sono i circuiti ExpressRoute

Un circuito ExpressRoute è la connessione logica tra l'infrastruttura locale e Cloud Microsoft. Un provider di connettività implementa tale connessione, anche se alcune organizzazioni usano più provider di connettività per motivi di ridondanza. Ogni circuito ha una larghezza di banda predefinito di 50, 100, 200 Mbps o 500 Mbps, o 1 Gbps o 10 Gbps e ognuno di questi circuiti eseguire il mapping a un provider di connettività e una località di peering. Ogni circuito ExpressRoute ha inoltre quote e limiti predefiniti.

Un circuito ExpressRoute non è equivalente a una connessione di rete o un dispositivo di rete. Ogni circuito è definito da un GUID, chiamato un' _assistenza_ oppure _s-key_. Questa chiave s fornisce il collegamento di connettività tra Microsoft, il provider di connettività e l'organizzazione, non è un segreto di crittografia. Ogni s-key include un mapping uno-a-uno verso un circuito Azure ExpressRoute.

Ogni circuito può avere fino a tre peering, ovvero una coppia di sessioni BGP configurati per la ridondanza. Sono:

- Privato di Azure
- Peering pubblico di Azure
- Microsoft

### <a name="routing-domains"></a>Domini di routing

Circuiti ExpressRoute quindi eseguire il mapping ai domini di routing, con ogni circuito ExpressRoute con più domini di routing. Questi domini sono le stesse tre peering elencati in precedenza. In una configurazione attiva-attiva ogni dominio di routing di ogni coppia di router è configurato in modo identico, per fornire la disponibilità elevata. I nomi Azure delle peering privato di pubblici e Azure rappresentano l'indirizzo IP gli schemi di indirizzamento.

#### <a name="azure-private-peering"></a>Peering privato di Azure

Peering privato di Azure si connette ai servizi di calcolo di Azure, ad esempio le macchine virtuali e servizi cloud distribuiti con una rete virtuale. A livello di sicurezza, il dominio di peering privato è semplicemente un'estensione della rete locale in Azure. Viene quindi abilitata la connettività bidirezionale tra tale rete e qualsiasi rete virtuale di Azure, rendendo visibili gli indirizzi IP della VM di Azure entro la rete interna.

> [!NOTE]
> È possibile connettere una sola rete virtuale al dominio di peering privato.

#### <a name="azure-public-peering"></a>Peering pubblico di Azure

Peering pubblico di Azure consente di stabilire connessioni private ai servizi che sono disponibili su indirizzi IP pubblici, ad esempio archiviazione di Azure, database SQL di Azure e servizi web di Azure. Con il peering pubblico, è possibile connettersi agli indirizzi IP pubblico del servizio senza il traffico viene instradato tramite Internet. La connettività è sempre dalla WAN ad Azure, non viceversa. Questo è anche un approccio di tipo tutto o niente, perché non è possibile selezionare i servizi per cui si desidera il peering pubblico abilitata.

> [!NOTE]
> Per i servizi PaaS di Azure, è consigliabile usare peering invece di peering pubblico di Microsoft.

#### <a name="microsoft-peering"></a>Peering Microsoft

Il peering Microsoft supporta le connessioni alle offerte SaaS basato su cloud, ad esempio Office 365 e Dynamics 365. Questa opzione di peering offre la connettività bidirezionale alla WAN aziendale e ai servizi cloud Microsoft.

### <a name="expressroute-health"></a>Integrità di ExpressRoute

Analogamente alla maggior parte delle funzionalità in Microsoft Azure, è possibile monitorare le connessioni di ExpressRoute per assicurare che le rispettive prestazioni siano soddisfacenti. Il monitoraggio include le aree seguenti:

- Disponibilità
- Connettività alle reti virtuali
- Utilizzo della larghezza di banda

Lo strumento principale per questa attività di monitoraggio è Monitoraggio prestazioni rete e in particolare Monitoraggio prestazioni rete per ExpressRoute.

## <a name="summary"></a>Riepilogo

Azure ExpressRoute viene usato per creare connessioni private tra i data center di Azure e l'infrastruttura disponibile localmente o in un ambiente con percorso condiviso. Le connessioni ExpressRoute non passano attraverso la rete Internet pubblica e offrono più affidabilità, velocità più elevate e minori latenze rispetto alle connessioni Internet tradizionali.
