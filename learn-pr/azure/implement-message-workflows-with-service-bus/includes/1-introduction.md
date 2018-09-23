Le applicazioni moderne sono spesso costituite da più parti in esecuzione in computer e dispositivi separati, che si trovano in diverse località in tutto il mondo. Tra questi componenti possono trovarsi reti complesse, con livelli di affidabilità e velocità variabili. Una sfida fondamentale con queste applicazioni distribuite consiste nel garantire una comunicazione affidabile tra i componenti.

Si supponga di essere uno sviluppatore cloud per Contoso Slices, una catena globale per la consegna a domicilio delle pizze. Il datore di lavoro sta aggiornando la tecnologia in modo che gli utenti possano effettuare ordini dal Web o dalle app per dispositivi mobili. Gli ordini verranno inviati al negozio preferito dell'utente, dove i dipendenti prepareranno la pizza. Quando l'impasto viene steso, la pizza viene messa in forno, viene inscatolata e viene caricata su un veicolo per la consegna, vengono inviati aggiornamenti all'app per dispositivi mobili dell'utente. Gli utenti ricevono anche aggiornamenti sulla posizione mentre l'autista sta dirigendosi verso di loro per la consegna.

Contoso Slices in precedenza aveva creato un sistema per gli ordini online che archiviava immediatamente i dati degli ordini in un database di SQL Server. Ogni negozio doveva ricordarsi di aggiornare manualmente la pagina degli ordini Web per rilevare la presenza di nuovi ordini. Durante gli orari di picco degli ordini di pizza, ad esempio durante la trasmissione televisiva di eventi sportivi, nel sistema si verificavano spesso eccezioni di deadlock e timeout. Infine, nel sistema precedente mancavano funzionalità di elaborazione del pagamento e aggiornamenti sullo stato per l'utente.

Per questo nuovo progetto più ambizioso, Contoso ha assunto un progettista cloud e prevede di usare un'architettura disaccoppiata.

In questo modulo si apprenderà in che modo il bus di servizio di Azure può aiutare a creare un'applicazione che rimane affidabile durante i picchi della domanda. Si vedrà anche come aggiungere facilmente funzionalità alle applicazioni grazie al bus di servizio di Azure. Nel corso del modulo, si scriverà il codice C# necessario per mettere in pratica quanto appreso. Si vedrà come usare code e argomenti del bus di servizio di Azure in un'architettura distribuita per garantire comunicazioni affidabili anche nei momenti di domanda elevata. Si scriverà anche il codice C# per la comunicazione tramite il bus di servizio.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

Contenuto del modulo:

- Scegliere se usare code, argomenti o inoltri del bus di servizio per comunicare in un'applicazione distribuita
- Configurare uno spazio dei nomi del bus di servizio di Azure in una sottoscrizione di Azure
- Creare un **argomento** del bus di servizio e usarlo per inviare e ricevere messaggi
- Creare una **coda** del bus di servizio e usarla per inviare e ricevere messaggi

## <a name="prerequisites"></a>Prerequisiti

Nessuno