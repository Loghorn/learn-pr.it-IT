Il team ha deciso di sfruttare le funzionalità di Hub eventi di Azure per gestire ed elaborare i crescenti volumi di transazioni gestiti dal sistema.

Un hub eventi è una risorsa di Azure, quindi il primo passaggio consiste nel creare un nuovo hub in Azure e configurarlo per soddisfare gli specifici requisiti delle applicazioni.

## <a name="what-is-an-azure-event-hub"></a>Che cos'è un hub eventi di Azure?

Hub eventi di Azure è un servizio di elaborazione di eventi basato sul cloud, in grado di ricevere ed elaborare milioni di eventi al secondo. Hub eventi opera come una porta principale per una pipeline di eventi, poiché riceve i dati in ingresso e li archivia finché non sono disponibili le risorse di elaborazione.

Un'entità che invia dati a Hub eventi è denominata *autore*, mentre un'entità che legge i dati da Hub eventi è denominata *consumer* o *sottoscrittore*. Hub eventi di Azure è posizionato tra queste due entità per suddividere la produzione (dall'autore) e l'uso (verso il sottoscrittore) di un flusso di eventi. Questa separazione consente di gestire scenari in cui il tasso di produzione di eventi è notevolmente superiore all'uso.

![Gli autori inviano più eventi a un singolo hub eventi e rendono disponibili i dati ai sottoscrittori](../media-draft/2-event-hub-overview.png "Panoramica di Hub eventi")

### <a name="events"></a>Eventi

Un **evento** è un pacchetto di piccole dimensioni che contiene una notifica di una modifica, senza tutti i dettagli sull'elemento che è stato modificato. È possibile configurare un'applicazione consumer per avviare una query allo scopo di ottenere i dettagli, ma questo non è un requisito e Hub eventi non gestisce questa operazione. Gli eventi possono essere pubblicati singolarmente o in batch, ma una pubblicazione (singola o in batch) non può superare i 256 KB.

Al contrario, nell'accodamento dei messaggi (nel bus di servizio di Azure) il **messaggio** contiene sia i dati che l'evento e l'autore del messaggio si aspetta che il consumer elabori il messaggio in un modo specifico.

### <a name="publishers-and-subscribers"></a>Autori e sottoscrittori

Un autore di eventi è qualsiasi applicazione o dispositivo che può inviare dati di eventi tramite HTTPS o Advance Message Queueing Protocol (AQMP) 1.0. 

Per gli autori che inviano dati di frequente, AMQP offre le migliori prestazioni. Tuttavia, presenta un maggiore sovraccarico iniziale per le sessioni, perché è prima necessario configurare un socket bidirezionale persistente e TLS (Transport Layer Security) o SSL/TLS. 

Per una pubblicazione più intermittente, è preferibile HTTPS. Anche se HTTPS richiede un sovraccarico SSL aggiuntivo per ogni richiesta, non presenta il sovraccarico di inizializzazione della sessione.

> [!NOTE] 
> Anche le applicazioni esistenti basate su Kafka, che usano client Apache Kafka 1.0 e versioni successive, possono operare come autori per Hub eventi.

I sottoscrittori di eventi sono applicazioni che usano uno dei due metodi supportati a livello di programmazione per ricevere ed elaborare gli eventi da un hub eventi.

- **EventHubReceiver**: un metodo semplice che include opzioni di gestione limitate.
- **EventProcessorHost**: un metodo efficiente che verrà usato più avanti in questo modulo.

### <a name="consumer-groups"></a>Gruppi di consumer

Un **gruppo di consumer** di un hub eventi rappresenta una visualizzazione specifica di un flusso di dati dell'hub eventi. Usando gruppi di consumer distinti, più applicazioni sottoscrittore possono elaborare un flusso di eventi in modo indipendente e senza influire sulle altre applicazioni. Tuttavia, l'uso di più gruppi di consumer non è un requisito e per molte applicazioni il gruppo di consumer predefinito è sufficiente.

### <a name="pricing"></a>Prezzi

Sono disponibili tre piani tariffari per Hub eventi di Azure: Basic, Standard e Dedicato. I livelli presentano differenze in termini di connessioni supportate, numero di gruppi di consumer disponibili e velocità effettiva. Quando si usa l'interfaccia della riga di comando di Azure per creare uno spazio dei nomi di Hub eventi, se non si specifica un piano tariffario, viene assegnato il valore predefinito **Standard** (20 gruppi di consumer, 1000 connessioni negoziate).

## <a name="creating-and-configuring-a-new-azure-event-hubs"></a>Creazione e configurazione di un nuovo hub eventi di Azure

La creazione e la configurazione di un nuovo hub eventi di Azure comprende due passaggi principali. Il primo passaggio consiste nel definire lo **spazio dei nomi** di Hub eventi. Il secondo passaggio consiste nel creare un hub eventi nello spazio dei nomi.

### <a name="defining-an-event-hubs-namespace"></a>Definizione di uno spazio dei nomi di Hub eventi

Uno spazio dei nomi di Hub eventi è un'entità contenitore per la gestione di uno o più hub eventi. 

La creazione di uno spazio dei nomi di Hub eventi in genere comporta quanto segue:

1. Definizione delle impostazioni a livello di spazio dei nomi. Alcune impostazioni, come la capacità dello spazio dei nomi (configurata tramite le **unità elaborate**), il piano tariffario e le metriche delle prestazioni, sono definite a livello di spazio dei nomi. Queste sono applicabili per tutti gli hub eventi nello spazio dei nomi. Se non si definiscono queste impostazioni, viene usato un valore predefinito: *1* per la capacità e *Standard* per il piano tariffario.

    Una volta impostate le unità elaborate, non è possibile modificarle. È necessario bilanciare la configurazione rispetto alle aspettative per il budget di Azure. È possibile valutare la configurazione di diversi hub eventi per differenti requisiti di velocità effettiva. Ad esempio, se si dispone di un'applicazione per i dati di vendita e si prevede di usare due hub eventi (uno per la raccolta con una velocità effettiva elevata di informazioni di telemetria in tempo reale sui dati di vendita e uno per la raccolta dei log eventi con una minore frequenza), potrebbe essere utile usare uno spazio dei nomi distinto per ogni hub. In questo modo, è sufficiente configurare (ed effettuare il pagamento per) la capacità con velocità effettiva elevata nell'hub di telemetria.

1. Selezione di un nome univoco per lo spazio dei nomi. Lo spazio dei nomi è accessibile all'URL: *_spazio dei nomi_.servicebus.windows.net*

1. Definizione delle proprietà facoltative seguenti:

    - Abilita Kafka. Questa opzione consente alle applicazioni Kafka di pubblicare eventi nell'hub eventi.
    - Imposta la ridondanza della zona per questo spazio dei nomi. La ridondanza della zona replica i dati tra data center distinti con infrastrutture di alimentazione, rete e raffreddamento indipendenti.
    - Abilita aumento automatico e Numero massimo di unità elaborate per l'aumento automatico. L'aumento automatico offre un'opzione per la scalabilità, aumentando il numero di unità elaborate fino a un valore massimo. Questo è utile per evitare la limitazione delle richieste nelle situazioni in cui le velocità dei dati in ingresso o in uscita superano il numero di unità elaborate attualmente impostato.

### <a name="azure-cli-commands-for-creating-an-event-hubs-namespace"></a>Comandi dell'interfaccia della riga di comando di Azure per la creazione di uno spazio dei nomi di Hub eventi

Per creare un nuovo spazio dei nomi di Hub eventi, usare i comandi seguenti:

1. Un gruppo di risorse di Azure è un contenitore con risorse di Azure correlate per facilitare la gestione. Se necessario, creare un nuovo gruppo di risorse per l'hub eventi.

    ```azurecli
    az group create --name <resource group name> --location <location>
    ```

1. Creare lo spazio dei nomi di Hub eventi con lo stesso gruppo di risorse e la stessa posizione del passaggio precedente.

    ```azurecli
    az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

1. Tutti gli hub eventi nello stesso spazio dei nomi di Hub eventi condividono credenziali di connessione comuni. Queste credenziali saranno necessarie per configurare le applicazioni per inviare e ricevere messaggi tramite l'hub eventi. Usare il comando seguente per restituire la stringa di connessione per lo spazio dei nomi di Hub eventi, con il gruppo di risorse e il nome dello spazio dei nomi di Hub eventi usati in precedenza.

    ```azurecli
    az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

### <a name="configuring-a-new-event-hub"></a>Configurazione di un nuovo hub eventi

Dopo aver creato lo spazio dei nomi di Hub eventi, è possibile creare un hub eventi. Quando si crea un nuovo hub eventi, esistono alcuni parametri obbligatori.

Per creare un hub eventi, sono necessari i parametri seguenti:

- **Nome hub eventi**: nome dell'hub eventi. Deve essere univoco all'interno della sottoscrizione e:
  - Compreso tra 1 e 50 caratteri.
  - Deve contenere solo lettere, numeri, punti, trattini e caratteri di sottolineatura.
  - Deve iniziare e terminare con una lettera o un numero.
- **Numero di partizioni**: numero di partizioni necessarie in un hub eventi (compreso tra 2 e 32). Deve essere direttamente correlato al numero di consumer concorrenti previsto. Questa opzione non può essere modificata dopo la creazione dell'hub. La partizione separa il flusso dei messaggi, in modo che per le applicazioni consumer o ricevitore sia sufficiente leggere uno specifico subset del flusso di dati. Se non è definito, il valore predefinito è *4*.
- **Conservazione messaggi**: numero di giorni (compreso tra 1 e 7) per cui rimarranno disponibili i messaggi, se dovesse essere necessario riprodurre il flusso dei dati per qualsiasi motivo. Se non è definito, il valore predefinito è *7*.

È anche possibile configurare un hub eventi per trasmettere i dati a un account di Archiviazione BLOB di Azure o Azure Data Lake Store.

### <a name="azure-cli-commands-for-creating-an-event-hub"></a>Comandi dell'interfaccia della riga di comando di Azure per la creazione di un hub eventi

Per creare un nuovo hub eventi, usare i comandi seguenti:

1. Creare l'hub eventi con lo stesso gruppo di risorse e la stessa posizione usati per la creazione dello spazio dei nomi.

    ```azurecli
    az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

1. Visualizzare i dettagli dell'hub eventi nello spazio dei nomi, con il gruppo di risorse, il nome dell'hub eventi e il nome dello spazio dei nomi usati in precedenza.

    ```azurecli
    az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>

## Summary

To deploy Azure Event Hubs, you must configure an Event Hubs namespace and then configure the event hub itself. In the next section, you will go through the detailed configuration steps to create a new namespace and event hub.