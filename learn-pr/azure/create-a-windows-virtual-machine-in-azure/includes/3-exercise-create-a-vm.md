<span data-ttu-id="ef8a3-101">In questo esercizio si creerà una macchina virtuale Windows in Azure.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-101">In this exercise, you'll create a Windows virtual machine in Azure.</span></span>

## <a name="motivation"></a><span data-ttu-id="ef8a3-102">Motivazione</span><span class="sxs-lookup"><span data-stu-id="ef8a3-102">Motivation</span></span>

<span data-ttu-id="ef8a3-103">Si consideri uno scenario alternativo per questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-103">Let's consider an alternate scenario for this exercise.</span></span> <span data-ttu-id="ef8a3-104">In questo scenario l'organizzazione è un istituto di istruzione che usa macchine virtuali Windows per creare rapidamente ambienti di test in cui gli studenti installano app Web, configurano domini ed esplorano i servizi e le funzionalità di Windows senza alterare i computer dell'istituto di istruzione.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-104">Here, your organization is a school that uses Windows virtual machines to spin up test environments on which students install web apps, configure domains, and explore Windows services and features without affecting the school's computers.</span></span> <span data-ttu-id="ef8a3-105">Gli insegnanti si connettono a queste macchine virtuali tramite il protocollo RDP e controllano i progressi degli studenti.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-105">Teachers connect to these VMs by using RDP and check on student progress.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="ef8a3-106">Creare una macchina virtuale Windows</span><span class="sxs-lookup"><span data-stu-id="ef8a3-106">Create a Windows VM</span></span>

<span data-ttu-id="ef8a3-107">Per creare una macchina virtuale Windows, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="ef8a3-107">To create a Windows VM, complete the following steps:</span></span>

1. <span data-ttu-id="ef8a3-108">Accedere ad Azure tramite il [portale di Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef8a3-108">Log onto Azure through the [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="ef8a3-109">Fare clic su **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-109">Click **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="ef8a3-110">Nella **barra di ricerca** immettere **Windows Server 2016 Datacenter** e quindi fare clic sul collegamento con lo stesso titolo.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-110">In the **search bar**, enter  **Windows Server 2016 Datacenter**  and then click on the link with the same title.</span></span>

### <a name="configure-the-vm-settings"></a><span data-ttu-id="ef8a3-111">Configurare le impostazioni della macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="ef8a3-111">Configure the VM settings</span></span>

1. <span data-ttu-id="ef8a3-112">In **Informazioni di base**, nel campo **Nome** immettere un nome per la macchina virtuale, ad esempio "StudentVM".</span><span class="sxs-lookup"><span data-stu-id="ef8a3-112">Under **Basics**, in the **Name** field, enter a name for your VM, such as "StudentVM."</span></span>

1. <span data-ttu-id="ef8a3-113">Nel campo **Tipo di disco della macchina virtuale** fare clic sul menu a discesa per visualizzare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-113">In the **VM Disk Type** field, click the drop-down menu to see the options.</span></span> <span data-ttu-id="ef8a3-114">Assicurarsi che sia selezionata l'opzione **Unità SSD**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-114">Ensure that **SSD** is selected.</span></span>

1. <span data-ttu-id="ef8a3-115">Nel campo **NomeUtente** immettere un nome utente idoneo da usare per accedere alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-115">In the **Username** field, enter a suitable user name to use to sign in to the VM.</span></span>

1. <span data-ttu-id="ef8a3-116">Nel campo **Password** immettere una password di almeno 12 caratteri.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-116">In the **Password** field, enter a password that's at least 12 characters long.</span></span> <span data-ttu-id="ef8a3-117">La password deve contenere caratteri maiuscoli e minuscoli, numeri e caratteri speciali.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-117">It must also have upper and lowercase characters, numbers, and special characters.</span></span>

1. <span data-ttu-id="ef8a3-118">In **Gruppo di risorse** fare clic su **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-118">Under **Resource group**, click **Create new**.</span></span> <span data-ttu-id="ef8a3-119">Assegnare un nome al gruppo di risorse, ad esempio "myTestRG".</span><span class="sxs-lookup"><span data-stu-id="ef8a3-119">Give the resource group a name, such as "myTestRG."</span></span>

1. <span data-ttu-id="ef8a3-120">Selezionare un percorso adatto per la macchina virtuale da creare e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-120">Select a suitable location for the VM to be created, and then click **OK**.</span></span>

### <a name="select-the-vm-image-size-and-options"></a><span data-ttu-id="ef8a3-121">Selezionare le dimensioni dell'immagine e le opzioni della macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="ef8a3-121">Select the VM image size and options</span></span>

1. <span data-ttu-id="ef8a3-122">Nella pagina **Scegli una dimensione** fare clic sull'immagine **B1s** e quindi fare clic su **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-122">In the **Choose a size** page, click the **B1s** image, and then click **Select**.</span></span>

   > [!Note] 
   > <span data-ttu-id="ef8a3-123">L'immagine B1 dispone solo di 1 GB di RAM e sarà soggetta a errori di memoria al primo accesso.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-123">The B1 image has only 1 GB of RAM and will result in memory errors when you first sign in.</span></span> <span data-ttu-id="ef8a3-124">È tuttavia possibile ridimensionarla in un secondo momento in un esercizio di laboratorio successivo.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-124">However, you can resize the image later as part of a later lab.</span></span>

1. <span data-ttu-id="ef8a3-125">Nella pagina **Impostazioni**, in **Selezionare le porte in ingresso pubbliche** selezionare **Non sono state trovate porte in ingresso pubbliche**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-125">In the **Settings** page, under **Select Public Inbound Ports**, select **No public inbound ports**.</span></span> <span data-ttu-id="ef8a3-126">Configureremo l'accesso RDP in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-126">We'll configure RDP access later.</span></span>

### <a name="finish-configuring-the-vm-and-create-the-image"></a><span data-ttu-id="ef8a3-127">Completare la configurazione della macchina virtuale e creare l'immagine</span><span class="sxs-lookup"><span data-stu-id="ef8a3-127">Finish configuring the VM and create the image</span></span>

1. <span data-ttu-id="ef8a3-128">Scorrere la pagina fino in fondo e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-128">Scroll to the bottom and click **OK**.</span></span>

1. <span data-ttu-id="ef8a3-129">In **Crea** controllare le impostazioni precedentemente configurate.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-129">Under **Create**, check the settings that you configured.</span></span> <span data-ttu-id="ef8a3-130">Nella parte inferiore della pagina fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-130">At the bottom, click **Create**.</span></span> <span data-ttu-id="ef8a3-131">Il dashboard di Azure mostrerà la macchina virtuale da distribuire.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-131">The Azure dashboard will show the VM that's being deployed.</span></span> <span data-ttu-id="ef8a3-132">Questa operazione può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-132">This may take several minutes.</span></span>

## <a name="summary"></a><span data-ttu-id="ef8a3-133">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ef8a3-133">Summary</span></span>

<span data-ttu-id="ef8a3-134">È stata a questo punto creata una macchina virtuale Windows Server idonea alle esigenze degli studenti e a cui si può accedere dalla rete dell'istituto di istruzione.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-134">You've now created a Windows Server virtual machine that's suitable for student requirements and accessible from the school network.</span></span> <span data-ttu-id="ef8a3-135">Nell'unità successiva si esaminerà come connettersi alla macchina virtuale tramite RDP e come gestirla.</span><span class="sxs-lookup"><span data-stu-id="ef8a3-135">In the next unit, you will look at how you connect to and manage that VM by using RDP.</span></span>
