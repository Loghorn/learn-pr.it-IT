Si supponga di usare un database PostgreSQL locale. Si gestiscono tutti gli aspetti relativi alla sicurezza ed è stato bloccato l'accesso ai server tramite le regole del firewall standard a livello del server PostgreSQL. A questo punto, si vuole assicurarsi di poter configurare le stesse regole del firewall a livello del server in Azure.

## <a name="server-security-considerations-and-connection-methods"></a>Considerazioni sulla sicurezza del server e metodi di connessione

Sono disponibili numerose opzioni per limitare l'accesso ai database e al server di Database di Azure per PostgreSQL. L'accesso alla rete può essere limitato a livello di rete, di server o di database. È possibile usare una delle opzioni seguenti:

- Account utente per limitare l'accesso al database
- Reti virtuali per limitare l'accesso alla rete
- Regole del firewall per limitare l'accesso al server

### <a name="authentication-and-authorization"></a>Autenticazione e autorizzazione

Il server di Database di Azure per PostgreSQL supporta l'autenticazione nativa a PostgreSQL. È possibile connettersi ed eseguire l'autenticazione al server con l'account di accesso amministratore del server. Si creeranno inoltre utenti da connettere a database specifici per limitare l'accesso.

### <a name="what-is-a-virtual-network"></a>Che cos'è una rete virtuale?

Una rete virtuale è una rete isolata logicamente creata all'interno della rete di Azure. È possibile usare una rete virtuale per controllare le risorse di Azure che possono connettersi ad altre risorse.

Si immagini di eseguire un'applicazione Web che si connette a un database. Si useranno le subnet per isolare le diverse parti della rete. Una subnet è una parte di una rete basata su un intervallo di indirizzi IP.

Per configurare queste subnet, verrà creata una rete virtuale e quindi la rete verrà suddivisa in subnet. L'applicazione Web opererà in una subnet e il database in un'altra. Ogni subnet avrà regole specifiche per la comunicazione da e verso l'altra rete. Queste regole consentono di limitare l'accesso dal database all'applicazione Web.

La creazione di una rete virtuale non rientra nell'ambito di questo modulo. Se sono necessarie altre informazioni, esplorare gli altri moduli di apprendimento correlati alle reti virtuali.

### <a name="what-is-a-firewall"></a>Che cos'è un firewall?

Un firewall è un servizio che concede ai server l'accesso in base all'indirizzo IP di origine di ogni richiesta. È possibile creare regole del firewall che specificano gli intervalli di indirizzi IP. Solo i client di questi indirizzi IP concessi potranno accedere al server. In generale, le regole del firewall includono anche informazioni specifiche sul protocollo di rete e sulla porta. Ad esempio, un server PostgreSQL per impostazione predefinita è in ascolto delle richieste TCP sulla porta 5432.

### <a name="azure-database-for-postgresql-server-firewall"></a>Firewall del server di Database di Azure per PostgreSQL

Il firewall del server di Database di Azure per PostgreSQL impedisce qualsiasi accesso al server di database finché non vengono specificati i computer autorizzati. La configurazione del firewall consente di specificare un intervallo di indirizzi IP autorizzati a connettersi al server. Il server usa sempre le informazioni di connessione di PostgreSQL predefinite.

![Diagramma funzionale del firewall di Azure](../media-draft/7-firewall-diagram.png)

### <a name="azure-database-for-postgresql-server-ssl-connections"></a>Connessioni SSL del server di Database di Azure per PostgreSQL

Il Database di Azure per PostgreSQL preferisce che le applicazioni client vengano connesse al servizio PostgreSQL usando la connettività SSL (Secure Sockets Layer). L'applicazione delle connessioni SSL tra il server di database e le applicazioni client aiuta a proteggersi dagli attacchi "man in the middle" e da altri attacchi simili crittografando i dati tra il server e il client. L'abilitazione di SSL richiede lo scambio di chiavi e un'autenticazione più restrittiva tra client e server affinché la connessione funzioni. Le informazioni dettagliate sull'utilizzo di SSL non rientrano nell'ambito di questo modulo di apprendimento. Se sono necessarie altre informazioni, esplorare gli altri moduli di apprendimento correlati a SSL.

## <a name="configure-connection-security"></a>Configurare la sicurezza della connessione

Esaminiamo le decisioni e passaggi da effettuare per configurare un firewall del server di Database di Azure per PostgreSQL. Verrà anche illustrato come connettersi al server creato in precedenza.

