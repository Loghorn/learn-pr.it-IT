Si supponga di usare un database PostgreSQL locale. Si gestiscono tutti gli aspetti relativi alla sicurezza ed è stato bloccato l'accesso ai server tramite le regole del firewall standard a livello del server PostgreSQL. A questo punto, si hanno tutte le conoscenze necessarie per configurare le stesse regole del firewall a livello del server in Azure

e sarà possibile connettersi a uno dei server di database di Azure per PostgreSQL creati usando `psql`.

## <a name="allow-azure-service-access"></a>Consentire l'accesso ai servizi di Azure

Prima di iniziare. Sarà necessario consentire l'accesso ai servizi di Azure per usare PowerShell e `psql` per connettersi al server. Si può consentire l'accesso in due modi.

La prima opzione consiste nell'abilitare **Consenti l'accesso a Servizi di Azure**. Quando si consente l'accesso, viene creata una regola del firewall anche se la regola non è immessa nell'elenco delle regole personalizzate create dall'utente.

La seconda opzione prevede la creazione di una regola del firewall per consentire l'accesso a tutti gli indirizzi IP. La regola includerà l'indirizzo IP per il client in cui è in esecuzione PowerShell da cui si vuole eseguire `psql`.

Sarà anche necessario disabilitare l'opzione **Imponi connessione SSL**.

Iniziamo.

Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true). Passare alla risorsa server per cui si vuole creare una regola del firewall.

Selezionare l'opzione **Sicurezza connessione** per aprire il pannello Sicurezza connessione a destra.

![Impostazioni di sicurezza del database PostgreSQL](../media-draft/7-db-security-settings.png)

Tenere presente che si vuole consentire l'accesso ai client PowerShell che eseguono `psql`.

È possibile scegliere se:

- Impostare **Consenti l'accesso a Servizi di Azure** su **SÌ**
- Impostare **Imponi connessione SSL** su **NO**
- Fare clic sul pulsante **Salva** per salvare le modifiche apportate

In alternativa, è possibile aggiungere una regola del firewall per consentire l'accesso a tutti gli indirizzi IP. Usare i valori seguenti:

- Nome regola: `AllowAll`
- Indirizzo IP iniziale: `0.0.0.0`
- Indirizzo IP finale: `255.255.255.255`
- Impostare **Imponi connessione SSL** su **NO**
- Fare clic sul pulsante **Salva** per salvare le modifiche apportate

> [!Warning]
> La creazione di questa regola del firewall consentirà a tutti gli indirizzi IP Internet di provare a connettersi al server. Anche se i client non potranno accedere al server senza nome utente e password, prestare attenzione quando si abilita questa regola e assicurarsi di comprendere tutte le implicazioni per la sicurezza. Negli ambienti di produzione si consentirà l'accesso solo a indirizzi IP client specifici.

Nella procedura seguente si avvierà una sessione di Azure Cloud Shell. Questa esercitazione usa `bash` come ambiente della riga di comando.

- Aprire Cloud Shell dal portale di Azure. Passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e fare clic sul pulsante Apri Cloud Shell:

  ![Pulsante Cloud Shell](../media-draft/cloud-shell-button.png)

- Se sono presenti numerose sottoscrizioni, assicurarsi di usare quella corretta. È anche possibile eseguire il comando seguente per impostare la sottoscrizione attiva. Ricordare di sostituire gli zeri con l'identificatore della sottoscrizione.

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

- Connettere psql al server usando il comando seguente:

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
  ```

   Tenere presente che `server-name` e `admin-user` sono i valori scelti per l'account amministratore al momento della creazione del server. `postgres` è il database di gestione predefinito con cui viene creato ogni server PostgreSQL. Verrà richiesto di immettere la stessa password specificata quando è stato creato il server.

- Dopo aver stabilito la connessione, eseguire il comando `\l` per visualizzare l'elenco di tutti i database.

   Questo comando restituirà due o più database predefiniti.

- Al termine dell'esecuzione delle operazioni psql nel server, eseguire il comando `\q` per uscire da `psql`.

- Infine, eseguire il comando `exit` per uscire da Cloud Shell.
