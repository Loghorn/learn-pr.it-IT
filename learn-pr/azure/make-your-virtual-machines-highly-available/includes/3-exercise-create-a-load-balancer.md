<span data-ttu-id="535f0-101">Si supponga di lavorare per Woodgrove Bank e che si stia per lanciare i servizi di banking online.</span><span class="sxs-lookup"><span data-stu-id="535f0-101">Recall that you work for Woodgrove Bank, and that you are about to launch online banking services.</span></span> <span data-ttu-id="535f0-102">Trattandosi di un settore estremamente competitivo, è necessario garantire una disponibilità minima dei servizi del 99,99%.</span><span class="sxs-lookup"><span data-stu-id="535f0-102">This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability.</span></span> <span data-ttu-id="535f0-103">È stato determinato che un'istanza di Azure Load Balancer con un pool di tre macchine virtuali consentirà di raggiungere questo obiettivo.</span><span class="sxs-lookup"><span data-stu-id="535f0-103">You have determined that Azure Load Balancer with a pool of three virtual machines will meet this goal.</span></span>

<span data-ttu-id="535f0-104">In questo esercizio verranno creati un servizio di bilanciamento del carico e una rete virtuale usando il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="535f0-104">In this exercise, you will create a load balancer and a virtual network using the Azure portal.</span></span> <span data-ttu-id="535f0-105">Poiché sarà necessario uno solo di questi elementi, il portale rappresenta uno strumento semplice per procedere alla creazione.</span><span class="sxs-lookup"><span data-stu-id="535f0-105">Since we only need one of these, the portal is an easy way to create it.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-public-load-balancer"></a><span data-ttu-id="535f0-106">Creare un servizio di bilanciamento del carico pubblico</span><span class="sxs-lookup"><span data-stu-id="535f0-106">Create a public load balancer</span></span>

1. <span data-ttu-id="535f0-107">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="535f0-107">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="535f0-108">Nella barra laterale fare clic su **Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="535f0-108">In the sidebar, click **Create a resource**.</span></span>

