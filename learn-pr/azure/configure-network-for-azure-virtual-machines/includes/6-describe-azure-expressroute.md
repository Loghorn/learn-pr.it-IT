Poiché la società gestisce dati a riservatezza elevata e grandi quantità di informazioni da archiviare in Azure, occorre tenere in considerazione alcuni aspetti relativi alla sicurezza e all'affidabilità delle connessioni sulla rete Internet pubblica. La società non intende eseguire la migrazione globale ad Azure a meno che non vengano garantiti livelli superiori di connettività, sicurezza e affidabilità.

In questo contesto, anziché connessioni tramite la rete Internet pubblica verranno usate linee dedicate dirette ai data center di Azure.

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute consente alle organizzazioni di superare i limiti delle reti locali e passare a Microsoft Cloud mediante una connessione privata implementata da un provider di connettività. Con questo approccio, la connettività verso i data center di Azure non viene ottenuta tramite Internet ma mediante un collegamento dedicato. ExpressRoute agevola anche connessioni efficienti con altri servizi basati su Microsoft Cloud, come Office 365 e Dynamics 365.

ExpressRoute offre molti vantaggi, inclusi i seguenti:

- Velocità più elevate, da 50 Mbps a 10 Gbps, con il ridimensionamento dinamico della larghezza di banda

- Latenza più bassa

- Maggiore affidabilità mediante il peering predefinito

- Sicurezza elevata

ExpressRoute offre anche altri vantaggi, ad esempio:

- Connettività a tutti i servizi di Azure supportati

- Connettività globale a tutte le aree (richiede un componente aggiuntivo Premium)

- Routing dinamico su Border Gateway Protocol

- Contratti di servizio per il tempo di attività della connessione

- Qualità del servizio (QoS) per Skype for Business

È anche disponibile il componente aggiuntivo Premium per ExpressRoute, che offre vantaggi come limiti più elevati per le route, connettività globale ai servizi e un numero superiore di collegamenti alla rete virtuale per ogni circuito.

## <a name="expressroute-connectivity-models"></a>Modelli di connettività di ExpressRoute

Le connessioni verso ExpressRoute possono essere stabilite mediante i meccanismi seguenti:

- Rete IP VPN (Any-to-Any)

- Cross Connection virtuale tramite scambio Ethernet

- Connessione Ethernet da punto a punto

 Le caratteristiche e le funzionalità di ExpressRoute sono identiche in tutti i modelli di connettività sopra descritti.

### <a name="what-is-layer-3-connectivity"></a>Informazioni sulla connettività di livello 3

Microsoft usa il protocollo di routing dinamico standard del settore BGP per lo scambio di route tra la rete locale, le istanze in Azure e gli indirizzi pubblici Microsoft. Vengono stabilite più sessioni BGP con la rete per profili di traffico diversi.

### <a name="any-to-any-ipvpn-networks"></a>Reti Any-to-Any (IPVPN)

I provider IPVPN forniscono in genere la connettività tra le succursali e il data center aziendale su connessioni di livello 3 gestite. Con ExpressRoute, i data center di Azure risultano come un'altra succursale.

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>Cross Connection virtuale tramite scambio Ethernet

Se l'organizzazione condivide il percorso con una struttura di scambio cloud, vengono richieste Cross Connection a Microsoft Cloud tramite lo scambio Ethernet del provider. Queste Cross Connection a Microsoft Cloud possono operare come connessioni gestite di livello 3 o di livello 2, come nel modello di rete OSI.

### <a name="point-to-point-ethernet-connection"></a>Connessione Ethernet da punto a punto

I collegamenti Ethernet da punto a punto possono offrire connessioni di livello 2 o connessioni gestite di livello 3 tra i data center o le sedi locali e Microsoft Cloud.

## <a name="how-expressroute-works"></a>Funzionamento di ExpressRoute

Azure ExpressRoute usa una combinazione di circuiti ExpressRoute e domini di routing per offrire connettività con larghezza di banda elevata a Microsoft Cloud.

### <a name="what-are-expressroute-circuits"></a>Informazioni sui circuiti ExpressRoute

