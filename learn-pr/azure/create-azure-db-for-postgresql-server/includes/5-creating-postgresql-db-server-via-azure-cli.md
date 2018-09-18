<span data-ttu-id="b1c90-101">Si supponga di usare un database PostgreSQL locale.</span><span class="sxs-lookup"><span data-stu-id="b1c90-101">Let's assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="b1c90-102">L'azienda ha ora in programma di espandere le funzionalità di supporto dispositivi, disponibilità, rilevamento dati ed elaborazione spostando il server in Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-102">Your company is now looking at expanding device support, availability, data tracking, and processing features by moving your server into Azure.</span></span> <span data-ttu-id="b1c90-103">Occorrerà esaminare lo sforzo necessario per automatizzare la creazione di un Database di Azure per PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b1c90-103">You'll investigate how much effort it takes to automate the creation of an Azure Database for PostgreSQL.</span></span>

<span data-ttu-id="b1c90-104">La creazione di un singolo server di Database di Azure per PostgreSQL tramite il portale di Azure è semplice.</span><span class="sxs-lookup"><span data-stu-id="b1c90-104">Creating a single Azure Database for PostgreSQL server using the Azure portal is easy.</span></span> <span data-ttu-id="b1c90-105">La creazione di più database e l'esecuzione delle operazioni di manutenzione usando solo il portale potrebbero risultare noiose.</span><span class="sxs-lookup"><span data-stu-id="b1c90-105">Creating more than one database and running ongoing maintenance using only the portal may become tedious.</span></span> <span data-ttu-id="b1c90-106">Per automatizzare le attività di gestione verrà usata l'interfaccia della riga di comando di Azure per creare gli script.</span><span class="sxs-lookup"><span data-stu-id="b1c90-106">You'll use the Azure CLI to create scripts when you want to automate management tasks.</span></span>

<span data-ttu-id="b1c90-107">La creazione di quasi tutte le risorse all'interno di Microsoft Azure può essere automatizzata tramite l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-107">Creating almost any resource within Microsoft Azure can be automated using the Azure CLI.</span></span> <span data-ttu-id="b1c90-108">In questa unità verrà illustrato come automatizzare la gestione dei server di Database di Azure per PostgreSQL tramite l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-108">In this unit, you'll learn how to automate management of your Azure Database for PostgreSQL servers using the Azure CLI.</span></span>

## <a name="what-is-azure-cli"></a><span data-ttu-id="b1c90-109">Che cos'è l'interfaccia della riga di comando di Azure?</span><span class="sxs-lookup"><span data-stu-id="b1c90-109">What is Azure CLI?</span></span>

<span data-ttu-id="b1c90-110">L'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/) è un ambiente della riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-110">[Azure CLI](https://docs.microsoft.com/cli/azure/) is Microsoft’s cross-platform command-line environment for managing Azure resources.</span></span> <span data-ttu-id="b1c90-111">È possibile usare l'interfaccia della riga di comando di Azure dal browser con Azure Cloud Shell oppure è possibile installare l'interfaccia della riga di comando di Azure in locale in Mac OS X, Linux o Windows.</span><span class="sxs-lookup"><span data-stu-id="b1c90-111">You can use the Azure CLI from your browser with Azure Cloud Shell, or you can install Azure CLI locally on Mac OS X, Linux, or Windows.</span></span> <span data-ttu-id="b1c90-112">L'interfaccia della riga di comando di Azure viene eseguita dalla riga di comando locale con bash o Powershell.</span><span class="sxs-lookup"><span data-stu-id="b1c90-112">The Azure CLI is run from a local command line using bash or Powershell.</span></span> <span data-ttu-id="b1c90-113">L'esecuzione dell'interfaccia della riga di comando di Azure in locale richiede tuttavia operazioni di configurazione aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="b1c90-113">Running Azure CLI locally however requires additional setup.</span></span> <span data-ttu-id="b1c90-114">Verrà usato Azure Cloud Shell per l'esecuzione dei comandi dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-114">We'll use the Azure Cloud Shell for executing Azure CLI commands.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="b1c90-115">Che cos'è Azure Cloud Shell?</span><span class="sxs-lookup"><span data-stu-id="b1c90-115">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="b1c90-116">Azure Cloud Shell è un'esperienza di shell basata su browser ospitata nel cloud e consente di connettersi ad Azure usando una sessione autenticata.</span><span class="sxs-lookup"><span data-stu-id="b1c90-116">Azure Cloud Shell is a browser-based shell experience that is hosted in the cloud and allows you to connect to Azure using an authenticated session.</span></span> <span data-ttu-id="b1c90-117">È possibile eseguire i comandi dell'interfaccia della riga di comando di Azure per automatizzare la gestione di un Database di Azure per PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b1c90-117">You can execute Azure CLI commands to automate the management of an Azure Database for PostgreSQL.</span></span> <span data-ttu-id="b1c90-118">Gli strumenti comuni dell'interfaccia della riga di comando di Azure sono preinstallati e configurati in Cloud Shell per l'uso con l'account.</span><span class="sxs-lookup"><span data-stu-id="b1c90-118">Common Azure CLI tools are pre-installed and configured in Cloud Shell for you to use with your account.</span></span>

