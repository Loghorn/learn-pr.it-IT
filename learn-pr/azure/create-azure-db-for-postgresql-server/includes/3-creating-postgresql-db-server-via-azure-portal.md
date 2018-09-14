Si supponga che attualmente si usa un database relazionale PostgreSQL in locale usando i tipi di dati flessibile e il supporto geospaziale. L'azienda esegue la ricerca in espansione, che richiede il database per la scalabilità. Come alternativa a investire in hardware aggiuntivo, è il compito di trovare un'offerta di database ospitato nel cloud ottimali. Si è deciso di utilizzare un Database di Azure per il server PostgreSQL.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Che cos'è un database di Azure per il server PostgreSQL?

Il server PostgreSQL è un punto di amministrazione centrale per uno o più database. Il servizio PostgreSQL in Azure è una risorsa gestita che fornisce garanzie di prestazioni e fornisce l'accesso e le funzionalità a livello di server.

Un' **Database di Azure per PostgreSQL** server è l'elemento padre _risorsa_ per un database. Oggetto _risorsa_ è un elemento gestibile disponibile tramite Azure. Creazione di questa risorsa consente di configurare l'istanza del server.

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a>Che cos'è un Database di Azure per la risorsa del server PostgreSQL?

Un Database di Azure per la risorsa del server PostgreSQL è un contenitore con implicazioni relative alla durata per il server e database. Se la risorsa del server viene eliminata, vengono eliminati anche tutti i database. Tenere presente che tutte le risorse che appartengono all'elemento padre sono ospitate nella stessa area.

Il nome della risorsa server consente di definire il nome dell'endpoint server. Ad esempio, se è il nome della risorsa **mypgsqlserver**, quindi il nome del server diventa **mypgsqlserver.postgres.database.azure.com**.

La risorsa server fornisce anche il __ambito della connessione__ per criteri di gestione che si applicano al relativo database. Ad esempio: account di accesso, firewall, utenti, ruoli e configurazione.

Proprio come la versione open source di PostgreSQL, il server è disponibile in versioni diverse e consente l'installazione delle estensioni. Si sceglierà la versione di server da installare.

> [!NOTE]
> Le estensioni consentono di creare bundle di più oggetti SQL insieme in un singolo pacchetto che può essere caricato o rimosso con un unico comando. Un esempio di un'estensione è `chkpass`, che fornisce un tipo di dati per le password con crittografia automatica.

## <a name="pricing-tiers"></a>Piani tariffari

Database di Azure per PostgreSQL offre all'utente la possibilità di scegliere tra tre piani tariffari in base a parametri come potenza di calcolo e archiviazione.

### <a name="basic-tier"></a>Livello Basic

Il **base** livello è ideale per carichi di lavoro che richiedono risorse di calcolo leggere e prestazioni dei / o. In questo livello, è possibile accedere all'hardware seguente:

- Calcolo che CPU 4 generazione basare su v3 di Intel E5-2673 processori 2,4 GHz (Haswell) disponibili come 1 o 2 vCore configurazione
- Calcolo che CPU 5 generazione basare su Intel E5-2673 v4 processori 2,3 GHz (Broadwell) disponibili come 1 o 2 vCore configurazione
- Archiviazione fino a 1 TB
- Backup con ridondanza locale

Si userà un'impostazione specifica di livello dei prezzi per illustrare il supporto di uno specifico scenario, quando si crea un server di esempio in un secondo momento. Tenere presente che per i server di produzione, si sceglierà un piano tariffario in base all'ambiente.

### <a name="general-purpose-tier"></a>Livello utilizzo generico

Il **generico** livello è ideale per la maggior parte dei carichi di lavoro aziendali che richiedono calcolo bilanciata e memoria con velocità effettiva dei / o scalabile. In questo livello, è possibile accedere all'hardware seguente:

- Calcolo CPU 4 generazione basare su v3 di Intel E5-2673 processori 2,4 GHz (Haswell) disponibili in 2, 4, 8, 18, 32 vCore configurazioni
- Calcolo CPU 5 generazione basare su Intel E5-2673 v4 processori 2,3 GHz (Broadwell) disponibili in 2, 4, 8, 18, 32 vCore configurazioni
- Archiviazione fino a 4 TB
- Backup con ridondanza locale
- Backup con ridondanza geografica

### <a name="memory-optimized-tier"></a>Livello con ottimizzazione per la memoria

Il **ottimizzate per la memoria** livello è ideale per carichi di lavoro di database ad alte prestazioni che richiedono prestazioni in memoria per una più rapida elaborazione delle transazioni e una concorrenza maggiore. In questo livello, è possibile accedere all'hardware seguente:

- Calcolo CPU 5 generazione basare su Intel E5-2673 v4 processori 2,3 GHz (Broadwell) disponibili in 2, 4, 8, 16 vCore configurazioni
- Archiviazione fino a 4 TB
- Backup con ridondanza locale
- Backup con ridondanza geografica

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a>Passaggi per creare un Database di Azure per il server PostgreSQL

