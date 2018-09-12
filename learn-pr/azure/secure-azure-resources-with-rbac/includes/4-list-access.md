<span data-ttu-id="63029-101">Presso la First Up Consultants, all'utente è stato concesso l'accesso alla sottoscrizione di Azure per il team di marketing.</span><span class="sxs-lookup"><span data-stu-id="63029-101">At First Up Consultants, you've been granted access to the Azure subscription for the marketing team.</span></span> <span data-ttu-id="63029-102">Si desidera acquisire familiarità con il portale di Azure e visualizzare i ruoli attualmente assegnati.</span><span class="sxs-lookup"><span data-stu-id="63029-102">You want to familiarize yourself with the Azure portal and see what roles are currently assigned.</span></span>

## <a name="list-role-assignments-for-yourself"></a><span data-ttu-id="63029-103">Elencare i ruoli assegnati all’utente stesso</span><span class="sxs-lookup"><span data-stu-id="63029-103">List role assignments for yourself</span></span>

<span data-ttu-id="63029-104">Seguire questa procedura per vedere quali sono i ruoli attualmente assegnati all'utente.</span><span class="sxs-lookup"><span data-stu-id="63029-104">Follow these steps to see what roles are currently assigned to you.</span></span>

1. <span data-ttu-id="63029-105">Nell'elenco di spostamento fare clic su **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="63029-105">In the navigation list, click **Azure Active Directory**.</span></span>

1. <span data-ttu-id="63029-106">Fare clic su **Utenti** e aprire **Tutti gli utenti**.</span><span class="sxs-lookup"><span data-stu-id="63029-106">Click **Users** to open **All users**.</span></span>

    ![Utenti di Azure Active Directory](../media-draft/4-aad-all-users.png)

1. <span data-ttu-id="63029-108">Trovare e selezionare il nome utente **LabAdmin -_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="63029-108">Find and click the **LabAdmin-_XXXXXXX_** user name.</span></span>

    ![Utenti lab di Azure Active Directory](../media-draft/4-aad-all-users-lab.png)

1. <span data-ttu-id="63029-110">Nella sezione **Gestisci** fare clic su **Risorse di Azure**.</span><span class="sxs-lookup"><span data-stu-id="63029-110">In the **Manage** section, click **Azure resources**.</span></span>

    ![Risorse di Azure](../media-draft/4-aad-user-azure-resources.png)

    <span data-ttu-id="63029-112">Nel pannello delle risorse di Azure, è possibile visualizzare le risorse e ruoli a cui si ha accesso.</span><span class="sxs-lookup"><span data-stu-id="63029-112">On the Azure resources blade, you can see the resources and roles you have access to.</span></span> <span data-ttu-id="63029-113">L'elenco avrà un aspetto diverso.</span><span class="sxs-lookup"><span data-stu-id="63029-113">Your list will look different.</span></span>

## <a name="list-role-assignments-for-a-resource-group"></a><span data-ttu-id="63029-114">Elencare le assegnazioni di ruolo per un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="63029-114">List role assignments for a resource group</span></span>

<span data-ttu-id="63029-115">Seguire questa procedura per visualizzare quali ruoli vengono assegnati nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="63029-115">Follow these steps to see what roles are assigned at the resource group scope.</span></span>

1. <span data-ttu-id="63029-116">Nell'elenco di spostamento fare clic su **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="63029-116">In the navigation list, click **Resource groups**.</span></span>

   ![Gruppi di risorse](../media-draft/4-resource-groups.png)

1. <span data-ttu-id="63029-118">Fare clic sul gruppo di risorse denominato **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="63029-118">Click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.</span></span>

1. <span data-ttu-id="63029-119">Fare clic su **Controllo di accesso (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="63029-119">Click **Access control (IAM)**.</span></span>

   <span data-ttu-id="63029-120">Nel pannello Controllo di accesso (IAM) è possibile vedere chi ha accesso al gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="63029-120">On the Access control (IAM) blade, you can see who has access to this resource group.</span></span> <span data-ttu-id="63029-121">Si noterà che l'ambito di alcuni ruoli è **Questa risorsa**, mentre quello di altri è **(Ereditato)** da un altro ambito.</span><span class="sxs-lookup"><span data-stu-id="63029-121">Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.</span></span>

   ![Controllo di accesso (IAM) per il gruppo di risorse](../media-draft/4-resource-group-access-control.png)

## <a name="list-roles"></a><span data-ttu-id="63029-123">Elenco dei ruoli</span><span class="sxs-lookup"><span data-stu-id="63029-123">List roles</span></span>

<span data-ttu-id="63029-124">Come illustrato nell'unità di precedente, un ruolo è una raccolta di autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="63029-124">As you learned in the previous unit, a role is a collection of permissions.</span></span> <span data-ttu-id="63029-125">Azure dispone di più di 70 ruoli predefiniti che è possibile usare nelle assegnazioni di ruolo.</span><span class="sxs-lookup"><span data-stu-id="63029-125">Azure has over 70 built-in roles that you can use in your role assignments.</span></span> <span data-ttu-id="63029-126">Per elencare i ruoli, eseguire questa procedura.</span><span class="sxs-lookup"><span data-stu-id="63029-126">Follow these steps to list the roles.</span></span>

- <span data-ttu-id="63029-127">Nella parte superiore del pannello Controllo di accesso (IAM), fare clic su **Ruoli** per visualizzare un elenco di tutti i ruoli predefiniti e personalizzati.</span><span class="sxs-lookup"><span data-stu-id="63029-127">At the top of the Access control (IAM) blade, click **Roles** to see a list of all the built-in and custom roles.</span></span>

   ![Opzione Ruoli](../media-draft/4-roles-option.png)

   <span data-ttu-id="63029-129">È possibile visualizzare il numero di utenti e gruppi assegnati a ogni ruolo.</span><span class="sxs-lookup"><span data-stu-id="63029-129">You can see the number of users and groups that are assigned to each role.</span></span>

   ![Elenco dei ruoli](../media-draft/4-roles-list.png)

## <a name="summary"></a><span data-ttu-id="63029-131">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="63029-131">Summary</span></span>

<span data-ttu-id="63029-132">In questa unità viene illustrato come elencare le assegnazioni di ruolo per se stessi nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="63029-132">In this unit, you learned how to list the role assignments for yourself in the Azure portal.</span></span> <span data-ttu-id="63029-133">Inoltre, sarà illustrato come elencare le assegnazioni di ruolo in ambiti diversi nel portale.</span><span class="sxs-lookup"><span data-stu-id="63029-133">You also learned how to list the role assignments at different scopes.</span></span> <span data-ttu-id="63029-134">Nell'unità successiva, verrà illustrato il passaggio successivo e come usare RBAC (Controllo degli accessi in base al ruolo) per concedere l'accesso a un utente.</span><span class="sxs-lookup"><span data-stu-id="63029-134">In the next unit, you take the next step and use RBAC to grant access to a user.</span></span>
