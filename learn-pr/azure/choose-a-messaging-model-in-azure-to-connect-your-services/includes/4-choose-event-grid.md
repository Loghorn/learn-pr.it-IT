Molte applicazioni utilizzano una pubblicazione-sottoscrizione modello per notificare a componenti distribuiti, che è accaduto qualcosa, o che un oggetto è stato modificato. Si supponga di avere un'applicazione per la condivisione di musica con un'API Web in esecuzione in Azure. Quando un utente carica un nuovo brano, è necessario notificare a tutte le App per dispositivi mobili installate nei dispositivi degli utenti interessati a tale genere tutto il mondo.

In questa architettura, il server di pubblicazione del file audio non è necessario sapere riguardo a qualsiasi aspetto dei sottoscrittori interessati la musica condivisa. Inoltre, si vuole avere una relazione uno-a-molti in cui possiamo avere più sottoscrittori possono scegliere, facoltativamente, se sono interessati a questa nuova canzone. Griglia di eventi di Azure è la soluzione ideale per questo genere di architettura.

## <a name="what-is-azure-event-grid"></a>Che cos'è una griglia di eventi di Azure?
Griglia di eventi di Azure è un servizio di routing di eventi completamente gestito in esecuzione su Azure Service Fabric. Griglia di eventi distribuisce _eventi_ da origini diverse, ad esempio gli account di archiviazione Blob di Azure o servizi multimediali di Azure per i gestori diversi, ad esempio funzioni di Azure o i Webhook. Griglia di eventi è stata creata per rendere più semplice compilare basato su eventi e le applicazioni senza server in Azure.

Griglia di eventi supporta servizi di Azure più come server di pubblicazione o sottoscrittore e può essere utilizzata con i servizi di terze parti. Fornisce un sistema di messaggistica scalabile dinamicamente e a basso costo, che consente alle origini di pubblicazione di notificare una modifica di stato ai sottoscrittori. La figura seguente Mostra griglia di eventi di Azure riceve i messaggi provenienti da più origini e distribuirli ai gestori di eventi basati su sottoscrizione.

Esistono alcuni concetti che si connettono a un'origine a un sottoscrittore nella griglia di eventi di Azure:

1. **Eventi**: ciò che successo.
1. **Origini di eventi**: dove si è verificato l'evento.
1. **Argomenti**: l'endpoint a cui gli autori inviano gli eventi.
1. **Sottoscrizioni agli eventi**: l'endpoint o il meccanismo predefinito per instradare gli eventi, a volte a più gestori. Le sottoscrizioni vengono usate dai gestori anche per filtrare gli eventi in ingresso in modo intelligente.
1. **Gestori di eventi**: l'app o il servizio che reagisce all'evento.

