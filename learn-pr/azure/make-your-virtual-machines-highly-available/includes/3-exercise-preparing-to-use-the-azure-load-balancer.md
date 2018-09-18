<span data-ttu-id="f75a7-101">In questo esercizio si creeranno un servizio di bilanciamento del carico, una rete virtuale e più macchine virtuali usando il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f75a7-101">In this exercise, you will create a load balancer, a virtual network, and multiple virtual machines using the Azure portal.</span></span>

<span data-ttu-id="f75a7-102">Si supponga di lavorare per Woodgrove Bank, una startup che sta per lanciare servizi di banking online.</span><span class="sxs-lookup"><span data-stu-id="f75a7-102">Suppose you work for Woodgrove Bank, a startup that is about to launch online banking services.</span></span> <span data-ttu-id="f75a7-103">Trattandosi di un settore estremamente competitivo, è necessario garantire una disponibilità minima dei servizi del 99,99%.</span><span class="sxs-lookup"><span data-stu-id="f75a7-103">This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability.</span></span> <span data-ttu-id="f75a7-104">È stato determinato che un'istanza di Azure Load Balancer con un pool di tre macchine virtuali consentirà di raggiungere questo obiettivo.</span><span class="sxs-lookup"><span data-stu-id="f75a7-104">You have determined that an Azure Load Balancer with a pool of three virtual machines will meet this goal.</span></span>

## <a name="create-a-public-load-balancer"></a><span data-ttu-id="f75a7-105">Creare un servizio di bilanciamento del carico pubblico</span><span class="sxs-lookup"><span data-stu-id="f75a7-105">Create a public load balancer</span></span>

