<span data-ttu-id="3871d-101">Si supponga di eseguire un sito per la condivisione di foto, con dati archiviati in macchine virtuali di Azure che eseguono SQL Server e applicazioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="3871d-101">Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications.</span></span> <span data-ttu-id="3871d-102">Si vogliono apportare le modifiche seguenti.</span><span class="sxs-lookup"><span data-stu-id="3871d-102">You want to make the following adjustments.</span></span>

- <span data-ttu-id="3871d-103">È necessario modificare le impostazioni della cache del disco in una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="3871d-103">You need change the disk cache settings on a VM</span></span>
- <span data-ttu-id="3871d-104">Si vuole aggiungere un nuovo disco dati alla macchina virtuale con la memorizzazione nella cache abilitata</span><span class="sxs-lookup"><span data-stu-id="3871d-104">You want dd a new data disk to the VM with caching enabled</span></span>

<span data-ttu-id="3871d-105">Si è deciso di apportare queste modifiche tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="3871d-105">You've decided to make these changes through the Azure portal.</span></span>

<span data-ttu-id="3871d-106">In questo esercizio verrà illustrato come modificare una macchina virtuale secondo le modalità descritte in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3871d-106">In this exercise, we'll walk through making the changes to a VM that we described above.</span></span> <span data-ttu-id="3871d-107">Per prima cosa, accedere al portale e creare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="3871d-107">First, let's sign in to the portal and create a VM.</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="3871d-108">Accedere al portale di Azure</span><span class="sxs-lookup"><span data-stu-id="3871d-108">Sign in to the Azure portal</span></span>
<!---TODO: Update for sandbox?--->

1. <span data-ttu-id="3871d-109">Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="3871d-109">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

## <a name="create-a-virtual-machine"></a><span data-ttu-id="3871d-110">Creare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="3871d-110">Create a virtual machine</span></span>

<span data-ttu-id="3871d-111">In questo passaggio verrà creata una macchina virtuale con le proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="3871d-111">In this step, we're going to create a VM with the following properties.</span></span>

|<span data-ttu-id="3871d-112">Proprietà</span><span class="sxs-lookup"><span data-stu-id="3871d-112">Property</span></span>  |<span data-ttu-id="3871d-113">Valore</span><span class="sxs-lookup"><span data-stu-id="3871d-113">Value</span></span>  |
|---------|---------|
|<span data-ttu-id="3871d-114">Immagine</span><span class="sxs-lookup"><span data-stu-id="3871d-114">Image</span></span>     |   <span data-ttu-id="3871d-115">**Windows Server 2016 Datacenter**</span><span class="sxs-lookup"><span data-stu-id="3871d-115">**Windows Server 2016 Datacenter**</span></span>      |
|<span data-ttu-id="3871d-116">Nome</span><span class="sxs-lookup"><span data-stu-id="3871d-116">Name</span></span>     |   <span data-ttu-id="3871d-117">**fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="3871d-117">**fotoshareVM**</span></span>     |
|<span data-ttu-id="3871d-118">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="3871d-118">Resource group</span></span>     |   <span data-ttu-id="3871d-119">**fotoshare-rg**</span><span class="sxs-lookup"><span data-stu-id="3871d-119">**fotoshare-rg**</span></span>      |


1. <span data-ttu-id="3871d-120">Nel menu a sinistra del portale selezionare **Macchine virtuali**.</span><span class="sxs-lookup"><span data-stu-id="3871d-120">In the left menu of the portal, select **Virtual machines**</span></span>

1. <span data-ttu-id="3871d-121">Selezionare ora **+ Aggiungi** in alto a sinistra nella schermata **Macchine virtuali**.</span><span class="sxs-lookup"><span data-stu-id="3871d-121">Now select **+ Add** in the top left of the **Virtual machines** screen.</span></span> <span data-ttu-id="3871d-122">Questa azione avvia il processo di creazione.</span><span class="sxs-lookup"><span data-stu-id="3871d-122">This action starts the creation process.</span></span>

1. <span data-ttu-id="3871d-123">Nel pannello Calcolo che elenca le immagini delle macchine virtuali disponibili digitare *Windows Server 2016 Datacenter* nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="3871d-123">On the Compute panel that lists available VM images, type *Windows Server 2016 Datacenter* into the search box.</span></span>

