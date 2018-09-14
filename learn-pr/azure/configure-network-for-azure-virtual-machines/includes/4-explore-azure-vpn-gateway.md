Per integrare l'ambiente locale con Azure, è necessario poter creare una connessione crittografata. È possibile connettersi tramite Internet o su un collegamento dedicato. In questo caso, verrà esaminato il Gateway VPN di Azure, che fornisce un endpoint per le connessioni in ingresso da ambienti locali.

È stata impostata una rete virtuale di Azure e necessario garantire che trasferisce tutti i dati da Azure al sito e tra Azure vengono crittografate le reti virtuali. È anche necessario conoscere in che modo connettere le reti virtuali tra aree e sottoscrizioni.

## <a name="what-is-a-vpn-gateway"></a>Che cos'è un gateway VPN?

Un gateway VPN di Azure fornisce un endpoint per le connessioni in ingresso crittografate da posizioni locali ad Azure tramite Internet. Anche possibile inviare traffico crittografato tra le reti virtuali di Azure tramite rete dedicata di Microsoft che collega i Data Center di Azure in aree diverse. Questa configurazione consente di collegare in modo sicuro macchine virtuali e servizi in aree diverse.

Ogni rete virtuale può avere un solo gateway VPN e tutte le connessioni al gateway VPN condividono la larghezza di banda di rete disponibile.

All'interno di ogni gateway di rete virtuale sono presenti due o più macchine virtuali (VM). Le macchine virtuali sono state distribuite in una subnet speciale definita dall'utente e denominata _subnet del gateway_. Contengono le tabelle di routing per le connessioni ad altre reti, nonché servizi gateway specifici. Le macchine virtuali e la subnet del gateway sono simili a un dispositivo di rete con protezione avanzata. Non è necessario configurare direttamente le macchine virtuali, né distribuire risorse aggiuntive nella subnet del gateway.

La creazione di un gateway di rete virtuale può richiedere diverso tempo, ed è quindi importante pianificare le attività in modo appropriato. Quando si crea un gateway di rete virtuale, il processo di provisioning genera le macchine virtuali del gateway e le distribuisce nella subnet del gateway. Le impostazioni delle macchine virtuali saranno quelle configurate nel gateway.

Un'impostazione importante è il **_tipo di gateway_**, che per un gateway VPN sarà di tipo "vpn". Le opzioni per i gateway VPN includono:

- Connessioni di rete-a-network con VPN IPsec/IKE tunneling, collegando i gateway VPN ad altri gateway VPN.

- Tunneling VPN IPsec/IKE cross-premise, per la connessione di reti locali ad Azure tramite dispositivi VPN dedicati per la creazione di connessioni da sito a sito.

- Connessioni da punto a sito su IKEv2 o SSTP, per collegare i computer client alle risorse in Azure.

Fattori da considerare per la pianificazione del gateway VPN.

## <a name="plan-a-vpn-gateway"></a>Pianificare un gateway VPN

Quando si prevede un gateway VPN, esistono tre architetture da considerare:

- Da punto a sito tramite Internet
- Da sito a sito tramite Internet
- Da sito a sito tramite una rete dedicata, ad esempio Azure ExpressRoute

### <a name="planning-factors"></a>Fattori relativi alla pianificazione

I fattori da tenere in considerazione durante il processo di pianificazione sono:

- Velocità effettiva: Mbps o Gbps
- Backbone: Internet o privato?
- Disponibilità di un indirizzo IP (statico) pubblico
- Compatibilità del dispositivo VPN
- Più connessioni client o collegamento da sito a sito?
- Tipo di gateway VPN
- SKU del gateway VPN di Azure

La tabella seguente mostra un riepilogo delle problematiche di pianificazione. Le rimanenti verranno illustrate più avanti.

|                           |  Da punto a sito            | Sito a sito                          |  ExpressRoute                 |
| -------------             | -------------             | -------------                         | ---------                     |
| Servizi supportati di Azure  | Servizi cloud e macchine virtuali    | Servizi cloud e macchine virtuali                | Tutti i servizi supportati        |
| Larghezza di banda tipiche         | Dipende dalla SKU del Gateway VPN    | Fino a 1 Gbps con aggregazione         | Da 50 Mbps a 10 Gbps       |
| Protocolli supportati       | IPsec e SSTP            | IPsec                                 | Connessione diretta, reti VLAN      |
| Routing                   | RouteBased (dinamico)      | PolicyBased (statico) e RouteBased   | BGP                           |
| Resilienza connessione     | Modello attivo/passivo            | attivo-passivo o attivo-attivo       | Modello attivo/attivo                 |
| Caso d'uso                  | Testing e creazione prototipi   | Sviluppo, test e produzione su scala ridotta  | Enterprise/mission-critical   |

### <a name="gateway-skus"></a>SKU del gateway

Azure offre gli SKU seguenti per i servizi gateway:

