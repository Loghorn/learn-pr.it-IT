Molte applicazioni usano un modello pubblicazione-sottoscrizione per notificare ai componenti distribuiti che si è verificato qualcosa o che è stato modificato un oggetto. Si supponga di avere un'applicazione per la condivisione di musica con un'API Web in esecuzione in Azure. Quando un utente carica un nuovo brano, è necessario inviare una notifica a tutte le app per dispositivi mobili installate nei dispositivi degli utenti di tutto il mondo interessati a tale genere.

In questa architettura non è necessario che l'origine di pubblicazione del file audio conosca i sottoscrittori interessati alla musica condivisa. Si vuole anche stabilire una relazione uno-a-molti in cui si possono avere più sottoscrittori che possono facoltativamente decidere se sono interessati a questo nuovo brano. Griglia di eventi di Azure è la soluzione ideale per questo genere di architettura.

## <a name="what-is-azure-event-grid"></a>Che cos'è Griglia di eventi di Azure?
Griglia di eventi di Azure è un servizio di routing di eventi completamente gestito in esecuzione in Azure Service Fabric. Griglia di eventi distribuisce gli _eventi_ provenienti da origini diverse, come account di archiviazione BLOB di Azure o Servizi multimediali di Azure, a gestori diversi, ad esempio Funzioni di Azure o webhook. Il servizio Griglia di eventi è stato creato per semplificare la creazione di applicazioni basate su eventi e serverless in Azure.

Griglia di eventi supporta la maggior parte dei servizi di Azure come origine di pubblicazione o sottoscrittore e questa soluzione può essere usata con servizi di terze parti. Fornisce un sistema di messaggistica scalabile dinamicamente e a basso costo, che consente alle origini di pubblicazione di inviare ai sottoscrittori una notifica di modifica dello stato. Nell'immagine seguente viene illustrata la ricezione da parte di Griglia di eventi di Azure di messaggi provenienti da origini diverse e la loro distribuzione a gestori eventi in base alla sottoscrizione.

Ci sono alcuni concetti in Griglia di eventi di Azure relativi alla connessione di un'origine a un sottoscrittore:

1. **Eventi**: ciò che è successo.
1. **Origini eventi**: dove si è verificato l'evento.
1. **Argomenti**: endpoint a cui le origini di pubblicazione inviano gli eventi.
1. **Sottoscrizioni di eventi**: endpoint o meccanismo predefinito per il routing degli eventi, a volte a più gestori. Le sottoscrizioni vengono usate anche dai gestori per filtrare in modo intelligente gli eventi in ingresso.
1. **Gestori eventi**: l'app o il servizio che reagisce all'evento.

![Figura che mostra una Griglia di eventi di Azure che si trova tra più origini eventi e più gestori eventi. Le origini eventi inviano gli eventi a Griglia di eventi di Azure, che inoltra quelli rilevanti ai sottoscrittori. Griglia di eventi usa gli argomenti per decidere quali eventi inviare a quali gestori. Le origini eventi contrassegnano ogni evento con uno o più argomenti e i gestori eventi sottoscrivono gli argomenti a cui sono interessati.](../media/4-event-grid.png)

### <a name="what-is-an-event"></a>Che cos'è un evento?
Gli **eventi** sono i messaggi di dati che passano attraverso Griglia di eventi e descrivono cosa è avvenuto. Ogni evento è indipendente, può avere dimensioni fino a 64 KB e contiene diverse informazioni basate su uno schema definito da Griglia di eventi:

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
| **topic** | Percorso completo della risorsa all'origine evento. Questo valore viene fornito da Griglia di eventi. |
| **subject** | Percorso dell'oggetto dell'evento definito dall'origine di pubblicazione. |
| **id** | Identificatore univoco per l'evento. |
| **eventType** | Uno dei tipi di evento registrati per l'origine evento. Questo valore può essere usato per la creazione di filtri, ad esempio `CustomerCreated`, `BlobDeleted`, `HttpRequestReceived` e così via. |
| **eventTime** | Ora di generazione dell'evento in base all'ora UTC del provider. |
| **data** | Informazioni specifiche rilevanti per il tipo di evento. Un evento di creazione di un nuovo file in Archiviazione di Azure, ad esempio, contiene i dettagli sul file, come il valore `lastTimeModified`. Un evento di Hub eventi include invece l'URL del file di acquisizione. Questo campo è facoltativo. |
| **dataVersion** | Versione dello schema dell'oggetto dati. La versione dello schema è definita dall'origine di pubblicazione. |
| **metadataVersion** | Versione dello schema dei metadati dell'evento. Lo schema delle proprietà di primo livello è definito da Griglia di eventi. Questo valore viene fornito da Griglia di eventi. |

