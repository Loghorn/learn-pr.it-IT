Si supponga che si usa un database PostgreSQL in locale. Si gestiscono tutti gli aspetti di sicurezza e tutti gli accessi è stato bloccato per i server con le regole del firewall a livello di server PostgreSQL standard. A questo punto si desidera assicurarsi che è possibile configurare lo stesso livello di server le regole firewall in Azure.

## <a name="server-security-considerations-and-connection-methods"></a>Considerazioni sulla sicurezza di server e i metodi di connessione

Si dispone di numerose opzioni per limitare l'accesso al Database di Azure per il server PostgreSQL e database. Accesso alla rete può essere limitata a una rete, server o a livello di database. È possibile usare una delle opzioni seguenti:

- Account utente per limitare l'accesso al database
- Reti virtuali per limitare l'accesso alla rete
- Regole del firewall per limitare l'accesso al server

### <a name="authentication-and-authorization"></a>Autenticazione e autorizzazione

Il Database di Azure per il server PostgreSQL supporta l'autenticazione nativa a PostgreSQL. È possibile connettersi e autenticare il server con account di accesso amministratore del server. Si creerà inoltre agli utenti di connettersi a database specifici per limitare l'accesso.

### <a name="what-is-a-virtual-network"></a>Informazioni sulla rete virtuale

Una rete virtuale è una rete isolata logicamente che viene creata all'interno della rete di Azure. È possibile usare una rete virtuale per controllare quali le risorse di Azure possono connettersi ad altre risorse.

Si supponga che si sta eseguendo un'applicazione web che si connette a un database. Si userà le subnet per isolare le diverse parti della rete. Una subnet è una parte di una rete basata su un intervallo di indirizzi IP.

Per configurare queste subnet, viene creata una rete virtuale e quindi suddividere la rete in subnet. L'applicazione web operano su una subnet e il database in un'altra subnet. Ogni subnet avrà una proprio regole per la comunicazione da e verso l'altra rete. Queste regole consentono di limitare l'accesso dal database all'applicazione web.

Creazione di una rete virtuale esula dall'ambito di questo modulo. Se sono necessarie altre informazioni, per esplorare altri moduli di formazione correlati alle reti virtuali.

### <a name="what-is-a-firewall"></a>Che cos'è un firewall?

Un firewall è un servizio che concede l'accesso al server basato su indirizzo IP di origine di ogni richiesta. Creare le regole del firewall che specificano gli intervalli di indirizzi IP. Solo i client da questi concessi gli indirizzi IP saranno possibile accedere al server. In generale, le regole del firewall, includono anche informazioni sul protocollo e porta di rete specifici. Ad esempio, un server PostgreSQL per impostazione predefinita è in ascolto delle richieste TCP sulla porta 5432.

### <a name="azure-database-for-postgresql-server-firewall"></a>Database di Azure per il firewall del server PostgreSQL

Il Database di Azure per il firewall del server PostgreSQL impedisce qualsiasi accesso al server di database finché non si specificano i computer autorizzati. La configurazione del firewall consente di specificare un intervallo di indirizzi IP che è autorizzati a connettersi al server. Il server utilizza sempre le informazioni di connessione di PostgreSQL predefinito.

