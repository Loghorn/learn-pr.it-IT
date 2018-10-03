<span data-ttu-id="5254a-101">Ora che la macchina virtuale è pronta e in esecuzione si vedrà come è possibile usarla.</span><span class="sxs-lookup"><span data-stu-id="5254a-101">Now that your VM is up and running, let's do something with it.</span></span> <span data-ttu-id="5254a-102">Di seguito verrà installato un server Web per visualizzare una semplice pagina Web che mostra il nome host della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-102">Here you'll install a web server and serve up a basic web page that displays the VM's hostname.</span></span>

<span data-ttu-id="5254a-103">Per configurare una macchina virtuale, sono disponibili diverse opzioni.</span><span class="sxs-lookup"><span data-stu-id="5254a-103">To configure a VM, you have several choices.</span></span> <span data-ttu-id="5254a-104">È possibile connettersi direttamente e configurare in modo interattivo il sistema.</span><span class="sxs-lookup"><span data-stu-id="5254a-104">You can connect directly and interactively configure your system.</span></span> <span data-ttu-id="5254a-105">Nei sistemi Windows, ad esempio, è possibile creare una sessione di Desktop remoto per connettersi all'interfaccia utente del computer Windows remoto, come se fosse a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="5254a-105">For example, on Windows systems you can create a Remote Desktop session to connect to the UI of your remote Windows computer as if you were seated at it.</span></span> <span data-ttu-id="5254a-106">Nei sistemi Linux è possibile creare una connessione SSH per usare in modo sicuro il sistema Linux remoto dal terminale.</span><span class="sxs-lookup"><span data-stu-id="5254a-106">On Linux systems, you can create an SSH connection to securely work with your remote Linux system from the terminal.</span></span>

<span data-ttu-id="5254a-107">La configurazione manuale è un buon punto di partenza, ma è possibile automatizzare le distribuzioni man mano che si aggiungono sistemi.</span><span class="sxs-lookup"><span data-stu-id="5254a-107">Manual configuration is a good start, but as you add systems, you can automate your deployments.</span></span> <span data-ttu-id="5254a-108">L'automazione prevede l'esecuzione di processi ripetibili, ad esempio programmi e script che gestiscono automaticamente le operazioni più ripetitive.</span><span class="sxs-lookup"><span data-stu-id="5254a-108">Automation involves running repeatable processes such as programs and scripts that take care of the heavy lifting for you.</span></span>

::: zone pivot="windows-cloud"

<span data-ttu-id="5254a-109">Qui si vedrà come configurare IIS in modalità remota dalla sessione di Cloud Shell usando una funzionalità delle macchine virtuali di Azure basata su Windows denominata estensione per script personalizzati.</span><span class="sxs-lookup"><span data-stu-id="5254a-109">Here, you'll configure IIS remotely from your Cloud Shell session using a feature of Windows-based Azure virtual machines called the Custom Script Extension.</span></span>

::: zone-end

::: zone pivot="linux-cloud"

<span data-ttu-id="5254a-110">Qui si vedrà come configurare Nginx in modalità remota dalla sessione di Cloud Shell usando una funzionalità delle macchine virtuali di Azure basata su Linux denominata estensione per script personalizzati.</span><span class="sxs-lookup"><span data-stu-id="5254a-110">Here, you'll configure Nginx remotely from your Cloud Shell session using a feature of Linux-based Azure virtual machines called the Custom Script Extension.</span></span>

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a><span data-ttu-id="5254a-111">Che cos'è IIS?</span><span class="sxs-lookup"><span data-stu-id="5254a-111">What is IIS?</span></span>

<span data-ttu-id="5254a-112">Internet Information Services, o IIS, è un server Web eseguito in Windows.</span><span class="sxs-lookup"><span data-stu-id="5254a-112">Internet Information Services, or IIS, is a web server that runs on Windows.</span></span> <span data-ttu-id="5254a-113">È possibile usare IIS per rendere disponibile contenuto Web standard (HTML, CSS e JavaScript) o per eseguire ASP.NET o altri tipi di applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="5254a-113">You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET and other kinds of web applications.</span></span> <span data-ttu-id="5254a-114">IIS è incluso in Windows Server, ma è necessario attivarlo per iniziare a servire pagine Web.</span><span class="sxs-lookup"><span data-stu-id="5254a-114">IIS comes with Windows Server, but you need to activate it to start serving web pages.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="5254a-115">Che cos'è l'estensione per script personalizzati?</span><span class="sxs-lookup"><span data-stu-id="5254a-115">What's the Custom Script Extension?</span></span>

