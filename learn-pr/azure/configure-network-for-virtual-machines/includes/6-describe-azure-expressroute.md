Poiché la società gestisce dati a riservatezza elevata e include grandi quantità di informazioni da archiviare in Azure, occorre tenere in considerazione alcuni aspetti relativi alla sicurezza e all'affidabilità delle connessioni su Internet pubblico. La società non vuole eseguire la migrazione globale in Azure, a meno che non vengano garantiti livelli superiori di connettività, sicurezza e affidabilità.

In questo contesto verranno usate linee dedicate dirette ai data center di Azure invece di connessioni su Internet pubblico.

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute consente alle organizzazioni di superare i limiti delle rispettive reti locali e passare a Microsoft Cloud mediante una connessione privata implementata da un provider di connettività. In base a questo approccio la connettività verso i data center di Azure non viene ottenuta tramite Internet ma tramite un collegamento dedicato. ExpressRoute consente anche connessioni efficienti con altri servizi basati su Microsoft Cloud, come Office 365 e Dynamics 365.

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

È anche disponibile il componente aggiuntivo Premium per ExpressRoute, che offre vantaggi quali limiti più elevati per le route, connettività globale ai servizi e un numero superiore di collegamenti alla rete virtuale per ogni circuito.

## <a name="expressroute-connectivity-models"></a>Modelli di connettività di ExpressRoute

Le connessioni verso ExpressRoute possono essere stabilite mediante i meccanismi seguenti:

- Rete IP VPN (Any-to-Any)

- Cross Connection virtuale tramite scambio Ethernet

- Connessione Ethernet da punto a punto

 Le caratteristiche e le funzionalità di ExpressRoute sono identiche in tutti i modelli di connettività sopra descritti.

### <a name="what-is-layer-3-connectivity"></a>Informazioni sulla connettività di livello 3

Microsoft usa il protocollo di routing dinamico standard del settore (BGP) per lo scambio di route tra la rete locale, le istanze in Azure e gli indirizzi pubblici Microsoft. Vengono stabilite più sessioni BGP con la rete per profili di traffico diversi.
### <a name="any-to-any-ipvpn-networks"></a>Reti (IPVPN) Any-to-Any

I provider IPVPN forniscono in genere la connettività tra le succursali e il data center aziendale su connessioni gestite di livello 3. Con ExpressRoute i data center di Azure vengono visualizzati come se fossero un'altra succursale.

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>Cross Connection virtuale tramite scambio Ethernet

Se l'organizzazione condivide il percorso con una struttura di scambio cloud, vengono richieste Cross Connection a Microsoft Cloud tramite lo scambio Ethernet del provider. Le Cross Connection a Microsoft Cloud possono operare a livello 2 o livello 3 delle connessioni gestite, come nel modello di rete OSI.

### <a name="point-to-point-ethernet-connection"></a>Connessione Ethernet da punto a punto

I collegamenti Ethernet da punto a punto possono fornire connessioni di livello 2 o di livello 3 gestite tra i data center o le sedi locali a Microsoft Cloud.

## <a name="how-expressroute-works"></a>Funzionamento di ExpressRoute

Azure ExpressRoute usa una combinazione di circuiti ExpressRoute e di domini di routing per fornire una connettività a larghezza di banda elevata a Microsoft Cloud.

### <a name="what-are-expressroute-circuits"></a>Informazioni sui circuiti ExpressRoute

Un circuito ExpressRoute rappresenta la connessione logica tra l'infrastruttura locale e Microsoft Cloud. Un provider di connettività implementa tale connessione, anche se alcune organizzazioni usano più provider di connettività per motivi di ridondanza. Ogni circuito ha una larghezza di banda fissa pari a 50, 100, 200 o 500 Mbps oppure 1 o 10 Gbps e ogni circuito esegue il mapping a un provider di connettività e a una località peer. Ogni circuito ExpressRoute ha inoltre quote e limiti predefiniti.

Un circuito ExpressRoute non equivale a una connessione di rete o a un dispositivo di rete. Ogni circuito viene definito da un GUID, indicato come _servizio_ o _s- key_. Il valore s-key fornisce un collegamento di connettività tra Microsoft, il provider di connettività e l'organizzazione. Non si tratta di un segreto crittografico. Ogni s-key include un mapping uno-a-uno verso un circuito Azure ExpressRoute.

Ogni circuito può avere fino a 3 peering. Ogni peering è una coppia di sessioni BGP configurate per la ridondanza, ovvero:

- Privato di Azure
- Pubblico di Azure
- Microsoft

### <a name="routing-domains"></a>Domini di routing

I circuiti ExpressRoute eseguono quindi il mapping ai domini di routing. Ogni circuito ExpressRoute ha più domini di routing, che corrispondono ai tre peering indicati in precedenza. In una configurazione attiva-attiva ogni dominio di routing di ogni coppia di router è configurato in modo identico, per fornire la disponibilità elevata. I nomi di peering Pubblico di Azure e Privato di Azure rappresentano gli schemi di indirizzi IP.

#### <a name="azure-private-peering"></a>Peering privato di Azure

Il peering privato di Azure si connette ai servizi di calcolo di Azure quali macchine virtuali e servizi cloud distribuiti con una rete virtuale. A livello di sicurezza, il dominio di peering privato è semplicemente un'estensione della rete locale in Azure. Viene quindi abilitata la connettività bidirezionale tra tale rete e qualsiasi rete virtuale di Azure, rendendo visibili gli indirizzi IP della VM di Azure entro la rete interna.

> [!NOTE]
> È possibile connettere solo UNA rete virtuale al dominio di peering privato

#### <a name="azure-public-peering"></a>Peering pubblico di Azure

Il peering pubblico di Azure consente le connessioni private ai servizi disponibili in indirizzi IP pubblici, ad esempio Archiviazione di Azure, database SQL di Azure e servizi Web di Azure. Il peering pubblico permette di connettersi agli indirizzi IP pubblici di tali servizi senza che il traffico venga indirizzato su Internet. La connettività è sempre dalla WAN ad Azure, non viceversa. Si tratta di un approccio drastico, perché non consente di selezionare i servizi per cui si vuole abilitare il peering pubblico.

> [!NOTE]
> Per i servizi PaaS di Azure è consigliabile usare il peering Microsoft invece del peering pubblico

#### <a name="microsoft-peering"></a>Peering Microsoft

Il peering Microsoft supporta connessioni a offerte SaaS basate sul cloud, ad esempio Office 365 e Dynamics 365. Questa opzione di peering offre la connettività bidirezionale alla WAN aziendale e ai servizi cloud Microsoft.

### <a name="expressroute-health"></a>Integrità di ExpressRoute

Analogamente alla maggior parte delle funzionalità in Microsoft Azure, è possibile monitorare le connessioni di ExpressRoute per assicurare che le rispettive prestazioni siano soddisfacenti. Il monitoraggio include le aree seguenti:

- Disponibilità
- Connettività alle reti virtuali
- Utilizzo della larghezza di banda

Lo strumento principale per questa attività di monitoraggio è Monitoraggio prestazioni rete e in particolare Monitoraggio prestazioni rete per ExpressRoute.

## <a name="summary"></a>Riepilogo

Azure ExpressRoute viene usato per creare connessioni private tra i data center di Azure e l'infrastruttura disponibile localmente o in un ambiente con percorso condiviso. Le connessioni ExpressRoute non usano la rete Internet pubblica e offrono maggiore affidabilità, velocità più elevate e latenze più basse rispetto alle connessioni Internet tradizionali.
