<span data-ttu-id="762c3-101">In questo esercizio si presuppone che l'azienda esegua un server di posta elettronica SMTP (Simple Mail Transfer Protocol).</span><span class="sxs-lookup"><span data-stu-id="762c3-101">In this exercise, let's assume your company runs a Simple Mail Transfer Protocol (SMTP) email server.</span></span> <span data-ttu-id="762c3-102">Si vuole eseguire la migrazione di questo server in Azure.</span><span class="sxs-lookup"><span data-stu-id="762c3-102">You want to migrate this server into Azure.</span></span> <span data-ttu-id="762c3-103">Si vuole che il server SMTP archivi i messaggi in ingresso per il proprio dominio in una cartella denominata "Drop" in un disco rigido virtuale dedicato.</span><span class="sxs-lookup"><span data-stu-id="762c3-103">You want the SMTP server to store incoming messages for your own domain in a folder called "Drop" on a dedicated VHD.</span></span>

<span data-ttu-id="762c3-104">L'obiettivo dell'esercizio è creare una macchina virtuale (VM) Windows e collegare un nuovo disco rigido virtuale (VHD) denominato "Incoming" per archiviare la directory "Drop".</span><span class="sxs-lookup"><span data-stu-id="762c3-104">The goal of the exercise is to create a Windows virtual machine (VM) and attach a new virtual hard disk (VHD) called "Incoming" to store the "Drop" directory.</span></span>

## <a name="sign-in-to-azure"></a><span data-ttu-id="762c3-105">Accedere ad Azure</span><span class="sxs-lookup"><span data-stu-id="762c3-105">Sign in to Azure</span></span>
<!---TODO: Need update for sanbox?--->

1. <span data-ttu-id="762c3-106">Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="762c3-106">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

## <a name="create-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="762c3-107">Creare una macchina virtuale Windows nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="762c3-107">Create a Windows VM in the Azure portal</span></span>

<span data-ttu-id="762c3-108">Per creare una macchina virtuale per ospitare il server SMTP con le unità dati, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="762c3-108">To create a VM to host the SMTP server with its data drives, follow these steps:</span></span>

1. <span data-ttu-id="762c3-109">Scegliere **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="762c3-109">Choose **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="762c3-110">Nella casella di ricerca sopra l'elenco di risorse di Azure Marketplace cercare e selezionare **Windows Server 2016 Datacenter** e quindi scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="762c3-110">In the search box above the list of Azure Marketplace resources, search for and select **Windows Server 2016 Datacenter**, and then choose **Create**.</span></span>

1. <span data-ttu-id="762c3-111">Nel riquadro **Generale** che viene visualizzato a destra immettere i valori delle proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="762c3-111">In the **Basics** pane that opens to the right, enter the following property values.</span></span> 


