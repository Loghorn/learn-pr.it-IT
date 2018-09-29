<span data-ttu-id="a3091-101">In questa unità si userà un modello di Azure Resource Manager per decrittografare la macchina virtuale Windows creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a3091-101">In this unit, you'll use an Azure Resource Manager template to decrypt our Windows VM we created earlier.</span></span> <span data-ttu-id="a3091-102">Nella macchina virtuale Windows è stata crittografata l'unità del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="a3091-102">We encrypted the OS drive on our Windows VM.</span></span> <span data-ttu-id="a3091-103">Nell'unità del sistema operativo, tuttavia, non sarà presente alcuna informazione e di conseguenza è possibile lasciarla decrittografata.</span><span class="sxs-lookup"><span data-stu-id="a3091-103">However, the OS drive won't have any confidential information on it, so we could leave it unencrypted.</span></span> <span data-ttu-id="a3091-104">È possibile usare un modello per decrittografare l'unità del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="a3091-104">Let's use a template to decrypt the OS drive.</span></span>

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a><span data-ttu-id="a3091-105">Configurare e distribuire una nuova macchina virtuale usando un modello di Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a3091-105">Configure and deploy a new VM using an Azure Resource Manager template</span></span>

<span data-ttu-id="a3091-106">Viene usato un modello di che Microsoft ha pubblicato su Github per creare una nuova macchina virtuale con la crittografia attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a3091-106">We're going to use a template Microsoft has published on Github to create a new VM with encryption turned on by default.</span></span>

1. <span data-ttu-id="a3091-107">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) con lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="a3091-107">Sign into the [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) with the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="a3091-108">Fare clic su **Crea risorsa** nella barra laterale sinistra.</span><span class="sxs-lookup"><span data-stu-id="a3091-108">Click **Create a Resource** in the left sidebar.</span></span>

1. <span data-ttu-id="a3091-109">Digitare **modello** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="a3091-109">Type **Template** in the search box.</span></span>

1. <span data-ttu-id="a3091-110">Selezionare **Modello distribuzione** nell'elenco dei risultati e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a3091-110">Select **Template Deployment** from the resulting list and click **Create**.</span></span>

    ![Schermata che illustra l'elemento di distribuzione del modello selezionato con il pulsante Crea evidenziato](../media/6-create-template.png)

1. <span data-ttu-id="a3091-112">Nella casella di ricerca **Selezionare modello** iniziare a digitare "201-decrypt" e selezionare il modello "201-decrypt-running-windows-vm-without-aad".</span><span class="sxs-lookup"><span data-stu-id="a3091-112">In the **Select a template** search box, start typing "201-decrypt" and select the "201-decrypt-running-windows-vm-without-aad" template.</span></span>

    ![Schermata che illustra la casella di ricerca di selezione di un modello con il completamento automatico](../media/6-custom-deployment.png)

1. <span data-ttu-id="a3091-114">Fare clic su **Seleziona modello** per avviare lo strumento di esecuzione del modello.</span><span class="sxs-lookup"><span data-stu-id="a3091-114">Click **Select Template** to launch the template runner.</span></span>

1. <span data-ttu-id="a3091-115">Nella vista impostazioni immettere le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a3091-115">In the settings view, enter the following information:</span></span>
    - <span data-ttu-id="a3091-116">Selezionare _Concierge Subscription_ (Sottoscrizione Concierge) per il campo **Sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="a3091-116">Select _Concierge Subscription_ for the **Subscription**.</span></span>
    - <span data-ttu-id="a3091-117">Selezionare il gruppo di risorse creato nell'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="a3091-117">Select your Resource Group, created in the sandbox.</span></span>
    - <span data-ttu-id="a3091-118">Selezionare la località in cui è stata creata la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="a3091-118">Select the location you created the VM in.</span></span>
    - <span data-ttu-id="a3091-119">Immettere "fmdata vm01" per il **Nome macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="a3091-119">Enter "fmdata-vm01" for the **VM Name**.</span></span>
    - <span data-ttu-id="a3091-120">Lasciare **Tipo di volume** impostato su _Tutti_.</span><span class="sxs-lookup"><span data-stu-id="a3091-120">Leave the **Volume Type** as _All_.</span></span>

1. <span data-ttu-id="a3091-121">Selezionare la casella di controllo **Accetto i termini e le condizioni**.</span><span class="sxs-lookup"><span data-stu-id="a3091-121">Select the **I agree to the terms and conditions** check box.</span></span>
1. <span data-ttu-id="a3091-122">Fare clic su **Acquisto** per eseguire il modello.</span><span class="sxs-lookup"><span data-stu-id="a3091-122">Click **Purchase** to run the template.</span></span> <span data-ttu-id="a3091-123">Si noti che non sono previsti costi perché questo è un pulsante standard.</span><span class="sxs-lookup"><span data-stu-id="a3091-123">Note that there is no cost to this - it's a standard button.</span></span>

<span data-ttu-id="a3091-124">Per il completamento della distribuzione possono essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="a3091-124">The deployment may take a few minutes to complete.</span></span>

## <a name="verify-the-encryption-status-of-the-vm"></a><span data-ttu-id="a3091-125">Verificare lo stato di crittografia della macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="a3091-125">Verify the encryption status of the VM</span></span>

1. <span data-ttu-id="a3091-126">Nella barra laterale del portale di Azure fare clic su **Macchine virtuali** e quindi selezionare la macchina virtuale **fmdata-vm01**.</span><span class="sxs-lookup"><span data-stu-id="a3091-126">In the sidebar of the Azure portal, click **Virtual machines** and select your VM **fmdata-vm01**.</span></span> <span data-ttu-id="a3091-127">In alternativa, è possibile cercare la macchina virtuale in base al nome in **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="a3091-127">Alternatively, you can search for your VM by name from **All Resources**.</span></span>

1. <span data-ttu-id="a3091-128">Nel pannello **Macchina virtuale**, in **IMPOSTAZIONI**, fare clic su **Dischi**.</span><span class="sxs-lookup"><span data-stu-id="a3091-128">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="a3091-129">Nel pannello **Dischi** notare che lo stato di crittografia dei dischi del sistema operativo è ora **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="a3091-129">On the **Disks** blade, notice the OS disk encryption status is now **Disabled**.</span></span>
