<span data-ttu-id="7cd00-101">È presente un data center locale che si prevede di mantenere, ma si vuole usare Azure per l'offload dei picchi di traffico tramite macchine virtuali ospitate in Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-101">You have an on-premises datacenter that you plan to keep, but you want to use Azure to offload peak traffic using virtual machines (VMs) hosted in Azure.</span></span> <span data-ttu-id="7cd00-102">Si vuole sapere se è possibile mantenere lo schema di indirizzi IP e le appliance di rete esistenti, assicurando al tempo stesso la protezione di qualsiasi trasferimento di dati.</span><span class="sxs-lookup"><span data-stu-id="7cd00-102">You need to know if you can keep your existing IP addressing scheme and network appliances, while ensuring that any data transfer is secure.</span></span>

## <a name="what-is-azure-virtual-networking"></a><span data-ttu-id="7cd00-103">Informazioni sulla rete virtuale di Azure</span><span class="sxs-lookup"><span data-stu-id="7cd00-103">What is Azure virtual networking?</span></span>

<span data-ttu-id="7cd00-104">Le **reti virtuali di Azure** consentono alle risorse di Azure, ad esempio le macchine virtuali, le app Web e i database, di comunicare tra loro, con gli utenti su Internet e con i computer client locali.</span><span class="sxs-lookup"><span data-stu-id="7cd00-104">**Azure virtual networks** enable Azure resources, such as virtual machines, web apps, and databases, to communicate with: each other, users on the internet, and on-premises client computers.</span></span> <span data-ttu-id="7cd00-105">Una rete di Azure può essere considerata come un set di risorse che consente il collegamento di altre risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-105">You can think of an Azure network as a set of resources that links other Azure resources.</span></span>

<span data-ttu-id="7cd00-106">Le reti virtuali di Azure offrono funzionalità di rete essenziali, tra cui:</span><span class="sxs-lookup"><span data-stu-id="7cd00-106">Azure virtual networks provide key networking capabilities:</span></span>

- <span data-ttu-id="7cd00-107">Isolamento e segmentazione</span><span class="sxs-lookup"><span data-stu-id="7cd00-107">Isolation and segmentation</span></span>
- <span data-ttu-id="7cd00-108">Comunicazioni Internet</span><span class="sxs-lookup"><span data-stu-id="7cd00-108">Internet communications</span></span>
- <span data-ttu-id="7cd00-109">Comunicazione tra risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="7cd00-109">Communicate between Azure resources</span></span>
- <span data-ttu-id="7cd00-110">Comunicazione con le risorse locali</span><span class="sxs-lookup"><span data-stu-id="7cd00-110">Communicate with on-premises resources</span></span>
- <span data-ttu-id="7cd00-111">Routing del traffico di rete</span><span class="sxs-lookup"><span data-stu-id="7cd00-111">Route network traffic</span></span>
- <span data-ttu-id="7cd00-112">Filtri per il traffico di rete</span><span class="sxs-lookup"><span data-stu-id="7cd00-112">Filter network traffic</span></span>
- <span data-ttu-id="7cd00-113">Connessione di reti virtuali</span><span class="sxs-lookup"><span data-stu-id="7cd00-113">Connect virtual networks</span></span>

### <a name="isolation-and-segmentation"></a><span data-ttu-id="7cd00-114">Isolamento e segmentazione</span><span class="sxs-lookup"><span data-stu-id="7cd00-114">Isolation and segmentation</span></span>

<span data-ttu-id="7cd00-115">Azure consente di creare più reti virtuali isolate.</span><span class="sxs-lookup"><span data-stu-id="7cd00-115">Azure allows you to create multiple isolated virtual networks.</span></span> <span data-ttu-id="7cd00-116">Quando si configura una rete virtuale, si definisce uno spazio indirizzi IP (Internet Protocol) privato mediante intervalli di indirizzi IP pubblici o privati.</span><span class="sxs-lookup"><span data-stu-id="7cd00-116">When you set up a virtual network, you define a private Internet Protocol (IP) address space, using either public or private IP address ranges.</span></span> <span data-ttu-id="7cd00-117">Quindi è possibile segmentare tale spazio indirizzi IP in subnet e allocare parte dello spazio indirizzi definito a ogni subnet denominata.</span><span class="sxs-lookup"><span data-stu-id="7cd00-117">You can then segment that IP address space into subnets, and allocate part of the defined address space to each named subnet.</span></span>

