<span data-ttu-id="69170-101">Si supponga di usare un database PostgreSQL locale.</span><span class="sxs-lookup"><span data-stu-id="69170-101">Lets' assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="69170-102">Si gestiscono tutti gli aspetti relativi alla sicurezza ed è stato bloccato l'accesso ai server tramite le regole del firewall standard a livello del server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="69170-102">You're managing all security aspects and locked down all access to your servers using the standard PostgreSQL server level firewall rules.</span></span> <span data-ttu-id="69170-103">A questo punto, si hanno tutte le conoscenze necessarie per configurare le stesse regole del firewall a livello del server in Azure</span><span class="sxs-lookup"><span data-stu-id="69170-103">You now have a good understanding of how to configure the same server level firewall rules in Azure.</span></span>

<span data-ttu-id="69170-104">e sarà possibile connettersi a uno dei server di database di Azure per PostgreSQL creati usando `psql`.</span><span class="sxs-lookup"><span data-stu-id="69170-104">You now have the chance to connect to one of the previous Azure Databases for PostgreSQL servers you created using `psql`.</span></span>

## <a name="allow-azure-service-access"></a><span data-ttu-id="69170-105">Consentire l'accesso ai servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="69170-105">Allow Azure service access</span></span>

<span data-ttu-id="69170-106">Prima di iniziare.</span><span class="sxs-lookup"><span data-stu-id="69170-106">Before we begin.</span></span> <span data-ttu-id="69170-107">Sarà necessario consentire l'accesso ai servizi di Azure per usare PowerShell e `psql` per connettersi al server.</span><span class="sxs-lookup"><span data-stu-id="69170-107">You'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server.</span></span> <span data-ttu-id="69170-108">Si può consentire l'accesso in due modi.</span><span class="sxs-lookup"><span data-stu-id="69170-108">Recall, that you can allow access in two ways.</span></span>

<span data-ttu-id="69170-109">La prima opzione consiste nell'abilitare **Consenti l'accesso a Servizi di Azure**.</span><span class="sxs-lookup"><span data-stu-id="69170-109">Your first option is to enable **Allow access to Azure services**.</span></span> <span data-ttu-id="69170-110">Quando si consente l'accesso, viene creata una regola del firewall anche se la regola non è immessa nell'elenco delle regole personalizzate create dall'utente.</span><span class="sxs-lookup"><span data-stu-id="69170-110">Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.</span></span>

<span data-ttu-id="69170-111">La seconda opzione prevede la creazione di una regola del firewall per consentire l'accesso a tutti gli indirizzi IP.</span><span class="sxs-lookup"><span data-stu-id="69170-111">Your second option is to create a firewall rule that allows access to all IP addresses.</span></span> <span data-ttu-id="69170-112">La regola includerà l'indirizzo IP per il client in cui è in esecuzione PowerShell da cui si vuole eseguire `psql`.</span><span class="sxs-lookup"><span data-stu-id="69170-112">The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.</span></span>

<span data-ttu-id="69170-113">Sarà anche necessario disabilitare l'opzione **Imponi connessione SSL**.</span><span class="sxs-lookup"><span data-stu-id="69170-113">You also need to disable the **Enforce SSL connection**.</span></span>

<span data-ttu-id="69170-114">Iniziamo.</span><span class="sxs-lookup"><span data-stu-id="69170-114">Let's begin.</span></span>

<span data-ttu-id="69170-115">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="69170-115">Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true).</span></span> <span data-ttu-id="69170-116">Passare alla risorsa server per cui si vuole creare una regola del firewall.</span><span class="sxs-lookup"><span data-stu-id="69170-116">Navigate to the server resource for which you would like to create a firewall rule.</span></span>

<span data-ttu-id="69170-117">Selezionare l'opzione **Sicurezza connessione** per aprire il pannello Sicurezza connessione a destra.</span><span class="sxs-lookup"><span data-stu-id="69170-117">Select the **Connection Security** option to open the connection security blade to the right.</span></span>

![Impostazioni di sicurezza del database PostgreSQL](../media-draft/7-db-security-settings.png)

<span data-ttu-id="69170-119">Tenere presente che si vuole consentire l'accesso ai client PowerShell che eseguono `psql`.</span><span class="sxs-lookup"><span data-stu-id="69170-119">Recall, you want to allow access to PowerShell clients running `psql`.</span></span>

<span data-ttu-id="69170-120">È possibile scegliere se:</span><span class="sxs-lookup"><span data-stu-id="69170-120">You can choose to either:</span></span>

- <span data-ttu-id="69170-121">Impostare **Consenti l'accesso a Servizi di Azure** su **SÌ**</span><span class="sxs-lookup"><span data-stu-id="69170-121">Set **Allow access to Azure services** to **ON**</span></span>
- <span data-ttu-id="69170-122">Impostare **Imponi connessione SSL** su **NO**</span><span class="sxs-lookup"><span data-stu-id="69170-122">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="69170-123">Fare clic sul pulsante **Salva** per salvare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="69170-123">Click the **Save** button to save your changes</span></span>