> [!NOTE]
> <span data-ttu-id="b1c90-119">Cloud Shell richiede una risorsa di archiviazione di Azure per rendere persistenti i file creati mentre si lavora in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b1c90-119">Cloud Shell requires an Azure storage resource to persist any files you create while working in the Cloud Shell.</span></span> <span data-ttu-id="b1c90-120">Al primo avvio, Cloud Shell chiede di creare un gruppo di risorse, un account di archiviazione e una condivisione di File di Azure per conto dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b1c90-120">On first launch Cloud Shell prompts to create a resource group, storage account, and Azure Files share on your behalf.</span></span> <span data-ttu-id="b1c90-121">Questo passaggio viene eseguito una sola volta e verrà automaticamente collegato per tutte le sessioni future di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b1c90-121">This is a one-time step and will be automatically attached for all future Cloud Shell sessions.</span></span>

## <a name="create-an-azure-database-for-postgresql-server-using-azure-cli"></a><span data-ttu-id="b1c90-122">Creare un server di Database di Azure per PostgreSQL tramite l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="b1c90-122">Create an Azure Database for PostgreSQL server using Azure CLI</span></span>

<span data-ttu-id="b1c90-123">Verrà usato Azure Cloud Shell per creare un server di Database di Azure per PostgreSQL con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-123">You'll use Azure Cloud Shell to create an Azure Database for PostgreSQL server using Azure CLI.</span></span> <span data-ttu-id="b1c90-124">Di seguito vengono riportati i passaggi da eseguire.</span><span class="sxs-lookup"><span data-stu-id="b1c90-124">Let's look at the steps you'll take.</span></span>

<span data-ttu-id="b1c90-125">Per prima cosa, accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-125">First, sign into the Azure portal.</span></span>

<span data-ttu-id="b1c90-126">Aprire Cloud Shell dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-126">Open the Cloud Shell from the Azure portal.</span></span> <span data-ttu-id="b1c90-127">Aprire il browser e passare al [portale di Azure](https://portal.azure.com?azure-portal=true), quindi fare clic sul pulsante Apri Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="b1c90-127">Open your browser and go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:</span></span>

![Pulsante Cloud Shell](../media-draft/cloud-shell-button.png)

<span data-ttu-id="b1c90-129">Cloud Shell consente di eseguire i comandi sia in `bash` che in `PowerShell`.</span><span class="sxs-lookup"><span data-stu-id="b1c90-129">Cloud Shell allows you to run your commands either in `bash` or `PowerShell`.</span></span> <span data-ttu-id="b1c90-130">Per tutti gli esempi verrà usata l'opzione della riga di comando `bash`.</span><span class="sxs-lookup"><span data-stu-id="b1c90-130">We'll use the `bash` command-line option for all examples.</span></span>

<span data-ttu-id="b1c90-131">Se si hanno più sottoscrizioni, verificare che sia attiva la sottoscrizione appropriata con il comando seguente, sostituendo gli zero con l'identificatore della sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="b1c90-131">If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.</span></span>

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

