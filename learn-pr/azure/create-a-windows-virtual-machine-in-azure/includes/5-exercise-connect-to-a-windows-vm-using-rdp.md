<span data-ttu-id="f6157-101">In questo esercizio si userà il client RDP per connettersi alla VM di Windows creata nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="f6157-101">In this exercise, you'll use the RDP client to connect to the Windows VM that you created in the previous unit.</span></span> <span data-ttu-id="f6157-102">È possibile connettersi scaricando ed eseguendo un file RDP dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f6157-102">You can connect by downloading and running an RDP file from the Azure portal.</span></span> <span data-ttu-id="f6157-103">Il file RDP conterrà:</span><span class="sxs-lookup"><span data-stu-id="f6157-103">This RDP file will have:</span></span>

* <span data-ttu-id="f6157-104">L'indirizzo IP pubblico della VM.</span><span class="sxs-lookup"><span data-stu-id="f6157-104">The public IP address of the VM.</span></span>
* <span data-ttu-id="f6157-105">Il numero della porta.</span><span class="sxs-lookup"><span data-stu-id="f6157-105">The port number.</span></span>

## <a name="motivation"></a><span data-ttu-id="f6157-106">Motivazione</span><span class="sxs-lookup"><span data-stu-id="f6157-106">Motivation</span></span>

<span data-ttu-id="f6157-107">Dallo scenario di questo esercizio, l'utente è uno studente che richiede informazioni sull'aggiunta di ruoli e funzionalità a un computer Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f6157-107">From our exercise scenario, you're now a student and you want to learn about adding roles and features to a Windows Server computer.</span></span> <span data-ttu-id="f6157-108">L'amministratore di rete non vuole tuttavia che l'operazione venga eseguita in un computer fisico nella rete e i computer dell'istituto di istruzione non sono sufficientemente specificati per l'esecuzione di Windows Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f6157-108">However, your network administrator doesn't want you to do this on a physical computer on the network, and the school's computers are too poorly specified to run Windows Hyper-V.</span></span> <span data-ttu-id="f6157-109">Azure offre la soluzione perfetta.</span><span class="sxs-lookup"><span data-stu-id="f6157-109">Azure provides the perfect solution.</span></span>

## <a name="configure-network-and-public-ip-address-settings"></a><span data-ttu-id="f6157-110">Configurare le impostazioni di rete e dell'indirizzo IP pubblico</span><span class="sxs-lookup"><span data-stu-id="f6157-110">Configure network and public IP address settings</span></span>

1. <span data-ttu-id="f6157-111">Nel portale di Azure, verificare che il pannello per la macchina virtuale creata in precedenza sia aperto.</span><span class="sxs-lookup"><span data-stu-id="f6157-111">In the Azure portal, ensure the blade for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="f6157-112">Il pannello è disponibile in **Tutte le risorse** se è necessario aprirlo.</span><span class="sxs-lookup"><span data-stu-id="f6157-112">You can find the blade under **All Resources** if you need to open it.</span></span>

1. <span data-ttu-id="f6157-113">Passare alla sezione **Rete**.</span><span class="sxs-lookup"><span data-stu-id="f6157-113">Go to the **Networking** section.</span></span> <span data-ttu-id="f6157-114">La parte superiore di questa sezione include collegamenti alla subnet virtuale e all'indirizzo IP dinamico creati con la macchina virtuale, dal momento che sono state usate le impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="f6157-114">The top of this section has links to the virtual subnet and dynamic IP address that were created along with our VM since we used the defaults.</span></span> <span data-ttu-id="f6157-115">Verrebbero seguiti questi collegamenti se si intendesse modificare le risorse (ad esempio, passando a un indirizzo IP statico).</span><span class="sxs-lookup"><span data-stu-id="f6157-115">We would follow these links if we wanted to change those resources (for example, changing to a static IP address).</span></span>

1. <span data-ttu-id="f6157-116">Fare clic su **Aggiungi regola porta in ingresso**.</span><span class="sxs-lookup"><span data-stu-id="f6157-116">Click **Add inbound port rule**.</span></span>

1. <span data-ttu-id="f6157-117">Nella parte superiore della finestra di dialogo **Aggiungi regola di sicurezza in ingresso** fare clic su **Base**.</span><span class="sxs-lookup"><span data-stu-id="f6157-117">At the top of the **Add inbound security rule** dialog box, click **basic**.</span></span>

1. <span data-ttu-id="f6157-118">In **Servizio** selezionare **RDP**, quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f6157-118">Under **Service**, select **RDP**, and then click **Add**.</span></span>

## <a name="connect-to-the-vm-by-using-rdp"></a><span data-ttu-id="f6157-119">Connettersi alla VM mediante RDP</span><span class="sxs-lookup"><span data-stu-id="f6157-119">Connect to the VM by using RDP</span></span>

