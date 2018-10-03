> [!NOTE]
> <span data-ttu-id="e8418-101">Dopo l'avvio del lab, il nome utente e la password necessari sono disponibili nella scheda **Risorse** accanto alle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e8418-101">After launching the lab, the username and password you need is located on the **Resources** tab next to the instructions.</span></span>

<span data-ttu-id="e8418-102">Presso la First Up Consultants, all'utente è stato concesso l'accesso a un gruppo di risorse per il team di marketing.</span><span class="sxs-lookup"><span data-stu-id="e8418-102">At First Up Consultants, you've been granted access to a resource group for the marketing team.</span></span> <span data-ttu-id="e8418-103">Si desidera acquisire familiarità con il portale di Azure e visualizzare i ruoli attualmente assegnati.</span><span class="sxs-lookup"><span data-stu-id="e8418-103">You want to familiarize yourself with the Azure portal and see what roles are currently assigned.</span></span>

## <a name="list-role-assignments-for-yourself"></a><span data-ttu-id="e8418-104">Elencare i ruoli assegnati all’utente stesso</span><span class="sxs-lookup"><span data-stu-id="e8418-104">List role assignments for yourself</span></span>

<span data-ttu-id="e8418-105">Seguire questa procedura per vedere quali sono i ruoli attualmente assegnati all'utente.</span><span class="sxs-lookup"><span data-stu-id="e8418-105">Follow these steps to see what roles are currently assigned to you.</span></span>

1. <span data-ttu-id="e8418-106">Nell'angolo superiore destro del portale di Azure fare clic sul nome utente per aprire il menu.</span><span class="sxs-lookup"><span data-stu-id="e8418-106">In the upper-right corner of the Azure portal, click your user name to open the menu.</span></span>

    ![Menu Autorizzazioni personali](../media/4-my-permissions-menu.png)

1. <span data-ttu-id="e8418-108">Fare clic su **Autorizzazioni personali** per aprire il riquadro Autorizzazioni personali.</span><span class="sxs-lookup"><span data-stu-id="e8418-108">Click **My permissions** to open the My permissions pane.</span></span>

    ![Riquadro Autorizzazioni personali](../media/4-my-permissions-pane.png)

    <span data-ttu-id="e8418-110">Nel riquadro Autorizzazioni personali l'utente può vedere l'elenco dei ruoli che gli sono stati assegnati e l'ambito.</span><span class="sxs-lookup"><span data-stu-id="e8418-110">On the My permissions pane, you can see a list of the roles that you have been assigned and the scope.</span></span> <span data-ttu-id="e8418-111">L'elenco avrà un aspetto diverso.</span><span class="sxs-lookup"><span data-stu-id="e8418-111">Your list will look different.</span></span>

## <a name="list-role-assignments-for-a-resource-group"></a><span data-ttu-id="e8418-112">Elencare le assegnazioni di ruolo per un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="e8418-112">List role assignments for a resource group</span></span>

<span data-ttu-id="e8418-113">Seguire questa procedura per visualizzare quali ruoli vengono assegnati nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e8418-113">Follow these steps to see what roles are assigned at the resource group scope.</span></span>

1. <span data-ttu-id="e8418-114">Nell'elenco di spostamento fare clic su **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="e8418-114">In the navigation list, click **Resource groups**.</span></span>

   ![Gruppi di risorse](../media/4-resource-groups.png)

1. <span data-ttu-id="e8418-116">Individuare il gruppo di risorse denominato **FirstUpConsultantsRG1-_XXXXXXX_** e farci clic sopra.</span><span class="sxs-lookup"><span data-stu-id="e8418-116">Find and click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.</span></span>

1. <span data-ttu-id="e8418-117">Fare clic su **Controllo di accesso (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="e8418-117">Click **Access control (IAM)**.</span></span>

   ![Controllo di accesso per il gruppo di risorse](../media/4-resource-group-access-control.png)

    <span data-ttu-id="e8418-119">È possibile vedere chi ha accesso al gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e8418-119">You can see who has access to this resource group.</span></span> <span data-ttu-id="e8418-120">Si noterà che l'ambito di alcuni ruoli è **Questa risorsa**, mentre quello di altri è **(Ereditato)** da un altro ambito.</span><span class="sxs-lookup"><span data-stu-id="e8418-120">Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.</span></span>

   ![Controllo di accesso - Assegnazione di ruolo per il gruppo di risorse](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a><span data-ttu-id="e8418-122">Elencare i ruoli</span><span class="sxs-lookup"><span data-stu-id="e8418-122">List roles</span></span>

<span data-ttu-id="e8418-123">Come illustrato nell'unità di precedente, un ruolo è una raccolta di autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="e8418-123">As you learned in the previous unit, a role is a collection of permissions.</span></span> <span data-ttu-id="e8418-124">Azure dispone di più di 70 ruoli predefiniti che è possibile usare nelle assegnazioni di ruolo.</span><span class="sxs-lookup"><span data-stu-id="e8418-124">Azure has over 70 built-in roles that you can use in your role assignments.</span></span> <span data-ttu-id="e8418-125">Seguire questi passaggi per elencare i ruoli.</span><span class="sxs-lookup"><span data-stu-id="e8418-125">Follow this step to list the roles.</span></span>

- <span data-ttu-id="e8418-126">Nella parte superiore del riquadro fare clic su **Ruoli** per vedere l'elenco di tutti i ruoli predefiniti e personalizzati.</span><span class="sxs-lookup"><span data-stu-id="e8418-126">At the top of the pane, click **Roles** to see a list of all the built-in and custom roles.</span></span>

   <span data-ttu-id="e8418-127">È possibile visualizzare il numero di utenti e gruppi assegnati a ogni ruolo.</span><span class="sxs-lookup"><span data-stu-id="e8418-127">You can see the number of users and groups that are assigned to each role.</span></span>

   ![Elenco dei ruoli](../media/4-roles-list.png)

<span data-ttu-id="e8418-129">In questa unità si è appreso come elencare le assegnazioni di ruolo per se stessi nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e8418-129">In this unit, you learned how to list the role assignments for yourself in the Azure portal.</span></span> <span data-ttu-id="e8418-130">Inoltre, si è appreso come elencare le assegnazioni di ruolo per un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e8418-130">You also learned how to list the role assignments for a resource group.</span></span>