<span data-ttu-id="5254a-116">L'estensione per script personalizzati è un modo semplice per scaricare ed eseguire script nelle macchine virtuali di Azure.</span><span class="sxs-lookup"><span data-stu-id="5254a-116">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="5254a-117">È solo uno dei tanti modi in cui è possibile configurare il sistema dopo aver preparato e attivato la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-117">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="5254a-118">È possibile archiviare gli script in Archiviazione di Azure o in una posizione pubblica, ad esempio GitHub.</span><span class="sxs-lookup"><span data-stu-id="5254a-118">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="5254a-119">Gli script possono essere eseguiti manualmente o come parte di una distribuzione più automatizzata.</span><span class="sxs-lookup"><span data-stu-id="5254a-119">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="5254a-120">In questo caso, si eseguirà un comando dell'interfaccia della riga di comando di Azure per scaricare uno script di PowerShell da GitHub ed eseguirlo nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-120">Here, you'll run an Azure CLI command to download a PowerShell script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="5254a-121">Lo script configura IIS.</span><span class="sxs-lookup"><span data-stu-id="5254a-121">The script configures IIS.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="5254a-122">Configurare IIS</span><span class="sxs-lookup"><span data-stu-id="5254a-122">Configure IIS</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="5254a-123">In questo caso si userà l'estensione per script personalizzati per configurare IIS in modalità remota nella macchina virtuale da Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="5254a-123">Here you'll use the Custom Script Extension to configure IIS remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="5254a-124">Verrà anche configurato il firewall per consentire l'accesso di rete in ingresso sulla porta 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="5254a-124">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="5254a-125">Da Cloud Shell, eseguire questo comando `az vm extension set` per scaricare ed eseguire uno script di PowerShell che installa IIS e configura una semplice home page.</span><span class="sxs-lookup"><span data-stu-id="5254a-125">From Cloud Shell, run this `az vm extension set` command to download and execute a PowerShell script that installs IIS and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    <span data-ttu-id="5254a-126">Il processo di configurazione di IIS, impostazione dei contenuti della home page e avvio del servizio richiede un paio di minuti per il completamento.</span><span class="sxs-lookup"><span data-stu-id="5254a-126">The process to configure IIS, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="5254a-127">Nel frattempo è possibile [esaminare lo script di PowerShell](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) in una scheda separata del browser.</span><span class="sxs-lookup"><span data-stu-id="5254a-127">In the meantime, you can [examine the PowerShell script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="5254a-128">Lo script installa IIS e configura la home page per visualizzare un messaggio di benvenuto insieme al nome computer della macchina virtuale, "myVM".</span><span class="sxs-lookup"><span data-stu-id="5254a-128">The script installs IIS and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="5254a-129">Eseguire questo comando `az vm open-port` per aprire la porta 80 (HTTP) attraverso il firewall.</span><span class="sxs-lookup"><span data-stu-id="5254a-129">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="5254a-130">Verificare la configurazione</span><span class="sxs-lookup"><span data-stu-id="5254a-130">Verify the configuration</span></span>

<span data-ttu-id="5254a-131">Ora che IIS è configurato, verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5254a-131">Now that IIS is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="5254a-132">Eseguire questo comando `az vm list-ip-addresses` per elencare gli indirizzi IP pubblici della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-132">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="5254a-133">L'argomento `--query` rende leggermente complesso questo comando.</span><span class="sxs-lookup"><span data-stu-id="5254a-133">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="5254a-134">Man mano che si esplora e si approfondisce Azure, diventerà più chiaro.</span><span class="sxs-lookup"><span data-stu-id="5254a-134">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="5254a-135">Viene visualizzato l'indirizzo IP pubblico della macchina virtuale, ad esempio 104.211.9.245.</span><span class="sxs-lookup"><span data-stu-id="5254a-135">You see your VM's public IP address, for example, 104.211.9.245.</span></span>

