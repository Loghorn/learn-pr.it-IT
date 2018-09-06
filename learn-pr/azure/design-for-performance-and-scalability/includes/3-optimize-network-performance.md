Le prestazioni della rete possono avere un impatto significativo sull'esperienza degli utenti. Nelle architetture complesse con molti servizi diversi, ridurre al minimo la latenza ad ogni hop può avere un impatto enorme sulle prestazioni complessive. Questa unità tratterà dell'importanza della latenza di rete e di come ridurla all'interno della propria architettura. Verrà illustrato anche il modo in cui Lamna Healthcare ha adottato strategie per ridurre al minimo la latenza di rete tra le sue risorse di Azure e tra i propri utenti e Azure.

## <a name="the-importance-of-network-latency"></a>Importanza della latenza di rete

La latenza è una misura del ritardo. La latenza di rete è il tempo necessario per passare da un'origine a una destinazione in un'infrastruttura di rete. Questo periodo di tempo è comunemente noto come round-trip delay, o il tempo impiegato per passare dall'origine alla destinazione e viceversa.

In un data center tradizionale la latenza dell'ambiente può essere minima in quanto le risorse spesso condividono la stessa posizione e un insieme comune di infrastrutture. Il tempo necessario per arrivare dall'origine alla destinazione è inferiore quando le risorse sono fisicamente vicine.

In confronto, un ambiente cloud viene compilato per la scalabilità. Le risorse ospitate nel cloud potrebbero non essere nello stesso rack, data center o anche area. Questo approccio distribuito può avere un impatto sul tempo di round-trip delle comunicazioni di rete. Mentre tutte le aree di Azure sono interconnesse tramite un backbone fiber ad alta velocità, la velocità della luce resta una limitazione fisica. Le chiamate tra servizi in posizioni fisiche diverse avranno comunque una latenza di rete direttamente correlata alla distanza tra loro.

Oltre a ciò, più frequenti sono le interazioni con un'applicazione, più round trip sono necessari. Ogni round trip possiede un'imposta di latenza e ogni round trip si aggiunge alla latenza complessiva.

![NetworkLatency](../media/networkLatency.png)

Viene illustrato ora come migliorare le prestazioni tra le risorse di Azure e a partire dagli utenti finali fino alle risorse di Azure.

## <a name="latency-amongst-multiple-azure-resources"></a>Latenza tra più risorse di Azure

Si supponga che Lamna Healthcare stia sperimentando un nuovo sistema di prenotazione per i pazienti usando un server web e un database nell'area di Azure dell'Europa occidentale. Il sito web recupera gli asset dei file multimediali statici (immagini, javascript, fogli di stile) dalla risorsa di archiviazione blob di Azure nella stessa area. Questa architettura riduce al minimo il tempo di trasmissione dei dati via cavo, poiché le risorse sono co-localizzate all'interno di un'area di Azure.

Si supponga che il progetto pilota del sistema abbia avuto esito positivo e che sia stato esteso agli utenti in Australia. Questi utenti dovranno fare i conti con il tempo di round-trip dall'Irlanda all'Australia per visualizzare il sito web e l'esperienza dell'utente finale sarà scarsa a causa della latenza della rete.

Il team di Lamna Healthcare decide di ospitare un'altra istanza front-end e un altro account di archiviazione nella regione dell'Australia orientale per ridurre la latenza degli utenti. Sebbene questa struttura aiuti a ridurre i tempi di restituzione dei contenuti agli utenti finali da parte del server web, l'esperienza è ancora scarsa in quanto vi è una latenza di comunicazione significativa tra il server web front-end in Australia orientale e il database in Europa occidentale.

Vi sono alcuni modi per ridurre la latenza rimanente:

* Creare una replica in lettura del database in Australia orientale. Ciò permetterebbe di ottenere buone prestazioni nelle operazioni di lettura, ma le operazioni di scrittura subirebbero comunque una latenza. La replica geografica del database SQL di Azure consente le repliche di lettura.
* È possibile sincronizzare i dati tra le aree con la sincronizzazione dati SQL di Azure.
* È possibile usare un database distribuito a livello globale come Azure Cosmos DB. In questo modo, sia le operazioni di lettura che quelle di scrittura possono avvenire indipendentemente dalla posizione.

