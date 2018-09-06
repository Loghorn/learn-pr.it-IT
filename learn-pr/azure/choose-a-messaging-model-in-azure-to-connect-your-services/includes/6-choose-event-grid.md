Molte applicazioni usano gli eventi per notificare ai componenti distribuiti che è accaduto qualcosa o che un oggetto è stato modificato. È possibile usare Griglia di eventi di Azure per semplificare la distribuzione di tali eventi in Azure.

Si supponga di avere un'applicazione per la condivisione di musica con un'API Web in esecuzione in Azure. Quando un utente carica un nuovo file audio, è necessario inviare una notifica a tutte le app per dispositivi mobili installate nei dispositivi degli utenti in tutto il mondo.

È possibile usare le sottoscrizioni a Griglia di eventi per ottenere questo risultato in modo affidabile e rapido.

## <a name="what-is-event-grid"></a>Che cos'è Griglia di eventi?

Griglia di eventi è un servizio che distribuisce gli eventi provenienti da origini, come account di archiviazione BLOB di Azure o Servizi multimediali di Azure, ai sottoscrittori, ad esempio Funzioni di Azure o webhook.

Un'origine evento è qualsiasi componente che può generare un evento. Ad esempio, un utente potrebbe aggiungere un nuovo file multimediale a un account di archiviazione BLOB. Questa operazione genera un evento che può essere distribuito da Griglia di eventi.

Un sottoscrittore di eventi è qualsiasi componente che può ricevere eventi da Griglia di eventi. Ad esempio, il servizio Funzioni può eseguire codice che risponde al nuovo file multimediale nell'account di archiviazione BLOB.

Griglia di eventi semplifica la connessone delle origini a più sottoscrittori.

![Diagramma delle origini e dei sottoscrittori di Griglia di eventi](../images/6-event-grid.png)

## <a name="event-sources"></a>Origini eventi

Gli eventi possono essere generati dai tipi di risorse di Azure seguenti:

- **Gruppi di risorse e sottoscrizioni di Azure.** I gruppi di risorse e le sottoscrizioni generano eventi correlati alle operazioni di gestione in Azure. Ad esempio, quando un utente crea una macchina virtuale, questa origine genera un evento.
- **Account di archiviazione.** Gli account di archiviazione possono generare eventi quando gli utenti aggiungono BLOB, file, voci di tabella o messaggi in coda. È possibile usare sia account BLOB che account per utilizzo generico come origini di eventi.
- **Servizi multimediali.** Servizi multimediali ospita contenuti multimediali audio e video e offre funzionalità avanzate di gestione per i file multimediali. Servizi multimediali può generare eventi all'avvio o al completamento di un processo di codifica per un file video.
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
- **Webhook.** Un webhook è l'API Web che implementa un'architettura di push.
- **App per la logica di Azure.** Un'app per la logica di Azure ospita un processo di business come un flusso di lavoro.
- **Microsoft Flow.** Anche Flow ospita flussi di lavoro, ma è più facile da usare per il personale non tecnico.

## <a name="how-to-choose"></a>Come scegliere

Usare Griglia di eventi quando servono queste caratteristiche:

- **Semplicità.** È molto semplice connettere le origini ai sottoscrittori in Griglia di eventi.
- **Filtri avanzati.** Le sottoscrizioni possono controllare in maggiore dettagli gli eventi ricevuti da un argomento.
- **Ampia estensione.** È possibile sottoscrivere un numero illimitato di endpoint per gli stessi eventi e argomenti.
- **Affidabilità.** Griglia di eventi esegue nuovi tentativi di recapito degli eventi per 24 ore per ogni sottoscrizione.
- **Pagamento per evento.** Si paga solo per il numero di eventi trasmessi.

## <a name="summary"></a>Riepilogo

Griglia di eventi è un sistema di distribuzione di eventi semplice ma versatile. Usarlo per pubblicare eventi nei sottoscrittori, che riceveranno tali eventi in modo rapido e affidabile.