1. <span data-ttu-id="3871d-124">Selezionare **Windows Server 2016 Datacenter** dai risultati della ricerca e quindi scegliere **Crea** per avviare il processo di creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="3871d-124">Select **Windows Server 2016 Datacenter** from the search results and then select **Create** to start the VM creation process.</span></span>

1. <span data-ttu-id="3871d-125">Nel pannello **Nozioni di base**, nella casella **Nome** immettere **fotoshareVM**.</span><span class="sxs-lookup"><span data-stu-id="3871d-125">In the **Basics** panel, in the **Name** box, enter **fotoshareVM**</span></span>

1. <span data-ttu-id="3871d-126">Nelle caselle **Nome utente** e **Password** immettere un nome e una password per un account amministratore su questo server.</span><span class="sxs-lookup"><span data-stu-id="3871d-126">In the **Username** and **Password boxes**, enter a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="3871d-127">Nell'elenco a discesa **Sottoscrizione** selezionare la sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="3871d-127">In the **Subscription** dropdown, select your Azure subscription.</span></span>

1. <span data-ttu-id="3871d-128">In **Gruppo di risorse** selezionare **Crea nuovo** e digitare **fotoshare-rg** nella casella.</span><span class="sxs-lookup"><span data-stu-id="3871d-128">Under **Resource Group**, select **Create new**, and in the box, type **fotoshare-rg**.</span></span>

1. <span data-ttu-id="3871d-129">Selezionare un'area nelle vicinanze nell'elenco a discesa **Località**.</span><span class="sxs-lookup"><span data-stu-id="3871d-129">In the **Location** drop-down list, select a region near you.</span></span>

<span data-ttu-id="3871d-130">Di seguito è riportato un esempio di come dovrebbero essere le opzioni di configurazione nel pannello **Nozioni di base**.</span><span class="sxs-lookup"><span data-stu-id="3871d-130">The following is an example of what the **Basics** configuration looks like when filled out.</span></span>

![Screenshot delle opzioni di configurazione nel pannello Nozioni di base della macchina virtuale.](../media-draft/vm-basics-settings.PNG)

1. <span data-ttu-id="3871d-132">Scegliere **OK** per procedere al passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="3871d-132">Select **OK** to move onto the next step.</span></span>

<span data-ttu-id="3871d-133">È ora necessario scegliere una dimensione per la macchina virtuale e quindi avviare la distribuzione:</span><span class="sxs-lookup"><span data-stu-id="3871d-133">You now need to choose a size for the VM, and then start the deployment:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3871d-134">Non dimenticare che la memorizzazione nella cache del disco non può essere modificata per le macchine virtuali serie L e serie B.</span><span class="sxs-lookup"><span data-stu-id="3871d-134">Remember that disk caching can't be changed for L-Series and B-series virtual machines.</span></span> <span data-ttu-id="3871d-135">Verrà scelta una dimensione diversa.</span><span class="sxs-lookup"><span data-stu-id="3871d-135">We'll select a different size.</span></span>

1. <span data-ttu-id="3871d-136">Nella sezione **Scegli una dimensione** selezionare uno SKU **Standard**, ad esempio **F1s**, e quindi scegliere **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="3871d-136">In the **Choose a size** section, select a **Standard** SKU, such as **F1s**, and then choose **Select**.</span></span>

1. <span data-ttu-id="3871d-137">Scorrere verso il basso il riquadro **Impostazioni** e osservare che non è disponibile alcuna opzione per configurare la memorizzazione nella cache del disco.</span><span class="sxs-lookup"><span data-stu-id="3871d-137">Scroll down the **Settings** pane and observe that there is no option to configure disk caching.</span></span> <span data-ttu-id="3871d-138">Accettare le impostazioni predefinite in questo passaggio e scegliere **OK** nella parte inferiore del riquadro.</span><span class="sxs-lookup"><span data-stu-id="3871d-138">Let's accept the defaults on this step and choose **OK** at the bottom of the pane.</span></span>

1. <span data-ttu-id="3871d-139">Nel pannello **Crea** esaminare il riepilogo e quindi scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3871d-139">On the **Create** panel, review the summary, and then choose **Create**.</span></span>

