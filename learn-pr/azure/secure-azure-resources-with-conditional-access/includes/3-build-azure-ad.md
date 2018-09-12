<span data-ttu-id="82a1e-101">Si decide di distribuire Azure AD e usare i criteri di accesso condizionale in modo che Azure richieda Multi-Factor Authentication quando qualcuno accede al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="82a1e-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="82a1e-102">È necessario creare una directory e avere a disposizione licenze temporanee.</span><span class="sxs-lookup"><span data-stu-id="82a1e-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="82a1e-103">Creare una directory</span><span class="sxs-lookup"><span data-stu-id="82a1e-103">Create a directory</span></span>
<span data-ttu-id="82a1e-104">Viene creata una nuova directory per First Up Consultants in cui è possibile eseguire test senza temere conseguenze per gli utenti di produzione.</span><span class="sxs-lookup"><span data-stu-id="82a1e-104">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="82a1e-105">Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="82a1e-105">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="82a1e-106">Nel riquadro di spostamento a sinistra fare clic su **Crea una risorsa** > **Identità** > **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-106">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

1. <span data-ttu-id="82a1e-107">Nel pannello **Crea directory** specificare i valori seguenti per **Nome organizzazione** e **Nome di dominio iniziale**:</span><span class="sxs-lookup"><span data-stu-id="82a1e-107">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="82a1e-108">NOME ORGANIZZAZIONE: `First Up Consultants`.</span><span class="sxs-lookup"><span data-stu-id="82a1e-108">ORGANIZATION NAME: `First Up Consultants`.</span></span>
   1. <span data-ttu-id="82a1e-109">NOME DI DOMINIO INIZIALE: `firstupconsultants<XXXX>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="82a1e-109">INITIAL DOMAIN NAME: `firstupconsultants<XXXX>.onmicrosoft.com`.</span></span>

1. <span data-ttu-id="82a1e-110">Attendere che la directory venga creata.</span><span class="sxs-lookup"><span data-stu-id="82a1e-110">Wait for the directory to be created.</span></span> <span data-ttu-id="82a1e-111">Fare clic sul collegamento per passare alla nuova directory oppure fare clic su **Filtro per directory e sottoscrizione** nella parte superiore della finestra e quindi scegliere la directory appena creata.</span><span class="sxs-lookup"><span data-stu-id="82a1e-111">Click the link to switch to the new directory, or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="82a1e-112">Ottenere le licenze di valutazione</span><span class="sxs-lookup"><span data-stu-id="82a1e-112">Get trial licenses</span></span>

<span data-ttu-id="82a1e-113">Per usare funzionalità come l'accesso condizionale e Multi-Factor Authentication, è necessaria almeno una licenza di valutazione.</span><span class="sxs-lookup"><span data-stu-id="82a1e-113">In order for you to use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="82a1e-114">La procedura seguente illustra come abilitare una licenza di valutazione:</span><span class="sxs-lookup"><span data-stu-id="82a1e-114">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="82a1e-115">Nel riquadro **Panoramica** di Azure AD fare clic sul collegamento per **avviare una versione di valutazione gratuita**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-115">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="82a1e-116">Sotto la voce **Azure AD Premium P2** fare clic su **Versione di valutazione gratuita**, quindi fare clic su **Attiva**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-116">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="82a1e-117">Creare un utente di test</span><span class="sxs-lookup"><span data-stu-id="82a1e-117">Create a test user</span></span>

<span data-ttu-id="82a1e-118">Per eseguire il test è necessario un utente.</span><span class="sxs-lookup"><span data-stu-id="82a1e-118">We're going to need to test this out with a user.</span></span> <span data-ttu-id="82a1e-119">Isabella Simonsen, un altro membro del team, ha offerto il proprio aiuto. Avrà bisogno di un account nella directory, quindi verrà esaminata la procedura di creazione del suo account.</span><span class="sxs-lookup"><span data-stu-id="82a1e-119">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="82a1e-120">Passare a **Azure Active Directory** > **Utenti** > **Tutti gli utenti**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-120">Browse to **Azure Active Directory** > **Users** > **All users**.</span></span>

1. <span data-ttu-id="82a1e-121">Fare clic su **Nuovo utente**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-121">Click **New user**.</span></span>

1. <span data-ttu-id="82a1e-122">Creare un utente denominato **Isabella Simonsen** con questo nome utente:</span><span class="sxs-lookup"><span data-stu-id="82a1e-122">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   * `Isabella@firstupconsultants<XXXX>.onmicrosoft.com`

1. <span data-ttu-id="82a1e-123">Selezionare la casella per **visualizzare la password** per l'utente.</span><span class="sxs-lookup"><span data-stu-id="82a1e-123">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="82a1e-124">Prendere nota della password in modo da usarla in un secondo tempo durante il test.</span><span class="sxs-lookup"><span data-stu-id="82a1e-124">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="82a1e-125">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-125">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="82a1e-126">Creare un gruppo pilota</span><span class="sxs-lookup"><span data-stu-id="82a1e-126">Create a pilot group</span></span>

<span data-ttu-id="82a1e-127">Il criterio creato verrà assegnato a un gruppo di utenti, ma è necessario creare un gruppo per questo criterio.</span><span class="sxs-lookup"><span data-stu-id="82a1e-127">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="82a1e-128">I passaggi seguenti consentono di creare un gruppo di sicurezza per la distribuzione pilota.</span><span class="sxs-lookup"><span data-stu-id="82a1e-128">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="82a1e-129">Passare a **Azure Active Directory** > **Gruppi** > **Tutti i gruppi**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-129">Browse to **Azure Active Directory** > **Groups** > **All groups**.</span></span>

1. <span data-ttu-id="82a1e-130">Fare clic su **Nuovo gruppo**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-130">Click **New group**.</span></span>

1. <span data-ttu-id="82a1e-131">Tipo di gruppo **Sicurezza**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-131">Group type **Security**.</span></span>

1. <span data-ttu-id="82a1e-132">Nome del gruppo **CA-MFA-AzurePortal**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-132">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="82a1e-133">Tipo di appartenenza **Assegnato**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-133">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="82a1e-134">Selezionare l'utente creato nel passaggio precedente e scegliere **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-134">Select the user that we created in the previous step, and choose **Select**.</span></span>

1. <span data-ttu-id="82a1e-135">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="82a1e-135">Click **Create**.</span></span>

## <a name="summary"></a><span data-ttu-id="82a1e-136">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="82a1e-136">Summary</span></span>

<span data-ttu-id="82a1e-137">In questa unità si è appreso come creare una directory con licenza di valutazione, un utente di test e un gruppo pilota nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="82a1e-137">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>
