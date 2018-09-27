<span data-ttu-id="1827a-101">Le risorse chiave per l'architettura della banca online sono state create.</span><span class="sxs-lookup"><span data-stu-id="1827a-101">You have created the key resources for your online bank architecture.</span></span> <span data-ttu-id="1827a-102">In questa esercitazione viene completata l'installazione configurando le regole di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="1827a-102">In this exercise, you will complete the setup by configuring the load-balancing rules.</span></span>

<span data-ttu-id="1827a-103">Al termine dell'installazione, viene eseguito il test del bilanciamento del carico rimuovendo una macchina virtuale dal pool e verificando che le richieste client non vengano più inviate a questa macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="1827a-103">Once setup is complete, you will test the load balancer by removing a VM from the pool and verifying that client requests are no longer sent to this VM.</span></span>

<span data-ttu-id="1827a-104">L'esercitazione ha inizio con la definizione del pool back-end nel servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="1827a-104">We'll start by defining our backend pool in the load balancer.</span></span> <span data-ttu-id="1827a-105">Il pool specifica la posizione a cui vengono indirizzate le richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="1827a-105">This determines where inbound requests are routed.</span></span>

## <a name="create-a-backend-address-pool"></a><span data-ttu-id="1827a-106">Creare un pool di indirizzi back-end</span><span class="sxs-lookup"><span data-stu-id="1827a-106">Create a backend address pool</span></span>

1. <span data-ttu-id="1827a-107">Nel [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) nel menu a sinistra selezionare **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="1827a-107">In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), in the left menu, Select **All resources**.</span></span> <span data-ttu-id="1827a-108">Quindi, nell'elenco delle risorse selezionare il servizio di bilanciamento del carico (**woodgrove-LB**).</span><span class="sxs-lookup"><span data-stu-id="1827a-108">Then, in the resource list, select your load balancer (**woodgrove-LB**).</span></span>

1. <span data-ttu-id="1827a-109">Selezionare **Impostazioni** > **Pool back-end**.</span><span class="sxs-lookup"><span data-stu-id="1827a-109">Select **Settings** > **Backend pools**.</span></span> <span data-ttu-id="1827a-110">Si noti che non è ancora stato definito alcun elemento.</span><span class="sxs-lookup"><span data-stu-id="1827a-110">Notice we don't have any defined yet.</span></span>

1. <span data-ttu-id="1827a-111">Fare clic su **Aggiungi** per aggiungere un nuovo pool back-end.</span><span class="sxs-lookup"><span data-stu-id="1827a-111">Click **Add** to add a new backend pool.</span></span>

    ![Screenshot che illustra come aggiungere un pool back-end.](../media/6-backend-pools.png)

1. <span data-ttu-id="1827a-113">Nel pannello **Aggiungi pool back-end** aggiungere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1827a-113">On the **Add backend pool** blade, add the following information:</span></span>
    - <span data-ttu-id="1827a-114">Assegnare il nome _woodgrove-BEP_.</span><span class="sxs-lookup"><span data-stu-id="1827a-114">Name it _woodgrove-BEP_.</span></span>
    - <span data-ttu-id="1827a-115">Associare il pool a un **set di disponibilità**.</span><span class="sxs-lookup"><span data-stu-id="1827a-115">Associated the pool to an **Availability set**.</span></span>
    - <span data-ttu-id="1827a-116">Per il set di disponibilità, selezionare _woodgrove-AS_.</span><span class="sxs-lookup"><span data-stu-id="1827a-116">For the availability set, select _woodgrove-AS_.</span></span>

1. <span data-ttu-id="1827a-117">Fare clic su **Aggiungi una configurazione IP della rete di destinazione**.</span><span class="sxs-lookup"><span data-stu-id="1827a-117">Click **Add a target network IP configuration**.</span></span>

