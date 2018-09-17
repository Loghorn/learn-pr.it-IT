Molte applicazioni usano un modello pubblicazione-sottoscrizione per notificare ai componenti distribuiti che è accaduto qualcosa o che un oggetto è stato modificato. Si supponga di avere un'applicazione per la condivisione di musica con un'API Web in esecuzione in Azure. Quando un utente carica un nuovo brano, è necessario inviare notifica a tutte le app per dispositivi mobili installate nei dispositivi degli utenti in tutto il mondo, interessati al genere appena caricato.

In questa architettura non è necessario che l'origine di pubblicazione del file audio conosca i sottoscrittori interessati alla musica da condividere. Si vuole anche stabilire una relazione uno-a-molti in cui si possono avere più sottoscrittori che possono facoltativamente decidere se sono interessati a questo nuovo brano. Griglia di eventi di Azure è la soluzione ideale per questo genere di architettura.

## <a name="what-is-event-grid"></a>Che cos'è Griglia di eventi?
Griglia di eventi è un servizio che distribuisce gli _eventi_ provenienti da origini diverse, come account di archiviazione BLOB di Azure o servizi multimediali di Azure, a sottoscrittori diversi, ad esempio Funzioni di Azure o webhook.

### <a name="what-is-an-event-source"></a>Che cos'è un'origine evento?
Un'origine evento è qualsiasi componente che può generare un evento (talvolta definito "origine di pubblicazione"). Ad esempio, un utente potrebbe aggiungere un nuovo brano a un account di archiviazione BLOB tramite l'API Web. Questa operazione genera un evento che può essere distribuito da Griglia di eventi ai sottoscrittori di eventi interessati. Le origini di pubblicazione inviano gli eventi, ma non prevedono da che cosa verranno ricevuti o elaborati.

### <a name="what-is-an-event-subscriber"></a>Che cos'è un sottoscrittore di eventi?
Un sottoscrittore di eventi è qualsiasi componente che può ricevere eventi da Griglia di eventi. Funzioni di Azure, ad esempio, può eseguire un codice in risposta al nuovo brano che viene aggiunto all'account di archiviazione BLOB. I sottoscrittori possono decidere quali eventi vogliono gestire e Griglia di eventi invierà una notifica a ogni sottoscrittore interessato quando è disponibile un nuovo evento, senza bisogno di polling.

Griglia di eventi supporta la maggior parte dei servizi di Azure come origine di pubblicazione o sottoscrittore e può anche essere usato con servizi di terze parti. Offre un sistema di messaggistica scalabile dinamicamente e a basso costo, che consente ai server di pubblicazione di notificare una modifica di stato ai sottoscrittori. Nell'immagine seguente viene illustrata la ricezione da parte di Griglia di eventi di Azure di messaggi provenienti da origini diverse e la loro distribuzione a gestori di eventi in base alla sottoscrizione.

![Illustrazione con Griglia di eventi di Azure posizionato tra più origini evento e più gestori eventi. Le origini eventi inviano gli eventi a Griglia di eventi di Azure che inoltra quelli rilevanti ai sottoscrittori. Griglia di eventi usa gli argomenti per decidere quali eventi inviare a quali gestori. Le origini eventi contrassegnano ogni evento con uno o più argomenti e i gestori eseguono la sottoscrizione agli eventi ritenuti interessanti.](../media-draft/5-event-grid.png)

> [!NOTE]
> Griglia di eventi invia un evento per notificare una modifica ai sottoscrittori, ma l'_oggetto effettivo_ che è stato modificato non fa parte del recapito dell'evento.

## <a name="types-of-event-sources"></a>Tipi di origini evento
Gli eventi possono essere generati dai tipi di risorse di Azure seguenti:

