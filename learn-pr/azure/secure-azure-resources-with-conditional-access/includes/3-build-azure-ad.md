<span data-ttu-id="e13de-101">Si decide di distribuire Azure AD e usare i criteri di accesso condizionale in modo che Azure richieda Multi-Factor Authentication quando qualcuno accede al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e13de-101">You decide to deploy Azure AD and use conditional access policies that Azure require Multi-Factor Authentication when anyone accesses the Azure portal.</span></span> <span data-ttu-id="e13de-102">È necessario creare una directory e avere a disposizione licenze temporanee.</span><span class="sxs-lookup"><span data-stu-id="e13de-102">You need to create a directory and get temporary licenses in place.</span></span>

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a><span data-ttu-id="e13de-103">Avviare il lab e accedere al portale di Azure</span><span class="sxs-lookup"><span data-stu-id="e13de-103">Launch lab and sign in to the Azure portal</span></span>

1. <span data-ttu-id="e13de-104">Per avviare il lab, fare clic sul collegamento **Launch Lab** (Avvia lab) riportato sopra.</span><span class="sxs-lookup"><span data-stu-id="e13de-104">Click the **Launch Lab** link above to launch the lab.</span></span>

> [!NOTE]
> <span data-ttu-id="e13de-105">Dopo l'avvio del lab, il nome utente e la password necessari per eseguire l'accesso sono disponibili nella scheda **Risorse** accanto alle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e13de-105">After launching the lab, the username and password you need to sign in with is located on the **Resources** tab next to the instructions.</span></span>

## <a name="create-a-directory"></a><span data-ttu-id="e13de-106">Creare una directory</span><span class="sxs-lookup"><span data-stu-id="e13de-106">Create a directory</span></span>

<span data-ttu-id="e13de-107">Si creerà una nuova directory per First Up Consultants in cui è possibile eseguire test senza temere conseguenze per gli utenti di produzione.</span><span class="sxs-lookup"><span data-stu-id="e13de-107">We will create a new directory for First Up Consultants where we can test without fear of impacting production users.</span></span>

1. <span data-ttu-id="e13de-108">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="e13de-108">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

2. <span data-ttu-id="e13de-109">Nel riquadro di navigazione a sinistra fare clic su **Crea una risorsa** > **Identità** > **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e13de-109">In the left navigation pane, click **Create a resource** > **Identity** > **Azure Active Directory**.</span></span>

3. <span data-ttu-id="e13de-110">Nel pannello **Crea directory** specificare i valori seguenti per **Nome organizzazione** e **Nome di dominio iniziale**:</span><span class="sxs-lookup"><span data-stu-id="e13de-110">In the **Create directory** blade, provide the following values for the **Organization name** and **Initial domain name**:</span></span>

   1. <span data-ttu-id="e13de-111">Nome organizzazione: `First Up Consultants`.</span><span class="sxs-lookup"><span data-stu-id="e13de-111">Organization Name: `First Up Consultants`.</span></span>
   2. <span data-ttu-id="e13de-112">Nome di dominio iniziale: `firstupconsultants<XXXXXXX>` in cui <XXXXXXX> è il numero che segue il nome utente nella scheda **Risorse** nella parte superiore della finestra delle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e13de-112">Initial Domain Name: `firstupconsultants<XXXXXXX>` where <XXXXXXX> is the number after the username on the **Resources** tab at the top of the instructions window.</span></span>

4. <span data-ttu-id="e13de-113">Attendere che la directory venga creata.</span><span class="sxs-lookup"><span data-stu-id="e13de-113">Wait for the directory to be created.</span></span> <span data-ttu-id="e13de-114">Fare clic sul collegamento per passare alla nuova directory oppure fare clic su **Filtro per directory e sottoscrizione** nella parte superiore della finestra e quindi scegliere la directory appena creata.</span><span class="sxs-lookup"><span data-stu-id="e13de-114">Click the link to switch to the new directory or click the **Directory and subscription filter** at the top of the window and then choose the newly created directory.</span></span>

## <a name="get-trial-licenses"></a><span data-ttu-id="e13de-115">Ottenere le licenze per la versione di valutazione</span><span class="sxs-lookup"><span data-stu-id="e13de-115">Get trial licenses</span></span>

<span data-ttu-id="e13de-116">Per usare funzionalità come l'accesso condizionale e l'autenticazione a più fattori è necessaria almeno una licenza di valutazione.</span><span class="sxs-lookup"><span data-stu-id="e13de-116">To use features like conditional access and Multi-Factor Authentication, you will need at least a trial license.</span></span> <span data-ttu-id="e13de-117">La procedura seguente illustra come abilitare una licenza di valutazione:</span><span class="sxs-lookup"><span data-stu-id="e13de-117">The following steps walk you through how to enable a trial license:</span></span>

1. <span data-ttu-id="e13de-118">Nel riquadro **Panoramica** di Azure AD fare clic sul collegamento per **avviare una versione di valutazione gratuita**.</span><span class="sxs-lookup"><span data-stu-id="e13de-118">In the Azure AD **Overview** pane, click the **Start a free trial** link.</span></span>

