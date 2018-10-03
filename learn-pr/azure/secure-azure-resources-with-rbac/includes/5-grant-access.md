<span data-ttu-id="e6905-101">Un collaboratore di First Up Consultants di nome Alain ha bisogno di creare e gestire macchine virtuali per un progetto a cui sta lavorando.</span><span class="sxs-lookup"><span data-stu-id="e6905-101">A co-worker named Alain at First Up Consultants needs the ability to create and manage virtual machines for a project he is working on.</span></span> <span data-ttu-id="e6905-102">Si è ricevuto dal proprio manager l'incarico di gestire questa richiesta.</span><span class="sxs-lookup"><span data-stu-id="e6905-102">Your manager has asked that you handle this request.</span></span> <span data-ttu-id="e6905-103">Seguendo la procedura consigliata secondo la quale occorre concedere agli utenti esclusivamente i privilegi che servono per svolgere il proprio lavoro, si decide di assegnare ad Alain il ruolo Collaboratore Macchina virtuale per un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e6905-103">Using the best practice to grant users the least privileges to get their work done, you decide to assign Alain the Virtual Machine Contributor role for a resource group.</span></span>

## <a name="grant-access"></a><span data-ttu-id="e6905-104">Concedere l'accesso</span><span class="sxs-lookup"><span data-stu-id="e6905-104">Grant access</span></span>

<span data-ttu-id="e6905-105">Seguire questa procedura per assegnare il ruolo Collaboratore Macchina virtuale a un utente nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e6905-105">Follow these steps to assign the Virtual Machine Contributor role to a user at the resource group scope.</span></span>

1. <span data-ttu-id="e6905-106">Nell'elenco di spostamento fare clic su **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="e6905-106">In the navigation list, click **Resource groups**.</span></span>

1. <span data-ttu-id="e6905-107">Trovare e selezionare il gruppo di risorse **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="e6905-107">Find and click the **FirstUpConsultantsRG1-_XXXXXXX_** resource group.</span></span>

1. <span data-ttu-id="e6905-108">Fare clic su **Controllo di accesso (IAM)** per visualizzare l'elenco corrente delle assegnazioni di ruoli.</span><span class="sxs-lookup"><span data-stu-id="e6905-108">Click **Access control (IAM)** to see the current list of role assignments.</span></span>

   ![Controllo di accesso - Assegnazione di ruolo per il gruppo di risorse](../media/5-resource-group-role-assignment.png)

1. <span data-ttu-id="e6905-110">Nella parte superiore fare clic su **Aggiungi** per aprire il riquadro **Aggiungi autorizzazioni**.</span><span class="sxs-lookup"><span data-stu-id="e6905-110">At the top, click **Add** to open the **Add permissions** pane.</span></span>

   ![Riquadro Aggiungi autorizzazioni](../media/5-add-permissions.png)

1. <span data-ttu-id="e6905-112">Nell'elenco a discesa **Ruolo** selezionare **Collaboratore Macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="e6905-112">In the **Role** drop-down list, select **Virtual Machine Contributor**.</span></span>

1. <span data-ttu-id="e6905-113">Nell'elenco **Seleziona** selezionare **LabUser-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="e6905-113">In the **Select** list, select **LabUser-_XXXXXXX_**.</span></span>

    <span data-ttu-id="e6905-114">Il nome utente è indicato nella scheda **Risorse** accanto alle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e6905-114">You can find the username on the **Resources** tab next to the instructions.</span></span>

   ![Riquadro Aggiungi autorizzazioni completato](../media/5-add-permissions-save.png)

1. <span data-ttu-id="e6905-116">Fare clic su **Salva** per creare l'assegnazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="e6905-116">Click **Save** to create the role assignment.</span></span>

   <span data-ttu-id="e6905-117">Dopo alcuni istanti, all'utente **LabUser-_XXXXXXX_** viene assegnato il ruolo Collaboratore Macchina virtuale nell'ambito del gruppo di risorse **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="e6905-117">After a few moments, the **LabUser-_XXXXXXX_** user is assigned the Virtual Machine Contributor role at the **FirstUpConsultantsRG1-_XXXXXXX_** resource group scope.</span></span> <span data-ttu-id="e6905-118">L'utente può ora creare e gestire macchine virtuali solo all'interno di questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e6905-118">The user can now create and manage virtual machines just within this resource group.</span></span>

   ![Assegnazione del ruolo Collaboratore Macchina virtuale](../media/5-vm-contributor-assignment.png)

## <a name="remove-access"></a><span data-ttu-id="e6905-120">Rimuovere l'accesso</span><span class="sxs-lookup"><span data-stu-id="e6905-120">Remove access</span></span>

<span data-ttu-id="e6905-121">Per rimuovere un accesso mediante il controllo degli accessi in base al ruolo, si rimuove un'assegnazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="e6905-121">In RBAC, to remove access, you remove a role assignment.</span></span>

1. <span data-ttu-id="e6905-122">Nell'elenco delle assegnazioni di ruolo selezionare l'utente **LabUser-_XXXXXXX_** con il ruolo Collaboratore Macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="e6905-122">In the list of role assignments, select the **LabUser-_XXXXXXX_** user with the Virtual Machine Contributor role.</span></span>

1. <span data-ttu-id="e6905-123">Fare clic su **Rimuovi**.</span><span class="sxs-lookup"><span data-stu-id="e6905-123">Click **Remove**.</span></span>

   ![Messaggio di rimozione assegnazione di ruolo](../media/5-remove-role-assignment.png)

1. <span data-ttu-id="e6905-125">Nella finestra di messaggio **Rimuovi assegnazioni di ruolo** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="e6905-125">In the **Remove role assignments** message that appears, click **Yes**.</span></span>

<span data-ttu-id="e6905-126">In questa unità si è appreso come concedere a un utente le autorizzazioni di accesso per creare e gestire macchine virtuali in un gruppo di risorse mediante il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e6905-126">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.</span></span>