|<span data-ttu-id="762c3-112">Proprietà</span><span class="sxs-lookup"><span data-stu-id="762c3-112">Property</span></span>  |<span data-ttu-id="762c3-113">Valore</span><span class="sxs-lookup"><span data-stu-id="762c3-113">Value</span></span>  |<span data-ttu-id="762c3-114">Note</span><span class="sxs-lookup"><span data-stu-id="762c3-114">Notes</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="762c3-115">Nome</span><span class="sxs-lookup"><span data-stu-id="762c3-115">Name</span></span>     |   <span data-ttu-id="762c3-116">**MailSenderVM**</span><span class="sxs-lookup"><span data-stu-id="762c3-116">**MailSenderVM**</span></span>      |         |
|<span data-ttu-id="762c3-117">Tipo di disco della macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="762c3-117">VM disk type</span></span>     |  <span data-ttu-id="762c3-118">**Unità disco rigido Standard**</span><span class="sxs-lookup"><span data-stu-id="762c3-118">**Standard HDD**</span></span>       |   <span data-ttu-id="762c3-119">Selezionare questo valore nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="762c3-119">Select this value from the dropdown.</span></span>      |
|<span data-ttu-id="762c3-120">Nome utente</span><span class="sxs-lookup"><span data-stu-id="762c3-120">User name</span></span>     |  <span data-ttu-id="762c3-121">**mailmaster**</span><span class="sxs-lookup"><span data-stu-id="762c3-121">**mailmaster**</span></span>       |         |
|<span data-ttu-id="762c3-122">Password</span><span class="sxs-lookup"><span data-stu-id="762c3-122">Password</span></span>     |  <span data-ttu-id="762c3-123">La password deve contenere almeno 12 caratteri e soddisfare i [requisiti di complessità definiti](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="762c3-123">The password must be at least 12 characters long and meet the [defined complexity requirements](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).</span></span>       | <span data-ttu-id="762c3-124">Assicurarsi di ricordare nome utente e password, che verranno usati in tutto il modulo.</span><span class="sxs-lookup"><span data-stu-id="762c3-124">Make sure to remember this user name and password because we'll use them throughout the module.</span></span>         |
|<span data-ttu-id="762c3-125">Sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="762c3-125">Subscription</span></span>     |  <span data-ttu-id="762c3-126">Scegliere la propria sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="762c3-126">Choose your subscription.</span></span>       |  <span data-ttu-id="762c3-127">Selezionare questo valore nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="762c3-127">Select this value from the dropdown.</span></span>       |
|<span data-ttu-id="762c3-128">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="762c3-128">Resource group</span></span>     |  <span data-ttu-id="762c3-129">Selezionare **Crea nuovo** e quindi digitare **MailInfrastructure**.</span><span class="sxs-lookup"><span data-stu-id="762c3-129">Select **Create new**, and then type **MailInfrastructure**.</span></span>       |  <span data-ttu-id="762c3-130">Le risorse usate in questo modulo verranno raccolte in un unico gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="762c3-130">We'll gather all resource used in this module into one resource group.</span></span>       |
|<span data-ttu-id="762c3-131">Località</span><span class="sxs-lookup"><span data-stu-id="762c3-131">Location</span></span>     |   <span data-ttu-id="762c3-132">Una località nelle vicinanze.</span><span class="sxs-lookup"><span data-stu-id="762c3-132">A location near you.</span></span>      | <span data-ttu-id="762c3-133">Selezionare questo valore nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="762c3-133">Select this value from the dropdown.</span></span>        |

4. <span data-ttu-id="762c3-134">Selezionare **OK** nella parte inferiore della pagina **Generale** per passare al riquadro di configurazione **Dimensione**.</span><span class="sxs-lookup"><span data-stu-id="762c3-134">Select **OK** at the bottom of the **Basics** page to continue to the **Size** configuration pane.</span></span>

1. <span data-ttu-id="762c3-135">Nel riquadro di configurazione **Dimensione** cercare e selezionare **B1ms** e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="762c3-135">In the **Size** configuration pane, search for and select **B1ms**, and then click **Select**.</span></span>

1. <span data-ttu-id="762c3-136">Nel riquadro **Impostazioni**, in **Usa dischi gestiti** fare clic su **No**.</span><span class="sxs-lookup"><span data-stu-id="762c3-136">In the **Settings** pane, under **Use managed disks**, click **No**.</span></span> <span data-ttu-id="762c3-137">I dischi gestiti verranno illustrati più avanti in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="762c3-137">We'll discuss managed disks later in this module.</span></span>

1. <span data-ttu-id="762c3-138">Nell'elenco a discesa **Selezionare le porte in ingresso pubbliche** selezionare **RDP (3389)**.</span><span class="sxs-lookup"><span data-stu-id="762c3-138">In the **Select public inbound ports** dropdown list, select **RDP (3389)**.</span></span> <span data-ttu-id="762c3-139">Si userà questa porta per la connessione remota alla macchina virtuale dopo la creazione.</span><span class="sxs-lookup"><span data-stu-id="762c3-139">We'll use this port to remote into our VM after it's created.</span></span>

1. <span data-ttu-id="762c3-140">Lasciare i valori predefiniti per tutte le altre impostazioni e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="762c3-140">Leave all the other settings at their default, and then click **OK**.</span></span>

1. <span data-ttu-id="762c3-141">Nel riquadro **Crea** esaminare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="762c3-141">In the **Create** pane, review the configuration.</span></span>

1. <span data-ttu-id="762c3-142">Una volta esaminata la configurazione, fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="762c3-142">When you have reviewed the configuration,  select **Create**.</span></span> <span data-ttu-id="762c3-143">Azure crea e avvia la nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="762c3-143">Azure creates and starts the new VM.</span></span>

> [!TIP]
> <span data-ttu-id="762c3-144">Per la creazione della macchina virtuale e la sua distribuzione in Azure possono essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="762c3-144">Creating your VM and deploying it in Azure can take a few minutes.</span></span> <span data-ttu-id="762c3-145">È possibile monitorare lo stato di avanzamento nell'hub **Notifiche**.</span><span class="sxs-lookup"><span data-stu-id="762c3-145">You can watch the progress in the **Notifications** hub.</span></span> <span data-ttu-id="762c3-146">Al termine, Azure visualizzerà una finestra di dialogo di notifica.</span><span class="sxs-lookup"><span data-stu-id="762c3-146">Azure will display a notification dialog when it finishes.</span></span>

## <a name="add-an-empty-data-disk-to-our-vm"></a><span data-ttu-id="762c3-147">Aggiungere un disco dati vuoto alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="762c3-147">Add an empty data disk to our VM</span></span>

<span data-ttu-id="762c3-148">Al disco che archivia la directory "Drop" per il server SMTP verrà assegnato il nome "Incoming".</span><span class="sxs-lookup"><span data-stu-id="762c3-148">We're going to name the disk stores the "Drop" directory for your SMTP server "Incoming".</span></span> <span data-ttu-id="762c3-149">Aggiungere un nuovo disco vuoto al server usando la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="762c3-149">Let's add a new empty disk to the server using the following steps:</span></span>

1. <span data-ttu-id="762c3-150">Nel riquadro di navigazione a sinistra, in **PREFERITI** selezionare **Macchine virtuali**.</span><span class="sxs-lookup"><span data-stu-id="762c3-150">In the navigation on the left, under **FAVORITES**, select **Virtual machines**.</span></span>

1. <span data-ttu-id="762c3-151">Nell'elenco di macchine virtuali selezionare **MailSenderVM**.</span><span class="sxs-lookup"><span data-stu-id="762c3-151">In the list of VMs, select **MailSenderVM**.</span></span>

1. <span data-ttu-id="762c3-152">In **IMPOSTAZIONI** nel menu di configurazione di **MailSenderVM** a sinistra selezionare **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="762c3-152">Under **SETTINGS** of the **MailSenderVM** configuration menu on the left, select **Disks**.</span></span>

1. <span data-ttu-id="762c3-153">In **Dischi dati** selezionare **Aggiungi disco dati**.</span><span class="sxs-lookup"><span data-stu-id="762c3-153">Under **Data disks**, select **Add data disk**.</span></span>

1. <span data-ttu-id="762c3-154">Nel riquadro **Collega disco non gestito** impostare le proprietà seguenti.</span><span class="sxs-lookup"><span data-stu-id="762c3-154">In the **Attach unmanaged disks** pane, set the following properties.</span></span>


|<span data-ttu-id="762c3-155">Proprietà</span><span class="sxs-lookup"><span data-stu-id="762c3-155">Property</span></span>  |<span data-ttu-id="762c3-156">Valore</span><span class="sxs-lookup"><span data-stu-id="762c3-156">Value</span></span>  |<span data-ttu-id="762c3-157">Note</span><span class="sxs-lookup"><span data-stu-id="762c3-157">Notes</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="762c3-158">Nome</span><span class="sxs-lookup"><span data-stu-id="762c3-158">Name</span></span>     |   <span data-ttu-id="762c3-159">**MailSenderVMIncoming**</span><span class="sxs-lookup"><span data-stu-id="762c3-159">**MailSenderVMIncoming**</span></span>      |         |
|<span data-ttu-id="762c3-160">Tipo origine</span><span class="sxs-lookup"><span data-stu-id="762c3-160">Source type</span></span>     |  <span data-ttu-id="762c3-161">**Nuovo (disco vuoto)**</span><span class="sxs-lookup"><span data-stu-id="762c3-161">**New (empty disk)**</span></span>       |   <span data-ttu-id="762c3-162">Selezionare questo valore nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="762c3-162">Select this value from the dropdown.</span></span>       |
|<span data-ttu-id="762c3-163">Tipo di account</span><span class="sxs-lookup"><span data-stu-id="762c3-163">Account type</span></span>     |  <span data-ttu-id="762c3-164">**Unità disco rigido Standard**</span><span class="sxs-lookup"><span data-stu-id="762c3-164">**Standard HDD**</span></span>       |  <span data-ttu-id="762c3-165">Selezionare questo valore nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="762c3-165">Select this value from the dropdown.</span></span>        |


6. <span data-ttu-id="762c3-166">A sinistra del campo **Contenitore di archiviazione** selezionare **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="762c3-166">To the left of the **Storage container** field, select **Browse**.</span></span>

1. <span data-ttu-id="762c3-167">Nell'elenco di account di archiviazione cercare l'account di archiviazione il cui nome inizia con **mailinfrastructure** e selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="762c3-167">In the list of storage accounts, search for the storage account whose name begins with **mailinfrastructure** and select it.</span></span>

1. <span data-ttu-id="762c3-168">Nell'elenco di contenitori fare clic su **vhd** e quindi scegliere **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="762c3-168">In the list of containers, click **vhds** and then choose **Select**.</span></span>

1. <span data-ttu-id="762c3-169">Tornare alla schermata **Collega disco non gestito** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="762c3-169">Back on the **Attach unmanaged disk** screen, select **OK**.</span></span>

1. <span data-ttu-id="762c3-170">Tornare alla schermata **MailSenderVM - Dischi** e selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="762c3-170">Back on the **MailSenderVM - Disks** screen, select **Save**.</span></span>

<span data-ttu-id="762c3-171">A questo punto è stato definito un disco denominato **MainSenderVMIncoming**.</span><span class="sxs-lookup"><span data-stu-id="762c3-171">We've now defined a disk called **MainSenderVMIncoming**.</span></span> <span data-ttu-id="762c3-172">Per usare il disco, è necessario prima di tutto partizionarlo e formattarlo quando si accede alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="762c3-172">To use the disk, we'll first need to partition and format it when we log into the VM.</span></span> 

## <a name="partition-and-format-a-data-disk"></a><span data-ttu-id="762c3-173">Partizionare e formattare un disco dati</span><span class="sxs-lookup"><span data-stu-id="762c3-173">Partition and format a data disk</span></span>

<span data-ttu-id="762c3-174">Come nel caso dei dischi fisici, è necessario inizializzare e formattare una partizione in un disco rigido virtuale prima di poterlo usare.</span><span class="sxs-lookup"><span data-stu-id="762c3-174">As with physical disks, initiate and format a partition on a VHD before it can be used.</span></span>

### <a name="log-into-our-windows-vm-using-rdp"></a><span data-ttu-id="762c3-175">Accedere alla macchina virtuale Windows usando RDP</span><span class="sxs-lookup"><span data-stu-id="762c3-175">Log into our Windows VM using RDP</span></span>

1. <span data-ttu-id="762c3-176">Nella schermata principale della macchina virtuale **MailSenderVM** selezionare **Panoramica**.</span><span class="sxs-lookup"><span data-stu-id="762c3-176">In the **MailSenderVM** virtual machine main screen, select **Overview**.</span></span>

1. <span data-ttu-id="762c3-177">Selezionare **Connetti** in alto a sinistra nella schermata Panoramica.</span><span class="sxs-lookup"><span data-stu-id="762c3-177">Select **Connect** from the top left of the overview screen.</span></span>

1. <span data-ttu-id="762c3-178">Nella finestra di dialogo **Connect to virtual machine** (Connetti a macchina virtuale) visualizzata a destra selezionare **Scarica file RDP**.</span><span class="sxs-lookup"><span data-stu-id="762c3-178">In the **Connect to virtual machine** dialog that opens on the right, select **Download to RDP File**.</span></span>

   ![Screenshot della finestra di dialogo "Connect to virtual machine" (Connetti a macchina virtuale) con il pulsante "Scarica file RDP" evidenziato.](../media-draft/download-rdp.png)

4. <span data-ttu-id="762c3-180">Un file denominato **MailSenderVM.rdp** viene scaricato nella cartella `Downloads` locale.</span><span class="sxs-lookup"><span data-stu-id="762c3-180">A file called **MailSenderVM.rdp** is downloaded to your local `Downloads` folder.</span></span> <span data-ttu-id="762c3-181">Si tratta del file di configurazione desktop remoto per la macchina virtuale MailSenderVM.</span><span class="sxs-lookup"><span data-stu-id="762c3-181">This file is the remote desktop configuration file for the MailSenderVM virtual machine.</span></span> <span data-ttu-id="762c3-182">Aprire il file per avviare il processo di connessione.</span><span class="sxs-lookup"><span data-stu-id="762c3-182">Open the file to start the connection process.</span></span>

1. <span data-ttu-id="762c3-183">Nella finestra di dialogo **Connessione desktop remoto** fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-183">In the **Remote Desktop Connection** dialog, click **Connect**.</span></span>

1. <span data-ttu-id="762c3-184">Nella finestra di dialogo **Sicurezza di Windows** fare clic su **Usa un altro account**.</span><span class="sxs-lookup"><span data-stu-id="762c3-184">In the **Windows Security** dialog, click **Use another account**.</span></span>

1. <span data-ttu-id="762c3-185">Nella casella di testo **Nome utente** digitare **mailmaster**.</span><span class="sxs-lookup"><span data-stu-id="762c3-185">In the **Username** textbox, type **mailmaster**.</span></span>

1. <span data-ttu-id="762c3-186">Nella casella di testo **Password** digitare la password immessa per il nome utente in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="762c3-186">In the **Password** textbox, type the password you entered for this user name in this exercise.</span></span> 

1. <span data-ttu-id="762c3-187">Nella finestra di dialogo **Connessione desktop remoto** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="762c3-187">In the **Remote Desktop Connection** dialog, click **Yes**.</span></span>

<span data-ttu-id="762c3-188">Verrà avviata una sessione Desktop remoto alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="762c3-188">A remote desktop session to the virtual machine is now started.</span></span> <span data-ttu-id="762c3-189">Per il primo accesso, potrebbero essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="762c3-189">It might take a few moments to sign in for the first time.</span></span> <span data-ttu-id="762c3-190">Una volta completato l'accesso, verrà visualizzato lo strumento **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="762c3-190">When sign-in is finished, the **Server Manager** tool will be displayed on the screen.</span></span>

### <a name="partition-and-format-our-data-disk-using-server-manager"></a><span data-ttu-id="762c3-191">Partizionare e formattare il disco dati tramite Server Manager</span><span class="sxs-lookup"><span data-stu-id="762c3-191">Partition and format our data disk using Server Manager</span></span>

1. <span data-ttu-id="762c3-192">Quando viene visualizzato **Server Manager**, selezionare **Servizi file e archiviazione** nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="762c3-192">When **Server Manager** is displayed, select **File and Storage Services** in the navigation on the left.</span></span>

1. <span data-ttu-id="762c3-193">In **Volumi** selezionare **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="762c3-193">Under **Volumes**, select **Disks**.</span></span>

1. <span data-ttu-id="762c3-194">Nell'elenco di dischi il disco **0** è il disco del sistema operativo e il disco **1** è il disco temporaneo.</span><span class="sxs-lookup"><span data-stu-id="762c3-194">In the list of disks, disk **0** is the operating system disk and disk **1** is the temporary disk.</span></span> <span data-ttu-id="762c3-195">Selezionare il disco **2**, che corrisponde al nuovo disco rigido virtuale appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="762c3-195">Select disk **2**, which is the new VHD you just added.</span></span>

1. <span data-ttu-id="762c3-196">Nella parte superiore del riquadro **VOLUMI** selezionare **ATTIVITÀ** e quindi **Nuovo volume**. Il menu nella parte superiore destra della schermata è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="762c3-196">At the top of the **VOLUMES** pane, select **TASKS** followed by **New Volume...**. The menu is in the top right of the screen as follows.</span></span>

   ![Screenshot del menu "ATTIVITÀ" espanso per visualizzare tre voci di menu.](../media-draft/tasks-menu.png)


1. <span data-ttu-id="762c3-199">Nella procedura guidata **Nuovo volume** fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-199">In the **New Volume** wizard, click **Next**.</span></span>

1. <span data-ttu-id="762c3-200">Nella pagina **Select server and disk** (Seleziona server e disco) selezionare **MailSenderVM** e **Disco 2** e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-200">In the **Select server and disk** page, select **MailSenderVM** and **Disk 2**, and then click **Next**.</span></span>

1. <span data-ttu-id="762c3-201">Nella finestra di dialogo **Offline or Uninitiated Disk** (Disco offline o non inizializzato) fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="762c3-201">In the **Offline or Uninitiated Disk** dialog, click **OK**.</span></span>

1. <span data-ttu-id="762c3-202">Nella pagina **Specifica dimensioni del volume** fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-202">In the **Specify the size of the volume** page, click **Next**.</span></span>

1. <span data-ttu-id="762c3-203">Nella pagina **Assegnazione lettera di unità** selezionare **F:** e quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-203">In the **Assign a drive letter** page, select **F:** followed by **Next**.</span></span>

1. <span data-ttu-id="762c3-204">Nella pagina **Selezionare le impostazioni del file system**, nella casella di testo **Etichetta volume** digitare **Incoming** e quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-204">In the **Select file system settings** page, in the **Volume label** textbox, type **Incoming** and then select **Next**.</span></span>

1. <span data-ttu-id="762c3-205">Nella pagina **Conferma selezioni** selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="762c3-205">In the **Confirm selections** page, select **Create**.</span></span> <span data-ttu-id="762c3-206">Windows inizializza il disco e formatta la nuova partizione.</span><span class="sxs-lookup"><span data-stu-id="762c3-206">Windows initiates the disk and formats the new partition.</span></span>

1. <span data-ttu-id="762c3-207">Nella pagina **Completamento** selezionare **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="762c3-207">In the **Completion** page, select **Close**.</span></span>

<span data-ttu-id="762c3-208">Esaminare il nostro nuovo volume del disco in Esplora File.</span><span class="sxs-lookup"><span data-stu-id="762c3-208">Let's have a look at our new disk volume in File Explorer.</span></span> 
1. <span data-ttu-id="762c3-209">Aprire **Esplora file**.</span><span class="sxs-lookup"><span data-stu-id="762c3-209">Open **File Explorer**.</span></span>

1. <span data-ttu-id="762c3-210">Nel riquadro di spostamento a sinistra fare clic su **Questo PC** e quindi fare doppio clic su **Incoming (F:)**.</span><span class="sxs-lookup"><span data-stu-id="762c3-210">In the navigation in the left, click **This PC** and then double-click **Incoming (F:)**.</span></span>

1. <span data-ttu-id="762c3-211">Selezionare **Home** e quindi **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="762c3-211">Select **Home**, and then **New Folder**.</span></span>

1. <span data-ttu-id="762c3-212">Digitare **Drop** e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="762c3-212">Type **Drop** and then press Enter.</span></span>

<span data-ttu-id="762c3-213">È ora disponibile un nuovo volume creato nel disco rigido virtuale denominato **Incoming** ed è stata creata una cartella denominata **Drop** in tale volume.</span><span class="sxs-lookup"><span data-stu-id="762c3-213">We now have a new volume created on our VHD called **Incoming**, and we've created a folder called **Drop** on that volume.</span></span>  

1. <span data-ttu-id="762c3-214">Chiudere Esplora risorse.</span><span class="sxs-lookup"><span data-stu-id="762c3-214">Close Windows Explorer.</span></span>

1. <span data-ttu-id="762c3-215">Sulla **barra della applicazioni** fare clic sul pulsante **Start**, quindi su **Arresta** e infine su **Disconnetti**.</span><span class="sxs-lookup"><span data-stu-id="762c3-215">On the **Task Bar**, click the **Start** button, click the **Power** button, and then click **Disconnect**.</span></span>

<span data-ttu-id="762c3-216">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="762c3-216">Congratulations!</span></span> <span data-ttu-id="762c3-217">È stata creata una macchina virtuale Windows, è stato collegato un nuovo disco rigido virtuale, è stato creato un volume nel disco rigido virtuale ed è stata aggiunta una cartella al volume.</span><span class="sxs-lookup"><span data-stu-id="762c3-217">You've successfully created a Windows VM, attached a new VHD, created a volume on that VHD and added a folder to that volume.</span></span> <span data-ttu-id="762c3-218">Per il nuovo disco rigido virtuale è stato usato il tipo **Unità disco rigido Standard**.</span><span class="sxs-lookup"><span data-stu-id="762c3-218">If you recall, the disk type we used for the new VHD was a **Standard HDD**.</span></span> <span data-ttu-id="762c3-219">Nell'unità successiva verranno illustrate le differenze tra Archiviazione Standard e Premium.</span><span class="sxs-lookup"><span data-stu-id="762c3-219">In the next unit, we'll learn the differences between standard and premium storage.</span></span> 
