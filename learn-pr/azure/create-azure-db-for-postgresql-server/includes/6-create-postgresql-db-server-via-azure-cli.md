Si decide di creare un server di Database di Azure per PostgreSQL per archiviare i percorsi acquisiti dai dispositivi di fitness dei runner. In base ai volumi di dati cronologici acquisiti si sa che i requisiti di archiviazione del server devono essere impostati su 20 GB. Per supportare i requisiti di elaborazione è necessario il supporto di una risorsa di calcolo Gen 5 con 1 vCore. Si sa anche di aver bisogno di un periodo di conservazione di 15 giorni per i backup dei dati.

## <a name="create-an-azure-postgresql-database-with-the-azure-cli"></a>Creare un database di Azure PostGreSQL con l'interfaccia della riga di comando di Azure

Tenere presente che le dimensioni di archiviazione del server devono essere impostate su 20 GB e che si deve scegliere il supporto di calcolo Gen 5 con 1 vCore e un periodo di conservazione di 15 giorni per i backup dei dati.

1. Usare il metodo `az postgres server create` per creare un nuovo database. È necessario specificare alcuni parametri:
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`
    
2. Provare a creare il comando e a completare i parametri senza guardare la soluzione riportata di seguito. Ecco alcuni suggerimenti.
    - Sostituire `<values>` con i propri valori: 
    - Tenere presente che il nome del server deve essere costituito da lettere in minuscolo comprese tra a e z, numeri da 0 a 9 e dal segno meno
    - Usare <rgn>[nome gruppo di risorse sandbox]</rgn> come gruppo di risorse.
    - Usare una posizione inclusa nell'elenco seguente: [!include[](../../../includes/azure-sandbox-regions-note.md)]
    
```bash
az postgres server create --name <unique_server_name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \ 
    --location eastus \
    --sku-name B_Gen5_1 \
    --storage-size 20480 \
    --backup-retention 15 \
    --version 10 \
    --admin-user <admin_user_name> \
    --admin-password <server_admin_password>
```

Quando viene seguito, il sistema impiegherà alcuni minuti per elaborare le informazioni. Attendere il completamento del comando.

Al termine, viene restituita una stringa JSON (JavaScript Object Notation) che descrive il server. In caso di errore, viene visualizzato un messaggio. È possibile usare le informazioni sull'errore per esaminare e correggere i parametri del comando e riprovare.

Verrà visualizzato un oggetto JSON simile al seguente:

```json
{
  "administratorLogin": "azureuser",
  "earliestRestoreDate": "2018-09-17T00:35:50.170000+00:00",
  "fullyQualifiedDomainName": "secondserver8.postgres.database.azure.com",
  "id": "/subscriptions/xxxxx/resourceGroups/<rgn>[Sandbox Resource Group]</rgn>/providers/Microsoft.DBforPostgreSQL/servers/secondserver8",
  "location": "eastus",
  "name": "secondserver8",
  "resourceGroup": "<rgn>[Sandbox Resource Group]</rgn>",
  "sku": {
    "capacity": 1,
    "family": "Gen5",
    "name": "B_Gen5_1",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 15,
    "geoRedundantBackup": "Disabled",
    "storageMb": 20480
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "10"
}
```

La creazione del server PostgreSQL tramite l'interfaccia della riga di comando di Azure è stata completata. Nell'unità di successiva verrà illustrato come configurare le impostazioni di sicurezza del server.
