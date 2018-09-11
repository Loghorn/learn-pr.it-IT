Per integrare l'ambiente locale con Azure, è necessario poter creare una connessione crittografata. È possibile connettersi tramite la rete Internet pubblica o tramite un collegamento dedicato. In questo caso, verrà illustrato il Gateway VPN di Azure, che fornisce un endpoint per le connessioni in ingresso dagli ambienti locali.

È stata configurata una rete virtuale di Azure ed è necessario assicurarsi che i trasferimenti di dati da Azure al sito e tra reti virtuali di Azure siano crittografati. È anche necessario conoscere in che modo connettere le reti virtuali tra aree e sottoscrizioni.

## <a name="describe-a-vpn-gateway"></a>Descrivere un gateway VPN

Il Gateway VPN di Azure fornisce un endpoint per le connessioni crittografate in ingresso dall'ambiente locale ad Azure tramite Internet. Può anche inviare traffico crittografato tra reti virtuali di Azure tramite la rete dedicata di Microsoft che collega i data center di Azure in varie aree. Questa configurazione consente di collegare in modo sicuro macchine virtuali e servizi in aree diverse.

Ogni rete virtuale può avere un solo gateway VPN e tutte le connessioni al gateway VPN condividono la larghezza di banda di rete disponibile.

In ogni gateway di rete virtuale sono presenti due o più macchine virtuali. Le macchine virtuali sono state distribuite in una subnet speciale definita dall'utente e denominata _subnet del gateway_. Contengono le tabelle di routing per le connessioni ad altre reti, nonché servizi gateway specifici. Le macchine virtuali e la subnet del gateway sono simili a un dispositivo di rete con protezione avanzata. Non è necessario configurare direttamente le macchine virtuali, né distribuire risorse aggiuntive nella subnet del gateway.

La creazione di un gateway di rete virtuale può richiedere diverso tempo, ed è quindi importante pianificare le attività in modo appropriato. Quando si crea un gateway di rete virtuale, il processo di provisioning genera le macchine virtuali del gateway e le distribuisce nella subnet del gateway. Le impostazioni delle macchine virtuali saranno quelle configurate nel gateway.

Un'impostazione importante è il **_tipo di gateway_**, che per un gateway VPN sarà di tipo "vpn". Le opzioni per i gateway VPN includono:

- Connessioni da rete virtuale a rete virtuale tramite tunneling VPN IPsec/IKE, che collega i gateway VPN ad altri gateway VPN.

- Tunneling VPN IPsec/IKE cross-premise, per la connessione di reti locali ad Azure tramite dispositivi VPN dedicati per la creazione di connessioni da sito a sito.

- Connessioni da punto a sito tramite IKEv2 o SSTP, per collegare i computer client alle risorse in Azure.

Fattori da considerare per la pianificazione del gateway VPN.

## <a name="plan-a-vpn-gateway"></a>Pianificare un gateway VPN

Quando si pianifica un gateway VPN, è necessario valutare tre architetture:

- Da punto a sito tramite Internet
- Da sito a sito tramite Internet
- Da sito a sito tramite una rete dedicata, ad esempio Azure ExpressRoute

### <a name="planning-factors"></a>Fattori di pianificazione

I fattori da tenere in considerazione durante il processo di pianificazione sono:

- Velocità effettiva: Mbps o Gbps
- Backbone: Internet o privato?
- Disponibilità di un indirizzo IP (statico) pubblico
- Compatibilità del dispositivo VPN
- Più connessioni client o collegamento da sito a sito?
- Tipo di gateway VPN
- SKU del gateway VPN di Azure

La tabella seguente mostra un riepilogo delle problematiche di pianificazione. Le rimanenti verranno illustrate più avanti.

|                           |  Da punto a sito            | Da sito a sito                          |  ExpressRoute                 |
| -------------             | -------------             | -------------                         | ---------                     |
| Servizi supportati di Azure  | Servizi cloud e macchine virtuali    | Servizi cloud e macchine virtuali                | Tutti i servizi supportati        |
| Larghezza di banda tipica         | A seconda dello SKU del gateway    | Fino a 1 Gbps con aggregazione         | Da 50 Mbps a 10 Gbps       |
| Protocolli supportati       | SSTP e IPSec            | IPsec                                 | Connessione diretta, reti VLAN      |
| Routing                   | RouteBased (dinamico)      | PolicyBased (statico) e RouteBased   | BGP                           |
| Resilienza connessione     | attiva-passiva            | attiva-passiva o attiva-attiva       | attiva-attiva                 |
| Caso d'uso                  | Testing e creazione prototipi   | Sviluppo, test e produzione su scala ridotta  | Aziendali/Mission critical   |

### <a name="gateway-skus"></a>SKU del gateway

Azure offre gli SKU seguenti per i servizi gateway:

