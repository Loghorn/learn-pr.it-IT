Si supponga di usare un database relazionale PostgreSQL locale con tipi di dati flessibile e supporto geospaziale. L'azienda ha in programma di espandersi con la conseguente necessità di ridimensionare il database. Come alternativa all'acquisto di ulteriore hardware, si è scelto di trovare un'offerta ottimale di database ospitato su cloud e si è deciso quindi di usare un server di Database di Azure per PostgreSQL.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Che cos'è un server di Database di Azure per PostgreSQL?

Il server PostgreSQL è un punto di amministrazione centrale per uno o più database. Il servizio PostgreSQL in Azure è una risorsa gestita che assicura prestazioni garantite e offre accesso e funzionalità a livello di server.

Un server di **Database di Azure per PostgreSQL** è la _risorsa_ padre per un database. Una _risorsa_ è un elemento gestibile disponibile tramite Azure. La creazione di questa risorsa consente di configurare un'istanza del server.

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a>Che cos'è una risorsa del server di Database di Azure per PostgreSQL?

Una risorsa del server di Database di Azure per PostgreSQL è un contenitore con implicazioni di lunga durata per il server e i database. Se la risorsa del server viene eliminata, vengono eliminati anche tutti i database. Tenere presente che tutte le risorse che appartengono all'elemento padre sono ospitate nella stessa area.

Il nome della risorsa del server viene usato per definire il nome dell'endpoint server. Ad esempio, se è il nome della risorsa è **mypgsqlserver** il nome del server diventa **mypgsqlserver.postgres.database.azure.com**

La risorsa del server fornisce anche l'__ambito della connessione__ per i criteri di gestione che vengono applicati al relativo database. Ad esempio, account di accesso, firewall, utenti, ruoli e configurazione.

Proprio come la versione open source di PostgreSQL, il server è disponibile in diverse versioni e consente l'installazione delle estensioni. È possibile scegliere la versione del server da installare.

> [!NOTE]
> Le estensioni consentono di creare bundle di più oggetti SQL in un singolo pacchetto che può essere caricato o rimosso con un singolo comando. Un esempio di estensione è ```chkpass```, che fornisce un tipo di dati per le password con crittografia automatica.

## <a name="pricing-tiers"></a>Piani tariffari

Il Database di Azure per PostgreSQL offre la possibilità di scegliere tra tre piani tariffari basati su parametri quali potenza di calcolo e spazio di archiviazione.

### <a name="basic"></a>Basic

Il livello **Basic** è ideale per i carichi di lavoro con esigenze di calcolo e di prestazioni I/O ridotte. In questo livello è possibile accedere all'hardware seguente:

- CPU di calcolo Gen 4 basate su processori Intel E5-2673 v3 (Haswell) a 2,4 GHz disponibili come configurazione a 1 o 2 vCore
- CPU di calcolo Gen 5 basate su processori Intel E5-2673 v4 (Broadwell) a 2,3 GHz disponibili come configurazione a 1 o 2 vCore
- Spazio di archiviazione fino a 1 TB
- Backup con ridondanza locale

Verrà usata un'impostazione specifica del piano tariffario per illustrare il supporto di uno specifico scenario quando più avanti verrà creato un server di esempio. Tenere presente che per i server di produzione si sceglierà un piano tariffario in base all'ambiente.

### <a name="general-purpose"></a>Utilizzo generico

Il livello **Utilizzo generico** è ideale per la maggior parte dei carichi di lavoro aziendali che richiedono risorse di calcolo e di memoria bilanciate con velocità effettiva di I/O scalabile. In questo livello è possibile accedere all'hardware seguente:

- CPU di calcolo Gen 4 basate su processori Intel E5-2673 v3 (Haswell) a 2,4 GHz disponibili in configurazioni a 2, 4, 8, 18, 32 vCore
- CPU di calcolo Gen 5 basate su processori Intel E5-2673 v4 (Broadwell) a 2,3 GHz disponibili in configurazioni a 2, 4, 8, 18, 32 vCore
- Spazio di archiviazione fino a 4 TB
- Backup con ridondanza locale
- Backup con ridondanza geografica

### <a name="memory-optimized"></a>Ottimizzato per la memoria

Il livello **Ottimizzato per la memoria** è ideale per carichi di lavoro di database ad alte prestazioni che richiedono prestazioni in memoria per l'elaborazione più rapida delle transazioni e una concorrenza maggiore. In questo livello è possibile accedere all'hardware seguente:

- CPU di calcolo Gen 5 basate su processori Intel E5-2673 v4 (Broadwell) a 2,3 GHz disponibili in configurazioni a 2, 4, 8, 16 vCore
- Spazio di archiviazione fino a 4 TB
- Backup con ridondanza locale
- Backup con ridondanza geografica

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a>Passaggi per creare un server di Database di Azure per PostgreSQL