<span data-ttu-id="f6157-120">Per scaricare il file RDP e connettersi alla VM, eseguire i passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="f6157-120">To download the RDP file and connect to the VM, carry out the following steps.</span></span>

### <a name="download-the-rdp-file"></a><span data-ttu-id="f6157-121">Scaricare il file RDP</span><span class="sxs-lookup"><span data-stu-id="f6157-121">Download the RDP file</span></span>

1. <span data-ttu-id="f6157-122">Nella sezione **Panoramica** del pannello della macchina virtuale fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="f6157-122">In the **Overview** section of the virtual machine's blade, click **Connect**.</span></span>

1. <span data-ttu-id="f6157-123">Nel pannello **Connetti a macchina virtuale** notare le impostazioni **Indirizzo IP** e **Numero di porta**, quindi fare clic su **Scarica file RDP**.</span><span class="sxs-lookup"><span data-stu-id="f6157-123">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings, then click **Download RDP File**.</span></span>

1. <span data-ttu-id="f6157-124">Nel browser fare clic su **Apri** oppure **Esegui** per aprire il file RDP.</span><span class="sxs-lookup"><span data-stu-id="f6157-124">In your browser, click **Open** or **Run** to open the RDP file.</span></span>

### <a name="connect-to-the-windows-vm"></a><span data-ttu-id="f6157-125">Connettersi alla VM di Windows</span><span class="sxs-lookup"><span data-stu-id="f6157-125">Connect to the Windows VM</span></span>

1. <span data-ttu-id="f6157-126">Nella finestra di dialogo **Connessione Desktop remoto** prendere nota dell'avviso di sicurezza e dell'indirizzo IP del computer remoto, quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="f6157-126">In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.</span></span>

1. <span data-ttu-id="f6157-127">Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati nei passaggi 6 e 7.</span><span class="sxs-lookup"><span data-stu-id="f6157-127">In the **Windows Security** dialog box, enter your user name and password that you used in steps 6 and 7.</span></span>

1. <span data-ttu-id="f6157-128">Nella seconda finestra di dialogo **Connessione Desktop remoto** prendere nota degli errori di certificato e fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f6157-128">In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.</span></span>

   > [!Note]
   > <span data-ttu-id="f6157-129">Il desktop della macchina virtuale richiede del tempo per essere visualizzato,</span><span class="sxs-lookup"><span data-stu-id="f6157-129">The virtual machine desktop takes a while to appear.</span></span> <span data-ttu-id="f6157-130">poiché l'immagine B1 non è sufficientemente specificata.</span><span class="sxs-lookup"><span data-stu-id="f6157-130">This effect is because the B1 image is under-specified.</span></span> <span data-ttu-id="f6157-131">Si potrebbe ricevere un messaggio di allocazione di memoria insufficiente.</span><span class="sxs-lookup"><span data-stu-id="f6157-131">You may receive a message about low memory allocation.</span></span>

1. <span data-ttu-id="f6157-132">Nella finestra di dialogo **Reti** fare clic su **No**.</span><span class="sxs-lookup"><span data-stu-id="f6157-132">In the **Networks** dialog, click **No**.</span></span>

### <a name="resize-the-vm-in-the-azure-portal"></a><span data-ttu-id="f6157-133">Ridimensionare la VM nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="f6157-133">Resize the VM in the Azure portal</span></span>

1. <span data-ttu-id="f6157-134">Tornare al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f6157-134">Switch back to the Azure portal.</span></span> <span data-ttu-id="f6157-135">Nella pagina delle proprietà della macchina virtuale, in **Impostazioni**, fare clic su **Dimensioni**.</span><span class="sxs-lookup"><span data-stu-id="f6157-135">On the virtual machine properties page, under **Settings**, click **Size**.</span></span>

1. <span data-ttu-id="f6157-136">Fare clic su **D2s_v3** (2 vCPU, RAM da 8 GB), quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="f6157-136">Click **D2s_v3** (2 vCPUs, 8-GB RAM), and then click **Select**.</span></span> <span data-ttu-id="f6157-137">Verrà visualizzato un messaggio di ridimensionamento della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f6157-137">A resizing virtual machine message will display.</span></span> <span data-ttu-id="f6157-138">Verrà chiusa anche la VM della finestra RDP.</span><span class="sxs-lookup"><span data-stu-id="f6157-138">The VM in the RDP window will also close down.</span></span>

1. <span data-ttu-id="f6157-139">Tornare al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f6157-139">Switch back to the Azure portal.</span></span> <span data-ttu-id="f6157-140">Nel riquadro sinistro fare clic su **Macchine virtuali**.</span><span class="sxs-lookup"><span data-stu-id="f6157-140">In the left pane, click **Virtual machines**.</span></span>

