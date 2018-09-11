<span data-ttu-id="810a6-101">L'uso del pannello **Controllo di accesso (IAM)**  nel portale di Azure funziona bene, ma ogni giorno si ricevono diverse richieste di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="810a6-101">Using the **Access control (IAM)** blade in the Azure portal has been working fine, but you are getting several permission requests each day.</span></span> <span data-ttu-id="810a6-102">Per riuscire a gestire le attività di gestione degli accessi, si decide di usare PowerShell per automatizzare alcuni passaggi.</span><span class="sxs-lookup"><span data-stu-id="810a6-102">To keep up with the access management tasks, you decide to use PowerShell to help automate some of the steps.</span></span>

## <a name="open-cloud-shell-powershell"></a><span data-ttu-id="810a6-103">Aprire PowerShell Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="810a6-103">Open Cloud Shell PowerShell</span></span>

1. <span data-ttu-id="810a6-104">Verificare di essere ancora connessi al portale di Azure come **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="810a6-104">Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span></span> <span data-ttu-id="810a6-105">Il nome utente e la password sono disponibili nella scheda **Risorse** nella parte superiore di questa finestra.</span><span class="sxs-lookup"><span data-stu-id="810a6-105">You can find the username and password on the **Resources** tab at the top of this window.</span></span>

1. <span data-ttu-id="810a6-106">Per aprire il riquadro di Cloud Shell, fare clic su **Cloud Shell** nella parte superiore del portale.</span><span class="sxs-lookup"><span data-stu-id="810a6-106">At the top of the portal, click **Cloud Shell** to open the Cloud Shell pane.</span></span>

    ![Pulsante Cloud Shell](../media-draft/6-cloud-shell-button.png)

1. <span data-ttu-id="810a6-108">Nell'angolo superiore sinistro del riquadro di Cloud Shell verificare che sia impostato su **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="810a6-108">In the upper left of the Cloud Shell pane, make sure it is set to **PowerShell**.</span></span> <span data-ttu-id="810a6-109">Se è impostato su **Bash**, modificarlo in **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="810a6-109">If it is set to **Bash**, change it to **PowerShell**.</span></span>

    <span data-ttu-id="810a6-110">Il caricamento potrebbe richiedere alcuni istanti.</span><span class="sxs-lookup"><span data-stu-id="810a6-110">It might take a few moments to load.</span></span> <span data-ttu-id="810a6-111">Al termine, sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="810a6-111">When finished, it will look similar to the following:</span></span>

    ![PowerShell Cloud Shell](../media-draft/6-cloud-shell-powershell.png)

## <a name="grant-access"></a><span data-ttu-id="810a6-113">Concedere l'accesso</span><span class="sxs-lookup"><span data-stu-id="810a6-113">Grant access</span></span>

<span data-ttu-id="810a6-114">Per concedere l'accesso a un utente con Azure PowerShell, si usa il comando [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment).</span><span class="sxs-lookup"><span data-stu-id="810a6-114">To grant access to a user using Azure PowerShell, you use the [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) command.</span></span> <span data-ttu-id="810a6-115">È necessario specificare l'entità di sicurezza, la definizione del ruolo e l'ambito.</span><span class="sxs-lookup"><span data-stu-id="810a6-115">You must specify the security principal, role definition, and scope.</span></span>

<span data-ttu-id="810a6-116">Seguire questa procedura per assegnare il ruolo Collaboratore Macchina virtuale a un utente **LabUser-_XXXXXXX_** nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="810a6-116">Follow these steps to assign the Virtual Machine Contributor role to the **LabUser-_XXXXXXX_** user at the resource group scope.</span></span>

