<span data-ttu-id="c6c81-101">Si supponga di usare un database PostgreSQL locale.</span><span class="sxs-lookup"><span data-stu-id="c6c81-101">Let's assume that you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="c6c81-102">Si gestiscono tutti gli aspetti relativi alla sicurezza e si è bloccato l'accesso ai server tramite le regole del firewall standard a livello di server di PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="c6c81-102">You're managing all security aspects and you've locked down all access to your servers using the standard PostgreSQL server-level firewall rules.</span></span> <span data-ttu-id="c6c81-103">A questo punto si hanno tutte le conoscenze necessarie per configurare le stesse regole del firewall a livello di server in Azure.</span><span class="sxs-lookup"><span data-stu-id="c6c81-103">You now have a good understanding of how to configure the same server-level firewall rules in Azure.</span></span>

<span data-ttu-id="c6c81-104">Di seguito viene stabilita la connessione a uno dei server di Database di Azure per PostgreSQL creati.</span><span class="sxs-lookup"><span data-stu-id="c6c81-104">Let's connect to one of the Azure Database for PostgreSQL servers that you created.</span></span>

## <a name="allow-azure-service-access"></a><span data-ttu-id="c6c81-105">Consentire l'accesso ai servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="c6c81-105">Allow Azure service access</span></span>

<span data-ttu-id="c6c81-106">Prima di iniziare è necessario consentire l'accesso ai servizi di Azure se si vuole usare PowerShell e `psql` per connettersi al server.</span><span class="sxs-lookup"><span data-stu-id="c6c81-106">Before we begin, you'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server.</span></span> <span data-ttu-id="c6c81-107">Si può consentire l'accesso in due modi.</span><span class="sxs-lookup"><span data-stu-id="c6c81-107">Recall that you can allow access in two ways.</span></span>

<span data-ttu-id="c6c81-108">La prima opzione consiste nell'abilitare **Consenti l'accesso a Servizi di Azure**.</span><span class="sxs-lookup"><span data-stu-id="c6c81-108">Your first option is to enable **Allow access to Azure services**.</span></span> <span data-ttu-id="c6c81-109">Quando si consente l'accesso, viene creata una regola del firewall anche se la regola non è immessa nell'elenco delle regole personalizzate create dall'utente.</span><span class="sxs-lookup"><span data-stu-id="c6c81-109">Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.</span></span>

<span data-ttu-id="c6c81-110">La seconda opzione prevede la creazione di una regola del firewall per consentire l'accesso a tutti gli indirizzi IP.</span><span class="sxs-lookup"><span data-stu-id="c6c81-110">Your second option is to create a firewall rule that allows access to all IP addresses.</span></span> <span data-ttu-id="c6c81-111">La regola includerà l'indirizzo IP per il client in cui è in esecuzione PowerShell da cui si vuole eseguire `psql`.</span><span class="sxs-lookup"><span data-stu-id="c6c81-111">The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.</span></span>

<span data-ttu-id="c6c81-112">Sarà anche necessario disabilitare l'opzione **Imponi connessione SSL**.</span><span class="sxs-lookup"><span data-stu-id="c6c81-112">You also need to disable the **Enforce SSL connection** option.</span></span>

### <a name="create-a-firewall-rule"></a><span data-ttu-id="c6c81-113">Creare una regola del firewall</span><span class="sxs-lookup"><span data-stu-id="c6c81-113">Create a firewall rule</span></span>

1. <span data-ttu-id="c6c81-114">Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui si è attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="c6c81-114">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span> 

1. <span data-ttu-id="c6c81-115">Passare alla risorsa server per la quale si vuole creare una regola del firewall.</span><span class="sxs-lookup"><span data-stu-id="c6c81-115">Navigate to the server resource for which you would like to create a firewall rule.</span></span>

1. <span data-ttu-id="c6c81-116">Selezionare l'opzione **Sicurezza connessione** per aprire il relativo pannello a destra.</span><span class="sxs-lookup"><span data-stu-id="c6c81-116">Select the **Connection Security** option to open the connection security blade to the right.</span></span>

    ![Screenshot del portale di Azure che illustra la sezione Sicurezza connessione del pannello della risorsa di database PostgreSQL](../media/7-db-security-settings.png)

<span data-ttu-id="c6c81-118">Tenere presente che si vuole consentire l'accesso ai client PowerShell che eseguono `psql`.</span><span class="sxs-lookup"><span data-stu-id="c6c81-118">Recall that you want to allow access to PowerShell clients running `psql`.</span></span>

<span data-ttu-id="c6c81-119">È possibile scegliere se:</span><span class="sxs-lookup"><span data-stu-id="c6c81-119">You can choose to either:</span></span>

- <span data-ttu-id="c6c81-120">Impostare **Consenti l'accesso a Servizi di Azure** su **SÌ**</span><span class="sxs-lookup"><span data-stu-id="c6c81-120">Set **Allow access to Azure services** to **ON**</span></span>
- <span data-ttu-id="c6c81-121">Impostare **Imponi connessione SSL** su **NO**</span><span class="sxs-lookup"><span data-stu-id="c6c81-121">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="c6c81-122">Fare clic sul pulsante **Salva** per salvare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="c6c81-122">Click the **Save** button to save your changes</span></span>

