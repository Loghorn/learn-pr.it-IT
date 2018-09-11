<span data-ttu-id="473b7-101">Nell'esercizio precedente abbiamo abilitato licenze di valutazione, creato una directory, creato un utente e creato un gruppo per testare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="473b7-101">In the previous exercise, we enabled trial licenses, created a directory, created a user, and created a group to test our solution.</span></span> <span data-ttu-id="473b7-102">In questo esercizio si creerà la regola di accesso condizionale per richiedere l'autenticazione a più fattori di Azure per il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="473b7-102">In this exercise, we will create our conditional access rule to require Azure Multi-Factor Authentication for the Azure portal.</span></span>

## <a name="enable-conditional-access-based-multi-factor-authentication"></a><span data-ttu-id="473b7-103">Abilitare l'autenticazione a più fattori basata sull'accesso condizionale</span><span class="sxs-lookup"><span data-stu-id="473b7-103">Enable conditional access-based Multi-Factor Authentication</span></span>

<span data-ttu-id="473b7-104">L'accesso condizionale consente agli amministratori di configurare quando deve verificarsi o meno un evento.</span><span class="sxs-lookup"><span data-stu-id="473b7-104">Conditional access allows administrators to configure when they do or do not want something to happen.</span></span> <span data-ttu-id="473b7-105">È possibile usare più regole in parallelo per concedere o negare l'accesso alle risorse.</span><span class="sxs-lookup"><span data-stu-id="473b7-105">They can use multiple rules in parallel to grant or deny access to resources.</span></span> <span data-ttu-id="473b7-106">Ecco la regola che è necessario creare:</span><span class="sxs-lookup"><span data-stu-id="473b7-106">Here's the rule that we need to create:</span></span>

<span data-ttu-id="473b7-107">**Quando si accede al portale di Azure: Richiedi autenticazione a più fattori**</span><span class="sxs-lookup"><span data-stu-id="473b7-107">**When accessing the Azure portal - Require multi-factor authentication**</span></span>

<span data-ttu-id="473b7-108">I passaggi seguenti spiegano in maniera dettagliata come creare una regola di accesso condizionale per richiedere agli utenti di eseguire l'autenticazione a più fattori quando accedono al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="473b7-108">The steps that follow will walk you through the process to create a conditional access rule to require users to perform multi-factor authentication when they access the Azure portal.</span></span>

1. <span data-ttu-id="473b7-109">Passare ad **Azure Active Directory** > **Accesso condizionale**.</span><span class="sxs-lookup"><span data-stu-id="473b7-109">Browse to **Azure Active Directory** > **Conditional access**.</span></span>

1. <span data-ttu-id="473b7-110">Fare clic su **Nuovo criterio**.</span><span class="sxs-lookup"><span data-stu-id="473b7-110">Click **New policy**.</span></span>

1. <span data-ttu-id="473b7-111">Denominare il criterio **Richiedi autenticazione a più fattori per il portale di Azure**.</span><span class="sxs-lookup"><span data-stu-id="473b7-111">Name the policy **Require MFA for Azure portal**.</span></span>

1. <span data-ttu-id="473b7-112">In **Assegnazioni** > **Utenti e gruppi** selezionare **Utenti e gruppi**.</span><span class="sxs-lookup"><span data-stu-id="473b7-112">Under **Assignments** > **Users and groups**, select **Users and groups**.</span></span> <span data-ttu-id="473b7-113">Selezionare il gruppo appena creato **CA-MFA-AzurePortal**.</span><span class="sxs-lookup"><span data-stu-id="473b7-113">Select the group that we created **CA-MFA-AzurePortal**.</span></span> <span data-ttu-id="473b7-114">Fare clic su **Fatto**.</span><span class="sxs-lookup"><span data-stu-id="473b7-114">and click **Done**.</span></span>

1. <span data-ttu-id="473b7-115">In **App cloud** > **Selezionare le app**, selezionare **Microsoft Azure Management**.</span><span class="sxs-lookup"><span data-stu-id="473b7-115">Under **Cloud apps** > **Select apps**, select **Microsoft Azure Management**.</span></span>

1. <span data-ttu-id="473b7-116">In **Controlli di accesso** > **Concedi** selezionare **Richiedi autenticazione a più fattori**.</span><span class="sxs-lookup"><span data-stu-id="473b7-116">Under **Access controls** > **Grant**, select **Require multi-factor authentication**.</span></span>

1. <span data-ttu-id="473b7-117">Impostare **Abilita criterio** su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="473b7-117">Set **Enable policy** to **On**.</span></span>

1. <span data-ttu-id="473b7-118">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="473b7-118">Click **Create**.</span></span>

## <a name="summary"></a><span data-ttu-id="473b7-119">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="473b7-119">Summary</span></span>

<span data-ttu-id="473b7-120">In questa unità, è stato descritto come creare una regola di accesso condizionale.</span><span class="sxs-lookup"><span data-stu-id="473b7-120">In this unit, you learned how to create a conditional access rule.</span></span> <span data-ttu-id="473b7-121">La regola richiede l'autenticazione a più fattori per l'accesso al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="473b7-121">The rule requires Multi-Factor Authentication when accessing the Azure portal.</span></span>