1. <span data-ttu-id="3871d-140">La creazione della macchina virtuale può richiedere tempo.</span><span class="sxs-lookup"><span data-stu-id="3871d-140">VM creation can take a while.</span></span> <span data-ttu-id="3871d-141">Attendere il completamento della distribuzione della macchina virtuale prima di continuare con l'esercizio.</span><span class="sxs-lookup"><span data-stu-id="3871d-141">Wait until the VM has deployed before continuing with the exercise.</span></span> <span data-ttu-id="3871d-142">Al termine del processo verrà visualizzato un messaggio nell'hub di notifica.</span><span class="sxs-lookup"><span data-stu-id="3871d-142">You'll get a message in the Notification hub when the process is complete.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3871d-143">Questa macchina virtuale verrà usata nella lezione successiva, pertanto non eliminarla.</span><span class="sxs-lookup"><span data-stu-id="3871d-143">We'll use this VM in the next lesson, so keep it around for a while!</span></span>

## <a name="view-os-disk-cache-status-in-the-portal"></a><span data-ttu-id="3871d-144">Visualizzare lo stato della cache del disco del sistema operativo nel portale</span><span class="sxs-lookup"><span data-stu-id="3871d-144">View OS disk cache status in the portal</span></span>

<span data-ttu-id="3871d-145">Dopo avere distribuito la macchina virtuale, è possibile verificare lo stato di memorizzazione nella cache del disco del sistema operativo usando la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="3871d-145">Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps.</span></span>

1. <span data-ttu-id="3871d-146">Nel menu a sinistra fare clic su **Tutte le risorse** e quindi selezionare la macchina virtuale **fotoshareVM**.</span><span class="sxs-lookup"><span data-stu-id="3871d-146">In the left menu, click **All resources**, and then select your VM,  **fotoshareVM**.</span></span>

1. <span data-ttu-id="3871d-147">Nella schermata **Macchina virtuale**, in **IMPOSTAZIONI** selezionare **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="3871d-147">On the **Virtual machine** screen, under **SETTINGS**, select **Disks**.</span></span>

1. <span data-ttu-id="3871d-148">Nel riquadro **Dischi** la macchina virtuale ha un disco, ovvero quello del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="3871d-148">On the **Disks** pane, the VM has one disk, the OS disk.</span></span> <span data-ttu-id="3871d-149">Il tipo di cache è attualmente impostato sul valore predefinito di **Lettura/scrittura**.</span><span class="sxs-lookup"><span data-stu-id="3871d-149">Its cache type is currently set to the default value of **Read/write**.</span></span>

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media-draft/os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a><span data-ttu-id="3871d-151">Modificare le impostazioni della cache del disco del sistema operativo nel portale</span><span class="sxs-lookup"><span data-stu-id="3871d-151">Change the cache settings of the OS disk in the portal</span></span>

1. <span data-ttu-id="3871d-152">Nel riquadro **Dischi** selezionare **Modifica** in alto a sinistra nella schermata.</span><span class="sxs-lookup"><span data-stu-id="3871d-152">On the **Disks** pane, select **Edit** in the upper left of the screen.</span></span>

1. <span data-ttu-id="3871d-153">Modificare il valore **Memorizzazione nella cache dell'host** per il disco del sistema operativo impostandolo su **Sola lettura** usando l'elenco a discesa e quindi selezionare **Salva** in alto a sinistra nella schermata.</span><span class="sxs-lookup"><span data-stu-id="3871d-153">Change the **HOST CACHING** value for the OS disk to **Read-only** using the dropdown and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="3871d-154">Questo aggiornamento richiede tempo.</span><span class="sxs-lookup"><span data-stu-id="3871d-154">This update takes a while.</span></span> <span data-ttu-id="3871d-155">Il motivo è che la modifica dell'impostazione della cache di un disco di Azure scollega e ricollega il disco di destinazione.</span><span class="sxs-lookup"><span data-stu-id="3871d-155">The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="3871d-156">Se si tratta del disco del sistema operativo, anche la macchina virtuale viene riavviata.</span><span class="sxs-lookup"><span data-stu-id="3871d-156">If it's the operating system disk, the VM is also restarted.</span></span> <span data-ttu-id="3871d-157">Al termine dell'aggiornamento verrà visualizzato un messaggio simile alla notifica seguente.</span><span class="sxs-lookup"><span data-stu-id="3871d-157">You'll get a message similar to the following notification when the update has finished.</span></span>

![Esempio di notifica visualizzata al termine dell'aggiornamento dell'impostazione della cache.](../media-draft/vm-disk-update-complete.PNG)