1. <span data-ttu-id="1827a-118">Nell'elenco **Macchina virtuale di destinazione** selezionare _woodgrove-VM1_.</span><span class="sxs-lookup"><span data-stu-id="1827a-118">In the **Target virtual machine** list, select _woodgrove-VM1_.</span></span>

1. <span data-ttu-id="1827a-119">Nell'elenco **Configurazione IP di rete** della VM selezionare il pool di indirizzi IP della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="1827a-119">In the **Network IP configuration** list for the VM, select the VM's IP address pool.</span></span>

1. <span data-ttu-id="1827a-120">Ripetere i passaggi per aggiungere _woodgrove-VM2_ e _woodgrove-VM3_ al pool back-end.</span><span class="sxs-lookup"><span data-stu-id="1827a-120">Repeat those steps to add _woodgrove-VM2_ and _woodgrove-VM3_ to the backend pool.</span></span>

    ![Screenshot che mostra il pannello Aggiungi pool back-end con tutte e tre le macchine virtuali selezionate.](../media/6-add-backend-pool.png)

1. <span data-ttu-id="1827a-122">Fare clic su **OK** per chiudere il pannello.</span><span class="sxs-lookup"><span data-stu-id="1827a-122">Click **OK** to close the blade.</span></span>

<span data-ttu-id="1827a-123">Attendere che la configurazione del bilanciamento del carico venga aggiornata prima di procedere con l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1827a-123">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="create-a-health-probe-for-the-load-balancer"></a><span data-ttu-id="1827a-124">Creare un probe di integrità per il bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="1827a-124">Create a health probe for the load balancer</span></span>

<span data-ttu-id="1827a-125">Aggiungere ora un probe di integrità per il protocollo HTTP sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="1827a-125">Next, add a health probe for HTTP over port 80.</span></span>

1. <span data-ttu-id="1827a-126">Nel pannello **woodgrove-LB** in **Impostazioni** fare clic su **Probe integrità**.</span><span class="sxs-lookup"><span data-stu-id="1827a-126">On the **woodgrove-LB** blade, under **Settings**, click **Health probes**.</span></span> <span data-ttu-id="1827a-127">Si noti che è attualmente vuoto.</span><span class="sxs-lookup"><span data-stu-id="1827a-127">Notice it's currently empty.</span></span>

1. <span data-ttu-id="1827a-128">Fare clic su **Aggiungi** per aggiungere un nuovo probe di integrità.</span><span class="sxs-lookup"><span data-stu-id="1827a-128">Click **Add** to add a new health probe.</span></span>

1. <span data-ttu-id="1827a-129">Nel pannello **Aggiungi probe integrità** impostare la configurazione seguente:</span><span class="sxs-lookup"><span data-stu-id="1827a-129">On the **Add health probe** blade, set the following configuration:</span></span>
    - <span data-ttu-id="1827a-130">**Nome:** _woodgrove-HP_</span><span class="sxs-lookup"><span data-stu-id="1827a-130">**Name:** _woodgrove-HP_</span></span>
    - <span data-ttu-id="1827a-131">**Protocollo:** _HTTP_</span><span class="sxs-lookup"><span data-stu-id="1827a-131">**Protocol:** _HTTP_</span></span>
    - <span data-ttu-id="1827a-132">**Porta:** _80_</span><span class="sxs-lookup"><span data-stu-id="1827a-132">**Port:** _80_</span></span>
    - <span data-ttu-id="1827a-133">**Percorso:** _/_</span><span class="sxs-lookup"><span data-stu-id="1827a-133">**Path:** _/_</span></span>
    - <span data-ttu-id="1827a-134">**Intervallo:** _15_</span><span class="sxs-lookup"><span data-stu-id="1827a-134">**Interval:** _15_</span></span>
    - <span data-ttu-id="1827a-135">**Soglia di non integrità:** _2_</span><span class="sxs-lookup"><span data-stu-id="1827a-135">**Unhealthy threshold:** _2_</span></span>