L'obiettivo è ridurre al minimo la latenza di rete tra ogni livello dell'applicazione. Il modo in cui ciò viene risolto dipende dall'applicazione usata e dall'architettura dei dati, ma Azure fornisce meccanismi per risolvere questo problema in diversi servizi.

## <a name="latency-in-the-context-of-users-to-azure"></a>Latenza nel contesto degli utenti in Azure

È stata esaminata la latenza tra le risorse di Azure, tuttavia si dovrebbe considerare anche la latenza tra gli utenti e l'applicazione cloud usata. L'obiettivo è quello di ottimizzare la distribuzione dell'interfaccia utente finale per gli utenti. Vengono esaminati ora alcuni modi per migliorare le prestazioni di rete tra gli utenti finali e l'applicazione.

### <a name="use-a-dns-load-balancer-for-endpoint-path-optimization"></a>È possibile usare un servizio di bilanciamento del carico DNS per l'ottimizzazione del percorso dell'endpoint

Nell'esempio di Lamna Healthcare, si è visto che il team ha creato un ulteriore nodo web front-end in Australia orientale. Tuttavia, gli utenti finali devono specificare in modo esplicito l'endpoint front-end che desiderano utilizzare. In quanto progettista di una soluzione, Lamna Healthcare vuole rendere l'esperienza il più semplice possibile per i suoi utenti.

Potrebbe essere utile Gestione traffico di Microsoft Azure. Gestione traffico di Microsoft Azure è un servizio di bilanciamento del carico basato su DNS e consente di distribuire il traffico all'interno e tra aree di Azure. Anziché dirigere l'utente verso un'istanza specifica del nostro front end web, Azure Traffic Manager può instradare gli utenti in base a una serie di caratteristiche:

* **Priorità** -si specifica un elenco ordinato di istanze front-end. Se quella con la priorità più alta non è disponibile, Gestione traffico instraderà l'utente verso quella successiva disponibile.
* **Ponderato** -è necessario impostare un peso rispetto a ogni istanza front-end. Gestione traffico distribuisce quindi il traffico in base a tali rapporti definiti.
* **Prestazioni** -gestione traffico di Azure consente di instradare gli utenti verso l'istanza front-end più vicina in base alla latenza di rete.
* **Geografico** -è possibile configurare le aree geografiche per le distribuzioni front-end, instradando gli utenti in base ai mandati di sovranità dei dati o alla localizzazione del contenuto.

I profili di Gestione traffico possono essere anche annidati. Innanzitutto, è possibile instradare gli utenti in diverse aree geografiche (ad esempio, Europa e Australia) mediante il routing geografico, quindi instradarli verso distribuzioni front-end locali usando il metodo di routing delle prestazioni.

Si consideri che Lamna Healthcare ha distribuito un front-end web in Europa occidentale e Australia. Si supponga che abbia distribuito il Database SQL di Azure con la distribuzione primaria nell'area Europa occidentale e una replica di lettura in Australia orientale. Si supponga inoltre che l'applicazione possa connettersi all'istanza di SQL locale per le query di lettura.

Il team distribuisce un'istanza di Gestione traffico di Microsoft Azure in modalità prestazioni e aggiunge le due istanze front-end come profili di Gestione traffico. In qualità di utente finale, si viene indirizzati verso un nome di dominio personalizzato (ad esempio, lamnahealthcare.com) che instrada verso Gestione traffico di Microsoft Azure. Gestione traffico di Microsoft Azure restituisce quindi il nome DNS del front-end dell'Europa occidentale o dell'Australia orientale in base alle migliori prestazioni di latenza di rete.

È importante notare che questo bilanciamento del carico è gestito solo tramite DNS, non vi è alcun bilanciamento del carico in linea o memorizzazione nella cache in questa fase, Gestione traffico restituisce semplicemente il nome DNS del front end più vicino all'utente.