<span data-ttu-id="7cd00-118">Per la risoluzione dei nomi è possibile usare il servizio di risoluzione dei nomi incorporato in Azure o configurare la rete virtuale per l'uso di un server DNS (Domain Name System) interno o esterno.</span><span class="sxs-lookup"><span data-stu-id="7cd00-118">For name resolution, you can use the name resolution service that's built in to Azure, or you can configure the virtual network to use either an internal or an external Domain Name System (DNS) server.</span></span>

### <a name="internet-communications"></a><span data-ttu-id="7cd00-119">Comunicazioni Internet</span><span class="sxs-lookup"><span data-stu-id="7cd00-119">Internet communications</span></span>

<span data-ttu-id="7cd00-120">Una macchina virtuale (VM) in Azure può connettersi a Internet per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7cd00-120">A VM in Azure can connect to the internet by default.</span></span> <span data-ttu-id="7cd00-121">È tuttavia necessario connettersi a tale VM e controllarla tramite l'interfaccia della riga di comando di Azure, Remote Desktop Protocol (RDP) o Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="7cd00-121">However, you need to connect to and control that VM, with either the Azure CLI, Remote Desktop Protocol (RDP), or Secure Shell (SSH).</span></span> <span data-ttu-id="7cd00-122">È possibile abilitare le comunicazioni in ingresso definendo un indirizzo IP pubblico o un servizio di bilanciamento del carico pubblico.</span><span class="sxs-lookup"><span data-stu-id="7cd00-122">You can enable incoming communications by defining a public IP address or a public load balancer.</span></span>

### <a name="communicate-between-azure-resources"></a><span data-ttu-id="7cd00-123">Comunicazione tra risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="7cd00-123">Communicate between Azure resources</span></span>

<span data-ttu-id="7cd00-124">È consigliabile consentire alle risorse di Azure di comunicare tra loro in modo sicuro.</span><span class="sxs-lookup"><span data-stu-id="7cd00-124">You will want to enable Azure resources to communicate securely with each other.</span></span> <span data-ttu-id="7cd00-125">È possibile ottenere questo risultato in due modi:</span><span class="sxs-lookup"><span data-stu-id="7cd00-125">You can do that in one of two ways:</span></span>

- <span data-ttu-id="7cd00-126">**Reti virtuali**</span><span class="sxs-lookup"><span data-stu-id="7cd00-126">**Virtual networks**</span></span>
    
    <span data-ttu-id="7cd00-127">Le reti virtuali possono connettere non solo le VM ma anche altre risorse di Azure, tra cui l'ambiente del servizio app, il servizio Kubernetes di Azure e i set di scalabilità delle macchine virtuali di Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-127">Virtual networks can connect not only VMs, but other Azure resources, such as the App Service Environment, Azure Kubernetes Service, and Azure virtual machine scale sets.</span></span>

- <span data-ttu-id="7cd00-128">**Endpoint di servizio**</span><span class="sxs-lookup"><span data-stu-id="7cd00-128">**Service endpoints**</span></span>
     
     <span data-ttu-id="7cd00-129">È possibile usare gli endpoint di servizio per la connessione ad altri tipi di risorse di Azure, ad esempio i database SQL di Azure e gli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="7cd00-129">You can use service endpoints to connect to other Azure resource types, such as Azure SQL databases and storage accounts.</span></span> <span data-ttu-id="7cd00-130">Questo approccio consente di collegare più risorse di Azure alle reti virtuali, migliorando la sicurezza e offrendo un routing ottimale tra le risorse.</span><span class="sxs-lookup"><span data-stu-id="7cd00-130">This approach enables you to link multiple Azure resources to virtual networks, thereby improving security and providing optimal routing between resources.</span></span>