![Diagramma funzionale di firewall di Azure](../media-draft/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a>Database di Azure per le connessioni SSL al server PostgreSQL

Database di Azure per PostgreSQL preferisce che le applicazioni client di connettersi al servizio PostgreSQL con Secure Sockets Layer (SSL). L'imposizione delle connessioni SSL tra il server di database e le applicazioni client aiuta a proteggersi da "man in the middle" e attacchi simili tramite la crittografia dei dati tra server e client. Abilitazione di SSL è necessario lo scambio di chiavi e l'autenticazione di tipo strict tra client e server per la connessione a funzionare. Informazioni dettagliate sull'utilizzo di SSL rientrano nell'ambito di questo modulo di apprendimento. Se sono necessarie altre informazioni, per esplorare altri moduli di formazione relativi a SSL.

## <a name="configure-connection-security"></a>Configurare la protezione della connessione

Esaminiamo le decisioni e passaggi che è apportare per configurare un Database di Azure per il firewall del server PostgreSQL. Scoprirai anche come connettersi al server creato in precedenza.

In primo luogo, si aprirà il [portale di Azure](https://portal.azure.com?azure-portal=true) e passare alla risorsa del server per il quale si desidera creare una regola del firewall.

Quindi, si selezionano le **sicurezza della connessione** opzione per aprire il pannello sicurezza connessione a destra.

![Screenshot del portale di Azure che illustra la sezione sicurezza di connessione del pannello della risorsa di database PostgreSQL](../media-draft/7-db-security-settings.png)

In questa schermata, sono disponibili diverse opzioni. È possibile:

- Aggiungere l'indirizzo IP che usa per accedere al portale come voce firewall facendo clic sui **Aggiungi IP client** pulsante.
- Consentire l'accesso ai servizi di Azure. Per impostazione predefinita, tutti i servizi di Azure **non** hanno accesso al server PostgreSQL.
- Aggiungere le regole del firewall tramite l'immissione di intervalli di indirizzi IP.
- Impostare le connessioni SSL. Questa opzione forza il client per connettersi al server usando un certificato SSL.

Ricordarsi di fare clic sui **salvare** sull'icona sopra i campi di immissione per salvare la configurazione aggiornata, dopo aver apportato le modifiche.

### <a name="allow-access-to-azure-services"></a>Possibilità di accedere ai servizi di Azure

Per usare Azure Cloud Shell per accedere o configurare il server, assicurarsi di abilitare **consentire l'accesso ai servizi di Azure**. Questo passaggio verrà aggiunta una regola del firewall per la configurazione del server per consentire l'accesso da Cloud Shell. Questa regola non verrà visualizzati come una delle regole personalizzate aggiunti.

È inoltre necessario disabilitare **Imponi connessione SSL**. PowerShell non è possibile connettersi al server se SSL è obbligatorio per le connessioni client.

Entrambe queste opzioni comporterà un messaggio di errore che ha visualizzato nella riga di comando se non è configurato correttamente.

Ad esempio, se l'accesso non è autorizzato a servizi di Azure e applicare le connessioni SSL è abilitato, verrà visualizzato qualcosa di simile a questo errore quando il firewall sta bloccando l'accesso:

> psql: FATAL: nessuna voce pg_hba. conf per l'host "123.45.67.89," utente "adminuser", database "postgres", SSL on FATAL: connessione SSL è obbligatorio. Please specify SSL options and retry.

### <a name="create-a-firewall-rule-using-the-portal"></a>Creare una regola del firewall usando il portale

Si supponga di che voler creare una regola del firewall che consente di accedere da qualsiasi indirizzo IP.

> [!WARNING]
> Creazione di questa regola del firewall consentirà qualsiasi indirizzo IP nella rete internet per tentare di connettersi al server. Anche se i client non essere in grado di accedere al server senza il nome utente e password, attivare la regola con cautela e assicurarsi di che comprendere le implicazioni di sicurezza.

Si crea una nuova regola del firewall tramite l'immissione di dati seguenti nei campi con etichettati:

- Nome della regola: `AllowAll`
- Indirizzo IP iniziale: `0.0.0.0`
- Indirizzo IP finale: `255.255.255.255`

Per rimuovere una regola del firewall, si sarà fare clic sui puntini di sospensione (...) alla fine della regola che si desidera eliminare. Scegliere il **eliminare** pulsante per eliminare la regola.

Fare clic sui **salvare** sull'icona sopra i campi di immissione per confermare l'eliminazione della regola.

### <a name="create-a-firewall-rule-using-the-azure-cli"></a>Creare una regola del firewall tramite la CLI di Azure

Utilizzare il comando di Azure per aggiungere regole del firewall per il server con il `az postgres server firewall-rule create` comando usando Azure cloud shell.

Si supponga di che voler creare le stesse regole come illustrato in precedenza. Si userà il comando seguente:

  ```azurecli
  az postgres server firewall-rule create --resource-group <resource_group_name> --server <server-name> --name AllowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
  ```

Per rimuovere le regole del firewall del server con il comando `az postgres server firewall-rule delete`.

Si supponga di che voler eliminare il firewall che è stato creato. È quindi usare il comando seguente:

  ```azurecli
  az postgres server firewall-rule delete --name AllowAll --resource-group <resource_group_name> --server-name <server-name>
  ```

## <a name="connecting-to-your-server"></a>La connessione al server

Come per qualsiasi database moderni, PostgreSQL richiede l'amministrazione di normali server per ottenere prestazioni ottimali. Si dispone di numerose opzioni per connettere e gestire il Database di Azure per il server PostgreSQL. Si userà `psql` per connettersi al server.

### <a name="what-is-psql"></a>Che cos'è psql?

Lo strumento da riga di comando denominato `psql` è PostgreSQL distribuito terminale interattivo per lavorare con i server PostgreSQL e database. `psql` funziona con il Database di Azure per PostgreSQL come con qualsiasi altra implementazione di PostgreSQL e viene fornita con Azure Cloud Shell. Il `psql` lo strumento consente di gestire i database, nonché eseguire query di struttura su questi database.

Usando `psql` richiede una connessione a un server PostgreSQL. Esistono una serie di parametri della riga di comando disponibili per l'uso quando si lavora con `psql`.

- `--host` -L'host a cui si desidera connettersi.
- `--username` -Nome/ID utente con cui stabilire la connessione.
- `--dbname` -Il nome del database a cui connettersi.

> [!Note]
> In genere si connetterà al `postgres` database di gestione quando gestiscono le configurazioni di accesso e il database del server.

Ecco il comando completo:

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=<database>
  ```

Dopo la connessione, si verrà visualizzato un prompt dei comandi e possono eseguire i comandi al server e al database.

Abbiamo visto i passaggi da eseguire per configurare il Database di Azure per PostgreSQL le impostazioni di sicurezza. Nell'unità successiva, si configurerà Database di Azure per PostgreSQL impostazioni di sicurezza. È anche possibile connettersi al server usa Cloud Shell.
