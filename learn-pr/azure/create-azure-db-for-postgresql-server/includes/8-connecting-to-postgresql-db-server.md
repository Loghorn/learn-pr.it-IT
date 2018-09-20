Si supponga di usare un database PostgreSQL locale. Si gestiscono tutti gli aspetti relativi alla sicurezza e si è bloccato l'accesso ai server tramite le regole del firewall standard a livello di server di PostgreSQL. A questo punto si hanno tutte le conoscenze necessarie per configurare le stesse regole del firewall a livello di server in Azure.

Di seguito viene stabilita la connessione a uno dei server di Database di Azure per PostgreSQL creati.

## <a name="allow-azure-service-access"></a>Consentire l'accesso ai servizi di Azure

Prima di iniziare è necessario consentire l'accesso ai servizi di Azure se si vuole usare PowerShell e `psql` per connettersi al server. Si può consentire l'accesso in due modi.

La prima opzione consiste nell'abilitare **Consenti l'accesso a Servizi di Azure**. Quando si consente l'accesso, viene creata una regola del firewall anche se la regola non è immessa nell'elenco delle regole personalizzate create dall'utente.

La seconda opzione prevede la creazione di una regola del firewall per consentire l'accesso a tutti gli indirizzi IP. La regola includerà l'indirizzo IP per il client in cui è in esecuzione PowerShell da cui si vuole eseguire `psql`.

Sarà anche necessario disabilitare l'opzione **Imponi connessione SSL**.

### <a name="create-a-firewall-rule"></a>Creare una regola del firewall

1. Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui si è attivata la sandbox. 

1. Passare alla risorsa server per la quale si vuole creare una regola del firewall.

1. Selezionare l'opzione **Sicurezza connessione** per aprire il relativo pannello a destra.

    ![Screenshot del portale di Azure che illustra la sezione Sicurezza connessione del pannello della risorsa di database PostgreSQL](../media/7-db-security-settings.png)

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
> La creazione di questa regola del firewall consentirà a tutti gli indirizzi IP in Internet di provare a connettersi al server. Negli ambienti di produzione è opportuno limitare l'accesso a indirizzi IP specifici.

### <a name="connect-to-the-database-with-psql"></a>Connettersi al database con psql

1. In Azure Cloud Shell nella parte destra della schermata connettere PSQL al server usando il comando seguente. Assicurarsi di sostituire il nome del server e il nome dell'amministratore.

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
    ```
    
    Usare i valori scelti per `server-name` e `admin-user`. 

1. **postgres** è il database di gestione predefinito con cui viene creato ogni server PostgreSQL. Verrà richiesto di immettere la stessa password specificata quando è stato creato il server.

1. Dopo aver stabilito la connessione, eseguire il comando <kbd>\l</kbd> per visualizzare l'elenco di tutti i database. Questo comando restituirà due o più database predefiniti.

1. Premere <kbd>q</kbd> per uscire dall'elenco.

1. È possibile provare altri comandi PSQL.
    - <kbd>\?</kbd> per ottenere la guida.
    - <kbd>\dt</kbd> per elencare le tabelle.

1. Al termine dell'esecuzione delle operazioni PSQL nel server, eseguire il comando <kbd>\q</kbd> per uscire da PSQL.