### <a name="communicate-with-on-premises-resources"></a><span data-ttu-id="7cd00-131">Comunicazione con le risorse locali</span><span class="sxs-lookup"><span data-stu-id="7cd00-131">Communicate with on-premises resources</span></span>

<span data-ttu-id="7cd00-132">Le reti virtuali di Azure consentono di collegare tra loro le risorse nell'ambiente locale ed entro la sottoscrizione di Azure, creando di fatto una rete che include gli ambienti locale e cloud.</span><span class="sxs-lookup"><span data-stu-id="7cd00-132">Azure virtual networks enable you to link resources together in your on-premises environment and within your Azure subscription, in effect creating a network that spans both your local and cloud environments.</span></span> <span data-ttu-id="7cd00-133">Sono disponibili tre meccanismi per ottenere tale connettività:</span><span class="sxs-lookup"><span data-stu-id="7cd00-133">There are three mechanisms for you to achieve this connectivity:</span></span>

- <span data-ttu-id="7cd00-134">**VPN da punto a sito**</span><span class="sxs-lookup"><span data-stu-id="7cd00-134">**Point-to-site VPNs**</span></span>

   <span data-ttu-id="7cd00-135">Questo approccio è analogo a una connessione VPN stabilita con la rete aziendale da un computer esterno all'organizzazione, ma con direzione opposta.</span><span class="sxs-lookup"><span data-stu-id="7cd00-135">This approach is like a VPN connection that a computer outside your organization makes back into your corporate network, except that it's working in the opposite direction.</span></span> <span data-ttu-id="7cd00-136">In questo caso il computer client attiva una connessione VPN crittografata ad Azure, connettendo il computer alla rete virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-136">In this case, the client computer initiates an encrypted VPN connection to Azure, connecting that computer to the Azure virtual network.</span></span>

- <span data-ttu-id="7cd00-137">**VPN da sito a sito** Una VPN da sito a sito collega il dispositivo o il gateway VPN locale al gateway VPN di Azure in una rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-137">**Site-to-site VPNs** A site-to-site VPN links your on-premises VPN device or gateway to the Azure VPN gateway in a virtual network.</span></span> <span data-ttu-id="7cd00-138">I dispositivi in Azure possono essere in effetti visualizzati come se si trovassero nella rete locale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-138">In effect, the devices in Azure can appear as being on the local network.</span></span> <span data-ttu-id="7cd00-139">La connessione è crittografata e viene effettuata tramite Internet.</span><span class="sxs-lookup"><span data-stu-id="7cd00-139">The connection is encrypted and works over the internet.</span></span>

- <span data-ttu-id="7cd00-140">**Azure ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="7cd00-140">**Azure ExpressRoute**</span></span>

    <span data-ttu-id="7cd00-141">Per gli ambienti che richiedono larghezza di banda superiore e livelli di sicurezza ancora più elevati, Azure ExpressRoute è l'approccio ottimale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-141">For environments where you need greater bandwidth and even higher levels of security, Azure ExpressRoute is the best approach.</span></span> <span data-ttu-id="7cd00-142">Azure ExpressRoute offre ad Azure una connettività privata dedicata, non basata su Internet.</span><span class="sxs-lookup"><span data-stu-id="7cd00-142">Azure ExpressRoute provides dedicated private connectivity to Azure that does not travel over the internet.</span></span>

### <a name="route-network-traffic"></a><span data-ttu-id="7cd00-143">Routing del traffico di rete</span><span class="sxs-lookup"><span data-stu-id="7cd00-143">Route network traffic</span></span>

<span data-ttu-id="7cd00-144">Per impostazione predefinita, Azure indirizzerà il traffico tra subnet su qualsiasi rete virtuale connessa, su reti locali e su Internet.</span><span class="sxs-lookup"><span data-stu-id="7cd00-144">By default, Azure will route traffic between subnets on any connected virtual networks, on-premises networks, and the internet.</span></span> <span data-ttu-id="7cd00-145">È tuttavia possibile controllare il routing ed eseguire l'override di queste impostazioni, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="7cd00-145">However, you can control routing and override those settings as follows:</span></span>

