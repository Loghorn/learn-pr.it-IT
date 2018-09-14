Si supponga che la società decide di vedere se Azure Load Balancer supporta l'applicazione della risorsa pianificazione ERP (Enterprise). L'applicazione ha un'interfaccia web per gli utenti e viene eseguito su più server. Ogni server ha una copia locale del database, ERP che viene sincronizzato tra tutti i server.

In questo caso, esaminerà come un servizio di bilanciamento del carico consente di garantire un'elevata disponibilità dei servizi. Verranno identificati la differenza tra le opzioni di servizio di bilanciamento del carico basic e standard e vedere come creare un servizio di bilanciamento del carico per le macchine virtuali di Azure.

## <a name="what-is-load-balancing"></a>Che cos'è il bilanciamento del carico?

_Il bilanciamento del carico_ vengono descritte varie tecniche per la distribuzione di carichi di lavoro tra più dispositivi, tra cui calcolo, archiviazione e i dispositivi di rete. L'obiettivo di bilanciamento del carico è ottimizzare l'utilizzo di più risorse, in modo da ottimizzare l'uso di queste risorse come un'infrastruttura di aumento del numero, e per garantire servizi vengono mantenuti se alcuni componenti non sono disponibili.

In questo caso, verrà esaminato il supporto per le macchine virtuali (VM) di bilanciamento del carico di Azure.

### <a name="what-is-high-availability"></a>Che cos'è la disponibilità elevata?

Disponibilità elevata (HA) misura la capacità di un'applicazione o servizio deve rimanere accessibile anche se un errore in qualsiasi componente di sistema. In teoria, non essere esisterà alcun notevole perdita di servizio.

Bilanciamento del carico è fondamentale per il recapito della disponibilità elevata poiché consente a più macchine virtuali di agire come un pool di server. Il pool possono continuare a servizio richieste anche nel caso di arresto anomalo del sistema alcune macchine virtuali o vengano portati offline per manutenzione.

## <a name="what-is-azure-load-balancer"></a>Che cos'è Azure Load Balancer?

**Azure Load Balancer** è un servizio di Azure che distribuisce le richieste in ingresso tra più macchine virtuali in un pool. Distribuisce il traffico di rete in ingresso in un set di macchine virtuali integre e consente di evitare tutte le macchine Virtuali che non sono in grado di rispondere.

 Azure Load Balancer opera a livello 4 (TCP, UDP) del modello OSI livello 7. Può essere configurato per il supporto TCP e UDP applicazione scenari in cui il traffico in ingresso alle macchine virtuali di Azure, nonché gli scenari in uscita in altri servizi di Azure sono attraversano il traffico TCP e UDP out macchine virtuali di Azure verso endpoint esterni.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yBWo]

## <a name="public-vs-internal-load-balancers"></a>Pubblica e servizi di bilanciamento del carico interno

Azure Load Balancer può essere rappresentata _pubbliche_ o _interno_ a seconda dell'origine delle richieste in ingresso.

Oggetto **servizio di bilanciamento del carico pubblico** gestisce le richieste client di fuori dell'infrastruttura di Azure. L'indirizzo IP pubblico del bilanciamento del carico viene configurato automaticamente come front-end di load balancer quando si crea l'indirizzo IP pubblico e la risorsa di bilanciamento del carico. La figura seguente illustra un servizio di bilanciamento del carico pubblico.

