
<span data-ttu-id="d4472-101">Si supponga di eseguire un sito per la condivisione di foto, con dati archiviati in macchine virtuali di Azure che eseguono SQL Server e applicazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="d4472-101">Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications.</span></span> <span data-ttu-id="d4472-102">Si vogliono apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="d4472-102">You want to make the following adjustments:</span></span>

- <span data-ttu-id="d4472-103">È necessario modificare le impostazioni della cache del disco in una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-103">You need to change the disk cache settings on a VM.</span></span>
- <span data-ttu-id="d4472-104">Si vuole aggiungere un nuovo disco dati alla macchina virtuale con la memorizzazione nella cache abilitata.</span><span class="sxs-lookup"><span data-stu-id="d4472-104">You want to add a new data disk to the VM with caching enabled.</span></span>

<span data-ttu-id="d4472-105">Si è deciso di apportare queste modifiche tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="d4472-105">You've decided to make these changes through the Azure portal.</span></span>

<span data-ttu-id="d4472-106">In questo esercizio verrà illustrato come modificare una macchina virtuale secondo le modalità descritte in precedenza.</span><span class="sxs-lookup"><span data-stu-id="d4472-106">In this exercise, we'll walk through making the changes to a VM that we described above.</span></span> <span data-ttu-id="d4472-107">Per prima cosa, accedere al portale e creare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-107">First, let's sign in to the portal and create a VM.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-virtual-machine"></a><span data-ttu-id="d4472-108">Creare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="d4472-108">Create a virtual machine</span></span>

<span data-ttu-id="d4472-109">In questo passaggio si creerà una macchina virtuale con le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="d4472-109">In this step, we're going to create a VM with the following properties:</span></span>

| <span data-ttu-id="d4472-110">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d4472-110">Property</span></span>        | <span data-ttu-id="d4472-111">Valore</span><span class="sxs-lookup"><span data-stu-id="d4472-111">Value</span></span>   |
|-----------------|---------|
| <span data-ttu-id="d4472-112">Immagine</span><span class="sxs-lookup"><span data-stu-id="d4472-112">Image</span></span>           | <span data-ttu-id="d4472-113">**Windows Server 2016 Datacenter**</span><span class="sxs-lookup"><span data-stu-id="d4472-113">**Windows Server 2016 Datacenter**</span></span> |
| <span data-ttu-id="d4472-114">Nome</span><span class="sxs-lookup"><span data-stu-id="d4472-114">Name</span></span>            | <span data-ttu-id="d4472-115">**fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="d4472-115">**fotoshareVM**</span></span> |
| <span data-ttu-id="d4472-116">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="d4472-116">Resource group</span></span>  |   <span data-ttu-id="d4472-117">**<rgn>[Nome gruppo di risorse sandbox]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="d4472-117">**<rgn>[Sandbox resource group name]</rgn>**</span></span> |
| <span data-ttu-id="d4472-118">Località</span><span class="sxs-lookup"><span data-stu-id="d4472-118">Location</span></span>        | <span data-ttu-id="d4472-119">Vedere qui di seguito.</span><span class="sxs-lookup"><span data-stu-id="d4472-119">See below.</span></span> |

1. <span data-ttu-id="d4472-120">Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="d4472-120">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="d4472-121">Selezionare **Creare una risorsa** nel menu sulla barra laterale a sinistra.</span><span class="sxs-lookup"><span data-stu-id="d4472-121">Select **Create a resource** in the sidebar menu on the left.</span></span>

1. <span data-ttu-id="d4472-122">_VM Windows Server 2016_ deve essere nell'elenco degli elementi del Marketplace **Più comuni**.</span><span class="sxs-lookup"><span data-stu-id="d4472-122">_Windows Server 2016 VM_ should be in the list of **Popular** Marketplace elements.</span></span> <span data-ttu-id="d4472-123">In caso contrario, provare a cercare "Windows Server 2016 DataCenter" usando la casella di ricerca in alto.</span><span class="sxs-lookup"><span data-stu-id="d4472-123">If not, try searching for "Windows Server 2016 DataCenter" using the search box on the top.</span></span>

1. <span data-ttu-id="d4472-124">Selezionare la macchina virtuale Windows e fare clic su **Crea** per avviare il processo di creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-124">Select the Windows VM and click **Create** to start the VM creation process.</span></span>