Un circuito ExpressRoute è la connessione logica tra l'infrastruttura locale e Microsoft Cloud. Un provider di connettività implementa tale connessione, anche se alcune organizzazioni usano più provider di connettività per motivi di ridondanza. Ogni circuito ha una larghezza di banda fissa pari a 50, 100, 200 o 500 Mbps oppure 1 o 10 Gbps e ogni circuito esegue il mapping a un provider di connettività e a una località peer. Ogni circuito ExpressRoute ha inoltre quote e limiti predefiniti.

Un circuito ExpressRoute non equivale a una connessione di rete o a un dispositivo di rete. Ogni circuito è definito da un GUID, denominato _chiave del servizio_ o _s-key_. La chiave del servizio fornisce un collegamento di connettività tra Microsoft, il provider di connettività e l'organizzazione. Non è un segreto crittografico. Ogni chiave del servizio include un mapping uno-a-uno a un circuito Azure ExpressRoute.

Ogni circuito può avere fino a tre peering. Ogni peering è una coppia di sessioni BGP configurate per la ridondanza. I peering sono i seguenti:

- Privato di Azure
- Pubblico di Azure
- Microsoft

### <a name="routing-domains"></a>Domini di routing

I circuiti ExpressRoute eseguono quindi il mapping ai domini di routing e ogni circuito ExpressRoute ha più domini di routing. Questi domini corrispondono ai tre peering elencati sopra. In una configurazione attiva-attiva, ogni dominio di routing è configurato in modo identico per ogni coppia di router, in modo da offrire disponibilità elevata. I nomi del peering pubblico di Azure e del peering privato di Azure rappresentano gli schemi di indirizzi IP.

#### <a name="azure-private-peering"></a>Peering privato di Azure

Il peering privato di Azure si connette a servizi di calcolo di Azure come le macchine virtuali e i servizi cloud distribuiti con una rete virtuale. A livello di sicurezza, il dominio del peering privato è semplicemente un'estensione della rete locale in Azure. Si abilita quindi la connettività bidirezionale tra tale rete e qualsiasi rete virtuale di Azure, rendendo visibili gli indirizzi IP delle VM di Azure nella rete interna.

> [!NOTE]
> Al dominio del peering privato può essere connessa una sola rete virtuale.

#### <a name="azure-public-peering"></a>Peering pubblico di Azure

Il peering pubblico di Azure consente connessioni private ai servizi disponibili in indirizzi IP pubblici, ad esempio Archiviazione di Azure, i database SQL di Azure e i servizi Web di Azure. Con il peering pubblico, è possibile connettersi agli indirizzi IP pubblici di tali servizi senza che il traffico venga indirizzato su Internet. La connettività è sempre dalla WAN ad Azure, non viceversa. Si tratta di un approccio drastico, perché non consente di selezionare i servizi per cui si vuole abilitare il peering pubblico.

> [!NOTE]
> Per i servizi PaaS di Azure è consigliabile usare il peering Microsoft invece del peering pubblico.

#### <a name="microsoft-peering"></a>Peering Microsoft

Il peering Microsoft supporta connessioni a offerte SaaS basate sul cloud, come Office 365 e Dynamics 365. Questa opzione di peering offre connettività bidirezionale tra la WAN aziendale e i servizi cloud Microsoft.

### <a name="expressroute-health"></a>Integrità di ExpressRoute

Così come per la maggior parte delle funzionalità in Microsoft Azure, è possibile monitorare le connessioni di ExpressRoute per assicurarsi che le relative prestazioni siano soddisfacenti. Il monitoraggio include le aree seguenti:

- Disponibilità
- Connettività alle reti virtuali
- Utilizzo della larghezza di banda

Lo strumento principale per questa attività di monitoraggio è Monitoraggio prestazioni rete e in particolare Monitoraggio prestazioni rete per ExpressRoute.

## <a name="summary"></a>Riepilogo

Azure ExpressRoute viene usato per creare connessioni private tra i data center di Azure e l'infrastruttura disponibile localmente o in un ambiente con percorso condiviso. Le connessioni ExpressRoute non usano la rete Internet pubblica e offrono maggiore affidabilità, velocità più elevate e latenze più basse rispetto alle connessioni Internet tradizionali.