![Un'illustrazione che mostra un servizio di bilanciamento del carico pubblico distribuire le richieste client da internet ai tre macchine virtuali in una rete virtuale.](../media/2-public-load-balancer.png)

Un' **bilanciamento del carico interno** elabora richieste da all'interno di una rete virtuale (o tramite una VPN). Distribuisce le richieste alle risorse all'interno di tale rete virtuale. Il servizio di bilanciamento del carico, indirizzi IP front-end e le reti virtuali non sono direttamente accessibili da internet. La figura seguente mostra un'architettura che contiene sia un bilanciamento del carico interno e pubblico. Il servizio di bilanciamento del carico pubblico gestisce le richieste esterne mentre il servizio di bilanciamento del carico interno inoltra le richieste per le macchine virtuali interne e i database per l'elaborazione.

![Un'illustrazione che mostra un bilanciamento del carico pubblico inoltrando le richieste client a un servizio di bilanciamento del carico interno. Il servizio di bilanciamento del carico interno distribuisce quindi le richieste a una subnet di livello web o una subnet di livello database in base al tipo della richiesta. Subnet del livello web sia nella subnet del livello di database dispone di più server per gestire le richieste.](../media/2-internal-load-balancer.png)

## <a name="how-does-azure-load-balancer-work"></a>Come funziona Azure Load Balancer?

Azure Load Balancer Usa informazioni configurate nella **regole** e **probe di integrità** per determinare come nuovo traffico in ingresso che è stato ricevuto in un bilanciamento del carico **front-end** è quindi distribuite a istanze di macchina virtuale in un **pool back-end**.

### <a name="front-end"></a>Front-end

Il front-end del servizio di bilanciamento di carico è una configurazione IP, che contiene uno o più indirizzi IP pubblici, che consente l'accesso al servizio di bilanciamento del carico e delle relative applicazioni tramite Internet.

### <a name="back-end-address-pool"></a>Pool di indirizzi back-end

Macchine virtuali di connettersi a un servizio di bilanciamento del carico usando la scheda di interfaccia di rete virtuale (vNIC). Il pool di indirizzi back-end contiene gli indirizzi IP scheda vnic connessi al servizio di bilanciamento del carico. Se si inseriscono tutte le VM nel set di disponibilità, è possibile usare per aggiungere facilmente le macchine virtuali a un pool di back-end quando si configura il bilanciamento del carico.

### <a name="health-probe"></a>Probe di integrità

Uso di servizi di bilanciamento di carico _probe di integrità_ per determinare quali macchine virtuali possono soddisfare le richieste. Il servizio di bilanciamento del carico distribuirà solo il traffico alle macchine virtuali disponibili e operative. 

Un probe di integrità monitora porte specifiche su ogni macchina virtuale. È possibile definire il tipo di risposta corrisponde a "integrità"; ad esempio, potrebbe richiedere un `HTTP 200 Available` risposta da un'applicazione web. Per impostazione predefinita, una macchina virtuale sarà essere contrassegnata come "non disponibile" dopo due errori consecutivi a intervalli di 15 secondi.

### <a name="load-balancer-rules"></a>Regole del servizio di bilanciamento del carico

Bilanciamento del carico _regole_ definiscono la distribuzione del traffico alle macchine virtuali di back-end. L'obiettivo è distribuire abbastanza le richieste tra le macchine virtuali integre nel pool di back-end.

Azure Load Balancer Usa un algoritmo basato su hash riscrivere le intestazioni dei pacchetti in ingresso. Per impostazione predefinita, bilanciamento del carico consente di creare un hash da:

- Indirizzi IP di origine
- Porte di origine
- Indirizzi IP di destinazione
- Porte di destinazione
- Numeri di protocollo IP

Questo meccanismo garantisce che tutti i pacchetti all'interno di un flusso dei pacchetti di client vengono inviati alla stessa istanza di macchina virtuale back-end. Un nuovo flusso da un client userà un diverso allocata in modo casuale la porta di origine. Questo significa che verrà modificato l'hash e il bilanciamento del carico può inviare questo flusso a un endpoint di back-end diverso.

## <a name="basic-vs-standard-load-balancer-skus"></a>Input della regola di base SKU di bilanciamento del carico standard

Sono disponibili due versioni di Azure Load Balancer: **base** e **Standard**. Differenze di scalabilità, funzionalità e prezzo. Ad esempio:

- Mentre Basic non standard supporta il protocollo HTTPS
- Dimensioni del pool possono essere molto più grandi nello Standard
- Basic è gratuito, mentre Standard viene addebitato in base a regole e la velocità effettiva.

Standard è un superset di base, pertanto qualsiasi scenario adatto per Basic dovrebbero funzionare anche sullo Standard. Lo SKU Basic è destinato a livello generale per la creazione di prototipi e test mentre Standard è consigliata per la produzione.

## <a name="start-the-deployment-of-a-basic-public-load-balancer"></a>Avviare la distribuzione di un servizio di bilanciamento del carico basic pubblico

Per creare un sistema VM con bilanciamento del carico, è necessario creare il servizio di bilanciamento del carico stesso, creare una rete virtuale per contenere le macchine virtuali e quindi aggiungere le macchine virtuali alla rete virtuale.

Per creare il servizio di bilanciamento del carico usando il portale di Azure, è necessario definire i seguenti:

- Nome del servizio di bilanciamento del carico
- Tipo: pubblico o interno
- SKU: Basic o Standard
- Indirizzo IP pubblico: dinamico o statico
- Percorso e il gruppo di risorse

Le macchine virtuali di back-end verranno essere tutte connesse alla stessa rete virtuale, pertanto è necessario configurare successivamente la risorsa:

- Nome della rete virtuale
- Spazio degli indirizzi da usare, ad esempio 172.20.0.0/16
- Gruppo di risorse
- Nome della subnet da usare
- Spazio degli indirizzi per la subnet (deve essere all'interno dello spazio principale), ad esempio 172.20.0.0/24

È quindi necessario creare e distribuire le VM back-end e configurare in modo che usino la rete virtuale. È anche necessario inserire le macchine virtuali nello stesso set di disponibilità. Set di disponibilità definiscono il livello di tolleranza di errore in un gruppo di macchine virtuali, ma per il bilanciamento del carico, sono anche utili per assegnare le macchine virtuali al pool back-end.

A questo punto è stato illustrato come usare Azure Load Balancer come parte di una soluzione a disponibilità elevata. Successivamente, si utilizzeranno questi passaggi per distribuire il proprio servizio di bilanciamento del carico.