- **Gruppi di risorse e sottoscrizioni di Azure.** I gruppi di risorse e le sottoscrizioni generano eventi correlati alle operazioni di gestione in Azure. Ad esempio, quando un utente crea una macchina virtuale, questa origine genera un evento.
- **Account di archiviazione.** Gli account di archiviazione possono generare eventi quando gli utenti aggiungono BLOB, file, voci di tabella o messaggi in coda. È possibile usare sia account BLOB che account per utilizzo generico come origini di eventi.
- **Servizi multimediali.** Servizi multimediali ospita contenuti multimediali audio e video e offre funzionalità avanzate di gestione per i file multimediali. Servizi multimediali può generare eventi all'avvio o al completamento di un processo di codifica in un file video.
- **Hub IoT di Azure.** L'hub IoT comunica con i dispositivi IoT e raccoglie dati di telemetria da questi dispositivi. Può generare eventi ogni volta che arrivano comunicazioni di questo tipo.
- **Eventi personalizzati.** Gli eventi personalizzati possono essere generati tramite l'API REST o con Azure SDK in Java, GO, .NET, Node, Python e Ruby. È ad esempio possibile creare un evento personalizzato nella funzionalità App Web di Servizio app di Azure. Ciò può avvenire nel ruolo di lavoro, quando un messaggio viene prelevato da una coda di archiviazione.

Questa ampia integrazione con svariate origini eventi all'interno di Azure garantisce che Griglia di eventi possa distribuire eventi correlati a quasi tutte le risorse di Azure.

## <a name="topics"></a>Argomenti
In Griglia di eventi un argomento è una raccolta di eventi correlati. Quando si pubblicano eventi da un'origine, si decide se per tali eventi è sufficiente un singolo argomento o se devono essere suddivisi in più argomenti. I componenti che ricevono e gestiscono gli eventi sottoscrivono gli argomenti per determinare gli eventi che riceveranno.

## <a name="subscriptions"></a>Sottoscrizioni
I gestori eventi usano le sottoscrizioni per indicare a Griglia di eventi quali eventi in un argomento vogliono ricevere. La sottoscrizione stabilisce anche l'endpoint, ovvero la posizione a cui Griglia di eventi invia le notifiche degli eventi. Una sottoscrizione può anche filtrare gli eventi in base al tipo o all'oggetto, in modo da assicurarsi che un gestore eventi riceva solo gli eventi pertinenti.

## <a name="event-handlers"></a>Gestori eventi
I tipi di oggetto seguenti in Azure possono ricevere e gestire gli eventi da Griglia di eventi:

- **Funzioni di Azure.** Una funzione di Azure è costituita da codice personalizzato che viene eseguito in Azure senza un contenitore o un server virtuale host. Usare una funzione di Azure come gestore eventi quando si vuole scrivere una risposta personalizzata per l'evento.
- **Webhook.** Un webhook è l'API Web che implementa un'architettura push.
- **App per la logica di Azure.** Un'app per la logica di Azure ospita un processo di business come un flusso di lavoro.
- **Microsoft Flow.** Anche Flow ospita flussi di lavoro, ma è più facile da usare per il personale non tecnico.

## <a name="should-you-use-event-grid"></a>È consigliabile usare Griglia di eventi?
Usare Griglia di eventi quando sono necessarie queste funzionalità:

- **Semplicità.** È molto semplice connettere le origini ai sottoscrittori in Griglia di eventi.
- **Filtri avanzati.** Le sottoscrizioni possono controllare strettamente gli eventi ricevuti da un argomento.
- **Ampia estensione.** È possibile sottoscrivere un numero illimitato di endpoint per gli stessi eventi e argomenti.
- **Affidabilità.** Griglia di eventi esegue nuovi tentativi di recapito degli eventi per 24 ore per ogni sottoscrizione.
- **Pagamento per evento.** Si paga solo per il numero di eventi trasmessi.

## <a name="summary"></a>Riepilogo
Griglia di eventi è un sistema di distribuzione di eventi semplice, ma versatile. Usarlo per recapitare eventi separati ai sottoscrittori, che riceveranno tali eventi in modo rapido e affidabile. Resta ancora da esaminare il modello di messaggistica in cui si recapita un _flusso_ di eventi di grandi dimensioni. In questo scenario Griglia di eventi non è la soluzione ideale perché è progettato per il recapito di un evento alla volta. È invece necessario usare un altro servizio di Azure: Hub eventi.