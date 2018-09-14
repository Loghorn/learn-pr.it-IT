Vediamo ora come eseguire lo staging del contenuto e la distribuzione riducendo al minimo i tempi di inattività.

## <a name="what-is-a-deployment-slot"></a>Che cos'è uno slot distribuzione?

Uno slot di distribuzione è un'app Web **indipendente** con contenuto e configurazione propri e un nome host univoco. Funziona quindi come qualsiasi altra app Web.

> Azure non addebita costi aggiuntivi per poter usare gli slot di distribuzione.

Gli slot di distribuzione sono accessibili dai rispettivi URL univoci. Si supponga ad esempio di aver aggiunto uno slot di distribuzione di **staging** denominandolo `BESTBIKE`. Gli URL per l'applicazione e lo slot di distribuzione sono i seguenti:

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a>Perché gli slot di distribuzione sono utili?

Distribuire l'app Web in modo tradizionale, vale a dire tramite FTP, Distribuzione Web, GIT o qualsiasi altro modo, presenta alcuni svantaggi:

- Dopo essere stata distribuita, l'app Web potrebbe riavviarsi causando un **avvio a freddo** dell'applicazione. La prima richiesta all'applicazione sarà più lenta.

- Potenzialmente, si distribuisce una versione *non corretta* dell'app Web. È quindi consigliabile testarla in un ambiente di produzione prima di rilasciarla al client.

È in questo caso che entrano in gioco gli slot di distribuzione. È possibile apportare modifiche all'app Web nello slot di distribuzione di **staging** e testare tali modifiche senza interrompere gli utenti che accedono allo slot di distribuzione di **produzione**. Quando si è pronti a spostare le nuove funzionalità nell'ambiente di produzione, è sufficiente **scambiare** gli slot di staging e di produzione **senza tempi di inattività**.

Un altro vantaggio dell'uso degli slot di distribuzione consiste nel fatto che è possibile **preparare** l'applicazione in uno slot di staging prima di scambiarla nello slot di produzione. Si eviteranno così **avvio a freddo** e codice di inizializzazione di lunga durata.

Infine, è possibile **effettuare un nuovo scambio** con lo slot di distribuzione precedente se ci si accorge che la nuova versione dell'applicazione non funziona come previsto.

## <a name="what-is-automated-deployment"></a>Che cos'è la distribuzione automatica?

La distribuzione automatica, o integrazione continua, è un processo usato per effettuare il push di nuove funzionalità e correggere bug in modo veloce e ripetitivo, con impatto minimo sugli utenti finali.

Azure supporta la distribuzione automatica direttamente da diverse origini. Sono disponibili le opzioni seguenti:

- **Visual Studio Team Services (VSTS)**: è possibile eseguire il push del codice in VSTS, compilare il codice nel cloud, eseguire i test, generare una versione dal codice e infine eseguire il push del codice in un'app Web di Azure.

- **GitHub**: Azure supporta la distribuzione automatizzata direttamente da GitHub. Quando si connette il repository di GitHub ad Azure per eseguire la distribuzione automatica, tutte le modifiche di cui si effettua il push nel ramo di produzione in GitHub saranno automaticamente distribuite all'utente.

- **Bitbucket**: analogamente a GitHub, è possibile configurare una distribuzione automatica con Bitbucket.

- **OneDrive**: è possibile connettere l'account di OneDrive alle app Web. In questo modo, quando si apportano modifiche a un file nell'account di OneDrive, Azure lo rileverà automaticamente ed effettuerà nuovamente il pull delle modifiche nei file dell'app Web. Si tratta di un'ottima soluzione per i siti Web statici. Si avrà la sensazione di lavorare con un file system locale che riflette ogni modifica in Azure, in modo uniforme e immediato.

- **Dropbox**: analogamente a OneDrive, è possibile ospitare i file dell'app Web in un account di Dropbox, eseguirne il push automaticamente e distribuirli in un'app Web di Azure.

- **Repository esterno**: è possibile configurare la distribuzione automatizzata con un repository GIT esterno.

### <a name="non-automated-deployment-to-azure"></a>Distribuzione non automatica in Azure

Per effettuare il push manuale del codice in Azure, sono disponibili alcune opzioni:

- **FTP/S**: FTP o FTPS è un modo tradizionale per effettuare il push del codice in qualsiasi ambiente di hosting. Nonostante non sia più consigliabile, è possibile continuare a usare questo modo.

- **Distribuzione Web/IDE**: è possibile usare l'IDE di Visual Studio per pubblicare l'app Web in Azure. Il meccanismo di pubblicazione di Visual Studio può usare la tecnologia Distribuzione Web per effettuare il push del codice in Azure.

- **GIT**: Azure mantiene un **repository GIT locale** per l'applicazione. È possibile semplicemente **eseguire il commit** del codice direttamente in esso.

> In questo modulo sarà eseguita una distribuzione non automatizzata tramite GIT.

## <a name="summary"></a>Riepilogo

Azure offre modi diversi per caricare il codice e adattarlo al meglio al flusso di lavoro corrente. È anche possibile usare gli slot di distribuzione per evitare tempi di inattività agli utenti.