1. <span data-ttu-id="f75a7-106">In un browser passare al [portale di Azure](https://portal.azure.com/?azure-portal=true) e accedere al proprio account.</span><span class="sxs-lookup"><span data-stu-id="f75a7-106">In a browser, navigate to the [Azure portal](https://portal.azure.com/?azure-portal=true) and sign in to your account.</span></span>

1. <span data-ttu-id="f75a7-107">Nella barra laterale fare clic su **Crea una risorsa** e quindi nel pannello **Nuovo** fare clic su **Rete** e infine su **Servizio di bilanciamento del carico**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-107">In the sidebar, click **Create a resource**, then in the **New** blade, click **Networking**, and then click **Load Balancer**.</span></span>

1. <span data-ttu-id="f75a7-108">Nel pannello Crea servizio di bilanciamento del carico immettere o selezionare le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="f75a7-108">In the Create load balancer blade, enter or select the following information:</span></span>
    - <span data-ttu-id="f75a7-109">Nome: **woodgrove-LB**</span><span class="sxs-lookup"><span data-stu-id="f75a7-109">Name: **woodgrove-LB**</span></span>
    - <span data-ttu-id="f75a7-110">Tipo: **Pubblico**</span><span class="sxs-lookup"><span data-stu-id="f75a7-110">Type: **Public**</span></span>
    - <span data-ttu-id="f75a7-111">SKU: **Basic**</span><span class="sxs-lookup"><span data-stu-id="f75a7-111">SKU: **Basic**</span></span>
    - <span data-ttu-id="f75a7-112">Indirizzo IP pubblico: selezionare **Crea nuovo**, digitare **woodgrove-LB-ip** nella casella di testo e lasciare l'assegnazione di tipo **Dinamico**</span><span class="sxs-lookup"><span data-stu-id="f75a7-112">Public IP address: Select **Create new** and in the text box, type **woodgrove-LB-ip** in the text box, and leave the Assignment as **Dynamic**</span></span>
    - <span data-ttu-id="f75a7-113">Gruppo di risorse: selezionare **Crea nuovo** e digitare **woodgrove-RG** nella casella</span><span class="sxs-lookup"><span data-stu-id="f75a7-113">Resource group: Select **Create new**, and in the box, type **woodgrove-RG**</span></span>
    - <span data-ttu-id="f75a7-114">Località: selezionare un'area nelle vicinanze</span><span class="sxs-lookup"><span data-stu-id="f75a7-114">Location: Select a region near you</span></span>

1. <span data-ttu-id="f75a7-115">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-115">Click **Create**.</span></span>

1. <span data-ttu-id="f75a7-116">Prima di continuare con l'esercizio attendere il completamento della distribuzione del servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="f75a7-116">Wait until the load balancer has deployed before continuing with the exercise.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="f75a7-117">Creare una rete virtuale</span><span class="sxs-lookup"><span data-stu-id="f75a7-117">Create a Virtual Network</span></span>

1. <span data-ttu-id="f75a7-118">Nel menu a sinistra fare clic su **Crea una risorsa** e quindi nel pannello **Nuovo** fare clic su **Rete** e infine su **Rete virtuale**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-118">In the left menu, click **Create a resource**, then in the **New** blade, click **Networking**, and then click **Virtual network**.</span></span>

1. <span data-ttu-id="f75a7-119">Nel pannello **Crea rete virtuale** immettere o selezionare le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="f75a7-119">In the **Create virtual network** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="f75a7-120">Nome: **woodgrove-VNET**</span><span class="sxs-lookup"><span data-stu-id="f75a7-120">Name: **woodgrove-VNET**</span></span>
    - <span data-ttu-id="f75a7-121">Spazio indirizzi: **172.20.0.0/16**</span><span class="sxs-lookup"><span data-stu-id="f75a7-121">Address space: **172.20.0.0/16**</span></span>
    - <span data-ttu-id="f75a7-122">Gruppo di risorse: selezionare **Usa esistente** e quindi **woodgrove-RG**</span><span class="sxs-lookup"><span data-stu-id="f75a7-122">Resource group: Select **Use existing**, and select **woodgrove-RG**.</span></span>
    - <span data-ttu-id="f75a7-123">Subnet: **backendSubnet**</span><span class="sxs-lookup"><span data-stu-id="f75a7-123">Subnet: **backendSubnet**</span></span>
    - <span data-ttu-id="f75a7-124">Spazio indirizzi: **172.20.0.0/24**</span><span class="sxs-lookup"><span data-stu-id="f75a7-124">Address space: **172.20.0.0/24**</span></span>
    - <span data-ttu-id="f75a7-125">Protezione DDoS: **Basic**</span><span class="sxs-lookup"><span data-stu-id="f75a7-125">DDoS protection: **Basic**</span></span>
    - <span data-ttu-id="f75a7-126">Endpoint servizio: **Disabilitato**</span><span class="sxs-lookup"><span data-stu-id="f75a7-126">Service endpoints: **Disabled**</span></span>

1. <span data-ttu-id="f75a7-127">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-127">Click **Create**.</span></span>

1. <span data-ttu-id="f75a7-128">Prima di continuare con l'esercizio attendere il completamento della distribuzione della rete virtuale.</span><span class="sxs-lookup"><span data-stu-id="f75a7-128">Wait until the virtual network has deployed before continuing with the exercise.</span></span>

## <a name="create-a-vm-template"></a><span data-ttu-id="f75a7-129">Creare un modello di VM</span><span class="sxs-lookup"><span data-stu-id="f75a7-129">Create a VM template</span></span>

<span data-ttu-id="f75a7-130">Per iniziare, definire le informazioni di base della VM:</span><span class="sxs-lookup"><span data-stu-id="f75a7-130">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="f75a7-131">Nel portale di Azure fare clic su **Macchine virtuali** nel menu a sinistra e quindi su **Crea macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-131">In the Azure portal, in the left menu, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="f75a7-132">Nel pannello Calcolo fare clic su **Windows Server** nella sezione **Consigliati**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-132">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="f75a7-133">Nel pannello **Windows Server** fare clic su **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-133">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="f75a7-134">Nel pannello **Windows Server 2016 Datacenter** fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-134">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="f75a7-135">Nel pannello **Informazioni di base** digitare **woodgrove-SVR01** nella casella **Nome**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-135">In the **Basics** blade, in the **Name** box, type **woodgrove-SVR01**.</span></span>

1. <span data-ttu-id="f75a7-136">Nelle caselle **Nome utente** e **Password** digitare un nome e una password sicuri per un account amministratore nel server.</span><span class="sxs-lookup"><span data-stu-id="f75a7-136">In the **Username** and **Password boxes**, type a secure name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="f75a7-137">Nella casella **Sottoscrizione** selezionare la sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="f75a7-137">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="f75a7-138">In **Gruppo di risorse** selezionare **Usa esistente** e quindi **woodgrove-RG** nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f75a7-138">Under **Resource group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="f75a7-139">Nell'elenco a discesa **Località** selezionare un'area nelle vicinanze.</span><span class="sxs-lookup"><span data-stu-id="f75a7-139">In the **Location** drop-down list, select a region near you.SAME</span></span>

1. <span data-ttu-id="f75a7-140">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-140">Click **OK**.</span></span>

<span data-ttu-id="f75a7-141">Scegliere una dimensione per la VM e configurare le impostazioni:</span><span class="sxs-lookup"><span data-stu-id="f75a7-141">Choose a size for the VM and configure settings:</span></span>

1. <span data-ttu-id="f75a7-142">Nel pannello **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **D2s_v3**, e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-142">On the **Choose a size** blade, select a **Standard** SKU, such as **D2s_v3**, and then click **Select**.</span></span>

1. <span data-ttu-id="f75a7-143">Nel pannello **Impostazioni** fare clic su **Set di disponibilità**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-143">On the **Settings** blade, click **Availability set**.</span></span>

1. <span data-ttu-id="f75a7-144">Nel pannello **Modifica set di disponibilità** fare clic su **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-144">On the **Change availability set** blade, click **Create new**.</span></span>

1. <span data-ttu-id="f75a7-145">Nel pannello **Crea nuovo** digitare **woodgrove-AS** nella casella **Nome** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-145">On the **Create new** blade, in the **Name** box, type **woodgrove-AS**, and then click **OK**.</span></span>

1. <span data-ttu-id="f75a7-146">Nel pannello **Impostazioni** fare clic su **Avanzate** in **Gruppo di sicurezza di rete** e quindi su **(nuovo) woodgrove-SVR01-nsg**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-146">On the **Settings** blade, under **Network Security Group**, click **Advanced**, and then click **(new) woodgrove-SVR01-nsg**.</span></span>

1. <span data-ttu-id="f75a7-147">Nel pannello **Crea gruppo di sicurezza di rete** modificare il nome in **woodgrove-NSG** nella casella **Nome** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-147">On the **Create network Security group** blade, in the **Name** box, change the name to **woodgrove-NSG**, and then click **OK**.</span></span>

1. <span data-ttu-id="f75a7-148">Nel pannello **Impostazioni** fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-148">On the **Settings** blade, click **OK**.</span></span>

<span data-ttu-id="f75a7-149">Salvare le impostazioni in un modello, per poter distribuire facilmente più VM.</span><span class="sxs-lookup"><span data-stu-id="f75a7-149">Save the settings to a template, so that you easily deploy multiple VMs.</span></span>

1. <span data-ttu-id="f75a7-150">Nel pannello **Crea** fare clic su **Scarica modello e parametri**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-150">On the **Create** blade, click **Download template and parameters**.</span></span>

1. <span data-ttu-id="f75a7-151">Nel pannello **Modello** fare clic su **Aggiungi elementi al Catalogo multimediale**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-151">On the **Template** blade, click **Add to library**.</span></span>

1. <span data-ttu-id="f75a7-152">Nel pannello **Salva modello** digitare **woodgrove-server-template** nelle caselle **Nome** e **Descrizione** e quindi fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-152">On the **Save template** blade, in the **Name** and **Description** boxes, type **woodgrove-server-template**, and then click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="f75a7-153">Per trovare questo modello, fare clic su **Tutti i servizi** nel menu a sinistra, digitare **modelli** nella casella del filtro e quindi fare clic su **Modelli (ANTEPRIMA)**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-153">If you need to find this template, click **All services** in the left menu, type **template** in the filter box, and then click **Templates (PREVIEW)**.</span></span>

## <a name="use-the-template-to-provision-the-first-vm"></a><span data-ttu-id="f75a7-154">Usare il modello per effettuare il provisioning della prima VM</span><span class="sxs-lookup"><span data-stu-id="f75a7-154">Use the template to provision the first VM</span></span>

1. <span data-ttu-id="f75a7-155">Nel pannello **Modello** fare clic su **Distribuisci**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-155">On the **Template** blade, click **Deploy**.</span></span>

1. <span data-ttu-id="f75a7-156">Nel pannello **Distribuzione personalizzata** selezionare **Usa esistente** in **Gruppo di risorse** e quindi **woodgrove-RG** nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f75a7-156">On the **Custom deployment** blade, under **Resource Group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="f75a7-157">Nel pannello **Distribuzione personalizzata** digitare la stessa password usata in precedenza nella casella **Password amministratore**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-157">On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.</span></span>

1. <span data-ttu-id="f75a7-158">Nel pannello **Distribuzione personalizzata** selezionare la casella di controllo **Accetto le condizioni riportate sopra** e quindi fare clic su **Acquista**. Verranno addebitati i normali costi di calcolo di Azure, in base al piano tariffario della VM.</span><span class="sxs-lookup"><span data-stu-id="f75a7-158">On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).</span></span>