1. <span data-ttu-id="5254a-136">In una nuova scheda del browser, passare all'indirizzo IP della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-136">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="5254a-137">Viene visualizzato il messaggio di benvenuto e il nome della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-137">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a><span data-ttu-id="5254a-138">Che cos'è Nginx?</span><span class="sxs-lookup"><span data-stu-id="5254a-138">What is Nginx?</span></span>

<span data-ttu-id="5254a-139">Nginx (pronunciato "engine-x") è un server Web diffuso, gratuito e open source che può essere eseguito in Unix, Linux, macOS e Windows.</span><span class="sxs-lookup"><span data-stu-id="5254a-139">Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, macOS, and Windows.</span></span> <span data-ttu-id="5254a-140">In questo caso si userà Nginx per presentare una semplice pagina Web.</span><span class="sxs-lookup"><span data-stu-id="5254a-140">Here you'll use Nginx to serve a basic web page.</span></span>

## <a name="whats-the-custom-script-extension"></a><span data-ttu-id="5254a-141">Che cos'è l'estensione per script personalizzati?</span><span class="sxs-lookup"><span data-stu-id="5254a-141">What's the Custom Script Extension?</span></span>

<span data-ttu-id="5254a-142">L'estensione per script personalizzati è un modo semplice per scaricare ed eseguire script nelle macchine virtuali di Azure.</span><span class="sxs-lookup"><span data-stu-id="5254a-142">The Custom Script Extension is an easy way to download and run scripts on your Azure VMs.</span></span> <span data-ttu-id="5254a-143">È solo uno dei tanti modi in cui è possibile configurare il sistema dopo aver preparato e attivato la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-143">It's just one of the many ways you can configure the system once your VM is up and running.</span></span>

<span data-ttu-id="5254a-144">È possibile archiviare gli script in Archiviazione di Azure o in una posizione pubblica, ad esempio GitHub.</span><span class="sxs-lookup"><span data-stu-id="5254a-144">You can store your scripts in Azure storage or in a public location such as GitHub.</span></span> <span data-ttu-id="5254a-145">Gli script possono essere eseguiti manualmente o come parte di una distribuzione più automatizzata.</span><span class="sxs-lookup"><span data-stu-id="5254a-145">You can run scripts manually or as part of a more automated deployment.</span></span> <span data-ttu-id="5254a-146">In questo caso, si eseguirà un comando dell'interfaccia della riga di comando di Azure per scaricare uno script Bash da GitHub ed eseguirlo nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-146">Here, you'll run an Azure CLI command to download a Bash script from GitHub and execute it on your VM.</span></span> <span data-ttu-id="5254a-147">Lo script configura Nginx.</span><span class="sxs-lookup"><span data-stu-id="5254a-147">The script configures Nginx.</span></span>

## <a name="configure-nginx"></a><span data-ttu-id="5254a-148">Configurare Nginx</span><span class="sxs-lookup"><span data-stu-id="5254a-148">Configure Nginx</span></span>

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

<span data-ttu-id="5254a-149">In questo caso si userà l'estensione per script personalizzati per configurare Nginx in modalità remota nella macchina virtuale da Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="5254a-149">Here you'll use the Custom Script Extension to configure Nginx remotely on your VM from Cloud Shell.</span></span> <span data-ttu-id="5254a-150">Verrà anche configurato il firewall per consentire l'accesso di rete in ingresso sulla porta 80 (HTTP).</span><span class="sxs-lookup"><span data-stu-id="5254a-150">You'll also configure the firewall to allow inbound network access on port 80 (HTTP).</span></span>

