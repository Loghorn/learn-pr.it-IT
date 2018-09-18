Si supponga di usare un database PostgreSQL locale. L'azienda ha ora in programma di espandere le funzionalità di supporto dispositivi, disponibilità, rilevamento dati ed elaborazione spostando il server in Azure. Occorrerà esaminare lo sforzo necessario per automatizzare la creazione di un Database di Azure per PostgreSQL.

La creazione di un singolo server di Database di Azure per PostgreSQL tramite il portale di Azure è semplice. La creazione di più database e l'esecuzione delle operazioni di manutenzione usando solo il portale potrebbero risultare noiose. Per automatizzare le attività di gestione verrà usata l'interfaccia della riga di comando di Azure per creare gli script.

La creazione di quasi tutte le risorse all'interno di Microsoft Azure può essere automatizzata tramite l'interfaccia della riga di comando di Azure. In questa unità verrà illustrato come automatizzare la gestione dei server di Database di Azure per PostgreSQL tramite l'interfaccia della riga di comando di Azure.

## <a name="what-is-azure-cli"></a>Che cos'è l'interfaccia della riga di comando di Azure?

L'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/) è un ambiente della riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È possibile usare l'interfaccia della riga di comando di Azure dal browser con Azure Cloud Shell oppure è possibile installare l'interfaccia della riga di comando di Azure in locale in Mac OS X, Linux o Windows. L'interfaccia della riga di comando di Azure viene eseguita dalla riga di comando locale con bash o Powershell. L'esecuzione dell'interfaccia della riga di comando di Azure in locale richiede tuttavia operazioni di configurazione aggiuntive. Verrà usato Azure Cloud Shell per l'esecuzione dei comandi dell'interfaccia della riga di comando di Azure.

## <a name="what-is-azure-cloud-shell"></a>Che cos'è Azure Cloud Shell?

Azure Cloud Shell è un'esperienza di shell basata su browser ospitata nel cloud e consente di connettersi ad Azure usando una sessione autenticata. È possibile eseguire i comandi dell'interfaccia della riga di comando di Azure per automatizzare la gestione di un Database di Azure per PostgreSQL. Gli strumenti comuni dell'interfaccia della riga di comando di Azure sono preinstallati e configurati in Cloud Shell per l'uso con l'account.

> [!NOTE]
> Cloud Shell richiede una risorsa di archiviazione di Azure per rendere persistenti i file creati mentre si lavora in Cloud Shell. Al primo avvio, Cloud Shell chiede di creare un gruppo di risorse, un account di archiviazione e una condivisione di File di Azure per conto dell'utente. Questo passaggio viene eseguito una sola volta e verrà automaticamente collegato per tutte le sessioni future di Cloud Shell.

## <a name="create-an-azure-database-for-postgresql-server-using-azure-cli"></a>Creare un server di Database di Azure per PostgreSQL tramite l'interfaccia della riga di comando di Azure

Verrà usato Azure Cloud Shell per creare un server di Database di Azure per PostgreSQL con l'interfaccia della riga di comando di Azure. Di seguito vengono riportati i passaggi da eseguire.

Per prima cosa, accedere al portale di Azure.

Aprire Cloud Shell dal portale di Azure. Aprire il browser e passare al [portale di Azure](https://portal.azure.com?azure-portal=true), quindi fare clic sul pulsante Apri Cloud Shell:

![Pulsante Cloud Shell](../media-draft/cloud-shell-button.png)

Cloud Shell consente di eseguire i comandi sia in `bash` che in `PowerShell`. Per tutti gli esempi verrà usata l'opzione della riga di comando `bash`.

Se si hanno più sottoscrizioni, verificare che sia attiva la sottoscrizione appropriata con il comando seguente, sostituendo gli zero con l'identificatore della sottoscrizione.

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

Verrà eseguito il comando seguente per elencare tutte le sottoscrizioni.

   ```bash
   az account list --output table
   ```

Il passaggio successivo consiste nel creare un gruppo di risorse per gestire il server e l'area in cui verrà posizionato il gruppo di risorse. È possibile usare un gruppo di risorse per gestire tutte le risorse correlate al server. L'opzione di località consente di specificare dove il server viene creato fisicamente. Verrà eseguito il comando successivo e `<resource_group_name>` e `<location>` verranno sostituiti rispettivamente con i valori appropriati.

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

L'ultimo passaggio consiste nel creare il server di Database di Azure per PostgreSQL.

   La Guida all'utilizzo dei comandi per la creazione del server dell'interfaccia della riga di comando di Azure che mostra tutti i parametri disponibili è simile a quella illustrata nell'esempio seguente:

   ```bash
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

   La riga di comando seguente mostra il set di parametri necessario per creare un server di Database di Azure per PostgreSQL. Si noterà che alcuni sono parametri facoltativi e non vengono elencati.

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a>Descrizioni dei parametri

Il parametro `--resource-group <resource_group_name>` specifica il gruppo di risorse in cui creare il server.

I parametri `admin-user` e `admin-password` del server specificati sono necessari per effettuare l'accesso al server e ai relativi database. Prendere nota di queste informazioni per usarle in seguito quando si interagirà con il nuovo server.

Il parametro `--sku-name` viene usato per specificare una parte del piano tariffario, in questo caso risorsa di calcolo. Il valore segue la convenzione `{pricing tier}_{compute generation}_{vCores}`.

Esempi:

- `--sku-name B_Gen4_4` esegue il mapping a Basic, Gen 4 e 4 vCore.
- `--sku-name GP_Gen5_32` esegue il mapping a utilizzo generico, Gen 5 e 32 vCore.
- `--sku-name MO_Gen5_2` esegue il mapping a Ottimizzato per la memoria, Gen 5 e 2 vCore.

I tre piani tariffari sono stati illustrati nell'unità in cui viene creato il server usando il portale.

Si supponga di voler usare una risorsa di calcolo con le caratteristiche seguenti: Basic, Gen 5 e 1 vCore. In tal caso, si specificherà il parametro come `--sku-name B_Gen5_1`.

Anche il parametro `--storage-size` viene usato per specificare una parte del piano tariffario. Se non viene specificato alcun valore, per impostazione predefinita verrà usato il valore 5.120 MB. Le dimensioni di archiviazione valide variano da 5.120 MB con incrementi di 1.024 MB a 1.048.576 MB.

Il parametro `--backup-retention` viene usato quando è necessario specificare il periodo di conservazione dei backup espresso in giorni. Se non viene specificato alcun valore, per impostazione predefinita verrà usato il valore di 7 giorni.

Il parametro `--version` viene usato per specificare la versione principale di PostgreSQL desiderata.

Sono stati finora illustrati i passaggi da eseguire per creare un Database di Azure per PostgreSQL con l'interfaccia della riga di comando di Azure. Nell'unità successiva verrà creato un server di Database di Azure per PostgreSQL tramite l'interfaccia della riga di comando di Azure.