1. <span data-ttu-id="810a6-117">Nella scheda **Risorse** nella parte superiore di questa finestra copiare il comando **Grant access PowerShell** (Concedi l'accesso a PowerShell).</span><span class="sxs-lookup"><span data-stu-id="810a6-117">On the **Resources** tab at the top of this window, copy the **Grant access PowerShell** command.</span></span>

1. <span data-ttu-id="810a6-118">Incollare il comando nel riquadro di PowerShell e premere INVIO per eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="810a6-118">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="810a6-119">Il testo seguente è un esempio di comando con il relativo output:</span><span class="sxs-lookup"><span data-stu-id="810a6-119">The following shows an example command and the output:</span></span>

    ```Example
    PS Azure:\> New-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    <span data-ttu-id="810a6-120">L'output mostra che il ruolo Collaboratore Macchina virtuale è stato assegnato all'utente LabUser-_XXXXXXX_ nell'ambito FirstUpConsultantsRG1-_XXXXXXX_.</span><span class="sxs-lookup"><span data-stu-id="810a6-120">The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.</span></span>

## <a name="list-access"></a><span data-ttu-id="810a6-121">Elencare l'accesso</span><span class="sxs-lookup"><span data-stu-id="810a6-121">List access</span></span>

<span data-ttu-id="810a6-122">Per verificare l'accesso al gruppo di risorse, usare il comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) per elencare le assegnazioni dei ruoli.</span><span class="sxs-lookup"><span data-stu-id="810a6-122">To verify the access for the resource group, you use the [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) command to list the role assignments.</span></span>

<span data-ttu-id="810a6-123">Seguire questa procedura per elencare le assegnazioni dei ruoli per l'utente **LabUser-XXXXXXX** nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="810a6-123">Follow these steps to list all the role assignments for the **LabUser-XXXXXXX** user at the resource group scope.</span></span>

1. <span data-ttu-id="810a6-124">Nella scheda **Risorse** nella parte superiore di questa finestra copiare il comando **List access PowerShell** (Elenca gli accessi a PowerShell).</span><span class="sxs-lookup"><span data-stu-id="810a6-124">On the **Resources** tab at the top of this window, copy the **List access PowerShell** command.</span></span>

1. <span data-ttu-id="810a6-125">Incollare il comando nel riquadro di PowerShell e premere INVIO per eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="810a6-125">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="810a6-126">Il testo seguente è un esempio di comando con il relativo output.</span><span class="sxs-lookup"><span data-stu-id="810a6-126">The following shows an example command and the output.</span></span>

    ```Example
    PS Azure:\> Get-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com 
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    <span data-ttu-id="810a6-127">L'output mostra che il ruolo Collaboratore Macchina virtuale è stato assegnato all'utente LabUser-_XXXXXXX_ nell'ambito FirstUpConsultantsRG1-_XXXXXXX_.</span><span class="sxs-lookup"><span data-stu-id="810a6-127">The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.</span></span>

    <span data-ttu-id="810a6-128">Se si aggiorna il pannello **Controllo di accesso (IAM)** per il gruppo di risorse nel portale di Azure, l'assegnazione del ruolo avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="810a6-128">If you refresh the **Access control (IAM)** blade for the resource group in the Azure portal, this is how the role assignment looks:</span></span>

    ![Assegnazioni dei ruoli a un utente nell'ambito del gruppo di risorse](../media-draft/6-cloud-shell-access-control.png)

## <a name="remove-access"></a><span data-ttu-id="810a6-130">Rimuovere l'accesso</span><span class="sxs-lookup"><span data-stu-id="810a6-130">Remove access</span></span>

<span data-ttu-id="810a6-131">Per rimuovere l'accesso per utenti, gruppi e applicazioni, usare [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) per rimuovere l'assegnazione di un ruolo.</span><span class="sxs-lookup"><span data-stu-id="810a6-131">To remove access for users, groups, and applications, you use [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) to remove a role assignment.</span></span>

<span data-ttu-id="810a6-132">Seguire questa procedura per rimuovere l'assegnazione del ruolo Collaboratore Macchina virtuale all'utente **LabUser-_XXXXXXX_** nell'ambito del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="810a6-132">Follow these steps to remove the Virtual Machine Contributor role assignment for the **LabUser-_XXXXXX_** user at the resource group scope.</span></span>

1. <span data-ttu-id="810a6-133">Nella scheda **Risorse** nella parte superiore di questa finestra copiare il comando **Remove access PowerShell** (Rimuovi l'accesso a PowerShell).</span><span class="sxs-lookup"><span data-stu-id="810a6-133">On the **Resources** tab at the top of this window, copy the **Remove access PowerShell** command.</span></span>

1. <span data-ttu-id="810a6-134">Incollare il comando nel riquadro di PowerShell e premere INVIO per eseguirlo.</span><span class="sxs-lookup"><span data-stu-id="810a6-134">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="810a6-135">Di seguito viene illustrato un esempio di comando.</span><span class="sxs-lookup"><span data-stu-id="810a6-135">The following shows an example command.</span></span>

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. <span data-ttu-id="810a6-136">Nel riquadro di PowerShell fare clic sul pulsante di chiusura (**X**) per chiudere il riquadro.</span><span class="sxs-lookup"><span data-stu-id="810a6-136">In the PowerShell pane, click the close (**X**) button to close the pane.</span></span>

    ![Pulsante di chiusura di Cloud Shell](../media-draft/6-cloud-shell-close.png)


## <a name="summary"></a><span data-ttu-id="810a6-138">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="810a6-138">Summary</span></span>

<span data-ttu-id="810a6-139">In questa unità si è appreso come concedere a un utente le autorizzazioni di accesso per creare e gestire macchine virtuali in un gruppo di risorse con Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="810a6-139">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell.</span></span> <span data-ttu-id="810a6-140">Nell'unità successiva si apprenderà come visualizzare le modifiche apportate nel tempo al controllo degli accessi in base al ruolo.</span><span class="sxs-lookup"><span data-stu-id="810a6-140">In the next unit, you learn how to view the RBAC changes over time.</span></span>