- <span data-ttu-id="7cd00-146">**Tabelle di route**</span><span class="sxs-lookup"><span data-stu-id="7cd00-146">**Route tables**</span></span>

    <span data-ttu-id="7cd00-147">Una tabella di route consente di definire regole per l'indirizzamento del traffico.</span><span class="sxs-lookup"><span data-stu-id="7cd00-147">A route table allows you to define rules as to how traffic should be directed.</span></span> <span data-ttu-id="7cd00-148">È possibile creare tabelle di route personalizzate che controllano il modo in cui i pacchetti vengono indirizzati tra subnet.</span><span class="sxs-lookup"><span data-stu-id="7cd00-148">You can create custom route tables that control how packets are routed between subnets.</span></span>

- <span data-ttu-id="7cd00-149">**Border Gateway Protocol**</span><span class="sxs-lookup"><span data-stu-id="7cd00-149">**Border Gateway Protocol**</span></span>

    <span data-ttu-id="7cd00-150">Border Gateway Protocol (BGP) interagisce con i gateway VPN di Azure o ExpressRoute per propagare route BGP locali nelle reti virtuali di Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-150">Border Gateway Protocol (BGP) works with Azure VPN gateways or ExpressRoute to propagate on-premises BGP routes to Azure virtual networks.</span></span>

### <a name="filter-network-traffic"></a><span data-ttu-id="7cd00-151">Filtrare il traffico di rete</span><span class="sxs-lookup"><span data-stu-id="7cd00-151">Filter network traffic</span></span>

<span data-ttu-id="7cd00-152">Le reti virtuali di Azure consentono di filtrare il traffico tra le subnet mediante gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="7cd00-152">Azure virtual networks enable you to filter traffic between subnets by using the following approaches:</span></span>

- <span data-ttu-id="7cd00-153">**Gruppi di sicurezza di rete**</span><span class="sxs-lookup"><span data-stu-id="7cd00-153">**Network security groups**</span></span>

    <span data-ttu-id="7cd00-154">Un gruppo di sicurezza di rete è una risorsa di Azure in grado di contenere più regole di sicurezza in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="7cd00-154">A network security group is an Azure resource that can contain multiple inbound and outbound security rules.</span></span> <span data-ttu-id="7cd00-155">È possibile definire queste regole per consentire o bloccare il traffico in base a fattori quali l'indirizzo IP di origine e di destinazione, la porta e il protocollo.</span><span class="sxs-lookup"><span data-stu-id="7cd00-155">You can define these rules to allow or block traffic, based on factors such as source and destination IP address, port, and protocol.</span></span>

- <span data-ttu-id="7cd00-156">**Appliance virtuali di rete**</span><span class="sxs-lookup"><span data-stu-id="7cd00-156">**Network virtual appliances**</span></span>
    
    <span data-ttu-id="7cd00-157">Un'appliance virtuale di rete è una macchina virtuale specializzata, analoga a un'appliance virtuale con protezione avanzata.</span><span class="sxs-lookup"><span data-stu-id="7cd00-157">A network virtual appliance is a specialized VM that can be compared to a hardened network appliance.</span></span> <span data-ttu-id="7cd00-158">L'appliance virtuale di rete svolge una funzione di rete specifica, ad esempio l'esecuzione di un firewall o di un'ottimizzazione WAN.</span><span class="sxs-lookup"><span data-stu-id="7cd00-158">A network virtual appliance carries out a particular network function, such as running a firewall or performing WAN optimization.</span></span>

## <a name="connect-virtual-networks"></a><span data-ttu-id="7cd00-159">Connessione di reti virtuali</span><span class="sxs-lookup"><span data-stu-id="7cd00-159">Connect virtual networks</span></span>

