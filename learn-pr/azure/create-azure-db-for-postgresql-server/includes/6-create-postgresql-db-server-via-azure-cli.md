<span data-ttu-id="c6331-101">Si decide di creare un server di Database di Azure per PostgreSQL per archiviare i percorsi acquisiti dai dispositivi di fitness dei runner.</span><span class="sxs-lookup"><span data-stu-id="c6331-101">You decide to create an Azure Database for PostgreSQL server to store routes captured from runners' fitness devices.</span></span> <span data-ttu-id="c6331-102">In base ai volumi di dati cronologici acquisiti si sa che i requisiti di archiviazione del server devono essere impostati su 20 GB.</span><span class="sxs-lookup"><span data-stu-id="c6331-102">Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB.</span></span> <span data-ttu-id="c6331-103">Per supportare i requisiti di elaborazione è necessario il supporto di una risorsa di calcolo Gen 5 con 1 vCore.</span><span class="sxs-lookup"><span data-stu-id="c6331-103">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="c6331-104">Si sa anche di aver bisogno di un periodo di conservazione di 15 giorni per i backup dei dati.</span><span class="sxs-lookup"><span data-stu-id="c6331-104">You also know that you require a retention period of 15 days for data backups.</span></span>

## <a name="create-an-azure-postgresql-database-with-the-azure-cli"></a><span data-ttu-id="c6331-105">Creare un database di Azure PostGreSQL con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="c6331-105">Create an Azure PostGreSQL database with the Azure CLI</span></span>

<span data-ttu-id="c6331-106">Tenere presente che le dimensioni di archiviazione del server devono essere impostate su 20 GB e che si deve scegliere il supporto di calcolo Gen 5 con 1 vCore e un periodo di conservazione di 15 giorni per i backup dei dati.</span><span class="sxs-lookup"><span data-stu-id="c6331-106">Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.</span></span>

1. <span data-ttu-id="c6331-107">Usare il metodo `az postgres server create` per creare un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="c6331-107">Use the `az postgres server create` method to create a new database.</span></span> <span data-ttu-id="c6331-108">È necessario specificare alcuni parametri:</span><span class="sxs-lookup"><span data-stu-id="c6331-108">There are several parameters that you'll specify:</span></span>
    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`
    
2. <span data-ttu-id="c6331-109">Provare a creare il comando e a completare i parametri senza guardare la soluzione riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="c6331-109">See if you can build the command and complete the parameters without looking at the solution below.</span></span> <span data-ttu-id="c6331-110">Ecco alcuni suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="c6331-110">Here are some tips.</span></span>
    - <span data-ttu-id="c6331-111">Sostituire `<values>` con i propri valori:</span><span class="sxs-lookup"><span data-stu-id="c6331-111">Replace the `<values>` with your own values.</span></span> 
    - <span data-ttu-id="c6331-112">Tenere presente che il nome del server deve essere costituito da lettere in minuscolo comprese tra a e z, numeri da 0 a 9 e dal segno meno</span><span class="sxs-lookup"><span data-stu-id="c6331-112">Remember that the server name must be  made up of lowercase letters 'a'-'z', the numbers 0-9 and the hyphen.</span></span>
    - <span data-ttu-id="c6331-113">Usare <rgn>[nome gruppo di risorse sandbox]</rgn> come gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="c6331-113">Use <rgn>[Sandbox resource group name]</rgn> as the resource group.</span></span>
    - <span data-ttu-id="c6331-114">Usare una posizione inclusa nell'elenco seguente: [!include[](../../../includes/azure-sandbox-regions-note.md)]</span><span class="sxs-lookup"><span data-stu-id="c6331-114">Use a location from the following list:   [!include[](../../../includes/azure-sandbox-regions-note.md)]</span></span>
    
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

<span data-ttu-id="c6331-115">Quando viene seguito, il sistema impiegherà alcuni minuti per elaborare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="c6331-115">The system will take a few moments to process the information when executed.</span></span> <span data-ttu-id="c6331-116">Attendere il completamento del comando.</span><span class="sxs-lookup"><span data-stu-id="c6331-116">Go ahead and wait for the command to complete.</span></span>

<span data-ttu-id="c6331-117">Al termine, viene restituita una stringa JSON (JavaScript Object Notation) che descrive il server.</span><span class="sxs-lookup"><span data-stu-id="c6331-117">Once it's done, a JavaScript Object Notation (JSON) string that describes the server is returned.</span></span> <span data-ttu-id="c6331-118">In caso di errore, viene visualizzato un messaggio.</span><span class="sxs-lookup"><span data-stu-id="c6331-118">If there was a failure, an error message is displayed.</span></span> <span data-ttu-id="c6331-119">È possibile usare le informazioni sull'errore per esaminare e correggere i parametri del comando e riprovare.</span><span class="sxs-lookup"><span data-stu-id="c6331-119">You can use this error information to review and fix your command parameters and try again.</span></span>

<span data-ttu-id="c6331-120">Verrà visualizzato un oggetto JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c6331-120">The JSON object will look something like:</span></span>

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

<span data-ttu-id="c6331-121">La creazione del server PostgreSQL tramite l'interfaccia della riga di comando di Azure è stata completata.</span><span class="sxs-lookup"><span data-stu-id="c6331-121">You've successfully created a PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="c6331-122">Nell'unità di successiva verrà illustrato come configurare le impostazioni di sicurezza del server.</span><span class="sxs-lookup"><span data-stu-id="c6331-122">In the next unit, you'll see how to configure your server's security settings.</span></span>