In genere, si creerà un Database di Azure per il server PostgreSQL tramite il portale di Azure. Esaminiamo i passaggi che sono necessari.

In primo luogo, dovrai accedere al portale di Azure e quindi premere ESC **crea una risorsa**.

Si selezionano **database** e **per Database di Azure per PostgreSQL**. È anche possibile usare la **ricerca** funzionalità per trovare questa categoria.

Il portale visualizzerà una schermata di configurazione server PostgreSQL, detto anche un pannello e aver effettuato la selezione.

![Screenshot del portale di Azure che mostra il pannello di creazione per un nuovo database PostgreSQL](../media-draft/4-create-blade.png)

È necessario assegnare un valore per tutti gli elementi nel pannello, hanno diamo un'occhiata a ognuno.

### <a name="server-name"></a>Nome server

Si è accennato in precedenza, creare una risorsa server. Il nome del server è l'elemento che specifica questa risorsa. Di conseguenza, è possibile scegliere un nome univoco per il server. Il nome del server deve contenere solo lettere minuscolo e può contenere numeri e trattini.

Si supponga che si desidera assegnare un nome server _Adventure Works rilevamento_. È quindi necessario configurare il nome come `adventure-works-tracking`. Se si prova a creare un server con un nome che esiste già, si otterrà un errore in tal senso.

### <a name="subscription"></a>Sottoscrizione

Il campo della sottoscrizione viene utilizzato per la fatturazione. È sarà possibile selezionare una sottoscrizione specifica nel caso in cui si hanno più sottoscrizioni disponibili.

### <a name="resource-group"></a>Gruppo di risorse

Si userà un gruppo di risorse per gestire tutte le risorse correlate al server. È possibile creare un nuovo gruppo di risorse o riutilizzare un gruppo di risorse.

### <a name="source"></a>Sorgente

È possibile creare un server da zero scegliendo il _vuoto_ l'opzione predefinita o da un backup esistente. Il **Backup** opzione fornirà il ripristino di opportunità un backup geografico di un Database di Azure esistente per il server PostgreSQL.

### <a name="server-admin-login-name"></a>Nome di accesso amministratore server

Si crea l'utente amministratore del server. Scegliere un nome di account di accesso di Usa un account di accesso di amministratore per il nuovo server. Il nome dell'account di accesso amministratore non può essere azure_superuser, azure_pg_admin, admin, administrator, root, guest o pubblico. Non può iniziare con PG _. Ricordare o annotare il nome per un uso futuro.

### <a name="password"></a>Password

Si sceglie una password da usare con il nome dell'account di accesso amministratore precedente. La password deve contenere caratteri di tre delle categorie seguenti:
- Lettere maiuscole
- Lettere minuscole
- Numeri (da 0 a 9)
- Caratteri non alfanumerici (!, $, #, % e così via)

Ricordare o annotare la password per un uso futuro.

### <a name="confirm-password"></a>Conferma password

La password di conferma.

### <a name="location"></a>Posizione

L'opzione location consente di specificare dove il server viene creato fisicamente. Scegliere la località geografica più vicina all'utente. In uno scenario reale, il percorso deve essere la località più vicina alla maggior parte degli utenti.

### <a name="version"></a>Version

È possibile specificare la versione di PostgreSQL da utilizzare. Microsoft mira a supportare la versione corrente e due versioni precedenti di PostgreSQL. È possibile scegliere una versione corrispondente all'ambiente di produzione.

> [!NOTE]
> Per altre informazioni, vedere le seguenti fonti:
> - [Versioni supportate del database PostgreSQL](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [Criteri di controllo delle versioni](https://www.postgresql.org/support/versioning/)

### <a name="pricing-tier"></a>Piano tariffario

Si sceglierà un piano tariffario che supporterà il carico di lavoro del server. È importante ricordare che si dispone di tre livelli cui scegliere. Come abbiamo visto in precedenza, ciascuno di questi livelli consente di configurare le opzioni di calcolo e archiviazione. Di seguito è riportato un esempio con il livello di prezzo Basic selezionato, un calcolo generazione 5 e un periodo di conservazione dei backup 25 giorni.

![Screenshot del portale di Azure con il database per un database PostgreSQL nuovo piano tariffario](../media-draft/4-azure-db-pricing-tier.png)

È a questo punto è sufficiente esaminare i valori immessi dall'utente, apportare eventuali note potrebbe necessarie per usarlo in seguito e fare clic su **Create** per creare il server.

Distribuzione del server richiede alcuni minuti. Si riceverà una notifica quando la distribuzione è stata completata. Dalla notifica, è possibile passare al server appena creato.

A questo punto si è visto la procedura da seguire per creare un Database di Azure per il server PostgreSQL. Nell'unità successiva, è possibile l'opportunità di creare il proprio Database di Azure per il server PostgreSQL.