| SKU              |  Tunnel S2S/network-a-network | Connessioni P2S  |  Benchmark della velocità effettiva aggregata   | Usare per                         |
| -------------    | -------------             | -------------    | ---------                         | ---------                       |
| Basic            | Max 10                    | Max 128          | 100 Mbps                          | Sviluppo/test/modello di verifica                    |
| VpnGw1           | Max 30                    | Max 128          | 650 Mbps                          | Carichi di lavoro di produzione o critici   |
| VpnGw2           | Max 30                    | Max 128          | 1 Gbps                            | Carichi di lavoro di produzione o critici   |
| VpnGw3           | Max 30                    | Max 128          | 1,25 Gbps                          | Carichi di lavoro di produzione o critici   |

> [!Note]
> È importante scegliere lo SKU corretto. Se è stato configurato il gateway VPN con quello errato, è possibile portarlo verso il basso e ricreare il gateway, che può richiedere tempi lunghe.

## <a name="workflow"></a>Flusso di lavoro

Quando si progetta una strategia di connettività cloud con le reti private virtuali in Azure, è consigliabile adottare il flusso di lavoro seguente:

1. Progettare la topologia di connettività, elencando gli spazi indirizzi per tutte le reti da connettere.

1. Creare una rete virtuale di Azure.

1. Creare un gateway VPN per la rete virtuale.

1. Creare e configurare le connessioni a reti locali o altre reti virtuali, in base alle esigenze.

1. Se necessario, creare e configurare una connessione da punto a sito per il gateway VPN di Azure.

### <a name="design-considerations"></a>Considerazioni sulla progettazione

Quando si progettano i gateway VPN per connettere le reti virtuali, è necessario tenere conto dei fattori seguenti:

- Subnet non possono sovrapporsi

    È fondamentale che una subnet in una posizione non contenga lo stesso spazio degli indirizzi come in un'altra posizione.

- Gli indirizzi IP devono essere univoci

    È possibile avere due host con lo stesso indirizzo IP in posizioni diverse, come sarà Impossibile instradare il traffico tra i due host e la connessione di rete-a-network avrà esito negativo.

- I gateway VPN è necessaria una subnet del gateway denominata **GatewaySubnet**

    Deve avere questo nome per il funzionamento del gateway e non deve contenere tutte le altre risorse.

### <a name="create-an-azure-virtual-network"></a>Creare una rete virtuale di Azure

Prima di creare un gateway VPN, è necessario creare la rete virtuale di Azure.

### <a name="create-a-vpn-gateway"></a>Creare un gateway VPN

Il tipo di gateway VPN che verrà creato dipende dall'architettura corrente. Le opzioni sono:

- RouteBased

    Dispositivi VPN basati su route usano selettori di traffico any-to-any (jolly) e consentono il traffico diretto le tabelle di routing/inoltro tunnel IPsec diversi. Le connessioni basate su route vengono in genere create su piattaforme router in cui ogni tunnel IPsec è modellato come interfaccia di rete o interfaccia di tunnel virtuale.

- PolicyBased

    Dispositivi VPN basati su criteri usano le combinazioni di prefissi di entrambe le reti per definire come il traffico è crittografati/decrittografati tramite tunnel IPsec. Una connessione basata su criteri viene in genere creata su dispositivi firewall che eseguono il filtro dei pacchetti. La crittografia e la decrittografia dei tunnel IPsec vengono aggiunte al filtro dei pacchetti e al motore di elaborazione.

## <a name="set-up-a-vpn-gateway"></a>Configurare un gateway VPN

La procedura da eseguire dipende dal tipo di gateway VPN che viene installato. Ad esempio, per creare un gateway VPN da punto a sito usando il portale di Azure, potrebbe eseguire la procedura seguente:

1. Creare una rete virtuale

2. Aggiungere una subnet del gateway

3. Specificare un server DNS (facoltativo)

4. Creare un gateway di rete virtuale

5. Generare i certificati

6. Aggiungere il pool di indirizzi client

7. Configurare il tipo di tunnel

8. Configurare il tipo di autenticazione

9. Caricare i dati del certificato pubblico per il certificato radice

10. Installare un certificato client esportato

11. Generare e installare il pacchetto di configurazione del client VPN

12. Connettersi ad Azure

Poiché sono presenti più percorsi di configurazione con i gateway VPN di Azure, ognuno con più opzioni, non è possibile eseguire ogni programma di installazione in corso. Per altre informazioni, vedere la sezione Risorse aggiuntive.

## <a name="configure-the-gateway"></a>Configurare il gateway

Dopo aver creato il gateway, è necessario configurarlo.  Esistono diverse impostazioni di configurazione che si dovrà fornire, ad esempio nome, posizione, server DNS, e così via. Queste impostazioni verranno esaminate più in dettaglio nell'esercizio.

## <a name="summary"></a>Riepilogo

I gateway VPN di Azure sono un componente in reti virtuali di Azure che consentono a point-to-site, site-to-site o connessioni di rete-a-network. I gateway VPN di Azure abilitare singoli computer client per connettersi alle risorse in Azure, estendere le reti locali ad Azure o facilitare le connessioni tra reti virtuali in diverse aree e sottoscrizioni.