<span data-ttu-id="69170-124">In alternativa, è possibile aggiungere una regola del firewall per consentire l'accesso a tutti gli indirizzi IP.</span><span class="sxs-lookup"><span data-stu-id="69170-124">Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule.</span></span> <span data-ttu-id="69170-125">Usare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="69170-125">Use the following values:</span></span>

- <span data-ttu-id="69170-126">Nome regola: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="69170-126">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="69170-127">Indirizzo IP iniziale: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="69170-127">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="69170-128">Indirizzo IP finale: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="69170-128">End IP: `255.255.255.255`</span></span>
- <span data-ttu-id="69170-129">Impostare **Imponi connessione SSL** su **NO**</span><span class="sxs-lookup"><span data-stu-id="69170-129">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="69170-130">Fare clic sul pulsante **Salva** per salvare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="69170-130">Click the **Save** button to save your changes</span></span>

> [!Warning]
> <span data-ttu-id="69170-131">La creazione di questa regola del firewall consentirà a tutti gli indirizzi IP Internet di provare a connettersi al server.</span><span class="sxs-lookup"><span data-stu-id="69170-131">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="69170-132">Anche se i client non potranno accedere al server senza nome utente e password, prestare attenzione quando si abilita questa regola e assicurarsi di comprendere tutte le implicazioni per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="69170-132">Even though clients will not be able access the server without the username and password, enable this rule with caution and make sure you understand the security implications.</span></span> <span data-ttu-id="69170-133">Negli ambienti di produzione si consentirà l'accesso solo a indirizzi IP client specifici.</span><span class="sxs-lookup"><span data-stu-id="69170-133">In production environments, you'll only allow access to specific client IP addresses.</span></span>

<span data-ttu-id="69170-134">Nella procedura seguente si avvierà una sessione di Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="69170-134">For the next steps, you'll start an Azure Cloud Shell session.</span></span> <span data-ttu-id="69170-135">Questa esercitazione usa `bash` come ambiente della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="69170-135">This lab uses `bash` as the command-line environment.</span></span>

- <span data-ttu-id="69170-136">Aprire Cloud Shell dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="69170-136">Open the Cloud Shell from the Azure portal.</span></span> <span data-ttu-id="69170-137">Passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e fare clic sul pulsante Apri Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="69170-137">Go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:</span></span>

  ![Pulsante Cloud Shell](../media-draft/cloud-shell-button.png)

- <span data-ttu-id="69170-139">Se sono presenti numerose sottoscrizioni, assicurarsi di usare quella corretta.</span><span class="sxs-lookup"><span data-stu-id="69170-139">In case you have several subscriptions, check to make sure you're using the correct subscription when asked.</span></span> <span data-ttu-id="69170-140">È anche possibile eseguire il comando seguente per impostare la sottoscrizione attiva.</span><span class="sxs-lookup"><span data-stu-id="69170-140">You can also run the following command to set the active subscription.</span></span> <span data-ttu-id="69170-141">Ricordare di sostituire gli zeri con l'identificatore della sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="69170-141">Remember to replace the zeros with your subscription identifier.</span></span>

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

- <span data-ttu-id="69170-142">Connettere psql al server usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="69170-142">Connect psql to your server using the following command:</span></span>

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
  ```

   <span data-ttu-id="69170-143">Tenere presente che `server-name` e `admin-user` sono i valori scelti per l'account amministratore al momento della creazione del server.</span><span class="sxs-lookup"><span data-stu-id="69170-143">Recall, `server-name`, and `admin-user` are the values you chose for the administrator account when you created the server.</span></span> <span data-ttu-id="69170-144">`postgres` è il database di gestione predefinito con cui viene creato ogni server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="69170-144">`postgres` is the default management database every PostgreSQL server is created with.</span></span> <span data-ttu-id="69170-145">Verrà richiesto di immettere la stessa password specificata quando è stato creato il server.</span><span class="sxs-lookup"><span data-stu-id="69170-145">You'll be prompted for the password you provided when you created the server.</span></span>

- <span data-ttu-id="69170-146">Dopo aver stabilito la connessione, eseguire il comando `\l` per visualizzare l'elenco di tutti i database.</span><span class="sxs-lookup"><span data-stu-id="69170-146">Once successfully connected, execute the `\l` command to list all databases.</span></span>

   <span data-ttu-id="69170-147">Questo comando restituirà due o più database predefiniti.</span><span class="sxs-lookup"><span data-stu-id="69170-147">This command will result in two or more default databases returned from.</span></span>

- <span data-ttu-id="69170-148">Al termine dell'esecuzione delle operazioni psql nel server, eseguire il comando `\q` per uscire da `psql`.</span><span class="sxs-lookup"><span data-stu-id="69170-148">When you're finished executing psql operations on your server, execute the command `\q` to quit `psql`.</span></span>

- <span data-ttu-id="69170-149">Infine, eseguire il comando `exit` per uscire da Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="69170-149">Finally, to exit Cloud Shell, execute the command `exit`.</span></span>
