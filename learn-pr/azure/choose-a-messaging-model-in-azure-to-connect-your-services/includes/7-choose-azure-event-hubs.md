Esistono alcune applicazioni che generano un enorme numero di eventi da quasi altrettante origini. Spesso viene usato il termine "Big Data" per fare riferimento a queste situazioni, che richiedono un'infrastruttura speciale per gestirle.

Si supponga di lavorare per Contoso Aircraft Engines. I motori prodotti dall'azienda includono centinaia di sensori. Prima che un aereo possa essere operativo ogni giorno, i motori vengono connessi a un test harness e collaudati. Inoltre, i dati di volo memorizzati nella cache vengono trasmessi quando l'aereo viene connesso alla strumentazione di terra.

È necessario usare i dati cronologici dei sensori per individuare modelli nelle letture dei sensori che indichino un potenziale malfunzionamento del motore a breve termine. Si vuole che le letture dei sensori in tempo reale vengano confrontate con questi modelli di errore. È quindi possibile avvisare gli utenti quasi in tempo reale se un motore presenta letture preoccupanti.

## <a name="what-is-azure-event-hubs"></a>Che cos'è l'hub eventi di Azure

Un hub eventi di Azure è un intermediario per il modello di comunicazione basato su pubblicazione-sottoscrizione, ma a differenza di Griglia di eventi è ottimizzato per velocità effettive molto elevate, un numero elevato di origini di pubblicazione, la sicurezza e la resilienza.

Mentre Griglia di eventi si integra più o meno perfettamente nel modello di pubblicazione-sottoscrizione semplicemente perché gestisce le sottoscrizioni e indirizza le comunicazioni ai sottoscrittori, hub eventi consente di eseguire alcuni servizi aggiuntivi che in un certo modo lo rendono più simile a un bus di servizio o a una coda di messaggi, piuttosto che a un semplice strumento di trasmissione di eventi.

#### <a name="partitions"></a>Partitions ####
Quando l'hub eventi riceve le comunicazioni, le divide in partizioni. Le partizioni sono buffer in cui vengono salvate le comunicazioni. Considerati i buffer, gli eventi non sono completamente temporanei e un evento non viene perso solo perché un sottoscrittore è occupato o anche non online. Il sottoscrittore può sempre usare il buffer per mettersi al passo. Per impostazione predefinita gli eventi rimangono nel buffer 24 ore prima di scadere automaticamente.

I buffer sono chiamati partizioni perché i dati vengono divisi tra di loro. Ogni hub eventi ha almeno due partizioni e ogni partizione ha un set separato di sottoscrittori.

#### <a name="capture"></a>Acquisizione ####
Un hub eventi può inviare tutti gli eventi immediatamente ad Azure Data Lake o ad Archiviazione BLOB, come soluzioni economiche di archiviazione permanente.

#### <a name="authentication"></a>Authentication ####
Tutte le origini di pubblicazione vengono autenticate e ricevono un token. Questo significa che l'hub eventi può accettare eventi da dispositivi esterni e app per dispositivi mobili senza preoccuparsi che dati illeciti possano compromettere l'analisi. 

## <a name="using-azure-event-hub"></a>Uso dell'hub eventi di Azure

L'hub eventi include il supporto per l'invio di pipeline di flussi di eventi ad altri servizi di Azure. È possibile usarlo con Analisi di flusso, ad esempio, per eseguire analisi complesse dei dati quasi in tempo reale con la possibilità di correlare più eventi e di individuare modelli. In questo scenario, Analisi di flusso sarebbe considerato un sottoscrittore.

Per l'esempio dei motori degli aerei, l'architettura verrà configurata in modo che l'hub eventi esegua l'autenticazione delle comunicazioni dai motori. Verrà quindi impostato l'uso di Capture per salvare tutti i dati in Azure Data Lake e in un secondo momento sarà possibile usare tutti i dati per ripetere il training e migliorare i modelli di machine learning. Infine, i flussi di eventi verranno prelevati da sottoscrittori di Analisi di flusso di Azure. Analisi di flusso userà il modello di machine learning per individuare modelli nei dati dei sensori che potrebbero indicare un problema.

Dato che sono disponibili varie partizioni e che ogni motore invia tutti i relativi dati a una sola partizione, ogni istanza del sottoscrittore di Analisi di flusso deve gestire solo un subset dei dati complessivi, invece di dover eseguire operazioni di filtro e correlazione su tutti.

## <a name="choose-event-grid-or-event-hub"></a>Scegliere Griglia di eventi o l'hub eventi

Entrambi supportano la semantica *At-Least-Once*.

Se è necessaria una semplice infrastruttura di pubblicazione-sottoscrizione degli eventi con origini di pubblicazione attendibili (ad esempio il proprio server Web), è consigliabile scegliere Griglia di eventi di Azure.

### <a name="consider-event-hub-if"></a>Prendere in considerazione l'hub eventi se:
* È necessario supportare l'autenticazione di un numero elevato di origini di pubblicazione
* È necessario salvare un flusso di eventi in Azure Data Lake o in Archiviazione BLOB
* È necessario eseguire operazioni di aggregazione o analisi sul flusso di eventi
* Sono necessarie funzionalità di messaggistica affidabili o resilienza 