1. <span data-ttu-id="e13de-119">Sotto la voce **Azure AD Premium P2** fare clic su **Versione di valutazione gratuita**, quindi fare clic su **Attiva**.</span><span class="sxs-lookup"><span data-stu-id="e13de-119">Under the item **Azure AD Premium P2**, click **Free trial**, and then click **Activate**.</span></span>

## <a name="create-a-test-user"></a><span data-ttu-id="e13de-120">Creare un utente di test</span><span class="sxs-lookup"><span data-stu-id="e13de-120">Create a test user</span></span>

<span data-ttu-id="e13de-121">Per eseguire il test è necessario un utente.</span><span class="sxs-lookup"><span data-stu-id="e13de-121">We're going to need to test this out with a user.</span></span> <span data-ttu-id="e13de-122">Isabella Simonsen, un altro membro del team, ha offerto il proprio aiuto. Dato che avrà bisogno di un account nella directory, verrà esaminata la procedura di creazione del suo account.</span><span class="sxs-lookup"><span data-stu-id="e13de-122">Isabella Simonsen (another member of your team) has volunteered to help you out. She will need an account in the directory, so we will go through the steps to create her account.</span></span>

1. <span data-ttu-id="e13de-123">Passare a **Azure Active Directory** > **Utenti**.</span><span class="sxs-lookup"><span data-stu-id="e13de-123">Browse to **Azure Active Directory** > **Users**.</span></span>

1. <span data-ttu-id="e13de-124">Fare clic su **Nuovo utente**.</span><span class="sxs-lookup"><span data-stu-id="e13de-124">Click **New user**.</span></span>

1. <span data-ttu-id="e13de-125">Creare un utente denominato **Isabella Simonsen** con questo nome utente:</span><span class="sxs-lookup"><span data-stu-id="e13de-125">Create a user named **Isabella Simonsen** with a user name of:</span></span>

   `Isabella@firstupconsultants<XXXXXXX>.onmicrosoft.com`

   <span data-ttu-id="e13de-126">Fare in modo che il dominio dopo il simbolo @ corrisponda al dominio creato nella sezione *Creare una directory* sopra riportata.</span><span class="sxs-lookup"><span data-stu-id="e13de-126">Match the domain after the @ with the domain you created in the *Create a directory* section above.</span></span>

1. <span data-ttu-id="e13de-127">Selezionare la casella per **visualizzare la password** per l'utente.</span><span class="sxs-lookup"><span data-stu-id="e13de-127">Check the box to **Show Password** for the user.</span></span> <span data-ttu-id="e13de-128">Prendere nota della password in modo da usarla in un secondo tempo durante il test.</span><span class="sxs-lookup"><span data-stu-id="e13de-128">Make a note of the password so you can use it later when testing.</span></span>

1. <span data-ttu-id="e13de-129">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e13de-129">Click **Create**.</span></span>

## <a name="create-a-pilot-group"></a><span data-ttu-id="e13de-130">Creare un gruppo pilota</span><span class="sxs-lookup"><span data-stu-id="e13de-130">Create a pilot group</span></span>

<span data-ttu-id="e13de-131">Il criterio creato verrà assegnato a un gruppo di utenti, ma è necessario creare un gruppo per questo criterio.</span><span class="sxs-lookup"><span data-stu-id="e13de-131">We will be assigning the policy that we create to a group of users, but we need to create a group for this policy.</span></span> <span data-ttu-id="e13de-132">I passaggi seguenti consentono di creare un gruppo di sicurezza per la distribuzione pilota.</span><span class="sxs-lookup"><span data-stu-id="e13de-132">The following steps help you create a security group for the pilot deployment.</span></span>

1. <span data-ttu-id="e13de-133">Passare a **Azure Active Directory** > **Gruppi**.</span><span class="sxs-lookup"><span data-stu-id="e13de-133">Browse to **Azure Active Directory** > **Groups**.</span></span>

1. <span data-ttu-id="e13de-134">Fare clic su **Nuovo gruppo**.</span><span class="sxs-lookup"><span data-stu-id="e13de-134">Click **New group**.</span></span>

1. <span data-ttu-id="e13de-135">Tipo di gruppo **Sicurezza**.</span><span class="sxs-lookup"><span data-stu-id="e13de-135">Group type **Security**.</span></span>

1. <span data-ttu-id="e13de-136">Nome del gruppo **CA-MFA-AzurePortal**.</span><span class="sxs-lookup"><span data-stu-id="e13de-136">Group name **CA-MFA-AzurePortal**.</span></span>

1. <span data-ttu-id="e13de-137">Tipo di appartenenza **Assegnato**.</span><span class="sxs-lookup"><span data-stu-id="e13de-137">Membership type **Assigned**.</span></span>

1. <span data-ttu-id="e13de-138">Selezionare l'utente creato nel passaggio precedente e scegliere **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="e13de-138">Select the user that we created in the previous step and choose **Select**.</span></span>

1. <span data-ttu-id="e13de-139">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e13de-139">Click **Create**.</span></span>

<span data-ttu-id="e13de-140">In questa unità si è appreso come creare una directory con licenza di valutazione, un utente di test e un gruppo pilota nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e13de-140">In this unit, you learned how to create a trial licensed directory, a test user, and a pilot group in the Azure portal.</span></span>