4. <span data-ttu-id="3871d-159">Al termine dell'operazione, il tipo di cache del disco del sistema operativo è impostato su **Sola lettura**.</span><span class="sxs-lookup"><span data-stu-id="3871d-159">Once complete, the OS disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="3871d-160">È ora possibile passare alla configurazione della cache del disco dati.</span><span class="sxs-lookup"><span data-stu-id="3871d-160">Let's move on to data disk cache configuration.</span></span> <span data-ttu-id="3871d-161">Per configurare un disco, è prima necessario crearne uno.</span><span class="sxs-lookup"><span data-stu-id="3871d-161">To configure a disk, we'll need to first create one.</span></span>

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a><span data-ttu-id="3871d-162">Aggiungere un disco dati alla macchina virtuale e impostare il tipo di memorizzazione nella cache</span><span class="sxs-lookup"><span data-stu-id="3871d-162">Add a data disk to the VM and set caching type</span></span>

1. <span data-ttu-id="3871d-163">Nella visualizzazione **Dischi** della macchina virtuale nel portale proseguire e selezionare **Aggiungi disco dati**.</span><span class="sxs-lookup"><span data-stu-id="3871d-163">Back on the **Disks** view of our VM in the portal, go ahead and select **Add data disk**.</span></span> <span data-ttu-id="3871d-164">Viene immediatamente visualizzato un errore nel campo **Nome** che indica che il campo non può essere vuoto.</span><span class="sxs-lookup"><span data-stu-id="3871d-164">An error immediately appears in the **Name** field telling us that the field can't be empty.</span></span> <span data-ttu-id="3871d-165">Non si dispone ancora di un disco dati, pertanto è necessario crearne uno.</span><span class="sxs-lookup"><span data-stu-id="3871d-165">We don't have a data disk yet, so let's create one.</span></span>

1. <span data-ttu-id="3871d-166">Fare clic nell'elenco **Nome** e quindi su **Crea disco**.</span><span class="sxs-lookup"><span data-stu-id="3871d-166">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="3871d-167">Nel riquadro **Crea disco gestito**, nella casella **Nome** digitare **fotosharesVM-data**.</span><span class="sxs-lookup"><span data-stu-id="3871d-167">In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.</span></span>

1. <span data-ttu-id="3871d-168">In **Gruppo di risorse** selezionare **Usa esistente** e quindi selezionare **fotoshare-rg** dal menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="3871d-168">Under **Resource Group**, select **Use existing**, and select **fotoshare-rg** from the dropdown menu.</span></span>

1. <span data-ttu-id="3871d-169">Scegliere **Crea** nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="3871d-169">Select **Create** at the bottom of the screen.</span></span>

1. <span data-ttu-id="3871d-170">Attendere il completamento della creazione del disco prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="3871d-170">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="3871d-171">Modificare il valore **Memorizzazione nella cache dell'host** per il nuovo disco dati impostandolo su **Sola lettura** usando l'elenco a discesa e quindi selezionare **Salva** in alto a sinistra nella schermata.</span><span class="sxs-lookup"><span data-stu-id="3871d-171">Change the **HOST CACHING** value for our new data disk to **Read-only** using the dropdown and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="3871d-172">Attendere l'aggiornamento della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="3871d-172">Wait for the VM to update.</span></span> <span data-ttu-id="3871d-173">L'aggiornamento richiede tempo, poiché Azure scollega e ricollega il disco dati per modificare questa impostazione.</span><span class="sxs-lookup"><span data-stu-id="3871d-173">Updating takes a while because Azure detaches and reattaches the data disk to change this setting.</span></span>

1. <span data-ttu-id="3871d-174">Al termine dell'operazione, il tipo di cache del disco dati è impostato su **Sola lettura**.</span><span class="sxs-lookup"><span data-stu-id="3871d-174">Once complete, the data disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="3871d-175">In questo esercizio è stato usato il portale di Azure per configurare la memorizzazione nella cache in una nuova macchina virtuale, modificare le impostazioni della cache in un disco esistente e configurare la memorizzazione nella cache in un nuovo disco dati.</span><span class="sxs-lookup"><span data-stu-id="3871d-175">In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and to configure caching on a new data disk.</span></span> <span data-ttu-id="3871d-176">Lo screenshot seguente illustra la configurazione finale.</span><span class="sxs-lookup"><span data-stu-id="3871d-176">The following screenshot shows the final configuration.</span></span> 

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media-draft/disks-final-config-portal.PNG)