<span data-ttu-id="7cd00-160">È possibile collegare le reti virtuali mediante il _peering_ di rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-160">You can link virtual networks together using virtual network _peering_.</span></span> <span data-ttu-id="7cd00-161">Il peering consente alle risorse in ogni rete virtuale di comunicare tra loro.</span><span class="sxs-lookup"><span data-stu-id="7cd00-161">Peering enables resources in each virtual network to communicate with each other.</span></span> <span data-ttu-id="7cd00-162">Queste reti virtuali possono trovarsi in aree separate e consentono di creare una rete interconnessa globale tramite Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-162">These virtual networks can be in separate regions, allowing you to create a global interconnected network through Azure.</span></span>

## <a name="azure-virtual-network-settings"></a><span data-ttu-id="7cd00-163">Impostazione della rete virtuale di Azure</span><span class="sxs-lookup"><span data-stu-id="7cd00-163">Azure virtual network settings</span></span>

<span data-ttu-id="7cd00-164">È possibile creare e configurare reti virtuali di Azure dal portale di Azure, tramite Azure PowerShell nel computer locale o tramite Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="7cd00-164">You can create and configure Azure virtual networks from the Azure portal, Azure PowerShell on your local computer, or Azure Cloud Shell.</span></span>

### <a name="create-a-virtual-network"></a><span data-ttu-id="7cd00-165">Creare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="7cd00-165">Create a virtual network</span></span>

<span data-ttu-id="7cd00-166">Quando si crea una rete virtuale di Azure, è necessario configurare alcune impostazioni di base.</span><span class="sxs-lookup"><span data-stu-id="7cd00-166">When you create an Azure virtual network, you configure a number of basic settings.</span></span> <span data-ttu-id="7cd00-167">È anche possibile configurare impostazioni avanzate, ad esempio più subnet, protezione da attacchi Distributed Denial of Service (DDoS) ed endpoint di servizio.</span><span class="sxs-lookup"><span data-stu-id="7cd00-167">You also have the option to configure advanced settings, such as multiple subnets, distributed denial of service (DDoS) protection, and service endpoints.</span></span>

![Screenshot del portale di Azure con un esempio dei campi del pannello Crea rete virtuale.](../media/2-create-virtual-network.PNG)

<span data-ttu-id="7cd00-169">Ecco le impostazioni da configurare per una rete virtuale di base:</span><span class="sxs-lookup"><span data-stu-id="7cd00-169">Settings that you need to configure for a basic virtual network are:</span></span>

- <span data-ttu-id="7cd00-170">**Nome rete**</span><span class="sxs-lookup"><span data-stu-id="7cd00-170">**Network name**</span></span>

    <span data-ttu-id="7cd00-171">Questo nome deve essere univoco nella sottoscrizione, ma non è necessario che sia univoco a livello globale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-171">This name must be unique in your subscription but does not need to be globally unique.</span></span> <span data-ttu-id="7cd00-172">Specificare un nome descrittivo, facile da ricordare e distinguere da altre reti virtuali.</span><span class="sxs-lookup"><span data-stu-id="7cd00-172">Make the name a descriptive one that is easy to remember and distinguish from other virtual networks.</span></span>

- <span data-ttu-id="7cd00-173">**Spazio indirizzi**</span><span class="sxs-lookup"><span data-stu-id="7cd00-173">**Address space**</span></span>
    
    <span data-ttu-id="7cd00-174">Quando si configura una rete virtuale, si definisce lo spazio indirizzi interno in formato CIDR (Classless Interdomain Routing).</span><span class="sxs-lookup"><span data-stu-id="7cd00-174">When you set up a virtual network, you define the internal address space in Classless Interdomain Routing (CIDR) format.</span></span> <span data-ttu-id="7cd00-175">Questo spazio indirizzi deve essere univoco nella sottoscrizione, quindi non è possibile definire uno spazio indirizzi pari ad esempio a 10.0.0.0/24 per una rete virtuale e quindi a 10.1.0.0/8 per un'altra, perché la rete 10.1.0.0/8 si sovrapporrà a 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="7cd00-175">This address space needs to be unique within your subscription, so you cannot define an address space of, say, 10.0.0.0/24 for one virtual network and then 10.1.0.0/8 for another, as the 10.1.0.0/8 network will overlap 10.0.0.0/24.</span></span> <span data-ttu-id="7cd00-176">È tuttavia possibile specificare 10.0.0.0/16 e 10.1.0.0/16 e così via.</span><span class="sxs-lookup"><span data-stu-id="7cd00-176">However, you can have 10.0.0.0/16 and 10.1.0.0/16, etc.</span></span> 
    > [!NOTE] 
    > <span data-ttu-id="7cd00-177">È possibile aggiungere spazi indirizzi dopo avere creato la rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-177">You can add address spaces after creating the virtual network.</span></span>

