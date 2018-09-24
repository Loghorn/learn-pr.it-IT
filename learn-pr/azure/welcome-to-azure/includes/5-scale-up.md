<span data-ttu-id="b4a77-101">Il server Web è pronto e in esecuzione, ma ci si rende conto che è necessaria più potenza di calcolo per offrire un'esperienza ottimale agli utenti.</span><span class="sxs-lookup"><span data-stu-id="b4a77-101">Your web server is up and running, but you realize you need more computing power to make the experience great for your users.</span></span> <span data-ttu-id="b4a77-102">Come è possibile velocizzare l'esecuzione della macchina virtuale?</span><span class="sxs-lookup"><span data-stu-id="b4a77-102">How can you make your VM run faster?</span></span>

<span data-ttu-id="b4a77-103">Nel data center locale si potrebbe possibile spostare il server Web in hardware più potente per risolvere i problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b4a77-103">In your data center, you might move your web server to more powerful hardware to solve performance problems.</span></span> <span data-ttu-id="b4a77-104">Il problema è però la necessità di acquistare nuovi elementi per il sistema, installarli nel rack e alimentarli.</span><span class="sxs-lookup"><span data-stu-id="b4a77-104">The problem is you need to buy, rack, and power your new system.</span></span> <span data-ttu-id="b4a77-105">Con Azure, la risposta è molto più semplice.</span><span class="sxs-lookup"><span data-stu-id="b4a77-105">With Azure, the answer is much simpler.</span></span>

<span data-ttu-id="b4a77-106">Prima di aumentare le prestazioni della macchina virtuale passando a una dimensione più potente, verrà esaminato il significato di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="b4a77-106">Before you scale up your VM to a more powerful size, let's first define what scale means.</span></span>

## <a name="what-is-scale"></a><span data-ttu-id="b4a77-107">Cosa è la scalabilità?</span><span class="sxs-lookup"><span data-stu-id="b4a77-107">What is scale?</span></span>

<span data-ttu-id="b4a77-108">Il termine _scalabilità_ indica la possibilità di aggiungere larghezza di banda di rete, memoria, spazio di archiviazione o potenza di calcolo per ottenere prestazioni migliori.</span><span class="sxs-lookup"><span data-stu-id="b4a77-108">_Scale_ refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.</span></span>  

<span data-ttu-id="b4a77-109">Probabilmente si è già sentito parlare di _scalabilità verticale_ e _scalabilità orizzontale_.</span><span class="sxs-lookup"><span data-stu-id="b4a77-109">You may have heard the terms _scaling up_ and _scaling out_.</span></span>

<span data-ttu-id="b4a77-110">Con scalabilità verticale si intende l'aumento della memoria, dello spazio di archiviazione o della potenza di calcolo in una macchina virtuale esistente.</span><span class="sxs-lookup"><span data-stu-id="b4a77-110">Scaling up, or vertical scaling, means to increase the memory, storage, or compute power on an existing virtual machine.</span></span> <span data-ttu-id="b4a77-111">Ad esempio, è possibile aggiungere ulteriore memoria a un server Web o di database per renderlo più veloce.</span><span class="sxs-lookup"><span data-stu-id="b4a77-111">For example, you can add additional memory to a web or database server to make it run faster.</span></span>

<span data-ttu-id="b4a77-112">Con scalabilità orizzontale si intende l'aggiunta di macchine virtuali per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b4a77-112">Scaling out, or horizontal scaling, means to additional virtual machines to power your application.</span></span> <span data-ttu-id="b4a77-113">Ad esempio, si potrebbero creare molte macchine virtuali configurate esattamente allo stesso modo e usare il bilanciamento del carico per distribuire il lavoro tra di esse.</span><span class="sxs-lookup"><span data-stu-id="b4a77-113">For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.</span></span>

> [!TIP]
> <span data-ttu-id="b4a77-114">Il cloud è elastico.</span><span class="sxs-lookup"><span data-stu-id="b4a77-114">The cloud is elastic.</span></span> <span data-ttu-id="b4a77-115">È anche possibile _ridurre le prestazioni_ o _ridurre il numero di istanze_ della distribuzione, se l'aumento delle prestazioni o delle istanze è stato necessario solo temporaneamente.</span><span class="sxs-lookup"><span data-stu-id="b4a77-115">You can _scale down_ or _scale in_ your deployment if you needed to scale up or scale out only temporarily.</span></span> <span data-ttu-id="b4a77-116">Riducendo le prestazioni o le istanze, è possibile risparmiare denaro.</span><span class="sxs-lookup"><span data-stu-id="b4a77-116">Scaling down or scaling in can help you save money.</span></span><br><br><span data-ttu-id="b4a77-117">**Azure Advisor** e **Gestione costi di Azure** sono due servizi che aiutano a ottimizzare la spesa per il cloud.</span><span class="sxs-lookup"><span data-stu-id="b4a77-117">**Azure Advisor** and **Azure Cost Management** are two services that help you optimize cloud spend.</span></span> <span data-ttu-id="b4a77-118">È possibile usare questi servizi per identificare i casi in cui si stanno usando più risorse del necessario e quindi tornare alla capacità effettivamente usata.</span><span class="sxs-lookup"><span data-stu-id="b4a77-118">You can use these services to identify where you're using more than you need, and then scale back to the capacity you're actually using.</span></span>