1. <span data-ttu-id="f75a7-159">Prima di continuare con l'esercizio attendere il completamento della distribuzione della VM, per assicurarsi che il modello sia configurato correttamente prima di usarlo per il provisioning di altre VM e che siano state create tutte le risorse associate.</span><span class="sxs-lookup"><span data-stu-id="f75a7-159">Wait until the VM has deployed before continuing with the exercise; this is so you can be sure the template is correctly configured before you use it to provision additional VMs, and all the associated resources have been created.</span></span>

## <a name="use-the-template-to-provision-two-additional-vms"></a><span data-ttu-id="f75a7-160">Usare il modello per effettuare il provisioning di altre due VM</span><span class="sxs-lookup"><span data-stu-id="f75a7-160">Use the template to provision two additional VMs</span></span>

1. <span data-ttu-id="f75a7-161">Nel portale di Azure fare clic su **Distribuisci** nel pannello **Modello**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-161">In the Azure portal, on the **Template** blade, click **Deploy**.</span></span>

1. <span data-ttu-id="f75a7-162">Nel pannello **Distribuzione personalizzata** selezionare **Usa esistente** in **Gruppo di risorse** e quindi **woodgrove-RG** nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f75a7-162">On the **Custom deployment** blade, under **Resource Group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="f75a7-163">Nel pannello **Distribuzione personalizzata** modificare il nome in **woodgrove-SVR02** nella casella **Nome macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-163">On the **Custom deployment** blade, in the **Virtual Machine Name** box, change the name to **woodgrove-SVR02**.</span></span>

