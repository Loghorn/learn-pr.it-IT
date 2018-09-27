L'azienda vuole verificare se Azure Load Balancer supporta l'applicazione ERP (Enterprise Resource Planning). L'applicazione ha un'interfaccia Web per gli utenti e viene eseguita su più server. Ogni server ha una copia locale del database ERP che viene sincronizzata su tutti i server.

Di seguito si esaminerà come un servizio di bilanciamento del carico può garantire la disponibilità elevata dei servizi. Si identificherà la differenza tra le opzioni Basic e Standard del servizio di bilanciamento del carico e si osserverà come creare un servizio di bilanciamento del carico per Macchine virtuali di Azure.

## <a name="what-is-load-balancing"></a>Che cos'è il bilanciamento del carico?

Il _bilanciamento del carico_ include varie tecniche per distribuire i carichi di lavoro, ad esempio di calcolo, di archiviazione e di rete, in più dispositivi. L'obiettivo del bilanciamento del carico è ottimizzare l'uso di più risorse, in modo da garantirne la massima efficienza in caso di aumento del numero di istanze nell'infrastruttura e da assicurare il mantenimento dei servizi se alcuni componenti risultano non disponibili.

Di seguito verrà esaminato il supporto del bilanciamento del carico offerto da Azure per le macchine virtuali (VM).

### <a name="what-is-high-availability"></a>Che cos'è la disponibilità elevata?

La disponibilità elevata misura la capacità di un'applicazione o un servizio di rimanere accessibile nonostante un errore in qualsiasi componente del sistema. Idealmente, non dovrebbe verificarsi alcuna perdita evidente di servizio.

Il bilanciamento del carico è fondamentale per garantire la disponibilità elevata perché consente a più VM di fungere da pool di server. Il pool può continuare a gestire le richieste anche se alcune VM si arrestano in modo anomalo o vengono portate offline a scopo di manutenzione.

## <a name="what-is-azure-load-balancer"></a>Che cos'è Azure Load Balancer?

**Azure Load Balancer** è un servizio di Azure che distribuisce le richieste in ingresso tra più VM di un pool. Distribuisce il traffico di rete in ingresso tra un set di VM integre evitando le VM che non possono rispondere.

 Azure Load Balancer opera al livello 4 (TCP, UDP) del modello a 7 livelli OSI. Può essere configurato per supportare scenari di applicazione TCP e UDP con traffico in ingresso verso VM di Azure nonché scenari in uscita in cui altri servizi di Azure passano traffico TCP e UDP a endpoint esterni tramite VM di Azure.

#### <a name="what-is-a-load-balancer"></a>Che cos'è un servizio di bilanciamento del carico?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yBWo]

## <a name="public-vs-internal-load-balancers"></a>Servizi di bilanciamento del carico pubblici e interni

Un'istanza di Azure Load Balancer può essere _pubblica_ o _interna_ a seconda dell'origine delle richieste in ingresso.

Un **servizio di bilanciamento del carico pubblico** gestisce le richieste client provenienti dall'esterno dell'infrastruttura di Azure. L'indirizzo IP pubblico viene assegnato al servizio di bilanciamento del carico e instrada le richieste in ingresso a un set di risorse presenti in una rete virtuale privata. Questo approccio viene solitamente adottato per garantire la disponibilità elevata dei server Web. La figura seguente illustra un servizio di bilanciamento del carico pubblico.

![Figura con un servizio di bilanciamento del carico pubblico che distribuisce le richieste client da Internet a tre VM in una rete virtuale.](../media/2-public-load-balancer.png)

Un **servizio di bilanciamento del carico interno** elabora le richieste provenienti dall'interno di una rete virtuale o tramite una VPN. Distribuisce le richieste alle risorse all'interno della rete virtuale. Il servizio di bilanciamento del carico, gli indirizzi IP front-end e le reti virtuali non sono direttamente accessibili da Internet. La figura seguente illustra un'architettura contenente sia un servizio di bilanciamento del carico pubblico che uno interno. Il servizio di bilanciamento del carico pubblico gestisce le richieste esterne, mentre quello interno inoltra le richieste alle VM e ai database interni per l'elaborazione.

![Figura con un servizio di bilanciamento del carico pubblico che inoltra le richieste client a un servizio di bilanciamento del carico interno. In base al tipo di richiesta, il servizio di bilanciamento del carico interno distribuisce quindi le richieste a una subnet del livello Web o del livello database. Sia la subnet del livello Web che quella del livello database includono più server per la gestione delle richieste.](../media/2-internal-load-balancer.png)

## <a name="how-does-azure-load-balancer-work"></a>Come funziona Azure Load Balancer?

Azure Load Balancer usa le informazioni configurate nelle _regole_ e nei _probe di integrità_ per determinare come il traffico in ingresso ricevuto nel _front-end_ di Load Balancer viene distribuito alle istanze di VM di un _pool di indirizzi back-end_. Di seguito sono descritti i singoli componenti.

### <a name="what-is-the-load-balancer-front-end"></a>Che cos'è il front-end del servizio di bilanciamento del carico?

Il front-end del servizio di bilanciamento del carico è una configurazione IP contenente uno o più indirizzi IP pubblici che consente l'accesso al servizio e alle relative applicazioni tramite Internet.

### <a name="what-is-the-backend-address-pool"></a>Che cos'è il pool di indirizzi back-end?

