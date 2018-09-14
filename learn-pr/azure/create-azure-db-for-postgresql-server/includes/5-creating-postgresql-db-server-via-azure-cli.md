Si supponga che si usa un database PostgreSQL in locale. L'azienda è osservare ora l'espansione di supporto dei dispositivi, disponibilità, rilevamento dati e le caratteristiche di elaborazione spostando i server in Azure. Si passerà a esaminare lo sforzo necessario per automatizzare la creazione di un Database di Azure per il server PostgreSQL.

Creazione di un singolo Database di Azure per il server PostgreSQL tramite il portale di Azure è facile. Creazione di più di un database e in esecuzione in corso la manutenzione usando solo il portale potrebbe risultare noiosi. Si userà la CLI di Azure per creare script per automatizzare le attività di gestione.

Creazione di quasi tutte le risorse all'interno di Microsoft Azure può essere automatizzata tramite la CLI di Azure. In questa unità, si apprenderà come automatizzare la gestione del Database di Azure per i server PostgreSQL tramite la CLI di Azure.

## <a name="what-is-the-azure-cli"></a>Che cos'è l'interfaccia della riga di comando di Azure?

Il [CLI Azure](https://docs.microsoft.com/cli/azure/) è l'ambiente di riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È possibile usare il comando di Azure dal browser con Azure Cloud Shell oppure è possibile installare l'interfaccia CLI di Azure in locale in Mac OS X, Linux o Windows. Il comando di Azure viene eseguito dalla riga di comando locale con bash o PowerShell. Esegue il comando di Azure in locale richiede un'ulteriore configurazione. Si userà Azure Cloud Shell per eseguire i comandi di CLI di Azure.

## <a name="what-is-azure-cloud-shell"></a>Che cos'è Azure Cloud Shell?

Azure Cloud Shell è un'esperienza shell basata su browser che è ospitata nel cloud e consente di connettersi ad Azure usando una sessione autenticata. È possibile eseguire i comandi di CLI di Azure per automatizzare la gestione di un Database di Azure per il server PostgreSQL. Strumenti comuni della riga di comando di Azure sono pre-installati e configurati in Cloud Shell è possibile usare con il proprio account.

> [!NOTE]
> Cloud Shell richiede una risorsa di archiviazione di Azure per rendere persistenti i file creati mentre si lavora in Cloud Shell. Al primo avvio, Cloud Shell chiede di creare un gruppo di risorse, l'account di archiviazione e condivisione file di Azure per tuo conto. Questo passaggio è occasionale e verrà automaticamente collegato per tutte le sessioni future di Cloud Shell.

## <a name="create-an-azure-database-for-postgresql-server-using-the-azure-cli"></a>Creare un Database di Azure per il server PostgreSQL tramite la CLI di Azure

Si userà il terminale Azure Cloud Shell a destra per creare un Database di Azure per il server PostgreSQL tramite la CLI di Azure.

La Guida della riga di comando di Azure server creazione comando utilizzo che mostra tutti i parametri disponibili è simile al seguente:

   ```azurecli
   az postgres server create [-h] [--verbose] [--debug]
                             [--output {json,jsonc,table,tsv}]
                             [--query JMESPATH]
                              --resource-group RESOURCE_GROUP_NAME --name SERVER_NAME
                              --sku-name SKU_NAME [--location LOCATION]
                              --admin-user ADMINISTRATOR_LOGIN
                              [--admin-password ADMINISTRATOR_LOGIN_PASSWORD]
                              [--backup-retention BACKUP_RETENTION]
                              [--geo-redundant-backup GEO_REDUNDANT_BACKUP]
                              [--ssl-enforcement {Enabled,Disabled}]
                              [--storage-size STORAGE_MB]
                              [--tags [TAGS [TAGS ...]]]
                              [--version VERSION]
                              [--subscription _SUBSCRIPTION]

   ```

Riga di comando seguente viene illustrato il set di parametri per creare un Database di Azure per PostgreSQL server richiesto. Si noterà che alcuni parametri sono facoltativi e non sono presenti.

   ```azurecli
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a>Descrizioni dei parametri

Il `--resource-group <resource_group_name>` parametro specifica il gruppo di risorse in cui creare il server.

Il server `admin-user` e `admin-password` specificare è necessario effettuare l'accesso al server e i relativi database. Ricordare o registrare queste informazioni per in un secondo momento durante l'interazione con il nuovo server.

Si utilizza il `--sku-name` parametro per specificare una parte del piano tariffario, in questo caso, risorsa di calcolo. Il valore segue la convenzione `{pricing tier}_{compute generation}_{vCores}`.

Esempi:

- `--sku-name B_Gen4_4` esegue il mapping a Basic, Gen 4 e 4 vCore.
- `--sku-name GP_Gen5_32` esegue il mapping a utilizzo generico, Gen 5 e 32 vCore.
- `--sku-name MO_Gen5_2` esegue il mapping a ottimizzazione per la memoria, Gen 5 e 2 vCore.

È importante ricordare che è descritti tre piani tariffari dell'unità in cui è stato creato il server usando il portale.

Si supponga che si desidera utilizzare una semplice generazione 5 e risorsa di calcolo di 1 vCore. È possibile specificare il parametro come `--sku-name B_Gen5_1`.

Si utilizza il `--storage-size` parametro per specificare una parte del piano tariffario. Se il valore non è specificato, quindi per impostazione predefinita 5.120 MB. Archiviazione valido compreso tra 5.120 MB di dimensioni e aumenta in base a incrementi di 1.024 MB fino a 1.048.576 MB.

Il `--backup-retention` parametro viene utilizzato quando è necessario specificare il periodo di memorizzazione per i backup, che viene specificato in giorni. Se il valore non è specificato, quindi per impostazione predefinita di sette giorni.

Si utilizza il `--version` parametro per specificare la versione principale di PostgreSQL che si desidera utilizzare.

Abbiamo visto i passaggi per creare un Database di Azure per il server PostgreSQL tramite la CLI di Azure. Nell'unità successiva, si creerà un Database di Azure per il server PostgreSQL tramite la CLI di Azure.