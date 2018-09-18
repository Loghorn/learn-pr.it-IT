<span data-ttu-id="9c7cb-101">Si decide di creare un Database di Azure per PostgreSQL per archiviare i percorsi acquisiti dai dispositivi di fitness dei runner.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-101">You decide to create an Azure Database for PostgreSQL to store routes captured from runners' fitness devices.</span></span> <span data-ttu-id="9c7cb-102">In base ai volumi di dati cronologici acquisiti si sa che i requisiti di archiviazione del server devono essere impostati su 20 GB.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-102">Based on historic captured data volumes, you know your server storage requirements should be set at 20 GB.</span></span> <span data-ttu-id="9c7cb-103">Per supportare i requisiti di elaborazione è necessario il supporto di una risorsa di calcolo Gen 5 con 1 vCore.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-103">To support your processing requirements, you need compute Gen 5 support with 1 vCore.</span></span> <span data-ttu-id="9c7cb-104">Si sa anche di aver bisogno di un periodo di conservazione di 15 giorni per i backup dei dati.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-104">You also know that you require a retention period of 15 days for data backups.</span></span>

> [!TIP]
> <span data-ttu-id="9c7cb-105">Tutti gli esercizi di Microsoft Learn sono gratuiti, ma quando si inizia a esplorare per proprio conto, è necessario procurarsi una sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-105">All of the exercises you do in Microsoft Learn are free, but once you start exploring on your own, you will need an Azure subscription.</span></span> <span data-ttu-id="9c7cb-106">Se non si dispone di una sottoscrizione, creare un account [gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-106">If you don't have one yet, take a couple of minutes and create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="9c7cb-107">Iniziamo.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-107">Let's begin.</span></span>

<span data-ttu-id="9c7cb-108">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="9c7cb-108">Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

<span data-ttu-id="9c7cb-109">Sarà necessario avviare una sessione di Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-109">Recall that you'll need to start an Azure Cloud Shell session.</span></span> <span data-ttu-id="9c7cb-110">Selezionare l'icona Cloud Shell nella parte superiore della schermata per avviare la sessione.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-110">Select the Cloud Shell icon at the top of the screen to start the session.</span></span>

![Pulsante Cloud Shell](../media-draft/cloud-shell-button.png)

<span data-ttu-id="9c7cb-112">Se non si ha già un account di archiviazione da usare con Cloud Shell, è necessario crearne uno al primo accesso.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-112">If you don't already have a storage account to use with Cloud Shell, you'll need to create one with first access.</span></span> <span data-ttu-id="9c7cb-113">L'interfaccia del portale guiderà l'utente attraverso il processo di creazione di un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-113">The portal interface will step you through the process of creating a storage account.</span></span>

<span data-ttu-id="9c7cb-114">Questa esercitazione usa `bash` come ambiente della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-114">This lab uses `bash` as the command-line environment.</span></span>

1. <span data-ttu-id="9c7cb-115">Selezionare la sottoscrizione da usare per creare il server.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-115">Select the subscription you'll use to create the server.</span></span>

    <span data-ttu-id="9c7cb-116">Se si hanno più sottoscrizioni, verificare che sia attiva la sottoscrizione appropriata con il comando seguente, sostituendo gli zero con l'identificatore della sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-116">If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.</span></span>

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    <span data-ttu-id="9c7cb-117">È possibile elencare tutte le sottoscrizioni con il comando `az account list --output table`.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-117">Recall, you can list all your subscriptions using the `az account list --output table` command.</span></span> <span data-ttu-id="9c7cb-118">Nell'elenco scegliere l'identificatore della sottoscrizione da usare.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-118">Pick the subscription identifier from this list that you'd like to use.</span></span>

1. <span data-ttu-id="9c7cb-119">Se non è ancora stato fatto in un'unità precedente, creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-119">If you haven't already done so in a previous unit, create a resource group.</span></span> <span data-ttu-id="9c7cb-120">Eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-120">You'll run the following command.</span></span>

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > <span data-ttu-id="9c7cb-121">È possibile recuperare un elenco di tutte le località con il comando `az account list-locations`.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-121">You can retrieve a list of all locations using this command `az account list-locations`.</span></span> <span data-ttu-id="9c7cb-122">Selezionare il valore `displayName` o `name` e usarlo per il parametro `<location>`.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-122">Select the `displayName` or `name` value and use it for the `<location>` parameter.</span></span>

1. <span data-ttu-id="9c7cb-123">A questo punto è possibile eseguire il comando `az postgres server create`.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-123">You're now ready to run the `az postgres server create` command.</span></span>

    <span data-ttu-id="9c7cb-124">Tenere presente che le dimensioni di archiviazione del server devono essere impostate su 20 GB e che si deve scegliere il supporto di calcolo Gen 5 con 1 vCore e un periodo di conservazione di 15 giorni per i backup dei dati.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-124">Keep in mind you want to set your server storage size at 20 GB, compute Gen 5 support with 1 vCore and a retention period of 15 days for data backups.</span></span>

    <span data-ttu-id="9c7cb-125">È necessario specificare alcuni parametri:</span><span class="sxs-lookup"><span data-stu-id="9c7cb-125">There are several parameters that you'll specify:</span></span>

    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

    <span data-ttu-id="9c7cb-126">Provare a scrivere il comando e a completare i parametri senza guardare la soluzione riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-126">See if you can write the command and complete the parameters without looking at the solution below.</span></span> <span data-ttu-id="9c7cb-127">Sostituire i valori in `<>` con i propri valori.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-127">Replace the values in `<>` with your own values.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9c7cb-128">È possibile recuperare un elenco di tutte le località con il comando `az account list-locations`.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-128">You can retrieve a list of all locations using this command `az account list-locations`.</span></span> <span data-ttu-id="9c7cb-129">Selezionare il valore `displayName` o `name` e usarlo per il parametro `<location>`.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-129">Select the `displayName` or `name` value and use it for the `<location>` parameter.</span></span>

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

<span data-ttu-id="9c7cb-130">Quando viene seguito, il sistema impiegherà alcuni minuti per elaborare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-130">You'll see the system take a few moments to process the information when executed.</span></span> <span data-ttu-id="9c7cb-131">Se il server è stato creato, viene restituita una stringa JSON (Java Script Object Notation) che descrive il server.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-131">A Java Script Object Notation (JSON) string that describes the server is returned if the server was created.</span></span> <span data-ttu-id="9c7cb-132">Se il server non è stato creato, viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-132">An error message is displayed if the server isn't created.</span></span> <span data-ttu-id="9c7cb-133">Usare le informazioni sull'errore per esaminare e correggere i parametri del comando.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-133">You'll use this error information to review and fix your command parameters.</span></span>

<span data-ttu-id="9c7cb-134">La creazione del server PostgreSQL tramite l'interfaccia della riga di comando di Azure è stata completata.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-134">You've successfully created a PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="9c7cb-135">Nell'unità di successiva verrà illustrato come configurare le impostazioni di sicurezza del server.</span><span class="sxs-lookup"><span data-stu-id="9c7cb-135">In the next unit, you'll see how to configure your server's security settings.</span></span>