<span data-ttu-id="b1c90-132">Verrà eseguito il comando seguente per elencare tutte le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="b1c90-132">You'll run the following command to list all your subscriptions.</span></span>

   ```bash
   az account list --output table
   ```

<span data-ttu-id="b1c90-133">Il passaggio successivo consiste nel creare un gruppo di risorse per gestire il server e l'area in cui verrà posizionato il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="b1c90-133">The next step is to create a resource group to manage the server and where the resource group will be located.</span></span> <span data-ttu-id="b1c90-134">È possibile usare un gruppo di risorse per gestire tutte le risorse correlate al server.</span><span class="sxs-lookup"><span data-stu-id="b1c90-134">Recall, you'll use a resource group to manage all the resources related to your server.</span></span> <span data-ttu-id="b1c90-135">L'opzione di località consente di specificare dove il server viene creato fisicamente.</span><span class="sxs-lookup"><span data-stu-id="b1c90-135">The location option allows you to specify where the server is created physically.</span></span> <span data-ttu-id="b1c90-136">Verrà eseguito il comando successivo e `<resource_group_name>` e `<location>` verranno sostituiti rispettivamente con i valori appropriati.</span><span class="sxs-lookup"><span data-stu-id="b1c90-136">You'll run the next command and replace the `<resource_group_name>` and `<location>` respectively with appropriate values.</span></span>

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

<span data-ttu-id="b1c90-137">L'ultimo passaggio consiste nel creare il server di Database di Azure per PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b1c90-137">The last step is to create the Azure Database for PostgreSQL server.</span></span>

   <span data-ttu-id="b1c90-138">La Guida all'utilizzo dei comandi per la creazione del server dell'interfaccia della riga di comando di Azure che mostra tutti i parametri disponibili è simile a quella illustrata nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b1c90-138">The Azure CLI server creation command usage help showing all available parameters looks like the following example:</span></span>

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

   <span data-ttu-id="b1c90-139">La riga di comando seguente mostra il set di parametri necessario per creare un server di Database di Azure per PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b1c90-139">The following command line shows the required set of parameters to create an Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="b1c90-140">Si noterà che alcuni sono parametri facoltativi e non vengono elencati.</span><span class="sxs-lookup"><span data-stu-id="b1c90-140">You'll notice some are optional parameters and aren't listed.</span></span>

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a><span data-ttu-id="b1c90-141">Descrizioni dei parametri</span><span class="sxs-lookup"><span data-stu-id="b1c90-141">Parameter descriptions</span></span>

<span data-ttu-id="b1c90-142">Il parametro `--resource-group <resource_group_name>` specifica il gruppo di risorse in cui creare il server.</span><span class="sxs-lookup"><span data-stu-id="b1c90-142">The `--resource-group <resource_group_name>` parameter specifies the resource group within which to create the server.</span></span>

<span data-ttu-id="b1c90-143">I parametri `admin-user` e `admin-password` del server specificati sono necessari per effettuare l'accesso al server e ai relativi database.</span><span class="sxs-lookup"><span data-stu-id="b1c90-143">The server `admin-user` and `admin-password` that you specify is required to sign in to the server and its databases.</span></span> <span data-ttu-id="b1c90-144">Prendere nota di queste informazioni per usarle in seguito quando si interagirà con il nuovo server.</span><span class="sxs-lookup"><span data-stu-id="b1c90-144">Remember or record this information for later when interacting with the new server.</span></span>

<span data-ttu-id="b1c90-145">Il parametro `--sku-name` viene usato per specificare una parte del piano tariffario, in questo caso risorsa di calcolo.</span><span class="sxs-lookup"><span data-stu-id="b1c90-145">You use the `--sku-name` parameter is used to specify part of the pricing tier, in this case compute resource.</span></span> <span data-ttu-id="b1c90-146">Il valore segue la convenzione `{pricing tier}_{compute generation}_{vCores}`.</span><span class="sxs-lookup"><span data-stu-id="b1c90-146">The value follows the convention `{pricing tier}_{compute generation}_{vCores}`.</span></span>

<span data-ttu-id="b1c90-147">Esempi:</span><span class="sxs-lookup"><span data-stu-id="b1c90-147">Examples:</span></span>

