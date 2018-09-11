Si supponga di dover pianificare l'architettura di un'applicazione distribuita per la condivisione di musica. È necessario assicurarsi che l'applicazione sia il più possibile affidabile e scalabile e si prevede di usare tecnologie di Azure per creare un'infrastruttura di comunicazione solida.

Prima di poter scegliere la tecnologia di Azure corretta, è necessario conoscere ogni singola comunicazione che si scambiano i componenti dell'applicazione. Per ogni tipo di comunicazione è possibile scegliere una diversa tecnologia di Azure.

La prima cosa da capire per una comunicazione è se invia **messaggi** o **eventi**. Questa è una scelta fondamentale che aiuta a scegliere il servizio di Azure appropriato da usare.

## <a name="what-is-a-message"></a>Che cos'è un messaggio?
Nella terminologia delle applicazioni distribuite, i **messaggi** hanno le caratteristiche seguenti:

- Un messaggio contiene dati non elaborati, generati da un componente, che verranno utilizzati da un altro componente.
- Un messaggio contiene i dati stessi, non solo un riferimento a tali dati.
- Il componente di invio si aspetta che il contenuto del messaggio venga elaborato in un certo modo dal componente di destinazione. L'integrità dell'intero sistema può dipendere dal fatto che sia il mittente che il destinatario eseguano un processo specifico.

Ad esempio, si supponga che un utente carichi un nuovo brano con l'app per dispositivi mobili per la condivisione di musica. L'app per dispositivi mobili deve inviare tale brano all'API Web eseguita in Azure. Deve essere inviato il file multimediale del brano stesso, non solo un avviso che indica che è stato aggiunto un nuovo brano. L'app per dispositivi mobili si aspetta che l'API Web archivi il nuovo brano nel database e lo renda disponibile ad altri utenti. Ecco un esempio di messaggio.

## <a name="what-is-an-event"></a>Che cos'è un evento?

Gli **eventi** sono più leggeri rispetto ai messaggi e vengono spesso usati per le comunicazioni broadcast. I componenti che inviano l'evento sono detti **componenti di pubblicazione** e i destinatari sono noti come **sottoscrittori**.

Con gli eventi, i componenti di ricezione decideranno in genere a quali comunicazioni sono interessati e le "sottoscriveranno". La sottoscrizione viene in genere gestita da un intermediario, ad esempio Griglia di eventi di Azure o Hub eventi di Azure. Quando i componenti di pubblicazione inviano un evento, l'intermediario indirizzerà tale evento ai sottoscrittori interessati. Questi meccanismi sono noti come "architettura di pubblicazione-sottoscrizione". Non è l'unico modo per gestire gli eventi, ma è il più comune.

Gli eventi hanno le caratteristiche seguenti:

- Un evento è una notifica leggera che indica che è accaduto qualcosa.
- L'evento può essere inviato a più destinatari o a nessuno.
- Gli eventi sono spesso destinati a una distribuzione estesa, ovvero hanno un numero elevato di sottoscrittori per ogni origine di pubblicazione.
- Il componente di pubblicazione dell'evento non ha aspettative sull'azione eseguita da un componente di ricezione.
- Alcuni eventi sono unità discrete e non correlate ad altri eventi. 
- Alcuni eventi fanno parte di una serie ordinata e correlata.  

Si supponga, ad esempio, che il caricamento del file musicale sia stato completato e che il nuovo brano sia stato aggiunto al database. Per informare gli utenti del nuovo file, l'API Web deve informare gli utenti del front-end Web e dell'app per dispositivi mobili del nuovo file. Gli utenti possono scegliere se ascoltare il nuovo brano, quindi la notifica iniziale non include il file musicale ma si limita a informare gli utenti che è presente il brano. Il mittente non ha aspettative specifiche su quello che faranno i destinatari dell'evento in risposta alla ricezione di questo evento.

Questo è un esempio di evento discreto.

## <a name="how-to-choose-messages-or-events"></a>Come scegliere tra messaggi o eventi

È probabile che una singola applicazione usi gli eventi per certi scopi e i messaggi per altri. Prima di scegliere, è necessario analizzare l'architettura dell'applicazione e tutti i relativi casi d'uso per identificare tutti i diversi scopi che hanno i relativi componenti per comunicare tra loro.

Gli eventi vengono più probabilmente usati per le trasmissioni e sono spesso temporanei, vale a dire che una comunicazione potrebbe non essere gestita da alcun destinatario se non ci sono sottoscrizioni. È più probabile che i messaggi vengano usati quando l'applicazione distribuita richiede una garanzia di elaborazione della comunicazione.

Per tutte le comunicazioni prendere in considerazione la domanda seguente: **Il componente di invio si aspetta che la comunicazione venga elaborata in un modo specifico dal componente di destinazione?**

Se la risposta è _sì_, scegliere di usare un messaggio. Se la risposta è _no_, si potrebbe riuscire a usare gli eventi.

Capire come devono comunicare i componenti sarà utile per scegliere le modalità. Iniziamo con i messaggi.