In genere, è possibile creare un server di Database di Azure per PostgreSQL usando il portale di Azure. Di seguito vengono riportati i passaggi da eseguire.

Per prima cosa è necessario accedere al portale di Azure e quindi fare clic su **Crea una risorsa**.

Selezionare **Database** e **Database di Azure per PostgreSQL**. È anche possibile usare la funzionalità di ricerca per trovare questa categoria.

Il portale visualizzerà una schermata di configurazione del server PostgreSQL, detta anche pannello, in cui effettuare le selezioni.

![Pannello dell'interfaccia utente per la creazione del server PostgreSQL nel portale di Azure](../media-draft/4-create-blade.png)

È necessario assegnare un valore per tutti gli elementi presenti nel pannello, riportati di seguito.

### <a name="server-name"></a>Nome server

In precedenza si è accennato alla creazione di una risorsa del server. Il nome del server è l'elemento che specifica questa risorsa. Di conseguenza, sarà necessario scegliere un nome univoco per il server. Il nome del server deve essere composto solo da lettere minuscole e può contenere numeri e trattini.

Si supponga di voler assegnare al server il nome _Adventure Works Tracking_. È quindi necessario configurare il nome come ```adventure-works-tracking```. Se si prova a creare un server con lo stesso nome, verrà visualizzato un errore.

### <a name="subscription"></a>Sottoscrizione

Il campo della sottoscrizione viene usato per la fatturazione. È possibile selezionare una sottoscrizione specifica nel caso in cui siano disponibili più sottoscrizioni.

### <a name="resource-group"></a>Gruppo di risorse

È possibile usare un gruppo di risorse per gestire tutte le risorse correlate al server. È possibile creare un nuovo gruppo di risorse o usarne uno esistente.

### <a name="source"></a>Origine

È possibile creare un server da zero scegliendo l'opzione predefinita _Vuoto_ o da un backup esistente. L'opzione Backup consente di ripristinare un backup geografico di un server esistente di Database di Azure per PostgreSQL.

### <a name="server-admin-login-name"></a>Nome di accesso dell'amministratore server

È possibile creare l'utente amministratore del server. Scegliere un nome di accesso da usare come account di accesso dell'amministratore per il nuovo server. Il nome di accesso dell'amministratore non può essere azure_superuser, azure_pg_admin, admin, administrator, root, guest o public. Non può iniziare con pg_. Ricordare o annotare il nome per un uso futuro.

### <a name="password"></a>Password

È possibile scegliere una password da usare con il nome di accesso dell'amministratore. La password deve contenere caratteri di tre delle categorie seguenti: lettere maiuscole, lettere minuscole, numeri (da 0 a 9) e caratteri non alfanumerici (!, $, #, % e così via). Ricordare o annotare la password per un uso futuro.

### <a name="confirm-password"></a>Conferma password

Ridigitare la password come conferma.

### <a name="location"></a>Località

L'opzione di località consente di specificare dove il server viene creato fisicamente. Scegliere la località geografica più vicina all'utente. In uno scenario reale, la località deve essere la località più vicina alla maggior parte degli utenti.

### <a name="version"></a>Versione

È possibile specificare la versione di PostgreSQL da usare. Microsoft intende supportare la versione corrente e due versioni precedenti di PostgreSQL. È possibile scegliere una versione corrispondente all'ambiente di produzione.

> [!NOTE]
> Per altre informazioni, vedere le fonti seguenti:
> - [Versioni supportate del Database PostgreSQL](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [Criteri di controllo delle versioni](https://www.postgresql.org/support/versioning/)

### <a name="pricing-tier"></a>Piano tariffario

È possibile scegliere un piano tariffario che supporti il carico di lavoro del server. Sono disponibili tre livelli tra cui scegliere. Come illustrato in precedenza, ciascuno di questi livelli consente di configurare le opzioni di calcolo e archiviazione. Ad esempio, di seguito è riportato un esempio in cui è selezionato il piano tariffario Basic, un calcolo Gen 5 e un periodo di conservazione dei backup di 25 giorni.

![Piano tariffario](../media-draft/4-azure-db-pricing-tier.png)

A questo punto, rimane solo da verificare i valori immessi, apportare eventuali note che potrebbero servire più avanti e fare clic sul pulsante Crea per creare il server.

La distribuzione del server richiede alcuni minuti. Al termine della distribuzione si riceverà una notifica. Dalla notifica è possibile passare al server appena creato.

Sono stati finora illustrati i passaggi da eseguire per creare un Database di Azure per PostgreSQL. Nell'unità successiva sarà possibile creare un server personalizzato di Database di Azure per PostgreSQL.
