<span data-ttu-id="73251-101">Un collaboratore di First Up Consultants di nome Alain ha bisogno di creare e gestire macchine virtuali per un progetto a cui sta lavorando.</span><span class="sxs-lookup"><span data-stu-id="73251-101">A co-worker named Alain at First Up Consultants needs the ability to create and manage virtual machines for a project he is working on.</span></span> <span data-ttu-id="73251-102">Si è ricevuto dal proprio manager l'incarico di gestire questa richiesta.</span><span class="sxs-lookup"><span data-stu-id="73251-102">Your manager has asked that you handle this request.</span></span> <span data-ttu-id="73251-103">Seguendo la procedura consigliata secondo la quale occorre concedere agli utenti esclusivamente i privilegi che servono per svolgere il proprio lavoro, si decide di creare un gruppo di risorse e di assegnare ad Alain il ruolo Collaboratore Macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="73251-103">Using the best practice to grant users the least privileges to get their work done, you decide to create a new resource group and assign Alain the Virtual Machine Contributor role.</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="73251-104">Accedere al portale di Azure</span><span class="sxs-lookup"><span data-stu-id="73251-104">Sign in to the Azure portal</span></span>

- <span data-ttu-id="73251-105">Verificare di essere ancora connessi al portale di Azure come **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="73251-105">Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span></span> <span data-ttu-id="73251-106">Il nome utente e la password sono disponibili nella scheda **Risorse** nella parte superiore di questa finestra.</span><span class="sxs-lookup"><span data-stu-id="73251-106">You can find the username and password on the **Resources** tab at the top of this window.</span></span>

## <a name="grant-access"></a><span data-ttu-id="73251-107">Concedere l'accesso</span><span class="sxs-lookup"><span data-stu-id="73251-107">Grant access</span></span>

<span data-ttu-id="73251-108">Seguire questa procedura per assegnare il ruolo Collaboratore Macchina virtuale a un utente nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="73251-108">Follow these steps to assign the Virtual Machine Contributor role to a user at the resource group scope.</span></span>

1. <span data-ttu-id="73251-109">Nell'elenco di spostamento fare clic su **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="73251-109">In the navigation list, click **Resource groups**.</span></span>

1. <span data-ttu-id="73251-110">Trovare e selezionare il gruppo di risorse **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="73251-110">Find and click the **FirstUpConsultantsRG1-_XXXXXXX_** resource group.</span></span>

   ![Elenco dei gruppi di risorse](../media-draft/5-resource-groups.png)

1. <span data-ttu-id="73251-112">Fare clic su **Controllo di accesso (IAM)** per visualizzare l'elenco corrente delle assegnazioni di ruoli.</span><span class="sxs-lookup"><span data-stu-id="73251-112">Click **Access control (IAM)** to see the current list of role assignments.</span></span>

   ![Pannello Controllo di accesso (IAM) per il gruppo di risorse](../media-draft/5-resource-group-access-control.png)

1. <span data-ttu-id="73251-114">Nella parte superiore fare clic su **Aggiungi** per aprire il riquadro **Aggiungi autorizzazioni**.</span><span class="sxs-lookup"><span data-stu-id="73251-114">At the top, click **Add** to open the **Add permissions** pane.</span></span>

   ![Riquadro Aggiungi autorizzazioni](../media-draft/5-add-permissions.png)

1. <span data-ttu-id="73251-116">Nell'elenco a discesa **Ruolo** selezionare **Collaboratore Macchina virtuale**.</span><span class="sxs-lookup"><span data-stu-id="73251-116">In the **Role** drop-down list, select **Virtual Machine Contributor**.</span></span>

1. <span data-ttu-id="73251-117">Nell'elenco **Seleziona** selezionare **LabUser-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="73251-117">In the **Select** list, select **LabUser-_XXXXXXX_**.</span></span>

   ![Riquadro Aggiungi autorizzazioni completato](../media-draft/5-add-permissions-save.png)

1. <span data-ttu-id="73251-119">Fare clic su **Salva** per creare l'assegnazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="73251-119">Click **Save** to create the role assignment.</span></span>

   <span data-ttu-id="73251-120">Dopo alcuni istanti, all'utente **LabUser-_XXXXXXX_** viene assegnato il ruolo Collaboratore Macchina virtuale nell'ambito del gruppo di risorse **FirstUpConsultantsRG1-_XXXXXXX_**.</span><span class="sxs-lookup"><span data-stu-id="73251-120">After a few moments, the **LabUser-_XXXXXXX_** user is assigned the Virtual Machine Contributor role at the **FirstUpConsultantsRG1-_XXXXXXX_** resource group scope.</span></span> <span data-ttu-id="73251-121">L'utente può ora creare e gestire macchine virtuali solo all'interno di questo gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="73251-121">The user can now create and manage virtual machines just within this resource group.</span></span>

   ![Assegnazione del ruolo Collaboratore Macchina virtuale](../media-draft/5-vm-contributor-assignment.png)

## <a name="remove-access"></a><span data-ttu-id="73251-123">Rimuovere l'accesso</span><span class="sxs-lookup"><span data-stu-id="73251-123">Remove access</span></span>

<span data-ttu-id="73251-124">Per rimuovere un accesso mediante il controllo degli accessi in base al ruolo, si rimuove un'assegnazione di ruolo.</span><span class="sxs-lookup"><span data-stu-id="73251-124">In RBAC, to remove access, you remove a role assignment.</span></span>

1. <span data-ttu-id="73251-125">Nell'elenco delle assegnazioni di ruolo selezionare l'utente **LabUser-_XXXXXXX_** con il ruolo Collaboratore Macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="73251-125">In the list of role assignments, select the **LabUser-_XXXXXXX_** user with the Virtual Machine Contributor role.</span></span>

1. <span data-ttu-id="73251-126">Fare clic su **Rimuovi**.</span><span class="sxs-lookup"><span data-stu-id="73251-126">Click **Remove**.</span></span>

   ![Messaggio di rimozione assegnazione di ruolo](../media-draft/5-remove-role-assignment.png)

1. <span data-ttu-id="73251-128">Nella finestra di messaggio **Rimuovi assegnazioni di ruolo** fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="73251-128">In the **Remove role assignments** message that appears, click **Yes**.</span></span>

## <a name="summary"></a><span data-ttu-id="73251-129">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="73251-129">Summary</span></span>

<span data-ttu-id="73251-130">In questa unità si è appreso come concedere a un utente le autorizzazioni di accesso per creare e gestire macchine virtuali in un gruppo di risorse mediante il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="73251-130">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.</span></span> <span data-ttu-id="73251-131">Nell'unità successiva si imparerà a concedere l'accesso tramite PowerShell.</span><span class="sxs-lookup"><span data-stu-id="73251-131">In the next unit, you look at how to grant access using PowerShell.</span></span>
