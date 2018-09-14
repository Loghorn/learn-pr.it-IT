Si decide di creare un Database di Azure per il server PostgreSQL per archiviare le route acquisite da dispositivi adeguatezza degli strumenti di esecuzione. Basato su volumi di dati acquisita cronologici, si conosce che i requisiti di archiviazione server devono essere impostati su 20 GB. Per supportare i requisiti di elaborazione, è necessario calcolare il supporto di generazione 5 con 1 vCore. Si conosce anche richiedere un periodo di conservazione di 15 giorni per i backup dei dati.

Iniziamo.

Tenere importante ricordare che è opportuno impostare dimensioni di archiviazione del server su 20 GB, il supporto di generazione 5 con 1 vCore e un periodo di conservazione di 15 giorni per i backup dei dati di calcolo.

Esistono vari parametri che si specificherà:

- `--resource-group <resource_group_name>`
- `--name <new_server_name>`
- `--location <location>`
- `--admin-user <admin_user_name>`
- `--admin-password <server_admin_password>`
- `--sku-name <sku>`
- `--storage-size <size>`
- `--backup-retention <days>`
- `--version <version_number>`

Vedere se è possibile scrivere il comando e i parametri per il completamento senza esaminare la soluzione seguente. Sostituire i valori in `<>` con i propri valori.

> [!NOTE]
> È possibile recuperare un elenco di tutte le località usando questo comando `az account list-locations`. Selezionare il `displayName` oppure `name` il valore e usarlo per il `<location>` parametro.

```bash
az postgres server create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
```

Si noterà il sistema tra qualche istante per elaborare le informazioni quando eseguita. Se il server è stato creato, viene restituita una stringa di JavaScript Object Notation (JSON) che descrive il server. Se il server non è stato creato, viene visualizzato un messaggio di errore. Si userà queste informazioni sugli errori per esaminare e correggere i parametri del comando.

È stata creata correttamente un server PostgreSQL tramite la CLI di Azure. Nell'unità di successivo, verrà illustrato come configurare le impostazioni di sicurezza del server.