### <a name="use-cdn-to-cache-content-close-to-users"></a>Usare la rete CDN per memorizzare nella cache il contenuto vicino agli utenti

Il sito web userà probabilmente una qualche forma di contenuto statico (pagine intere o risorse come immagini e video). Questo contenuto potrebbe essere recapitato agli utenti più velocemente usando una rete per la distribuzione di contenuti (CDN) come Azure CDN. 

Quando Lamna distribuisce contenuti sulla rete CDN di Azure, questi elementi vengono copiati in più server in tutto il mondo. Si supponga che uno di tali elementi sia un video distribuito dall'archiviazione BLOB: `HowToCompleteYourBillingForms.MP4`. Il team quindi configurerà il sito web in modo che il link di ogni utente al video faccia effettivamente riferimento al server edge CDN più vicino, anziché alla memorizzazione BLOB. Questo approccio avvicina i contenuti alla destinazione, riducendo la latenza e migliorando l'esperienza utente.

![CDNExample](../media/cdnSketch.png)

Le reti per la distribuzione di contenuti _possono_ essere usate anche per ospitare contenuto dinamico memorizzato nella cache. Tuttavia, è necessaria una considerazione aggiuntiva, poiché il contenuto memorizzato nella cache potrebbe non essere aggiornato rispetto all'origine. La scadenza del contesto può essere controllata impostando una durata (TTL). Se la durata (TTL) è eccessiva, potrebbe essere visualizzato il contenuto non aggiornato e la cache dovrà essere ripulita.

Un modo per gestire il contenuto memorizzato nella cache è tramite una funzionalità denominata **accelerazione sito dinamico**, che può migliorare le prestazioni delle pagine web con contenuto dinamico. L'accelerazione sito dinamico può anche fornire un percorso di bassa latenza a servizi aggiuntivi nella soluzione usata (ad esempio, un endpoint API).

### <a name="use-expressroute-for-connectivity-from-on-premises-to-azure"></a>Usare ExpressRoute per la connettività da locale ad Azure

È importante anche l'ottimizzazione della connettività di rete dall'ambiente locale ad Azure. Per gli utenti che si connettono ad applicazioni, sia che siano ospitati su macchine virtuali che su offerte PaaS come Azure App Service, è auspicabile assicurarsi che abbiano la migliore connessione alle applicazioni in uso. 

È sempre possibile usare la rete Internet pubblica per collegare gli utenti ai propri servizi, ma le prestazioni di Internet possono variare e possono essere influenzate da problemi esterni. Inoltre, si potrebbe non voler esporre tutti i servizi tramite Internet e preferire una connessione privata alle risorse di Azure.

Una possibilità è offerta anche dal VPN da sito a sito in Internet, tuttavia per le architetture a velocità effettiva elevata, l'overhead VPN e la variabilità di Internet possono aumentare notevolmente la latenza.

Può venire in aiuto Azure ExpressRoute. ExpressRoute è una connessione privata dedicata tra la rete e Azure, che offre prestazioni garantite e garantisce che gli utenti finali abbiano il percorso migliore verso tutte le risorse di Azure.

![ExpressRoute](../media/expressroute-connection-overview.png)

Esaminando ancora una volta lo scenario di Lamna, questo ente decide di migliorare ulteriormente l'esperienza utente finale per gli utenti che visitano le loro strutture effettuando il provisioning di un circuito ExpressRoute sia in Australia orientale che in Europa occidentale, offrendo agli utenti finali una connessione diretta al loro sistema di prenotazione e garantendo la latenza più bassa possibile per la loro applicazione.

## <a name="summary"></a>Riepilogo

È importante considerare l'impatto della latenza di rete sull'architettura per garantire le migliori prestazioni possibili per gli utenti finali. Sono state esaminate alcune opzioni per ridurre la latenza di rete tra gli utenti finali e Azure e tra le risorse di Azure. Successivamente, si è passati a esaminare l'ottimizzazione delle prestazioni di archiviazione.