- <span data-ttu-id="7cd00-178">**Sottoscrizione**</span><span class="sxs-lookup"><span data-stu-id="7cd00-178">**Subscription**</span></span>

    <span data-ttu-id="7cd00-179">Applicabile solo se sono presenti più sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="7cd00-179">Only applies if you have multiple subscriptions to choose from.</span></span>

- <span data-ttu-id="7cd00-180">**Gruppo di risorse**</span><span class="sxs-lookup"><span data-stu-id="7cd00-180">**Resource group**</span></span>
    
    <span data-ttu-id="7cd00-181">Come qualsiasi altra risorsa di Azure, una rete virtuale deve trovarsi in un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="7cd00-181">Like any other Azure resource, a virtual network needs to exist in a resource group.</span></span> <span data-ttu-id="7cd00-182">È possibile selezionare un gruppo di risorse esistente o crearne uno nuovo.</span><span class="sxs-lookup"><span data-stu-id="7cd00-182">You can either select an existing RG or create a new one.</span></span>
    
- <span data-ttu-id="7cd00-183">**Posizione**</span><span class="sxs-lookup"><span data-stu-id="7cd00-183">**Location**</span></span>

    <span data-ttu-id="7cd00-184">Selezionare la posizione da usare per la rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-184">Select the location where you want the virtual network to exist.</span></span>

- <span data-ttu-id="7cd00-185">**Subnet**</span><span class="sxs-lookup"><span data-stu-id="7cd00-185">**Subnet**</span></span>
    
    <span data-ttu-id="7cd00-186">In ogni intervallo di indirizzi della rete virtuale è possibile creare una o più subnet che partizionano lo spazio indirizzi della rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-186">Within each virtual network address range, you can create one or more subnets that partition the virtual network's address space.</span></span> <span data-ttu-id="7cd00-187">Il routing tra le subnet dipenderà quindi dalle route di traffico predefinite, oppure è possibile definire route personalizzate.</span><span class="sxs-lookup"><span data-stu-id="7cd00-187">Routing between subnets will then depend on the default traffic routes, or you can define custom routes.</span></span> <span data-ttu-id="7cd00-188">È anche possibile definire una subnet che includa tutti gli intervalli di indirizzi della rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-188">Alternatively, you can define one subnet that encompasses all the virtual networks' address ranges.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7cd00-189">I nomi di subnet devono iniziare con una lettera o un numero, terminare con una lettera, un numero o un carattere di sottolineatura e possono contenere solo lettere, numeri, caratteri di sottolineatura, punti o trattini.</span><span class="sxs-lookup"><span data-stu-id="7cd00-189">Subnet names must begin with a letter or number, end with a letter, number or underscore, and may contain only letters, numbers, underscores, periods, or hyphens.</span></span>

- <span data-ttu-id="7cd00-190">**Protezione DDoS**</span><span class="sxs-lookup"><span data-stu-id="7cd00-190">**DDoS protection**</span></span>

    <span data-ttu-id="7cd00-191">È possibile selezionare la protezione DDoS Basic o Standard.</span><span class="sxs-lookup"><span data-stu-id="7cd00-191">You can select either Basic or Standard DDoS protection.</span></span> <span data-ttu-id="7cd00-192">La Protezione DDoS Standard è un servizio Premium.</span><span class="sxs-lookup"><span data-stu-id="7cd00-192">Standard DDoS Protection is a premium service.</span></span> <span data-ttu-id="7cd00-193">Per altre informazioni sulla protezione DDoS Standard.</span><span class="sxs-lookup"><span data-stu-id="7cd00-193">For more information on Standard DDoS protection.</span></span>