1. <span data-ttu-id="d4472-125">Nel pannello **Informazioni di base** verificare che la sottoscrizione selezionata in **Sottoscrizione** sia _Concierge Subscription_ (Sottoscrizione Concierge).</span><span class="sxs-lookup"><span data-stu-id="d4472-125">In the **Basics** panel, verify the selected **Subscription** is _Concierge Subscription_.</span></span>

1. <span data-ttu-id="d4472-126">In **Gruppo di risorse** selezionare **Utilizza esistente** e scegliere _<rgn>[Nome gruppo di risorse sandbox]</rgn>_.</span><span class="sxs-lookup"><span data-stu-id="d4472-126">Under **Resource Group**, select **Use Existing** and choose _<rgn>[Sandbox resource group name]</rgn>_.</span></span>

1. <span data-ttu-id="d4472-127">Nella casella **Nome macchina virtuale** immettere _fotoshareVM_.</span><span class="sxs-lookup"><span data-stu-id="d4472-127">In the **Virtual machine name** box, enter _fotoshareVM_.</span></span>

1. <span data-ttu-id="d4472-128">Nell'elenco a discesa **Località** selezionare l'area più vicina a quella in cui ci si trova nell'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="d4472-128">In the **Location** drop-down list, select the closest region to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="d4472-129">Per **Dimensioni** per la macchina virtuale, il valore predefinito è **DS1 Standard v2**, che assegna una singola CPU e 3,5 GB di memoria.</span><span class="sxs-lookup"><span data-stu-id="d4472-129">For the VM **Size**, the default is **DS1 v2** which gives you a single CPU and 3.5 GB of memory.</span></span> <span data-ttu-id="d4472-130">Si tratta di dimensioni appropriate per questo esempio.</span><span class="sxs-lookup"><span data-stu-id="d4472-130">That's fine for this example.</span></span>

1. <span data-ttu-id="d4472-131">Nella sezione **ACCOUNT AMMINISTRATORE** completare i campi **Nome utente** e **Password**/**Conferma password** per un account amministratore nella nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-131">In **ADMINISTRATOR ACCOUNT** section, enter a **Username** and **Password**/**Confirm password** for an administrator account on the new VM.</span></span>

1. <span data-ttu-id="d4472-132">Di seguito è riportato un esempio di come appariranno le opzioni di configurazione nel pannello **Informazioni di base**. Mantenere le impostazioni predefinite per le altre schede e campi rimanenti e fare clic su **Rivedi e crea**.</span><span class="sxs-lookup"><span data-stu-id="d4472-132">The following image is an example of what the **Basics** configuration looks like when filled out. Leave the defaults for the remaining tabs and fields and click **Review + create**.</span></span>

    ![Screenshot del portale di Azure che mostra il pannello Crea macchina virtuale con alcune opzioni di configurazione di esempio in Informazioni di base, descritte sopra.](../media/4-basics-vm.png)

1. <span data-ttu-id="d4472-134">Dopo aver esaminato le nuove impostazioni della macchina virtuale, fare clic su **Crea** per iniziare a distribuire la nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-134">After reviewing your new VM settings, click **Create** to start the deploying your new VM.</span></span>

<span data-ttu-id="d4472-135">La creazione della macchina virtuale può richiedere alcuni minuti per la creazione di tutte le varie risorse (archiviazione, interfaccia di rete e così via) per supportare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-135">VM creation can take a few minutes as it creates all the various resources (storage, network interface, etc.) to support the virtual machine.</span></span> <span data-ttu-id="d4472-136">Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="d4472-136">Wait until the VM has deployed before continuing with the exercise.</span></span>

## <a name="view-os-disk-cache-status-in-the-portal"></a><span data-ttu-id="d4472-137">Visualizzare lo stato della cache del disco del sistema operativo nel portale</span><span class="sxs-lookup"><span data-stu-id="d4472-137">View OS disk cache status in the portal</span></span>

<span data-ttu-id="d4472-138">Dopo avere distribuito la macchina virtuale, è possibile verificare lo stato di memorizzazione nella cache del disco del sistema operativo usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="d4472-138">Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps:</span></span>

1. <span data-ttu-id="d4472-139">Selezionare la risorsa **fotoshareVM** per aprire i dettagli della macchina virtuale nel portale.</span><span class="sxs-lookup"><span data-stu-id="d4472-139">Select the **fotoshareVM** resource to open the VM details in the portal.</span></span> <span data-ttu-id="d4472-140">In alternativa, è possibile fare clic su **Tutte le risorse** sulla barra laterale a sinistra e quindi selezionare la macchina virtuale **fotoshareVM**.</span><span class="sxs-lookup"><span data-stu-id="d4472-140">Alternatively, you can click **All resources** in the left sidebar, and then select your VM, **fotoshareVM**.</span></span>