1. <span data-ttu-id="1827a-136">Fare clic su **OK** per salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="1827a-136">Click **OK** to save the changes.</span></span>

<span data-ttu-id="1827a-137">Attendere che la configurazione del bilanciamento del carico venga aggiornata prima di procedere con l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1827a-137">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="create-a-load-balancer-rule"></a><span data-ttu-id="1827a-138">Creare una regola di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="1827a-138">Create a load balancer rule</span></span>

<span data-ttu-id="1827a-139">Infine, creare una regola di bilanciamento del carico per HTTP sulla porta 80 che associa il pool back-end al probe di integrità.</span><span class="sxs-lookup"><span data-stu-id="1827a-139">Finally, create a load-balancing rule for the HTTP over port 80, that associates the backend pool with the health probe.</span></span>

1. <span data-ttu-id="1827a-140">Nel pannello **woodgrove-LB**, in **Impostazioni** fare clic su **Regole di bilanciamento del carico**.</span><span class="sxs-lookup"><span data-stu-id="1827a-140">On the **woodgrove-LB** blade, under **Settings**, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="1827a-141">Fare clic su **Aggiungi** per aggiungere una nuova regola.</span><span class="sxs-lookup"><span data-stu-id="1827a-141">Click **Add** to add a new rule.</span></span>

1. <span data-ttu-id="1827a-142">Nel pannello **Aggiungi regola di bilanciamento del carico** impostare **Nome** su _woodgrove-HTTP-LBRule_.</span><span class="sxs-lookup"><span data-stu-id="1827a-142">On the **Add load balancing rule** blade, set the the **Name** to _woodgrove-HTTP-LBRule_.</span></span>

1. <span data-ttu-id="1827a-143">Verificare che le informazioni seguenti vengano immesse automaticamente:</span><span class="sxs-lookup"><span data-stu-id="1827a-143">Verify that the following information has been automatically entered:</span></span>
    - <span data-ttu-id="1827a-144">Versione indirizzo IP: **IPv4**</span><span class="sxs-lookup"><span data-stu-id="1827a-144">IP Version: **IPv4**</span></span>
    - <span data-ttu-id="1827a-145">Indirizzo IP front-end: **LoadBalancerFrontEnd**</span><span class="sxs-lookup"><span data-stu-id="1827a-145">Frontend IP address: **LoadBalancerFrontEnd**</span></span>
    - <span data-ttu-id="1827a-146">Protocollo: **TCP**</span><span class="sxs-lookup"><span data-stu-id="1827a-146">Protocol: **TCP**</span></span>
    - <span data-ttu-id="1827a-147">Porta: **80**</span><span class="sxs-lookup"><span data-stu-id="1827a-147">Port: **80**</span></span>
    - <span data-ttu-id="1827a-148">Porta back-end: **80**</span><span class="sxs-lookup"><span data-stu-id="1827a-148">Backend port: **80**</span></span>
    - <span data-ttu-id="1827a-149">Pool back-end: **woodgrove-BEP**</span><span class="sxs-lookup"><span data-stu-id="1827a-149">Backend pool: **woodgrove-BEP**</span></span>
    - <span data-ttu-id="1827a-150">Probe integrità: **woodgrove-HP**</span><span class="sxs-lookup"><span data-stu-id="1827a-150">Health probe: **woodgrove-HP**</span></span>
    - <span data-ttu-id="1827a-151">Salvataggio permanente sessione: **Nessuno**</span><span class="sxs-lookup"><span data-stu-id="1827a-151">Session persistence: **None**</span></span>
    - <span data-ttu-id="1827a-152">Timeout di inattività: **4 minuti**</span><span class="sxs-lookup"><span data-stu-id="1827a-152">Idle timeout: **4 minutes**</span></span>
    - <span data-ttu-id="1827a-153">Indirizzo IP mobile: **Disabilitato**</span><span class="sxs-lookup"><span data-stu-id="1827a-153">Floating IP: **Disabled**</span></span>