1. <span data-ttu-id="5254a-151">Da Cloud Shell, eseguire questo comando `az vm extension set` per scaricare ed eseguire uno script Bash che installa Nginx e configura una semplice home page.</span><span class="sxs-lookup"><span data-stu-id="5254a-151">From Cloud Shell, run this `az vm extension set` command to download and execute a Bash script that installs Nginx and configures a basic home page.</span></span>

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    <span data-ttu-id="5254a-152">Il processo di configurazione di Nginx, impostazione dei contenuti della home page e avvio del servizio richiede un paio di minuti per il completamento.</span><span class="sxs-lookup"><span data-stu-id="5254a-152">The process to configure Nginx, set the contents of the homepage, and start the service takes a couple minutes to complete.</span></span>

    <span data-ttu-id="5254a-153">Nel frattempo è possibile [esaminare lo script Bash](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) in una scheda separata del browser.</span><span class="sxs-lookup"><span data-stu-id="5254a-153">In the meantime, you can [examine the Bash script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) from a separate browser tab if you'd like.</span></span> <span data-ttu-id="5254a-154">Lo script installa Nginx e configura la home page per visualizzare un messaggio di benvenuto insieme al nome computer della macchina virtuale, "myVM".</span><span class="sxs-lookup"><span data-stu-id="5254a-154">The script installs Nginx and configures the home page to display a welcome message along with the VM's computer name, "myVM".</span></span>

1. <span data-ttu-id="5254a-155">Eseguire questo comando `az vm open-port` per aprire la porta 80 (HTTP) attraverso il firewall.</span><span class="sxs-lookup"><span data-stu-id="5254a-155">Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.</span></span>

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a><span data-ttu-id="5254a-156">Verificare la configurazione</span><span class="sxs-lookup"><span data-stu-id="5254a-156">Verify the configuration</span></span>

<span data-ttu-id="5254a-157">Ora che Nginx è configurato, verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5254a-157">Now that Nginx is set up, let's verify that it's running.</span></span>

1. <span data-ttu-id="5254a-158">Eseguire questo comando `az vm list-ip-addresses` per elencare gli indirizzi IP pubblici della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-158">Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.</span></span>

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > <span data-ttu-id="5254a-159">L'argomento `--query` rende leggermente complesso questo comando.</span><span class="sxs-lookup"><span data-stu-id="5254a-159">This `--query` argument makes this command a bit complex.</span></span> <span data-ttu-id="5254a-160">Man mano che si esplora e si approfondisce Azure, diventerà più chiaro.</span><span class="sxs-lookup"><span data-stu-id="5254a-160">But you'll be a pro at this as you dig in and explore Azure.</span></span>

    <span data-ttu-id="5254a-161">Viene visualizzato l'indirizzo IP pubblico della macchina virtuale, ad esempio 104.211.9.245.</span><span class="sxs-lookup"><span data-stu-id="5254a-161">You see your VM's public IP address, for example, 104.211.9.245.</span></span>

1. <span data-ttu-id="5254a-162">In una nuova scheda del browser, passare all'indirizzo IP della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-162">In a new browser tab, navigate to your VM's IP address.</span></span> <span data-ttu-id="5254a-163">Viene visualizzato il messaggio di benvenuto e il nome della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="5254a-163">You see your welcome message and your VM's name.</span></span>

    ![](../media/4-nginx-browser.png)

::: zone-end

## <a name="summary"></a><span data-ttu-id="5254a-164">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="5254a-164">Summary</span></span>

<span data-ttu-id="5254a-165">La macchina virtuale è in esecuzione e può ora servire pagine Web. E adesso?</span><span class="sxs-lookup"><span data-stu-id="5254a-165">Your VM is running and can now serve up web pages, but what does that mean for you?</span></span>

<span data-ttu-id="5254a-166">Tenere presente che ogni percorso inizia dalla base e che quasi tutte le innovazioni eccezionali nate nel cloud, da aziende di ogni dimensione, sono partite da una configurazione simile a quella qui presentata.</span><span class="sxs-lookup"><span data-stu-id="5254a-166">Remember, every journey starts with the basics, and almost any great innovation born in the cloud, from companies big and small, started with a similar setup to yours.</span></span> <span data-ttu-id="5254a-167">Man mano che evolvono, le idee inizieranno ad avere un impatto positivo sia sull'azienda che sugli utenti.</span><span class="sxs-lookup"><span data-stu-id="5254a-167">As your idea evolves, it begins making a positive impact on your business and your users.</span></span>