1. <span data-ttu-id="f6157-141">In **Macchine virtuali** attendere finché la VM non mostrerà lo stato **In esecuzione**.</span><span class="sxs-lookup"><span data-stu-id="f6157-141">Under **Virtual machines**, wait until your VM shows a status of **Running**.</span></span> <span data-ttu-id="f6157-142">Potrebbe essere necessario fare clic su **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="f6157-142">You may need to click **Refresh**.</span></span>

1. <span data-ttu-id="f6157-143">Fare clic sul nome della macchina virtuale, quindi in **Impostazioni** fare clic su **Dimensioni**.</span><span class="sxs-lookup"><span data-stu-id="f6157-143">Click the virtual machine's name, then in **Settings**, click **Size**.</span></span> <span data-ttu-id="f6157-144">Le dimensioni della VM sono ora impostate su **D2s_v3**.</span><span class="sxs-lookup"><span data-stu-id="f6157-144">Note the VM size is now set to **D2s_v3**.</span></span>

### <a name="reconnect-to-the-resized-vm"></a><span data-ttu-id="f6157-145">Riconnettersi alla VM ridimensionata</span><span class="sxs-lookup"><span data-stu-id="f6157-145">Reconnect to the resized VM</span></span>

1. <span data-ttu-id="f6157-146">Fare clic su **Macchine virtuali** e sulla VM.</span><span class="sxs-lookup"><span data-stu-id="f6157-146">Click **Virtual machines**, then click your VM.</span></span> <span data-ttu-id="f6157-147">Il valore di **Indirizzo IP pubblico** è probabilmente cambiato.</span><span class="sxs-lookup"><span data-stu-id="f6157-147">Note the **Public IP Address** value has probably changed.</span></span> <span data-ttu-id="f6157-148">Fare clic su **Connetti**, quindi su **Esegui** oppure **Apri** nel browser.</span><span class="sxs-lookup"><span data-stu-id="f6157-148">Click **Connect**, and then click **Run** or **Open** in your browser.</span></span>

1. <span data-ttu-id="f6157-149">Nella finestra di dialogo **Connessione Desktop remoto** prendere nota dell'avviso di sicurezza e dell'indirizzo IP del computer remoto, quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="f6157-149">In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.</span></span>

1. <span data-ttu-id="f6157-150">Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati nei passaggi 6 e 7.</span><span class="sxs-lookup"><span data-stu-id="f6157-150">In the **Windows Security** dialog box, enter your user name and password that you used in steps 6 and 7.</span></span>

1. <span data-ttu-id="f6157-151">Nella seconda finestra di dialogo **Connessione Desktop remoto** prendere nota degli errori di certificato e fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f6157-151">In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.</span></span> <span data-ttu-id="f6157-152">La macchina virtuale è ora molto più reattiva al processo di accesso.</span><span class="sxs-lookup"><span data-stu-id="f6157-152">Notice how much more responsive the virtual machine is to the sign-in process.</span></span>

1. <span data-ttu-id="f6157-153">Fare clic con il pulsante destro sulla barra delle applicazioni e scegliere **Gestione attività**.</span><span class="sxs-lookup"><span data-stu-id="f6157-153">Right-click the taskbar and click **Task Manager**.</span></span>

1. <span data-ttu-id="f6157-154">Nella finestra **Gestione attività** fare clic su **Altri dettagli**.</span><span class="sxs-lookup"><span data-stu-id="f6157-154">In the **Task Manager** window, click **More details**.</span></span>

1. <span data-ttu-id="f6157-155">Fare clic sulla scheda **Prestazioni**. La memoria totale disponibile è 8 GB, di cui circa 1,2 GB in uso.</span><span class="sxs-lookup"><span data-stu-id="f6157-155">Click the **Performance** tab. Note the total available memory is 8 GB of which about 1.2 GB will be in use.</span></span> <span data-ttu-id="f6157-156">Chiudere **Gestione attività**.</span><span class="sxs-lookup"><span data-stu-id="f6157-156">Close **Task Manager**.</span></span>

## <a name="summary"></a><span data-ttu-id="f6157-157">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f6157-157">Summary</span></span>

<span data-ttu-id="f6157-158">È stata eseguita la connessione a una VM di Windows mediante RDP.</span><span class="sxs-lookup"><span data-stu-id="f6157-158">You have connected to a Windows VM by using RDP.</span></span> <span data-ttu-id="f6157-159">Con l'accesso all'interfaccia utente del desktop è possibile amministrare questa VM come qualsiasi computer Windows.</span><span class="sxs-lookup"><span data-stu-id="f6157-159">With Desktop UI access, you can administer this VM as you would any Windows computer.</span></span>