1. <span data-ttu-id="1827a-154">Fare clic su **OK** per salvare la regola.</span><span class="sxs-lookup"><span data-stu-id="1827a-154">Click **OK** to save the rule.</span></span>

<span data-ttu-id="1827a-155">Attendere che la configurazione del bilanciamento del carico venga aggiornata prima di procedere con l'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1827a-155">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="test-the-load-balancer"></a><span data-ttu-id="1827a-156">Testare il bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="1827a-156">Test the load balancer</span></span>

1. <span data-ttu-id="1827a-157">Tornare alla pagina **Panoramica** del servizio di bilanciamento del carico e fare clic sul collegamento **Indirizzo IP pubblico** nella pagina per assegnare l'indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="1827a-157">Switch back to the **Overview** page of the load balancer and click the **Public IP address** link on the page to get to the IP address assigned.</span></span> <span data-ttu-id="1827a-158">In alternativa, è possibile usare **Tutte le risorse** e quindi nell'elenco delle risorse selezionare **woodgrove-LB-ip**.</span><span class="sxs-lookup"><span data-stu-id="1827a-158">Alternatively, you can use **All resources** and then in the resource list, select **woodgrove-LB-ip**.</span></span>

1. <span data-ttu-id="1827a-159">Nel pannello **Panoramica** selezionare l'**indirizzo IP** e quindi fare clic sul pulsante **Copia** accanto all'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="1827a-159">On the **Overview** blade, select the **IP address**, and click the **Copy** button next to it.</span></span> <span data-ttu-id="1827a-160">Questo indirizzo è l'indirizzo IP _pubblico_ del servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="1827a-160">This is the _public_ IP address for the load balancer.</span></span>

    ![Screenshot che mostra il pannello Panoramica dell'indirizzo IP pubblico](../media/6-public-ip.png)

1. <span data-ttu-id="1827a-162">Aprire una nuova scheda del browser e incollare l'indirizzo IP nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="1827a-162">Open a new browser tab and paste the IP address into the address bar of your browser.</span></span> <span data-ttu-id="1827a-163">Prendere nota del nome del server e mantenere aperta questa scheda.</span><span class="sxs-lookup"><span data-stu-id="1827a-163">Note the name of the server and keep this tab open.</span></span>

1. <span data-ttu-id="1827a-164">Nel portale di Azure, nel menu a sinistra, fare clic su **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="1827a-164">In the Azure portal, in the left menu, click **All resources**.</span></span> <span data-ttu-id="1827a-165">Quindi, nell'elenco di risorse, fare clic su server annotato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="1827a-165">Then, in the resource list, click the server you noted above.</span></span>

1. <span data-ttu-id="1827a-166">Nel pannello **Panoramica** fare clic su **Arresta** e quindi fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="1827a-166">On the **Overview** blade, click **Stop**, and then click **Yes**.</span></span>

1. <span data-ttu-id="1827a-167">Attendere fino al completamento dell'arresto della macchina virtuale e quindi passare alla scheda visualizzata nel passaggio 3.</span><span class="sxs-lookup"><span data-stu-id="1827a-167">Wait until the VM has stopped, and then switch to the tab you viewed in step 3.</span></span> <span data-ttu-id="1827a-168">Aggiornare la pagina.</span><span class="sxs-lookup"><span data-stu-id="1827a-168">Refresh the page.</span></span>

1. <span data-ttu-id="1827a-169">Il servizio di bilanciamento del carico invia la richiesta HTTP a una delle altre macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="1827a-169">The load balancer will now send your HTTP request to one of your other VMs.</span></span>

<span data-ttu-id="1827a-170">In questo esercizio è sta completata la distribuzione di macchine virtuali back-end e del servizio di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="1827a-170">In this exercise, you completed the deployment of your backend VMs and the load balancer.</span></span>