> [!TIP]
> Griglia di eventi invia un evento per indicare che qualcosa si è verificato o che c'è stata una modifica. L'_oggetto effettivo_ modificato, tuttavia, non fa parte dei dati dell'evento. Viene invece spesso passato un URL o un identificatore per fare riferimento all'oggetto modificato.

### <a name="what-is-an-event-source"></a>Che cos'è un'origine evento?
Le origini eventi sono responsabili dell'invio degli eventi a Griglia di eventi. Ogni origine evento è correlata a uno o più tipi di evento. Archiviazione di Azure è, ad esempio, l'origine degli eventi creati dai BLOB. Hub IoT è l'origine degli eventi creati dai dispositivi. L'applicazione è l'origine degli eventi personalizzati definiti dall'utente. Le origini eventi verranno analizzate in modo più dettagliato più avanti.

> [!NOTE]
> L'hub eventi di Azure definisce anche il concetto di origine di pubblicazione degli eventi. Un'origine di pubblicazione per un hub eventi è l'utente o l'organizzazione che decide di inviare eventi a Griglia di eventi. Microsoft pubblica ad esempio gli eventi per diversi servizi di Azure. È possibile pubblicare eventi dalla propria applicazione. Le organizzazioni che ospitano i servizi all'esterno di Azure possono pubblicare eventi tramite Griglia di eventi. Questo concetto viene spesso confuso con quello di origine evento. L'origine evento completamente definita include l'origine di pubblicazione e il servizio specifico che genera l'evento per tale origine di pubblicazione. In questo caso i termini "origine di pubblicazione" e "origine evento" vengono usati in modo intercambiabile per rappresentare l'entità che invia il messaggio all'hub eventi.

### <a name="what-is-an-event-topic"></a>Che cos'è un argomento di un evento?
Gli argomenti degli eventi classificano gli eventi in gruppi. Gli argomenti sono rappresentati da un endpoint pubblico e rappresentano la posizione di _destinazione_ degli eventi inviati dall'origine evento. Quando si progetta l'applicazione, è possibile decidere il numero di argomenti da creare. Per le soluzioni di dimensioni maggiori è possibile creare un argomento personalizzato per ogni categoria di eventi correlati, mentre le soluzioni più piccole potrebbero inviare tutti gli eventi a un singolo argomento. Ad esempio, si consideri un'applicazione che invia gli eventi correlati alla modifica degli account utente e all'elaborazione degli ordini. È improbabile che un gestore eventi richieda entrambe le categorie di eventi. Creare due argomenti personalizzati e consentire ai gestori eventi di sottoscrivere l'argomento di interesse. I sottoscrittori di eventi possono filtrare in base ai tipi di eventi desiderati di un argomento specifico.

Gli argomenti sono suddivisi in argomenti di **sistema** e argomenti **personalizzati**.

#### <a name="system-topics"></a>Argomenti di sistema
Gli argomenti di sistema sono argomenti predefiniti forniti dai servizi di Azure. Gli argomenti di sistema non vengono visualizzati nella sottoscrizione di Azure perché sono di proprietà dell'origine di pubblicazione, ma possono essere sottoscritti. Per eseguire la sottoscrizione, fornire le informazioni sulla risorsa da cui si vuole ricevere gli eventi. È possibile sottoscrivere gli eventi di una risorsa se si ha accesso alla risorsa.

#### <a name="custom-topics"></a>Argomenti personalizzati
Gli argomenti personalizzati sono argomenti di applicazioni e di terze parti. Quando l'utente crea o gli viene assegnato l'accesso a un argomento personalizzato, tale argomento personalizzato viene visualizzato nella sottoscrizione.

### <a name="what-is-an-event-subscription"></a>Che cos'è una sottoscrizione di eventi?
Le sottoscrizioni di eventi definiscono gli eventi relativi a un argomento che un gestore eventi vuole ricevere. Una sottoscrizione consente anche di filtrare gli eventi in base al tipo o all'oggetto, in modo che un gestore eventi riceva solo gli eventi pertinenti.

