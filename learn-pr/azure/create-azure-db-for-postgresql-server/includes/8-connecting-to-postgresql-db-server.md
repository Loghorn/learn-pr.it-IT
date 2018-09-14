Si supponga che si usi un database PostgreSQL locale. Si gestiscono tutti gli aspetti di sicurezza e tutti gli accessi è stato bloccato per i server con le regole del firewall a livello di server PostgreSQL standard. Ora possibile avere una buona conoscenza di come configurare le stesse regole del firewall a livello di server in Azure.

Sono ora la possibilità di connettersi a uno dei Database di Azure precedente per i server PostgreSQL creato usando `psql`.

## <a name="allow-azure-service-access"></a>Consentire l'accesso al servizio di Azure

Prima di iniziare, è possibile consentire l'accesso ai servizi di Azure se si desidera usare PowerShell e `psql` per connettersi al server. È importante ricordare che è possibile consentire l'accesso in due modi.

La prima opzione consiste nell'abilitare **consentire l'accesso ai servizi di Azure**. Che consente l'accesso creerà una regola del firewall, anche se la regola non viene inserita nell'elenco di regole personalizzate create.

La seconda opzione consiste nel creare una regola del firewall che consenta l'accesso a tutti gli indirizzi IP. La regola includeranno l'indirizzo IP per il client che esegue PowerShell che verrà usato per eseguire `psql` da.

È inoltre necessario disabilitare la **Imponi connessione SSL** opzione.

Iniziamo.

Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true). Passare alla risorsa del server per il quale si desidera creare una regola del firewall.

Selezionare il **sicurezza della connessione** opzione per aprire il pannello sicurezza connessione a destra.

![Screenshot del portale di Azure che illustra la sezione sicurezza di connessione del pannello della risorsa di database PostgreSQL](../media-draft/7-db-security-settings.png)

Tenere presente che si vuole consentire l'accesso ai client di PowerShell che eseguono `psql`.

È possibile scegliere se:

- Impostare **consentire l'accesso ai servizi di Azure** a **via**
- Impostare **Imponi connessione SSL** a **DISABILITATO**
- Scegliere il **salvare** pulsante per salvare le modifiche

In alternativa, è possibile aggiungere una regola del firewall per consentire l'accesso a tutti gli indirizzi IP mediante l'aggiunta di una regola del firewall. Usare i valori seguenti:

- Nome della regola: `AllowAll`
- Indirizzo IP iniziale: `0.0.0.0`
- Indirizzo IP finale: `255.255.255.255`
- Impostare **Imponi connessione SSL** a **DISABILITATO**
- Scegliere il **salvare** pulsante per salvare le modifiche

> [!Warning]
> Creazione di questa regola del firewall consentirà qualsiasi indirizzo IP nella rete internet per tentare di connettersi al server. Anche se i client non saranno in grado di accedere al server senza il nome utente e password, attivare la regola con cautela e assicurarsi di che comprendere le implicazioni di sicurezza. Negli ambienti di produzione, si consentirà solo l'accesso agli indirizzi IP client specifico.

In Azure Cloud Shell connettersi psql al server usando il comando seguente:

```bash
psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
```

Tenere presente, `server-name`, e `admin-user` sono i valori scelti per l'account dell'amministratore durante la creazione del server. `postgres` è ogni PostgreSQL server viene creato con il database di gestione predefinito. Verrà richiesto per la password specificata durante la creazione del server.

Una volta stabilita la connessione, eseguire il `\l` comando per elencare tutti i database. Questo comando avrà come risultato in due o più database predefinito restituiti.

Quando hai finito l'esecuzione di operazioni di psql sul server, eseguire il comando `\q` per uscire da `psql`.

Infine, per uscire da Cloud Shell, eseguire il comando `exit`.