Le macchine virtuali si connettono a un servizio di bilanciamento del carico tramite la scheda di interfaccia di rete virtuale (vNIC). Il pool di indirizzi back-end contiene gli indirizzi IP delle schede di interfaccia di rete virtuali connesse al servizio di bilanciamento del carico. Se si inseriscono tutte le VM in un set di disponibilità, è possibile usare il set per aggiungere facilmente le VM al pool back-end durante la configurazione del servizio di bilanciamento del carico.

### <a name="what-is-a-health-probe"></a>Che cos'è un probe di integrità?

I servizi di bilanciamento del carico usano i _probe di integrità_ per determinare quali macchine virtuali possono gestire le richieste. Il servizio di bilanciamento del carico distribuirà il traffico solo alle VM disponibili e operative.

Un probe di integrità monitora porte specifiche in ogni VM. È possibile definire il tipo di risposta corrispondente all'integrità. Da un'applicazione Web, ad esempio, si può richiedere una risposta `HTTP 200 Available`. Per impostazione predefinita, una VM verrà contrassegnata come non disponibile dopo due errori consecutivi a intervalli di 15 secondi.

### <a name="load-balancer-rules"></a>Regole del servizio di bilanciamento del carico

Le _regole_ del servizio di bilanciamento del carico definiscono come verrà distribuito il traffico alle VM back-end. L'obiettivo è distribuire equamente le richieste tra le VM integre del pool back-end.

Azure Load Balancer usa un algoritmo basato su hash per riscrivere le intestazioni dei pacchetti in ingresso. Per impostazione predefinita, Load Balancer crea un hash da:

- Indirizzo IP di origine
- Porta di origine
- Indirizzo IP di destinazione
- Porta di destinazione
- Numero di protocollo IP

Questo meccanismo garantisce che tutti i pacchetti in un flusso client di pacchetti vengano inviati alla stessa istanza di VM back-end. Un nuovo flusso da un client userà una diversa porta di origine allocata in modo casuale. Ciò significa che verrà modificato l'hash e il servizio di bilanciamento del carico può inviare il flusso a un endpoint back-end diverso.

## <a name="basic-vs-standard-load-balancer-skus"></a>SKU Basic e Standard di Load Balancer

Sono disponibili due versioni di Azure Load Balancer: **Basic** e **Standard**, con differenze di scalabilità, funzionalità e prezzo. Ad esempio:

- La versione Standard supporta il routing del traffico protetto tramite HTTPS.
- La dimensione dei pool back-end può essere maggiore nella versione Standard (1000 nello SKU Standard e 100 nello SKU Basic).
- Il traffico può essere indirizzato a un pool di endpoint più grande, che include diversi set di scalabilità, set di disponibilità e macchine virtuali. Lo SKU Basic è limitato a un singolo set di disponibilità, set di scalabilità o macchina virtuale.
- Il supporto delle porte a disponibilità elevata consente di bilanciare il carico di flussi TCP e UDP in tutte le porte simultaneamente quando viene usato come servizio di bilanciamento del carico interno.
- Lo SKU Basic è gratuito, mentre lo SKU Standard comporta l'addebito di costi in base alle regole e alla velocità effettiva.

Poiché lo SKU Standard è una versione avanzata dello SKU Basic, qualsiasi scenario idoneo alla versione Basic funzionerà anche nella versione Standard. Lo SKU Basic viene in genere usato per prototipi e test, mentre lo SKU Standard è consigliato per la produzione.

## <a name="start-the-deployment-of-a-basic-public-load-balancer"></a>Avviare la distribuzione di un servizio di bilanciamento del carico pubblico Basic

Per creare un sistema di VM con carico bilanciato, è necessario creare il servizio di bilanciamento del carico e una rete virtuale che conterrà le macchine virtuali e quindi aggiungere le VM alla rete virtuale.

Per creare il servizio di bilanciamento del carico con il portale di Azure, si definiscono gli elementi seguenti:

- Nome del servizio di bilanciamento del carico
- Tipo: pubblico o interno
- SKU: Basic o Standard
- Indirizzo IP pubblico: dinamico o statico
- Gruppo di risorse e località

Poiché le VM back-end verranno tutte connesse alla stessa rete virtuale, è necessario configurare le risorse seguenti:

- Nome della rete virtuale
- Spazio indirizzi da usare, ad esempio 172.20.0.0/16
- Gruppo di risorse
- Nome della subnet da usare
- Spazio indirizzi della subnet (che deve essere incluso nello spazio principale), ad esempio 172.20.0.0/24

Poiché si prevede un traffico in ingresso, è necessario creare regole di sicurezza di rete usando un gruppo di sicurezza di rete (NSG). In questo caso, è opportuno aprire la porta 80 per il traffico HTTP.

Infine, è necessario creare e distribuire le macchine virtuali e configurarle per l'uso della rete virtuale. Viene anche creato un set di disponibilità per raggrupparle insieme. I set di disponibilità definiscono il livello di tolleranza di errore in un gruppo di VM, ma per il bilanciamento del carico facilitano anche l'assegnazione delle VM ai pool back-end.

È stato illustrato come usare Azure Load Balancer nell'ambito di una soluzione a disponibilità elevata. Successivamente, questi passaggi verranno usati per distribuire il servizio di bilanciamento del carico personalizzato.