1. <span data-ttu-id="d4472-141">In **Impostazioni** selezionare **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="d4472-141">Under **Settings**, select **Disks**.</span></span>

1. <span data-ttu-id="d4472-142">Nel riquadro **Dischi** la macchina virtuale ha un disco, ovvero quello del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="d4472-142">On the **Disks** pane, the VM has one disk, the OS disk.</span></span> <span data-ttu-id="d4472-143">Il tipo di cache è attualmente impostato sul valore predefinito **Lettura/scrittura**.</span><span class="sxs-lookup"><span data-stu-id="d4472-143">Its cache type is currently set to the default value of **Read/write**.</span></span>

![Screenshot del portale di Azure che mostra la sezione Dischi del pannello Macchina virtuale, con il disco del sistema operativo visualizzato e impostato per la memorizzazione nella cache in sola lettura.](../media/4-os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a><span data-ttu-id="d4472-145">Modificare le impostazioni della cache del disco del sistema operativo nel portale</span><span class="sxs-lookup"><span data-stu-id="d4472-145">Change the cache settings of the OS disk in the portal</span></span>

1. <span data-ttu-id="d4472-146">Nel riquadro **Dischi** selezionare **Modifica** in alto a sinistra nella schermata.</span><span class="sxs-lookup"><span data-stu-id="d4472-146">On the **Disks** pane, select **Edit** in the upper left of the screen.</span></span>

1. <span data-ttu-id="d4472-147">Modificare il valore **Memorizzazione nella cache dell'host** per il disco del sistema operativo impostandolo su **Sola lettura** usando l'elenco a discesa e quindi selezionare **Salva** in alto a sinistra nella schermata.</span><span class="sxs-lookup"><span data-stu-id="d4472-147">Change the **HOST CACHING** value for the OS disk to **Read-only** using the drop-down list, and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="d4472-148">Questo aggiornamento può richiedere tempo.</span><span class="sxs-lookup"><span data-stu-id="d4472-148">This update can take some time.</span></span> <span data-ttu-id="d4472-149">Il motivo è che la modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione.</span><span class="sxs-lookup"><span data-stu-id="d4472-149">The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="d4472-150">Se si tratta del disco del sistema operativo, anche la macchina virtuale viene riavviata.</span><span class="sxs-lookup"><span data-stu-id="d4472-150">If it's the operating system disk, the VM is also restarted.</span></span> <span data-ttu-id="d4472-151">Al termine dell'operazione, si riceverà una notifica che indica che i dischi della macchina virtuale sono stati aggiornati.</span><span class="sxs-lookup"><span data-stu-id="d4472-151">When the operation completes, you'll get a notification saying the VM disks have been updated.</span></span>

1. <span data-ttu-id="d4472-152">Al termine dell'operazione, il tipo di cache del disco del sistema operativo è impostato su **Sola lettura**.</span><span class="sxs-lookup"><span data-stu-id="d4472-152">Once complete, the OS disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="d4472-153">È ora possibile passare alla configurazione della cache del disco dati.</span><span class="sxs-lookup"><span data-stu-id="d4472-153">Let's move on to data disk cache configuration.</span></span> <span data-ttu-id="d4472-154">Per configurare un disco, è prima necessario crearne uno.</span><span class="sxs-lookup"><span data-stu-id="d4472-154">To configure a disk, we'll need first to create one.</span></span>

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a><span data-ttu-id="d4472-155">Aggiungere un disco dati alla macchina virtuale e impostare il tipo di memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="d4472-155">Add a data disk to the VM and set caching type</span></span>

1. <span data-ttu-id="d4472-156">Di nuovo nella visualizzazione **Dischi** della macchina virtuale nel portale proseguire e fare clic su **Aggiungi disco dati**.</span><span class="sxs-lookup"><span data-stu-id="d4472-156">Back on the **Disks** view of our VM in the portal, go ahead and click **Add data disk**.</span></span> <span data-ttu-id="d4472-157">Viene immediatamente visualizzato un errore nel campo **Nome** che indica che il campo non può essere vuoto.</span><span class="sxs-lookup"><span data-stu-id="d4472-157">An error immediately appears in the **Name** field, telling us that the field can't be empty.</span></span> <span data-ttu-id="d4472-158">Poiché non è ancora presente un disco dati, è necessario crearne uno.</span><span class="sxs-lookup"><span data-stu-id="d4472-158">We don't have a data disk yet, so let's create one.</span></span>

1. <span data-ttu-id="d4472-159">Fare clic nell'elenco **Nome** e quindi su **Crea disco**.</span><span class="sxs-lookup"><span data-stu-id="d4472-159">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="d4472-160">Nella casella **Nome** nel riquadro **Crea disco gestito** digitare **fotosharesVM-data**.</span><span class="sxs-lookup"><span data-stu-id="d4472-160">In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.</span></span>

1. <span data-ttu-id="d4472-161">In **Gruppo di risorse** selezionare **Utilizza esistente** e scegliere _<rgn>[Nome gruppo di risorse sandbox]</rgn>_.</span><span class="sxs-lookup"><span data-stu-id="d4472-161">Under **Resource Group**, select **Use existing**, and select _<rgn>[Sandbox resource group name]</rgn>_.</span></span>

1. <span data-ttu-id="d4472-162">Osservare le impostazioni predefinite per gli altri campi:</span><span class="sxs-lookup"><span data-stu-id="d4472-162">Note the defaults for the remaining fields:</span></span>
    - <span data-ttu-id="d4472-163">SSD Premium</span><span class="sxs-lookup"><span data-stu-id="d4472-163">Premium SSD</span></span>
    - <span data-ttu-id="d4472-164">Dimensioni di 1023 GB</span><span class="sxs-lookup"><span data-stu-id="d4472-164">1023 GB in size</span></span>
    - <span data-ttu-id="d4472-165">Stessa località della macchina virtuale (non modificabile).</span><span class="sxs-lookup"><span data-stu-id="d4472-165">In the same location as the VM (not changeable).</span></span>
    - <span data-ttu-id="d4472-166">Limite per operazioni di I/O al secondo: 5000</span><span class="sxs-lookup"><span data-stu-id="d4472-166">IOPS limit - 5000</span></span>
    - <span data-ttu-id="d4472-167">Limite per velocità effettiva (MB/s): 200</span><span class="sxs-lookup"><span data-stu-id="d4472-167">Throughput limit (MB/s) - 200</span></span>

1. <span data-ttu-id="d4472-168">Fare clic su **Crea** nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="d4472-168">Click **Create** at the bottom of the screen.</span></span> 

    <span data-ttu-id="d4472-169">Attendere il completamento della creazione del disco prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="d4472-169">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="d4472-170">Modificare il valore di **MEMORIZZAZIONE NELLA CACHE DELL'HOST** per il nuovo disco dati impostandolo su **Sola lettura** usando l'elenco a discesa (potrebbe essere già impostato) e quindi fare clic su **Salva** in alto a sinistra nella schermata.</span><span class="sxs-lookup"><span data-stu-id="d4472-170">Change the **HOST CACHING** value for our new data disk to **Read-only** using the drop-down list (it might be set already), and then click **Save** in the upper left of the screen.</span></span>

    <span data-ttu-id="d4472-171">Al termine che la macchina virtuale completi l'aggiornamento del nuovo disco dati.</span><span class="sxs-lookup"><span data-stu-id="d4472-171">Wait for the VM to finish updating the new data disk.</span></span> <span data-ttu-id="d4472-172">Al termine, sarà presente un nuovo disco dati nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="d4472-172">Once complete, you will have a new data disk on your virtual machine.</span></span>

<span data-ttu-id="d4472-173">In questo esercizio è stato usato il portale di Azure per configurare la memorizzazione nella cache in una nuova macchina virtuale, modificare le impostazioni della cache in un disco esistente e configurare la memorizzazione nella cache in un nuovo disco dati.</span><span class="sxs-lookup"><span data-stu-id="d4472-173">In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and configure caching on a new data disk.</span></span> <span data-ttu-id="d4472-174">Lo screenshot seguente mostra la configurazione finale:</span><span class="sxs-lookup"><span data-stu-id="d4472-174">The following screenshot shows the final configuration:</span></span>

![Screenshot del portale di Azure che mostra il disco del sistema operativo e il nuovo disco dati nella sezione Disco del pannello Macchina virtuale, con entrambi i dischi impostati sulla memorizzazione nella cache in sola lettura.](../media/disks-final-config-portal.PNG)