## <a name="scale-up-your-vm"></a><span data-ttu-id="b4a77-119">Aumentare le prestazioni della macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="b4a77-119">Scale up your VM</span></span>

<span data-ttu-id="b4a77-120">Ricordarsi che è stata specificata la dimensione **Standard_DS2_v2** al momento della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="b4a77-120">Recall that you specified the size **Standard_DS2_v2** when you created your VM.</span></span> <span data-ttu-id="b4a77-121">La macchina virtuale dispone attualmente di due CPU virtuali e 7 GB di memoria.</span><span class="sxs-lookup"><span data-stu-id="b4a77-121">Your VM currently has two virtual CPUs and 7 GB of memory.</span></span>

<span data-ttu-id="b4a77-122">È possibile passare alla dimensione successiva, **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="b4a77-122">Let's bump up to the next size, **Standard_DS3_v2**.</span></span> <span data-ttu-id="b4a77-123">La macchina virtuale disporrà quindi di quattro CPU virtuali e 14 GB di memoria.</span><span class="sxs-lookup"><span data-stu-id="b4a77-123">Your VM will then have four virtual CPUs and 14 GB of memory.</span></span>

<span data-ttu-id="b4a77-124">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="b4a77-124">::: zone pivot="windows-cloud"</span></span>

1. <span data-ttu-id="b4a77-125">In Cloud Shell eseguire `az vm resize` per aumentare le dimensioni della macchina virtuale a **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="b4a77-125">From Cloud Shell, run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="b4a77-126">Il processo di aggiornamento richiede circa un minuto.</span><span class="sxs-lookup"><span data-stu-id="b4a77-126">The update process takes about a minute.</span></span> <span data-ttu-id="b4a77-127">Durante questo processo, la macchina virtuale viene riavviata.</span><span class="sxs-lookup"><span data-stu-id="b4a77-127">Your VM restarts during the process.</span></span>

1. <span data-ttu-id="b4a77-128">Eseguire `az vm show` per verificare che la macchina virtuale sia in esecuzione con la nuova dimensione.</span><span class="sxs-lookup"><span data-stu-id="b4a77-128">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="b4a77-129">Verrà visualizzata la nuova dimensione della macchina virtuale: **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="b4a77-129">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```output
    Standard_DS3_v2
    ```

<span data-ttu-id="b4a77-130">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="b4a77-130">::: zone-end</span></span>

<span data-ttu-id="b4a77-131">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="b4a77-131">::: zone pivot="linux-cloud"</span></span>

1. <span data-ttu-id="b4a77-132">In Cloud Shell eseguire `az vm resize` per aumentare le dimensioni della macchina virtuale a **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="b4a77-132">From Cloud Shell, run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="b4a77-133">Il processo di aggiornamento richiede circa un minuto.</span><span class="sxs-lookup"><span data-stu-id="b4a77-133">The update process takes about a minute.</span></span> <span data-ttu-id="b4a77-134">Durante questo processo, la macchina virtuale viene riavviata.</span><span class="sxs-lookup"><span data-stu-id="b4a77-134">Your VM restarts during the process.</span></span>

1. <span data-ttu-id="b4a77-135">Eseguire `az vm show` per verificare che la macchina virtuale sia in esecuzione con la nuova dimensione.</span><span class="sxs-lookup"><span data-stu-id="b4a77-135">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="b4a77-136">Verrà visualizzata la nuova dimensione della macchina virtuale: **Standard_DS3_v2**.</span><span class="sxs-lookup"><span data-stu-id="b4a77-136">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```output
    Standard_DS3_v2
    ```

<span data-ttu-id="b4a77-137">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="b4a77-137">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="b4a77-138">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b4a77-138">Summary</span></span>

<span data-ttu-id="b4a77-139">L'operazione è stata completata.</span><span class="sxs-lookup"><span data-stu-id="b4a77-139">Nice job!</span></span> <span data-ttu-id="b4a77-140">Con un solo comando è stato possibile raddoppiare la potenza della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="b4a77-140">With just one command, your VM is now twice as powerful.</span></span>

<span data-ttu-id="b4a77-141">La scalabilità verticale e la scalabilità orizzontale sono due modi per migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b4a77-141">Scaling up and scaling out are two ways to increase performance.</span></span> <span data-ttu-id="b4a77-142">In questo modulo si è visto come ridimensionare la macchina virtuale per aumentarne la potenza di calcolo.</span><span class="sxs-lookup"><span data-stu-id="b4a77-142">Here you scaled up your VM to increase its compute power.</span></span>