### <a name="what-is-an-event-handler"></a>Che cos'è un gestore eventi?
Un gestore eventi, detto anche "sottoscrittore" di eventi, è qualsiasi componente (applicazione o risorsa) che può ricevere eventi da Griglia di eventi. Funzioni di Azure, ad esempio, può eseguire codice in risposta al nuovo brano che viene aggiunto all'account di archiviazione BLOB. I sottoscrittori possono decidere quali eventi vogliono gestire e Griglia di eventi invierà una notifica a ogni sottoscrittore interessato quando è disponibile un nuovo evento, senza bisogno di polling.

## <a name="types-of-event-sources"></a>Tipi di origini eventi
Gli eventi possono essere generati dai tipi di risorse di Azure seguenti:

- **Sottoscrizioni e gruppi di risorse di Azure**: le sottoscrizioni e i gruppi di risorse generano eventi correlati alle operazioni di gestione in Azure. Ad esempio, quando un utente crea una macchina virtuale, questa origine genera un evento.
- **Registro contenitori**: il servizio Registro contenitori di Azure genera eventi quando vengono aggiunte, rimosse o modificate immagini nel registro.
- **Hub eventi**: l'hub eventi può essere usato per elaborare e archiviare eventi da un'ampia gamma di origini dati, in genere correlate a registrazione o telemetria. L'hub eventi può generare eventi in Griglia di eventi quando vengono acquisiti i file.
- **Bus di servizio**: il bus di servizio può generare eventi in Griglia di eventi quando ci sono messaggi attivi senza listener attivi.
- **Account di archiviazione**: gli account di archiviazione possono generare eventi quando gli utenti aggiungono BLOB, file, voci di tabella o messaggi in coda. È possibile usare sia account BLOB che account per utilizzo generico V2 come origini eventi.
- **Servizi multimediali**: Servizi multimediali ospita contenuti multimediali audio e video e offre funzionalità avanzate di gestione per i file multimediali. Servizi multimediali può generare eventi all'avvio o al completamento di un processo di codifica in un file video.
- **Hub IoT di Azure**: l'hub IoT comunica con i dispositivi IoT, da cui raccoglie dati di telemetria. Può generare eventi ogni volta che arrivano comunicazioni di questo tipo.
- **Eventi personalizzati**: gli eventi personalizzati possono essere generati tramite l'API REST o con Azure SDK in Java, GO, .NET, Node, Python e Ruby. È ad esempio possibile creare un evento personalizzato nella funzionalità App Web di Servizio app di Azure. Ciò può avvenire nel ruolo di lavoro, quando un messaggio viene prelevato da una coda di archiviazione.

Questa profonda integrazione con svariate origini eventi all'interno di Azure garantisce che Griglia di eventi possa distribuire eventi correlati a quasi tutte le risorse di Azure.

## <a name="event-handlers"></a>Gestori eventi
I tipi di oggetto seguenti in Azure possono ricevere e gestire gli eventi da Griglia di eventi:

- **Funzioni di Azure**: una funzione di Azure è costituita da codice personalizzato che viene eseguito in Azure senza un contenitore o un server virtuale host. Usare una funzione di Azure come gestore eventi quando si vuole scrivere una risposta personalizzata per l'evento.
- **Webhook**: un webhook è un'API Web che implementa un'architettura push.
- **App per la logica di Azure**: un'app per la logica di Azure ospita un processo di business come un flusso di lavoro.
- **Microsoft Flow**: anche Flow ospita flussi di lavoro, ma è più facile da usare per il personale non tecnico.

## <a name="should-you-use-event-grid"></a>È consigliabile usare Griglia di eventi?
Usare Griglia di eventi quando sono necessarie queste caratteristiche:

- **Semplicità**: in Griglia di eventi è molto semplice connettere le origini ai sottoscrittori.
- **Filtro avanzato**: le sottoscrizioni possono controllare in modo preciso gli eventi ricevuti da un argomento.
- **Ampia estensione**: è possibile sottoscrivere un numero illimitato di endpoint per gli stessi eventi e argomenti.
- **Affidabilità**: Griglia di eventi ritenta il recapito degli eventi per 24 ore per ogni sottoscrizione.
- **Pagamento per evento**: si paga solo per il numero di eventi trasmessi.

Griglia di eventi è un sistema di distribuzione di eventi semplice, ma versatile. Usarlo per recapitare eventi separati ai sottoscrittori, che riceveranno tali eventi in modo rapido e affidabile. Resta ancora da esaminare il modello di messaggistica in cui si recapita un _flusso_ di eventi di grandi dimensioni. In questo scenario Griglia di eventi non è la soluzione ideale perché è progettato per il recapito di un evento alla volta. È invece necessario usare un altro servizio di Azure: Hub eventi.