1. <span data-ttu-id="535f0-109">Selezionare la sezione **Rete** e quindi fare clic su **Servizio di bilanciamento del carico**.</span><span class="sxs-lookup"><span data-stu-id="535f0-109">Select the **Networking** section, and then click **Load Balancer**.</span></span> <span data-ttu-id="535f0-110">Se l'opzione non è visualizzata, è possibile usare la casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="535f0-110">If you don't see that choice, you can use the search box.</span></span>

    ![Screenshot che mostra Azure Marketplace con la sezione Rete selezionata e l'opzione Servizio di bilanciamento del carico evidenziata](../media/3-azure-marketplace.png)

1. <span data-ttu-id="535f0-112">Nel pannello **Crea servizio di bilanciamento del carico** immettere o selezionare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="535f0-112">In the **Create load balancer** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="535f0-113">**Nome:** _woodgrove-LB_</span><span class="sxs-lookup"><span data-stu-id="535f0-113">**Name:** _woodgrove-LB_</span></span>
    - <span data-ttu-id="535f0-114">**Tipo:** _Pubblico_</span><span class="sxs-lookup"><span data-stu-id="535f0-114">**Type:** _Public_</span></span>
    - <span data-ttu-id="535f0-115">**SKU:** _Basic_</span><span class="sxs-lookup"><span data-stu-id="535f0-115">**SKU:** _Basic_</span></span>
    - <span data-ttu-id="535f0-116">**Indirizzo IP pubblico:** selezionare **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="535f0-116">**Public IP address:** Select **Create new**.</span></span> <span data-ttu-id="535f0-117">Nella casella di testo digitare _woodgrove-LB-ip_.</span><span class="sxs-lookup"><span data-stu-id="535f0-117">In the text box, type _woodgrove-LB-ip_.</span></span> <span data-ttu-id="535f0-118">Lasciare l'assegnazione impostata su _Dinamica_.</span><span class="sxs-lookup"><span data-stu-id="535f0-118">Leave the Assignment as _Dynamic_.</span></span>
    - <span data-ttu-id="535f0-119">**Sottoscrizione:** dovrebbe essere già impostata su _Sottoscrizione Concierge_.</span><span class="sxs-lookup"><span data-stu-id="535f0-119">**Subscription:** It should already have _Concierge Subscription_ selected.</span></span>
    - <span data-ttu-id="535f0-120">**Gruppo di risorse:** selezionare **Usa esistente** e scegliere _<rgn>[nome gruppo risorse sandbox]</rgn>_.</span><span class="sxs-lookup"><span data-stu-id="535f0-120">**Resource group:** Select **Use existing** and choose _<rgn>[sandbox resource group name]</rgn>_.</span></span>
    - <span data-ttu-id="535f0-121">**Località:** selezionare un'area vicina dall'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="535f0-121">**Location:** Select a region near you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    ![Screenshot che mostra la schermata di creazione di un servizio di bilanciamento del carico](../media/3-create-load-balancer.png)

1. <span data-ttu-id="535f0-123">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="535f0-123">Click **Create**.</span></span>

<span data-ttu-id="535f0-124">Durante la creazione e la distribuzione della risorsa del servizio di bilanciamento del carico, creare la rete virtuale di Microsoft Azure che verrà usata per la subnet back-end.</span><span class="sxs-lookup"><span data-stu-id="535f0-124">While the load balancer resource is being created and deployed, let's create the Azure Virtual Network we'll use for the backend subnet.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="535f0-125">Creare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="535f0-125">Create a virtual network</span></span>

1. <span data-ttu-id="535f0-126">Nel menu di sinistra fare clic su **Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="535f0-126">In the left menu, click **Create a resource**.</span></span> <span data-ttu-id="535f0-127">Nel pannello **Nuovo** fare clic su **Rete** e quindi su **Rete virtuale**.</span><span class="sxs-lookup"><span data-stu-id="535f0-127">In the **New** blade, click **Networking**, and then click **Virtual network**.</span></span>

    ![Screenshot che mostra Azure Marketplace con la sezione Rete selezionata e l'opzione Servizio di bilanciamento del carico evidenziata](../media/3-azure-marketplace-2.png)

1. <span data-ttu-id="535f0-129">Nel pannello **Crea rete virtuale** immettere o selezionare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="535f0-129">In the **Create virtual network** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="535f0-130">**Nome:** _woodgrove-VNET_</span><span class="sxs-lookup"><span data-stu-id="535f0-130">**Name:** _woodgrove-VNET_</span></span>
    - <span data-ttu-id="535f0-131">**Spazio indirizzi:** _172.20.0.0/16_</span><span class="sxs-lookup"><span data-stu-id="535f0-131">**Address space:** _172.20.0.0/16_</span></span>
    - <span data-ttu-id="535f0-132">**Sottoscrizione:** dovrebbe essere già impostata su _Sottoscrizione Concierge_.</span><span class="sxs-lookup"><span data-stu-id="535f0-132">**Subscription:** It should already have _Concierge Subscription_ selected.</span></span>
    - <span data-ttu-id="535f0-133">**Gruppo di risorse:** selezionare il gruppo di risorse _<rgn>[Nome gruppo risorse]</rgn>_ esistente dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="535f0-133">**Resource group:** Select the existing _<rgn>[Resource Group Name]</rgn>_ resource group from the list.</span></span>
    - <span data-ttu-id="535f0-134">**Nome subnet:** _backendSubnet_</span><span class="sxs-lookup"><span data-stu-id="535f0-134">**Subnet name:** _backendSubnet_</span></span>
    - <span data-ttu-id="535f0-135">**Intervallo di indirizzi della subnet:** _172.20.0.0/24_</span><span class="sxs-lookup"><span data-stu-id="535f0-135">**Subnet address range:** _172.20.0.0/24_</span></span>
    - <span data-ttu-id="535f0-136">**Protezione DDoS:** _Basic_</span><span class="sxs-lookup"><span data-stu-id="535f0-136">**DDoS protection:** _Basic_</span></span>
    - <span data-ttu-id="535f0-137">**Endpoint servizio:** _Disabilitato_</span><span class="sxs-lookup"><span data-stu-id="535f0-137">**Service endpoints:** _Disabled_</span></span>

    ![Screenshot che mostra la schermata di creazione di un servizio di bilanciamento del carico](../media/3-create-vnet.png)

1. <span data-ttu-id="535f0-139">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="535f0-139">Click **Create**.</span></span>

<span data-ttu-id="535f0-140">Durante la distribuzione della rete virtuale, creare un altro elemento, ovvero un gruppo di sicurezza di rete.</span><span class="sxs-lookup"><span data-stu-id="535f0-140">While the virtual network is being deployed, let's create one more thing: a network security group.</span></span>

## <a name="create-and-configure-a-network-security-group"></a><span data-ttu-id="535f0-141">Creare e configurare un gruppo di sicurezza di rete</span><span class="sxs-lookup"><span data-stu-id="535f0-141">Create and configure a network security group</span></span>

1. <span data-ttu-id="535f0-142">Fare clic su **Crea una risorsa**</span><span class="sxs-lookup"><span data-stu-id="535f0-142">Click **Create a resource**</span></span>

1. <span data-ttu-id="535f0-143">Selezionare il gruppo **Rete** e fare clic su **Gruppo di sicurezza di rete**.</span><span class="sxs-lookup"><span data-stu-id="535f0-143">Select the **Networking** group, and Click on the **Network security group** item.</span></span>

    ![Screenshot che mostra Azure Marketplace con la sezione Rete selezionata e l'opzione Gruppo di sicurezza di rete evidenziata](../media/3-azure-marketplace-3.png)


1. <span data-ttu-id="535f0-145">Assegnare il nome **woodgrove-NSG** al gruppo e inserirlo nel gruppo di risorse dell'ambiente sandbox di Azure.</span><span class="sxs-lookup"><span data-stu-id="535f0-145">Give it the name **woodgrove-NSG** and put it into the Azure sandbox resource group.</span></span>

1. <span data-ttu-id="535f0-146">Assicurarsi che si trovi nello stesso percorso di Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="535f0-146">Make sure it's in the same location as the Azure Load Balancer.</span></span>

    ![Screenshot che mostra la schermata di creazione del gruppo di sicurezza di rete](../media/3-create-nsg.png)

1. <span data-ttu-id="535f0-148">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="535f0-148">Click **Create**.</span></span>

<span data-ttu-id="535f0-149">Attendere che il servizio di bilanciamento del carico, la rete virtuale e il gruppo di sicurezza di rete vengono distribuiti.</span><span class="sxs-lookup"><span data-stu-id="535f0-149">Wait until the load balancer, virtual network, and NSG are deployed.</span></span> <span data-ttu-id="535f0-150">È quindi possibile configurare la sicurezza di rete.</span><span class="sxs-lookup"><span data-stu-id="535f0-150">Then we can configure the network security.</span></span>

## <a name="configure-the-network-security-group"></a><span data-ttu-id="535f0-151">Configurare il gruppo di sicurezza di rete</span><span class="sxs-lookup"><span data-stu-id="535f0-151">Configure the network security group</span></span>

1. <span data-ttu-id="535f0-152">Selezionare il nuovo gruppo di sicurezza di rete dalla notifica di distribuzione o tramite la barra di ricerca nella parte superiore del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="535f0-152">Select your new network security group - either from the deployment notification, or through the search bar at the top of the Azure portal.</span></span>

    ![Screnshot che mostra la distribuzione completata nel pannello Notifica](../media/3-deployment-success.png)

1. <span data-ttu-id="535f0-154">Selezionare la sezione **Impostazioni** > **Regole di sicurezza in ingresso**.</span><span class="sxs-lookup"><span data-stu-id="535f0-154">Select the **Settings** > **Inbound security rules** section.</span></span> <span data-ttu-id="535f0-155">Si noti che sono presenti tre regole predefinite che vengono sempre applicate.</span><span class="sxs-lookup"><span data-stu-id="535f0-155">Notice that we have three pre-defined rules that are always applied.</span></span> <span data-ttu-id="535f0-156">Queste regole sono:</span><span class="sxs-lookup"><span data-stu-id="535f0-156">These rules are:</span></span>
    - <span data-ttu-id="535f0-157">**AllowVnetInbound**: consentire tutto il flusso di traffico interno nella rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="535f0-157">**AllowVnetInbound** - Allow all internal traffic flowing on the virtual network.</span></span> <span data-ttu-id="535f0-158">Questa regola consente alle macchine virtuali che condividono la rete di comunicare l'una con l'altra.</span><span class="sxs-lookup"><span data-stu-id="535f0-158">This rule allows VMs that share the network to talk to each other.</span></span>
    - <span data-ttu-id="535f0-159">**AllowAzureLoadBalancerInBound**: consentire al servizio di bilanciamento del carico di effettuare il ping dei servizi della rete per verificare se sono attivi.</span><span class="sxs-lookup"><span data-stu-id="535f0-159">**AllowAzureLoadBalancerInBound** - Allow the load balancer to "ping" services on the network to see whether they are alive.</span></span>
    - <span data-ttu-id="535f0-160">**DenyAllInbound**: blocca tutto il traffico rimanente.</span><span class="sxs-lookup"><span data-stu-id="535f0-160">**DenyAllInbound** - Deny all other traffic.</span></span>

    <span data-ttu-id="535f0-161">La regola **DenyAllInbound** è particolarmente importante poiché garantisce che tutto il traffico in ingresso senza una regola di priorità più elevata venga bloccato.</span><span class="sxs-lookup"><span data-stu-id="535f0-161">The **DenyAllInbound** rule is particularly important - it ensures that all inbound traffic that doesn't have a higher priority rule is blocked.</span></span> <span data-ttu-id="535f0-162">Per questa ragione sarà necessario aggiungere una nuova regola per consentire il traffico HTTP (80) per i server Web.</span><span class="sxs-lookup"><span data-stu-id="535f0-162">That's why we will need to add a new rule to allow HTTP (80) traffic for our web servers.</span></span>

    > [!NOTE]
    > <span data-ttu-id="535f0-163">La priorità delle regole dei gruppi di sicurezza di rete può avere un valore compreso tra 0 e 65500 e le regole vengono valutate nell'ordine.</span><span class="sxs-lookup"><span data-stu-id="535f0-163">Priority in NSG rules goes from 0 - 65500 and rules are evaluated in order.</span></span> <span data-ttu-id="535f0-164">La decisione sarà basata sulla prima regola corrispondente.</span><span class="sxs-lookup"><span data-stu-id="535f0-164">The first matching rule becomes the decision maker.</span></span> <span data-ttu-id="535f0-165">È sempre consigliabile creare regole con priorità piuttosto bassa, a partire da valori pari a 1000 in modo che abbiano la precedenza sulle regole predefinite.</span><span class="sxs-lookup"><span data-stu-id="535f0-165">You will always want to place your rules fairly low - starting around 1000 so they take precedence over the pre-defined ones.</span></span>

1. <span data-ttu-id="535f0-166">Fare clic su **Aggiungi** per aggiungere una nuova regola.</span><span class="sxs-lookup"><span data-stu-id="535f0-166">Click **Add** to add a new rule.</span></span>

    ![Screenshot che mostra le regole di sicurezza di rete in ingresso con il pulsante Aggiungi evidenziato](../media/3-inbound-security-rules.png)

1. <span data-ttu-id="535f0-168">Fare clic sul pulsante **Base** nella parte superiore per passare alla vista di base.</span><span class="sxs-lookup"><span data-stu-id="535f0-168">Click the **Basic** button at the top to switch the the "basic" view.</span></span>

1. <span data-ttu-id="535f0-169">Immettere i dettagli per la nuova regola.</span><span class="sxs-lookup"><span data-stu-id="535f0-169">Fill in the details for the new rule.</span></span>
    - <span data-ttu-id="535f0-170">Selezionare _HTTP_ per **Servizio**.</span><span class="sxs-lookup"><span data-stu-id="535f0-170">Select _HTTP_ for the **Service**.</span></span>
    - <span data-ttu-id="535f0-171">Impostare **Priorità** su _1000_.</span><span class="sxs-lookup"><span data-stu-id="535f0-171">Set the **Priority** to _1000_.</span></span>
    - <span data-ttu-id="535f0-172">Assegnare un nome alla regola o lasciare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="535f0-172">Give the rule a name (or leave the default).</span></span>

    ![Screenshot che mostra la finestra di dialogo Aggiungi regola di sicurezza in ingresso con i dettagli HTTP specificati](../media/3-add-inbound-rule.png)

1. <span data-ttu-id="535f0-174">Fare clic su **Aggiungi** per aggiungere la regola.</span><span class="sxs-lookup"><span data-stu-id="535f0-174">Click **Add** to add the rule.</span></span>

    ![Screenshot che mostra la regola HTTP appena aggiunta nell'elenco delle regole dei gruppi di sicurezza di rete](../media/3-new-added-rule.png)

## <a name="apply-the-network-security-group-to-the-vnet"></a><span data-ttu-id="535f0-176">Applicare il gruppo di sicurezza di rete alla rete virtuale</span><span class="sxs-lookup"><span data-stu-id="535f0-176">Apply the network security group to the VNet</span></span>

<span data-ttu-id="535f0-177">Si procede ora ad applicare il gruppo di sicurezza di rete alla rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="535f0-177">Next, let's apply this NSG to the virtual network.</span></span>

1. <span data-ttu-id="535f0-178">Individuare la rete virtuale (è possibile usare la casella di ricerca nella parte superiore).</span><span class="sxs-lookup"><span data-stu-id="535f0-178">Find your virtual network - you can use the search box at the top.</span></span> <span data-ttu-id="535f0-179">La rete è denominata **woodgrove-VNET**.</span><span class="sxs-lookup"><span data-stu-id="535f0-179">It's named **woodgrove-VNET**.</span></span>

1. <span data-ttu-id="535f0-180">Selezionare **Impostazioni** > **Subnet** per passare alle subnet definite.</span><span class="sxs-lookup"><span data-stu-id="535f0-180">Select **Settings** > **Subnets** to get to your defined subnets.</span></span>

1. <span data-ttu-id="535f0-181">Fare clic sulla voce **backendSubnet** per visualizzare le proprietà.</span><span class="sxs-lookup"><span data-stu-id="535f0-181">Click the **backendSubnet** entry to open the properties.</span></span>

    ![Screenshot che mostra le voci backendSubnet nell'area Subnet delle subnet](../media/3-subnets.png)

1. <span data-ttu-id="535f0-183">Fare clic sulla voce "Nessuno" in **Gruppo di sicurezza di rete**</span><span class="sxs-lookup"><span data-stu-id="535f0-183">Click the "None" entry on the **Network security group**</span></span>

    ![Screenshot che mostra il gruppo di sicurezza di rete vuoto in backendSubnet](../media/3-add-network-security-group.png)

1. <span data-ttu-id="535f0-185">Selezionare **woodgrove-NSG** per aggiungerlo alla rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="535f0-185">Select the **woodgrove-NSG** to add it to this VNET.</span></span>

1. <span data-ttu-id="535f0-186">Fare clic su **Salva** per applicare la modifica.</span><span class="sxs-lookup"><span data-stu-id="535f0-186">Click **Save** to apply the change.</span></span>

<span data-ttu-id="535f0-187">Dopo aver configurato la rete, è possibile creare alcune macchine virtuali da aggiungere alla rete.</span><span class="sxs-lookup"><span data-stu-id="535f0-187">Now that our network is ready, let's create some virtual machines to sit on this network!</span></span>