- <span data-ttu-id="b1c90-148">`--sku-name B_Gen4_4` esegue il mapping a Basic, Gen 4 e 4 vCore.</span><span class="sxs-lookup"><span data-stu-id="b1c90-148">`--sku-name B_Gen4_4` maps to Basic, Gen 4, and 4 vCores.</span></span>
- <span data-ttu-id="b1c90-149">`--sku-name GP_Gen5_32` esegue il mapping a utilizzo generico, Gen 5 e 32 vCore.</span><span class="sxs-lookup"><span data-stu-id="b1c90-149">`--sku-name GP_Gen5_32` maps to General Purpose, Gen 5, and 32 vCores.</span></span>
- <span data-ttu-id="b1c90-150">`--sku-name MO_Gen5_2` esegue il mapping a Ottimizzato per la memoria, Gen 5 e 2 vCore.</span><span class="sxs-lookup"><span data-stu-id="b1c90-150">`--sku-name MO_Gen5_2` maps to Memory Optimized, Gen 5, and 2 vCores.</span></span>

<span data-ttu-id="b1c90-151">I tre piani tariffari sono stati illustrati nell'unità in cui viene creato il server usando il portale.</span><span class="sxs-lookup"><span data-stu-id="b1c90-151">Recall, we discussed the three pricing tiers in the unit where we create the server using the portal.</span></span>

<span data-ttu-id="b1c90-152">Si supponga di voler usare una risorsa di calcolo con le caratteristiche seguenti: Basic, Gen 5 e 1 vCore. In tal caso, si specificherà il parametro come `--sku-name B_Gen5_1`.</span><span class="sxs-lookup"><span data-stu-id="b1c90-152">Let's assume you want to use a Basic, Gen 5, and 1 vCore compute resource, you'll then specify the parameter as `--sku-name B_Gen5_1`.</span></span>

<span data-ttu-id="b1c90-153">Anche il parametro `--storage-size` viene usato per specificare una parte del piano tariffario.</span><span class="sxs-lookup"><span data-stu-id="b1c90-153">You use the `--storage-size` parameter is also used the specify part of the pricing tier.</span></span> <span data-ttu-id="b1c90-154">Se non viene specificato alcun valore, per impostazione predefinita verrà usato il valore 5.120 MB.</span><span class="sxs-lookup"><span data-stu-id="b1c90-154">If the value isn't specified, then it defaults to 5,120 MB.</span></span> <span data-ttu-id="b1c90-155">Le dimensioni di archiviazione valide variano da 5.120 MB con incrementi di 1.024 MB a 1.048.576 MB.</span><span class="sxs-lookup"><span data-stu-id="b1c90-155">Valid storage sizes range from 5,120 MB and increases in additional increments of 1,024 MB up to 1,048,576 MB.</span></span>

<span data-ttu-id="b1c90-156">Il parametro `--backup-retention` viene usato quando è necessario specificare il periodo di conservazione dei backup espresso in giorni.</span><span class="sxs-lookup"><span data-stu-id="b1c90-156">The `--backup-retention` parameter is used when you need to specify the retention period for backups specified in days.</span></span> <span data-ttu-id="b1c90-157">Se non viene specificato alcun valore, per impostazione predefinita verrà usato il valore di 7 giorni.</span><span class="sxs-lookup"><span data-stu-id="b1c90-157">If the value isn't specified, then it defaults to seven days.</span></span>

<span data-ttu-id="b1c90-158">Il parametro `--version` viene usato per specificare la versione principale di PostgreSQL desiderata.</span><span class="sxs-lookup"><span data-stu-id="b1c90-158">You use the `--version` parameter is used to specify the major version of PostgreSQL you would like to use.</span></span>

<span data-ttu-id="b1c90-159">Sono stati finora illustrati i passaggi da eseguire per creare un Database di Azure per PostgreSQL con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-159">You've now seen the steps to create an Azure Database for PostgreSQL using Azure CLI.</span></span> <span data-ttu-id="b1c90-160">Nell'unità successiva verrà creato un server di Database di Azure per PostgreSQL tramite l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c90-160">In the next unit, you'll create an Azure Database for PostgreSQL server using Azure CLI.</span></span>