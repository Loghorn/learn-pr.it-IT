Vediamo ora come eseguire lo staging del contenuto e la distribuzione riducendo al minimo i tempi di inattività.

## <a name="what-is-a-deployment-slot"></a>Che cos'è uno slot distribuzione?

Uno slot di distribuzione è un' **indipendenti** app nel servizio App con il proprio contenuto, configurazione e anche un nome univoco di host. Pertanto, funziona come qualsiasi altra app nel servizio App.

Gli slot di distribuzione sono accessibili dai rispettivi URL univoci. Si supponga ad esempio di aver aggiunto uno slot di distribuzione di **staging** denominandolo `BESTBIKE`. Gli URL per l'applicazione e lo slot di distribuzione sono i seguenti:

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a>Perché gli slot di distribuzione sono utili?

Distribuire l'app Web in modo tradizionale, vale a dire tramite FTP, Distribuzione Web, GIT o qualsiasi altro modo, presenta alcuni svantaggi:

- Al termine della distribuzione, l'app venga riavviato, causando una **avvio a freddo** per l'applicazione. La prima richiesta all'applicazione sarà più lenta.

- Potenzialmente, si sta distribuendo un *errata* versione dell'app e si dovrebbe eseguirne il test (nell'ambiente di produzione) prima del rilascio al client.

È in questo caso che entrano in gioco gli slot di distribuzione. È possibile apportare modifiche all'App per un **di gestione temporanea** distribuzione nello slot e testare le modifiche senza alcun impatto sugli utenti che accedono alle **produzione** slot di distribuzione. Quando si è pronti a spostare le nuove funzionalità nell'ambiente di produzione, è sufficiente **scambiare** gli slot di staging e di produzione **senza tempi di inattività**.

Un altro vantaggio dell'uso degli slot di distribuzione consiste nel fatto che è possibile **preparare** l'applicazione in uno slot di staging prima di scambiarla nello slot di produzione. Si eviteranno così **avvio a freddo** e codice di inizializzazione di lunga durata.

Infine, è possibile **effettuare un nuovo scambio** con lo slot di distribuzione precedente se ci si accorge che la nuova versione dell'applicazione non funziona come previsto.

## <a name="what-is-automated-deployment"></a>Che cos'è la distribuzione automatizzata?

La distribuzione automatizzata, o integrazione continua, è un processo usato per eseguire il push di nuove funzionalità e correggere bug in modo veloce e ripetitivo, con impatto minimo sugli utenti finali.

Azure supporta la distribuzione automatizzata direttamente da diverse origini. Sono disponibili le opzioni seguenti:

- **Visual Studio Team Services (VSTS)**: è possibile eseguire il push del codice in VSTS, compilare il codice nel cloud, eseguire i test, generare una versione dal codice e infine eseguire il push del codice in un'app Web di Azure.

- **GitHub**: Azure supporta la distribuzione automatica direttamente da GitHub. Quando si connette il repository di GitHub ad Azure per eseguire la distribuzione automatizzata, tutte le modifiche di cui si esegue il push nel ramo di produzione in GitHub saranno automaticamente distribuite all'utente.

- **Bitbucket**: analogamente a GitHub, è possibile configurare una distribuzione automatizzata con Bitbucket.

- **OneDrive**: È possibile connettersi all'account OneDrive con il servizio App in modo che quando si modifica qualsiasi file nell'account OneDrive, Azure lo rileva automaticamente e il pull di tutte le modifiche nei file dell'app web. Si tratta di un'ottima soluzione per i siti Web statici. Si avrà la sensazione di lavorare con un file system locale che riflette ogni modifica in Azure, in modo uniforme e immediato.

- **Dropbox**: simile a OneDrive, è possibile ospitare i file dell'app web in un account Dropbox e chiedere di effettuare il push automaticamente e distribuiti tramite un'app web.

- **Repository esterno**: è possibile configurare la distribuzione automatica con un repository Git esterno.

### <a name="non-automated-deployment-to-azure"></a>Distribuzione non automatizzata in Azure

Per eseguire il push manuale del codice in Azure, sono disponibili alcune opzioni:

- **FTP/S**: FTP o FTPS è un modo tradizionale per eseguire il push del codice in qualsiasi ambiente di hosting. Nonostante non sia più consigliabile, è possibile continuare a usare questo modo.

- **Distribuzione Web/IDE**: è possibile usare l'IDE di Visual Studio per pubblicare l'app Web in Azure. Il meccanismo di pubblicazione di Visual Studio può usare la tecnologia Distribuzione Web per eseguire il push del codice in Azure.

- **Git**: Azure mantiene un **repository Git locale** per l'applicazione. È possibile **eseguire il commit** del codice direttamente in esso.

> In questo modulo si eseguirà una distribuzione non automatica tramite Git.

## <a name="summary"></a>Riepilogo

Azure offre modi diversi per caricare il codice e adattarlo al meglio al flusso di lavoro corrente. È anche possibile usare gli slot di distribuzione per evitare tempi di inattività agli utenti.