Per prima cosa, aprire il [portale di Azure](https://portal.azure.com?azure-portal=true) e passare alla risorsa server per cui si vuole creare una regola del firewall.

Selezionare quindi l'opzione **Sicurezza connessione** per aprire il pannello Sicurezza connessione a destra.

![Impostazioni di sicurezza del database PostgreSQL](../media-draft/7-db-security-settings.png)

In questa schermata sono disponibili diverse opzioni. È possibile:

- Aggiungere l'indirizzo IP da usare per accedere al portale come voce del firewall facendo clic sul pulsante **+ Aggiungi IP client**
- Consentire l'accesso ai servizi di Azure. Per impostazione predefinita, tutti i servizi di Azure **non** hanno accesso al server PostgreSQL
- Aggiungere le regole del firewall immettendo gli intervalli di indirizzi IP
- Applicare le connessioni SSL. Questa opzione forza il client a connettersi al server usando un certificato SSL.

Ricordarsi sempre di fare clic sull'icona **Salva** sopra i campi di immissione per salvare la configurazione aggiornata dopo aver apportato le modifiche.

### <a name="allow-access-to-azure-services"></a>Consentire l'accesso ai servizi di Azure

Per usare Azure Cloud Shell per accedere o configurare il server, assicurarsi di abilitare **Consenti l'accesso a Servizi di Azure**. Questo passaggio aggiungerà una regola del firewall alla configurazione del server per consentire l'accesso da Cloud Shell. Questa regola non verrà tuttavia mostrata come una delle regole personalizzate aggiunte.

Sarà anche necessario disabilitare l'opzione **Imponi connessione SSL**. PowerShell non può connettersi al server se è necessario il protocollo SSL per le connessioni client.

Entrambe queste opzioni restituiranno un messaggio di errore nella riga di comando se non vengono configurate correttamente.

Ad esempio, se non viene consentito l'accesso ai servizi di Azure ed è abilitata l'opzione Imponi connessione SSL, verrà visualizzato un messaggio di errore simile al seguente quando il firewall blocca l'accesso.

> psql: FATAL: no pg_hba.conf entry for host "123.45.67.89", user "adminuser", database "postgres", SSL on FATAL:  SSL connection is required. Please specify SSL options and retry.

### <a name="create-a-firewall-rule-using-the-portal"></a>Creare una regola del firewall usando il portale

Si supponga di voler creare una regola del firewall che consenta l'accesso da qualsiasi indirizzo IP.

> [!WARNING]
> La creazione di questa regola del firewall consentirà a tutti gli indirizzi IP Internet di provare a connettersi al server. Anche se i client non potranno accedere al server senza nome utente e password, prestare attenzione quando si abilita questa regola e assicurarsi di comprendere tutte le implicazioni per la sicurezza.

È possibile creare una nuova regola del firewall immettendo i dati seguenti nei campi contrassegnati:

- Nome regola: `AllowAll`
- Indirizzo IP iniziale: `0.0.0.0`
- Indirizzo IP finale: `255.255.255.255`

Per rimuovere una regola del firewall, fare clic sui puntini di sospensione alla fine della regola che si vuole eliminare. Fare clic sul pulsante Elimina per eliminare la regola.

Fare clic sull'icona **Salva** sopra i campi di immissione per confermare l'eliminazione della regola.

### <a name="create-a-firewall-rule-using-the-azure-cli"></a>Creare una regola del firewall usando l'interfaccia della riga di comando di Azure

È possibile usare l'interfaccia della riga di comando di Azure per aggiungere le regole del firewall al server con il comando `az postgres server firewall-rule create` mediante Azure Cloud Shell.

Se si vogliono creare le stesse regole indicate in precedenza, immettere il comando seguente:

  ```bash
  az postgres server firewall-rule create --resource-group <resource_group_name> --server <server-name> --name AllowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
  ```

È possibile rimuovere le regole del firewall dal server con il comando `az postgres server firewall-rule delete`.

Se si vuole eliminare il firewall creato, usare il comando seguente:

  ```bash
  az postgres server firewall-rule delete --name AllowAll --resource-group <resource_group_name> --server-name <server-name>
  ```

## <a name="connecting-to-your-server"></a>Connessione al server

Come per qualsiasi database moderno, PostgreSQL richiede un'amministrazione del server regolare per ottenere prestazioni ottimali. Sono disponibili numerose opzioni per la connessione e la gestione del server di Database di Azure per PostgreSQL. Per connettersi al server, verrà usato `psql`.

### <a name="what-is-psql"></a>Che cos'è psql?

Lo strumento da riga di comando denominato `psql` è il terminale interattivo distribuito di PostgreSQL da usare con il server e i database PostgreSQL. `psql` funziona con il Database di Azure per PostgreSQL come con qualsiasi altra implementazione di PostgreSQL ed è incluso con Azure Cloud Shell. Lo strumento `psql` consente di gestire i database, nonché di eseguire query di strutture su questi database.

Per usare `psql` è necessaria una connessione riuscita a un server PostgreSQL. Sono disponibili numerosi parametri della riga di comando da usare con `psql`.

- `--host` - host a cui connettersi
- `--username` -nome/identificativo dell'utente con cui connettersi
- `--dbname` - nome del database a cui connettersi.

> [!Note]
> In genere, è necessario connettersi al database di gestione `postgres` per gestire l'accesso al server e la configurazione dei database.

Il comando completo è il seguente:

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=<database>
  ```

Una volta connessi, verrà visualizzato un prompt dei comandi e sarà possibile eseguire i comandi per il server e i database.

Sono stati finora illustrati i passaggi da eseguire per configurare le impostazioni di sicurezza di un Database di Azure per PostgreSQL. Nell'unità successiva verranno configurate le impostazioni di sicurezza di un Database di Azure per PostgreSQL. Sarà anche possibile connettersi al server con Cloud Shell.