| SKU              |  Tunnel da sito a sito/da rete virtuale a rete virtuale | Connessioni da punto a sito  |  Benchmark velocità effettiva aggregata   | Usare per                         |
| -------------    | -------------             | -------------    | ---------                         | ---------                       |
| Basic            | Max 10                    | Max 128          | 100 Mbps                          | Sviluppo/test/modello di verifica                    |
| VpnGw1           | Max 30                    | Max 128          | 650 Mbps                          | Carichi di lavoro di produzione/critici   |
| VpnGw2           | Max 30                    | Max 128          | 1 Gbps                            | Carichi di lavoro di produzione/critici   |
| VpnGw3           | Max 30                    | Max 128          | 1,25 Gbps                          | Carichi di lavoro di produzione/critici   |

> [!Note] 
> È importante scegliere lo SKU corretto, in quanto se si configura il gateway VPN con uno SKU errato, sarà necessario eliminare e creare di nuovo il gateway, con una perdita di tempo significativa

## <a name="workflow"></a>Flusso di lavoro

Quando si progetta una strategia di connettività cloud con le reti private virtuali in Azure, è consigliabile adottare il flusso di lavoro seguente:

1. Progettare la topologia di connettività, elencando gli spazi indirizzi per tutte le reti da connettere.

1. Creare una rete virtuale di Azure.

1. Creare un gateway VPN per la rete virtuale.

1. Creare e configurare le connessioni a reti locali o altre reti virtuali, in base alle esigenze.

1. Se necessario, creare e configurare una connessione da punto a sito per il gateway VPN di Azure.

### <a name="design-considerations"></a>Considerazioni sulla progettazione

Quando si progettano i gateway VPN per connettere le reti virtuali, è necessario tenere conto dei fattori seguenti:

- Le subnet non possono sovrapporsi. È fondamentale che una subnet in una località non contenga lo stesso spazio indirizzi di una subnet in un'altra località.

- Gli indirizzi IP devono essere univoci. Non possono essere presenti due host con lo stesso indirizzo IP in località diverse, in quanto sarebbe impossibile indirizzare il traffico tra questi due host e la connessione da rete virtuale a rete virtuale avrebbe esito negativo.

- I gateway VPN necessitano di una subnet del gateway denominata **GatewaySubnet**. È necessario assegnarle questo nome affinché funzioni e non deve contenere altre risorse.

### <a name="create-an-azure-virtual-network"></a>Creare una rete virtuale di Azure

Prima di creare un gateway VPN, è necessario creare la rete virtuale di Azure.

### <a name="create-a-vpn-gateway"></a>Creare un gateway VPN

Il tipo di gateway VPN che verrà creato dipende dall'architettura corrente. Le opzioni sono:

- RouteBased: i dispositivi VPN basati su route usano selettori di traffico any-to-any (jolly) e consentono alle tabelle di routing/inoltro di indirizzare il traffico a tunnel IPsec diversi. Le connessioni basate su route vengono in genere create su piattaforme router in cui ogni tunnel IPsec è modellato come interfaccia di rete o interfaccia di tunnel virtuale.

- PolicyBased: i dispositivi VPN basati su criteri usano le combinazioni di prefissi di entrambe le reti per definire come crittografare/decrittografare il traffico tramite i tunnel IPsec. Una connessione basata su criteri viene in genere creata su dispositivi firewall che eseguono il filtro dei pacchetti. La crittografia e la decrittografia dei tunnel IPsec vengono aggiunte al filtro dei pacchetti e al motore di elaborazione.

## <a name="set-up-a-vpn-gateway"></a>Configurare un gateway VPN

La procedura da eseguire dipende dal tipo di gateway VPN che viene installato. Ad esempio, per creare un gateway VPN da punto a sito con il portale di Azure, sarà necessario eseguire questa procedura:

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

Poiché sono disponibili numerosi percorsi di configurazione per i gateway VPN di Azure, ognuno dei quali con più opzioni, non è possibile illustrare ogni tipo di configurazione in questo corso. Per altre informazioni, vedere la sezione Risorse aggiuntive.

## <a name="configure-the-gateway"></a>Configurare il gateway

Dopo aver creato il gateway, è necessario configurarlo.  Sarà necessario specificare numerose impostazioni di configurazione, ad esempio nome, località, server DNS e così via. Queste impostazioni verranno esaminate più in dettaglio nell'esercizio.

## <a name="summary"></a>Riepilogo

I gateway VPN di Azure sono un componente delle reti virtuali di Azure che abilita le connessioni da punto a sito, da sito a sito o da rete virtuale a rete virtuale. I gateway VPN di Azure consentono ai singoli computer client di connettersi alle risorse in Azure, estendere le reti locali in Azure o agevolare le connessioni tra reti virtuali in aree e sottoscrizioni diverse.