- <span data-ttu-id="7cd00-194">**Endpoint di servizio**</span><span class="sxs-lookup"><span data-stu-id="7cd00-194">**Service Endpoints**</span></span>
    
    <span data-ttu-id="7cd00-195">Questa opzione consente di abilitare gli endpoint di servizio e quindi di selezionare dall'elenco gli endpoint di servizio di Azure da abilitare.</span><span class="sxs-lookup"><span data-stu-id="7cd00-195">Here, you enable service endpoints, and then select from the list which Azure service endpoints you want to enable.</span></span> <span data-ttu-id="7cd00-196">Le opzioni disponibili includono Azure Cosmos DB, bus di servizio di Azure, Azure Key Vault e così via.</span><span class="sxs-lookup"><span data-stu-id="7cd00-196">Options include Azure Cosmos DB, Azure Service Bus, Azure Key Vault, and so on.</span></span>

<span data-ttu-id="7cd00-197">Dopo avere configurato queste impostazioni, fare clic sul pulsante **Crea**.</span><span class="sxs-lookup"><span data-stu-id="7cd00-197">When you have configured these settings, click the **Create** button.</span></span>

### <a name="define-additional-settings"></a><span data-ttu-id="7cd00-198">Definire impostazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7cd00-198">Define additional settings</span></span>

<span data-ttu-id="7cd00-199">Dopo aver creato una rete virtuale è possibile definire altre impostazioni.</span><span class="sxs-lookup"><span data-stu-id="7cd00-199">After creating a virtual network, you can then define further settings.</span></span> <span data-ttu-id="7cd00-200">Queste includono:</span><span class="sxs-lookup"><span data-stu-id="7cd00-200">These include:</span></span>

- <span data-ttu-id="7cd00-201">**Gruppo di sicurezza di rete**</span><span class="sxs-lookup"><span data-stu-id="7cd00-201">**Network security group**</span></span>
    
    <span data-ttu-id="7cd00-202">I gruppi di sicurezza di rete hanno regole di sicurezza che consentono di filtrare il tipo di traffico di rete consentito in ingresso e in uscita dalle subnet della rete virtuale e dalle interfacce di rete.</span><span class="sxs-lookup"><span data-stu-id="7cd00-202">Network security groups have security rules that enable you to filter the type of network traffic that can flow in and out of virtual network subnets and network interfaces.</span></span> <span data-ttu-id="7cd00-203">È possibile creare il gruppo di sicurezza di rete separatamente e quindi associarlo alla rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-203">You create the network security group separately, and then associate it with the virtual network.</span></span>

- <span data-ttu-id="7cd00-204">**Tabella di route**</span><span class="sxs-lookup"><span data-stu-id="7cd00-204">**Route table**</span></span>

    <span data-ttu-id="7cd00-205">Azure crea automaticamente una tabella di route per ogni subnet all'interno di una rete virtuale di Azure e aggiunge le route predefinite di sistema alla tabella.</span><span class="sxs-lookup"><span data-stu-id="7cd00-205">Azure automatically creates a route table for each subnet within an Azure virtual network and adds system default routes to the table.</span></span> <span data-ttu-id="7cd00-206">È tuttavia possibile aggiungere tabelle di route personalizzate per modificare il traffico tra reti virtuali.</span><span class="sxs-lookup"><span data-stu-id="7cd00-206">However, you can add custom route tables to modify traffic between virtual networks.</span></span>

<span data-ttu-id="7cd00-207">È anche possibile modificare gli endpoint di servizio.</span><span class="sxs-lookup"><span data-stu-id="7cd00-207">You can also amend the service endpoints.</span></span>