![Un'illustrazione che Mostra griglia di eventi di Azure posizionato tra più origini evento e più gestori eventi. Le origini eventi per inviano eventi a griglia di eventi e griglia di eventi verranno inoltrati eventi rilevanti per i sottoscrittori. Griglia di eventi di usare gli argomenti per decidere quali eventi da inviare ai quali gestori. Ogni evento con uno o più argomenti di etichetta origini eventi e gestori eventi sottoscrivono gli argomenti che sono interessati.](../media-draft/5-event-grid.png)

### <a name="what-is-an-event"></a>Che cos'è un evento?
**Gli eventi** sono i messaggi di dati passando tramite griglia di eventi che descrivono cosa ha avuto luogo. Ogni evento è indipendente, può essere fino a 64 KB e contiene diverse parti di informazioni in base a uno schema definito da griglia di eventi:

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

| Campo | Descrizione |
|-------|-------------|
| **topic** | Il percorso della risorsa completo per l'origine dell'evento. Questo valore viene fornito da Griglia di eventi. |
| **subject** | Percorso dell'oggetto dell'evento definito dall'autore. |
| **id** | Identificatore univoco per l'evento. |
| **eventType** | Uno dei tipi di evento registrati per l'origine evento. Si tratta di un valore che è possibile creare filtri, ad esempio `CustomerCreated`, `BlobDeleted`, `HttpRequestReceived`e così via. |
| **eventTime** | Il tempo che è stato generato l'evento in base all'ora UTC del provider. |
| **dati** | Informazioni specifiche rilevanti per il tipo di evento. Un evento di creazione di un nuovo file in Archiviazione di Azure, ad esempio, contiene i dettagli sul file, quale il valore `lastTimeModified`. In alternativa, un evento di Hub eventi include l'URL del file di acquisizione. Questo campo è facoltativo. |
| **dataVersion** | Versione dello schema dell'oggetto dati. La versione dello schema è definita dall'editore. |
| **metadataVersion** | Versione dello schema dei metadati dell'evento. Lo schema delle proprietà di primo livello è definito da Griglia di eventi. Questo valore viene fornito da Griglia di eventi. |

> [!TIP]
> Griglia di eventi invia un evento per indicare qualcosa è si è verificato o modificato. Tuttavia, il _oggetto effettivo_ che è stata modificata non fa parte dei dati dell'evento. Al contrario, un URL o un identificatore spesso viene passato per riferimento l'oggetto modificato.

### <a name="what-is-an-event-source"></a>Che cos'è un'origine evento?
Le origini evento sono responsabili dell'invio degli eventi in Griglia di eventi. Ogni origine evento è correlata a uno o più tipi di evento. Archiviazione di Azure è ad esempio l'origine degli eventi di creazione di BLOB. Hub IoT è l'origine evento per gli eventi creati da dispositivo. L'applicazione è l'origine evento per gli eventi personalizzati definiti dall'utente. Esamineremo le origini di eventi in modo più dettagliato più avanti.

> [!NOTE]
> Hub eventi di Azure definisce anche il concetto di un autore di eventi. Un server di pubblicazione a Hub eventi è l'utente o l'organizzazione che decida di inviare eventi a griglia di eventi. Ad esempio, Microsoft pubblica gli eventi per diversi servizi di Azure. È possibile pubblicare eventi dalla propria applicazione. Le organizzazioni che ospitano i servizi all'esterno di Azure possono pubblicare eventi attraverso Griglia di eventi. Si ottiene spesso combinato con l'origine dell'evento. L'origine evento completamente definito è il server di pubblicazione e al servizio specifico che genera l'evento per tale server di pubblicazione. In questo caso, si userà "publisher" e "origine evento" in modo intercambiabile per rappresentare entità che invia il messaggio all'Hub eventi.

### <a name="what-is-an-event-topic"></a>Che cos'è un argomento di eventi?
Negli argomenti di evento classificano gli eventi in gruppi. Gli argomenti sono rappresentati da un endpoint pubblico e sono in cui l'origine eventi invia gli eventi _a_. Quando si progetta l'applicazione, è possibile decidere il numero di argomenti da creare. Soluzioni di maggiori dimensioni creerà un argomento personalizzato per ogni categoria di eventi correlati, mentre le soluzioni più piccole potrebbero inviare tutti gli eventi a un singolo argomento. Ad esempio, si consideri un'applicazione che invia gli eventi correlati alla modifica di account utente e all'elaborazione degli ordini. È improbabile che un gestore eventi richieda entrambe le categorie di eventi. Creare due argomenti personalizzati e consentire ai gestori di eventi di sottoscrivere all'argomento di interesse. I sottoscrittori di eventi possono filtrare per i tipi di evento desiderata da un argomento specifico.

Gli argomenti sono suddivisi in **system** gli argomenti, e **personalizzato** argomenti.

#### <a name="system-topics"></a>Argomenti di sistema
Gli argomenti di sistema sono argomenti predefiniti forniti dai servizi di Azure. Non verranno visualizzati argomenti di sistema nella sottoscrizione di Azure perché l'editore dispone degli argomenti, ma è possibile sottoscriversi a essi. Per eseguire la sottoscrizione, inserire le informazioni sulla risorsa da cui si desidera ricevere gli eventi. Purché si disponga di accesso alla risorsa, è possibile sottoscrivere gli eventi.

#### <a name="custom-topics"></a>Argomenti personalizzati
Gli argomenti personalizzati sono argomenti di applicazioni e di terze parti. Quando l'utente crea o gli viene assegnato l'accesso a un argomento personalizzato, tale argomento personalizzato viene visualizzato nella sottoscrizione.

### <a name="what-is-an-event-subscription"></a>Che cos'è una sottoscrizione di eventi?
Le sottoscrizioni di eventi consente di definire gli eventi relativi a un argomento un gestore eventi deve ricevere. Una sottoscrizione può anche filtrare eventi in base al tipo o oggetto, il modo è possibile garantire che un gestore eventi riceva solo gli eventi rilevanti.

### <a name="what-is-an-event-handler"></a>Che cos'è un gestore eventi?
Un gestore eventi (talvolta detto un evento "subscriber") è un componente (applicazione o risorsa) che può ricevere eventi da griglia di eventi. Funzioni di Azure, ad esempio, può eseguire un codice in risposta al nuovo brano che viene aggiunto all'account di archiviazione BLOB. I sottoscrittori possono decidere quali eventi vogliono gestire e Griglia di eventi invierà una notifica a ogni sottoscrittore interessato quando è disponibile un nuovo evento, senza bisogno di polling.

## <a name="types-of-event-sources"></a>Tipi di origini evento
Gli eventi possono essere generati dai tipi di risorse di Azure seguenti:

- **Sottoscrizioni di Azure e gruppi di risorse.** I gruppi di risorse e le sottoscrizioni generano eventi correlati alle operazioni di gestione in Azure. Ad esempio, quando un utente crea una macchina virtuale, questa origine genera un evento.
- **Registro contenitori.** Il servizio Registro contenitori di Azure genera eventi quando le immagini nel Registro di sistema vengono aggiunti, rimossi o modificate.
- **Hub eventi.** Hub eventi è utilizzabile per elaborare e archiviare gli eventi da un'ampia gamma di origini dati: in genere la registrazione o i dati di telemetria correlati. Hub eventi può generare eventi a griglia di eventi quando vengono acquisiti i file.
- **Bus di servizio.** Il bus di servizio può generare eventi a griglia di eventi quando sono presenti messaggi attivi con nessun listener attivi.
- **Account di archiviazione.** Gli account di archiviazione possono generare eventi quando gli utenti aggiungono BLOB, file, voci di tabella o messaggi in coda. È possibile utilizzare gli account blob sia ad account per l'uso generico V2 come origini di eventi.
- **Servizi multimediali.** Servizi multimediali ospita contenuti audio e video e offre funzionalità avanzate di gestione per i file multimediali. Servizi multimediali può generare eventi all'avvio o al completamento di un processo di codifica in un file video.
- **Hub IoT di Azure.** L'hub IoT comunica con i dispositivi IoT e raccoglie dati di telemetria da questi dispositivi. Può generare eventi ogni volta che arrivano comunicazioni di questo tipo.
- **Eventi personalizzati.** Gli eventi personalizzati possono essere generati tramite l'API REST o con Azure SDK in Java, GO, .NET, Node, Python e Ruby. È ad esempio possibile creare un evento personalizzato nella funzionalità App Web di Servizio app di Azure. Questa situazione può verificarsi nel ruolo di lavoro quando preleva un messaggio da una coda di archiviazione.

Questa stretta integrazione con le origini eventi diversi all'interno di Azure garantisce che griglia di eventi consente di distribuire gli eventi correlati a quasi tutte le risorse di Azure.

## <a name="event-handlers"></a>Gestori eventi
I tipi di oggetto seguenti in Azure possono ricevere e gestire gli eventi da Griglia di eventi:

- **Funzioni di Azure.** Una funzione di Azure è costituita da codice personalizzato che viene eseguito in Azure senza un contenitore o un server virtuale host. Usare una funzione di Azure come gestore eventi quando si vuole scrivere una risposta personalizzata per l'evento.
- **Webhook.** Un webhook è un'API web che implementa un'architettura di push.
- **App per la logica di Azure.** Un'app per la logica di Azure ospita un processo di business come un flusso di lavoro.
- **Microsoft Flow.** Anche Flow ospita flussi di lavoro, ma è più facile da usare per il personale non tecnico.

## <a name="should-you-use-event-grid"></a>È consigliabile usare Griglia di eventi?
Usare Griglia di eventi quando sono necessarie queste funzionalità:

- **Semplicità.** È facile connettere origini ai sottoscrittori in griglia di eventi.
- **Filtri avanzati.** Le sottoscrizioni possono controllare strettamente gli eventi ricevuti da un argomento.
- **Ampia estensione.** È possibile sottoscrivere un numero illimitato di endpoint per gli stessi eventi e gli argomenti.
- **Affidabilità.** Griglia di eventi esegue nuovi tentativi di recapito degli eventi per 24 ore per ogni sottoscrizione.
- **Pagamento per evento.** Si paga solo per il numero di eventi trasmessi.

## <a name="summary"></a>Riepilogo
Griglia di eventi è un sistema di distribuzione di eventi semplice, ma versatile. Usarlo per recapitare eventi separati ai sottoscrittori, che riceveranno tali eventi in modo rapido e affidabile. Resta ancora da esaminare il modello di messaggistica in cui si recapita un _flusso_ di eventi di grandi dimensioni. In questo scenario Griglia di eventi non è la soluzione ideale perché è progettato per il recapito di un evento alla volta. È invece necessario usare un altro servizio di Azure: Hub eventi.