1. <span data-ttu-id="f75a7-164">Nel pannello **Distribuzione personalizzata** modificare il nome in **woodgrovesvr02222** nella casella **Network Interface Name** (Nome interfaccia di rete).</span><span class="sxs-lookup"><span data-stu-id="f75a7-164">On the **Custom deployment** blade, in the **Network Interface Name** box, change the name to **woodgrovesvr02222**.</span></span>

1. <span data-ttu-id="f75a7-165">Nel pannello **Distribuzione personalizzata** digitare la stessa password usata in precedenza nella casella **Password amministratore**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-165">On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.</span></span>

1. <span data-ttu-id="f75a7-166">Nel pannello **Distribuzione personalizzata** modificare il nome in **woodgrove-SVR02-ip** nella casella **Nome indirizzo IP pubblico**.</span><span class="sxs-lookup"><span data-stu-id="f75a7-166">On the **Custom deployment** blade, in the **Public Ip Address Name** box, change the name to **woodgrove-SVR02-ip**.</span></span>

1. <span data-ttu-id="f75a7-167">Nel pannello **Distribuzione personalizzata** selezionare la casella di controllo **Accetto le condizioni riportate sopra** e quindi fare clic su **Acquista**. Verranno addebitati i normali costi di calcolo di Azure, in base al piano tariffario della VM.</span><span class="sxs-lookup"><span data-stu-id="f75a7-167">On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).</span></span>

1. <span data-ttu-id="f75a7-168">Ripetere i passaggi da 1 a 7 usando le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="f75a7-168">Repeat steps 1 - 7, using the following information:</span></span>
    - <span data-ttu-id="f75a7-169">Nome macchina virtuale: **woodgrove-SVR03**</span><span class="sxs-lookup"><span data-stu-id="f75a7-169">Virtual Machine Name: **woodgrove-SVR03**</span></span>
    - <span data-ttu-id="f75a7-170">Nome interfaccia di rete: **woodgrovesvr03333**</span><span class="sxs-lookup"><span data-stu-id="f75a7-170">Network Interface Name: **woodgrovesvr03333**</span></span>
    - <span data-ttu-id="f75a7-171">Nome indirizzo IP pubblico: **woodgrove-SVRr03-ip**</span><span class="sxs-lookup"><span data-stu-id="f75a7-171">Public Ip Address Name: **woodgrove-SVRr03-ip**</span></span>

1. <span data-ttu-id="f75a7-172">Prima di continuare con l'esercizio attendere il completamento della distribuzione delle VM.</span><span class="sxs-lookup"><span data-stu-id="f75a7-172">Wait until the VMs have deployed before continuing with the exercise.</span></span>

<span data-ttu-id="f75a7-173">Sono ora disponibili un servizio di bilanciamento del carico pubblico pronto per la configurazione e tre VM pronte per essere usate con tale servizio.</span><span class="sxs-lookup"><span data-stu-id="f75a7-173">You now have a public load balancer ready to configure, and three VMs ready to use with this load balancer.</span></span>