![Screenshot del portale di Azure con un esempio del pannello per la modifica delle impostazioni della rete virtuale.](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a><span data-ttu-id="7cd00-209">Configurare reti virtuali</span><span class="sxs-lookup"><span data-stu-id="7cd00-209">Configure virtual networks</span></span>

<span data-ttu-id="7cd00-210">Dopo avere creato una rete virtuale è possibile modificare altre impostazioni dal pannello Reti virtuali nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-210">When you have created a virtual network, you can change any further settings from the Virtual Networks blade in the Azure portal.</span></span> <span data-ttu-id="7cd00-211">In alternativa è possibile usare i comandi di PowerShell o i comandi dell'interfaccia della riga di comando in Cloud Shell per apportare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7cd00-211">Alternatively, you can use PowerShell commands or commands in Cloud Shell to make changes.</span></span>

![Screenshot del portale di Azure con un pannello di esempio per la configurazione della rete virtuale.](../media/2-configure-virtual-network.PNG)

<span data-ttu-id="7cd00-213">È quindi possibile verificare e modificare le impostazioni in altri pannelli secondari.</span><span class="sxs-lookup"><span data-stu-id="7cd00-213">You can then review and change settings in further sub-blades.</span></span>
<span data-ttu-id="7cd00-214">Tali impostazioni includono:</span><span class="sxs-lookup"><span data-stu-id="7cd00-214">These settings include:</span></span>

- <span data-ttu-id="7cd00-215">Spazi indirizzi</span><span class="sxs-lookup"><span data-stu-id="7cd00-215">Address spaces</span></span>

    <span data-ttu-id="7cd00-216">È possibile aggiungere altri spazi indirizzi alla definizione iniziale</span><span class="sxs-lookup"><span data-stu-id="7cd00-216">You can add further address spaces to the initial definition</span></span>

- <span data-ttu-id="7cd00-217">Dispositivi connessi</span><span class="sxs-lookup"><span data-stu-id="7cd00-217">Connected devices</span></span>

    <span data-ttu-id="7cd00-218">È possibile usare la rete virtuale per connettere i computer</span><span class="sxs-lookup"><span data-stu-id="7cd00-218">Use the virtual network to connect machines</span></span>

- <span data-ttu-id="7cd00-219">Subnet</span><span class="sxs-lookup"><span data-stu-id="7cd00-219">Subnets</span></span>

    <span data-ttu-id="7cd00-220">È possibile aggiungere altre subnet</span><span class="sxs-lookup"><span data-stu-id="7cd00-220">Add further subnets</span></span>

- <span data-ttu-id="7cd00-221">Peering</span><span class="sxs-lookup"><span data-stu-id="7cd00-221">Peerings</span></span>

    <span data-ttu-id="7cd00-222">È possibile collegare le reti virtuali in modalità di peering</span><span class="sxs-lookup"><span data-stu-id="7cd00-222">Link virtual networks in peering arrangements</span></span>

<span data-ttu-id="7cd00-223">È anche possibile monitorare e risolvere i problemi delle reti virtuali o creare uno script di automazione per generare la rete virtuale corrente.</span><span class="sxs-lookup"><span data-stu-id="7cd00-223">You can also monitor and troubleshoot virtual networks, or create an automation script to generate the current virtual network.</span></span>

## <a name="summary"></a><span data-ttu-id="7cd00-224">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7cd00-224">Summary</span></span>

<span data-ttu-id="7cd00-225">Le reti virtuali sono meccanismi avanzati e a configurabilità elevata per la connessione di entità in Azure.</span><span class="sxs-lookup"><span data-stu-id="7cd00-225">Virtual networks are powerful and highly configurable mechanisms for connecting entities in Azure.</span></span> <span data-ttu-id="7cd00-226">È possibile connettere le risorse di Azure tra loro o a risorse disponibili in locale.</span><span class="sxs-lookup"><span data-stu-id="7cd00-226">You can connect Azure resources to one another or to resources you have on-premises.</span></span> <span data-ttu-id="7cd00-227">È possibile isolare, filtrare e indirizzare il traffico di rete. Azure consente anche di aumentare la sicurezza in caso di necessità.</span><span class="sxs-lookup"><span data-stu-id="7cd00-227">You can isolate, filter and route your network traffic, and Azure allows you to increase security where you feel you need it.</span></span>