<span data-ttu-id="c6c81-123">In alternativa, è possibile aggiungere una regola del firewall per consentire l'accesso a tutti gli indirizzi IP.</span><span class="sxs-lookup"><span data-stu-id="c6c81-123">Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule.</span></span> <span data-ttu-id="c6c81-124">Usare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="c6c81-124">Use the following values:</span></span>

- <span data-ttu-id="c6c81-125">Nome regola: `AllowAll`</span><span class="sxs-lookup"><span data-stu-id="c6c81-125">Rule Name: `AllowAll`</span></span>
- <span data-ttu-id="c6c81-126">Indirizzo IP iniziale: `0.0.0.0`</span><span class="sxs-lookup"><span data-stu-id="c6c81-126">Start IP: `0.0.0.0`</span></span>
- <span data-ttu-id="c6c81-127">Indirizzo IP finale: `255.255.255.255`</span><span class="sxs-lookup"><span data-stu-id="c6c81-127">End IP: `255.255.255.255`</span></span>
- <span data-ttu-id="c6c81-128">Impostare **Imponi connessione SSL** su **NO**</span><span class="sxs-lookup"><span data-stu-id="c6c81-128">Set **Enforce SSL connection** to **DISABLED**</span></span>
- <span data-ttu-id="c6c81-129">Fare clic sul pulsante **Salva** per salvare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="c6c81-129">Click the **Save** button to save your changes</span></span>

> [!Warning]
> <span data-ttu-id="c6c81-130">La creazione di questa regola del firewall consentirà a tutti gli indirizzi IP in Internet di provare a connettersi al server.</span><span class="sxs-lookup"><span data-stu-id="c6c81-130">Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server.</span></span> <span data-ttu-id="c6c81-131">Negli ambienti di produzione è opportuno limitare l'accesso a indirizzi IP specifici.</span><span class="sxs-lookup"><span data-stu-id="c6c81-131">In production environments, you'll want to restrict access to specific IP addresses.</span></span>

### <a name="connect-to-the-database-with-psql"></a><span data-ttu-id="c6c81-132">Connettersi al database con psql</span><span class="sxs-lookup"><span data-stu-id="c6c81-132">Connect to the database with psql</span></span>

1. <span data-ttu-id="c6c81-133">In Azure Cloud Shell nella parte destra della schermata connettere PSQL al server usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="c6c81-133">In the Azure Cloud Shell on the right, connect PSQL to your server using the following command.</span></span> <span data-ttu-id="c6c81-134">Assicurarsi di sostituire il nome del server e il nome dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="c6c81-134">Make sure to replace the server name and admin name.</span></span>

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
    ```
    
    <span data-ttu-id="c6c81-135">Usare i valori scelti per `server-name` e `admin-user`.</span><span class="sxs-lookup"><span data-stu-id="c6c81-135">Use the values you chose for the `server-name`, and `admin-user`.</span></span> 

1. <span data-ttu-id="c6c81-136">**postgres** è il database di gestione predefinito con cui viene creato ogni server PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="c6c81-136">**postgres** is the default management database every PostgreSQL server is created with.</span></span> <span data-ttu-id="c6c81-137">Verrà richiesto di immettere la stessa password specificata quando è stato creato il server.</span><span class="sxs-lookup"><span data-stu-id="c6c81-137">You'll be prompted for the password you provided when you created the server.</span></span>

1. <span data-ttu-id="c6c81-138">Dopo aver stabilito la connessione, eseguire il comando <kbd>\l</kbd> per visualizzare l'elenco di tutti i database.</span><span class="sxs-lookup"><span data-stu-id="c6c81-138">Once successfully connected, execute the <kbd>\l</kbd> command to list all databases.</span></span> <span data-ttu-id="c6c81-139">Questo comando restituirà due o più database predefiniti.</span><span class="sxs-lookup"><span data-stu-id="c6c81-139">This command will result in two or more default databases returned.</span></span>

1. <span data-ttu-id="c6c81-140">Premere <kbd>q</kbd> per uscire dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="c6c81-140">Hit <kbd>q</kbd> to exit the list.</span></span>

1. <span data-ttu-id="c6c81-141">È possibile provare altri comandi PSQL.</span><span class="sxs-lookup"><span data-stu-id="c6c81-141">You can try other PSQL commands.</span></span>
    - <kbd>\?</kbd> <span data-ttu-id="c6c81-142">per ottenere la guida.</span><span class="sxs-lookup"><span data-stu-id="c6c81-142">to get help.</span></span>
    - <span data-ttu-id="c6c81-143"><kbd>\dt</kbd> per elencare le tabelle.</span><span class="sxs-lookup"><span data-stu-id="c6c81-143"><kbd>\dt</kbd> to list the tables.</span></span>

1. <span data-ttu-id="c6c81-144">Al termine dell'esecuzione delle operazioni PSQL nel server, eseguire il comando <kbd>\q</kbd> per uscire da PSQL.</span><span class="sxs-lookup"><span data-stu-id="c6c81-144">When you're finished executing PSQL operations on your server, execute the command <kbd>\q</kbd> to quit PSQL.</span></span>
