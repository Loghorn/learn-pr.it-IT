L'uso del pannello **Controllo di accesso (IAM)**  nel portale di Azure funziona bene, ma ogni giorno si ricevono diverse richieste di autorizzazione. Per riuscire a gestire le attività di gestione degli accessi, si decide di usare PowerShell per automatizzare alcuni passaggi.

## <a name="open-cloud-shell-powershell"></a>Aprire PowerShell Cloud Shell

1. Verificare di essere ancora connessi al portale di Azure come **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**. Il nome utente e la password sono disponibili nella scheda **Risorse** nella parte superiore di questa finestra.

1. Per aprire il riquadro di Cloud Shell, fare clic su **Cloud Shell** nella parte superiore del portale.

    ![Pulsante Cloud Shell](../media-draft/6-cloud-shell-button.png)

1. Nell'angolo superiore sinistro del riquadro di Cloud Shell verificare che sia impostato su **PowerShell**. Se è impostato su **Bash**, modificarlo in **PowerShell**.

    Il caricamento potrebbe richiedere alcuni istanti. Al termine, sarà simile al seguente:

    ![PowerShell Cloud Shell](../media-draft/6-cloud-shell-powershell.png)

## <a name="grant-access"></a>Concedere l'accesso

Per concedere l'accesso a un utente con Azure PowerShell, si usa il comando [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment). È necessario specificare l'entità di sicurezza, la definizione del ruolo e l'ambito.

Seguire questa procedura per assegnare il ruolo Collaboratore Macchina virtuale a un utente **LabUser-_XXXXXXX_** nell'ambito del gruppo di risorse.

1. Nella scheda **Risorse** nella parte superiore di questa finestra copiare il comando **Grant access PowerShell** (Concedi l'accesso a PowerShell).

1. Incollare il comando nel riquadro di PowerShell e premere INVIO per eseguirlo. Il testo seguente è un esempio di comando con il relativo output:

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

    L'output mostra che il ruolo Collaboratore Macchina virtuale è stato assegnato all'utente LabUser-_XXXXXXX_ nell'ambito FirstUpConsultantsRG1-_XXXXXXX_.

## <a name="list-access"></a>Elencare l'accesso

Per verificare l'accesso al gruppo di risorse, usare il comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) per elencare le assegnazioni dei ruoli.

Seguire questa procedura per elencare le assegnazioni dei ruoli per l'utente **LabUser-XXXXXXX** nell'ambito del gruppo di risorse.

1. Nella scheda **Risorse** nella parte superiore di questa finestra copiare il comando **List access PowerShell** (Elenca gli accessi a PowerShell).

1. Incollare il comando nel riquadro di PowerShell e premere INVIO per eseguirlo. Il testo seguente è un esempio di comando con il relativo output.

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

    L'output mostra che il ruolo Collaboratore Macchina virtuale è stato assegnato all'utente LabUser-_XXXXXXX_ nell'ambito FirstUpConsultantsRG1-_XXXXXXX_.

    Se si aggiorna il pannello **Controllo di accesso (IAM)** per il gruppo di risorse nel portale di Azure, l'assegnazione del ruolo avrà un aspetto simile al seguente:

    ![Assegnazioni dei ruoli a un utente nell'ambito del gruppo di risorse](../media-draft/6-cloud-shell-access-control.png)

## <a name="remove-access"></a>Rimuovere l'accesso

Per rimuovere l'accesso per utenti, gruppi e applicazioni, usare [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) per rimuovere l'assegnazione di un ruolo.

Seguire questa procedura per rimuovere l'assegnazione del ruolo Collaboratore Macchina virtuale all'utente **LabUser-_XXXXXXX_** nell'ambito del gruppo di risorse.

1. Nella scheda **Risorse** nella parte superiore di questa finestra copiare il comando **Remove access PowerShell** (Rimuovi l'accesso a PowerShell).

1. Incollare il comando nel riquadro di PowerShell e premere INVIO per eseguirlo. Di seguito viene illustrato un esempio di comando.

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. Nel riquadro di PowerShell fare clic sul pulsante di chiusura (**X**) per chiudere il riquadro.

    ![Pulsante di chiusura di Cloud Shell](../media-draft/6-cloud-shell-close.png)


## <a name="summary"></a>Riepilogo

In questa unità si è appreso come concedere a un utente le autorizzazioni di accesso per creare e gestire macchine virtuali in un gruppo di risorse con Azure PowerShell. Nell'unità successiva si apprenderà come visualizzare le modifiche apportate nel tempo al controllo degli accessi in base al ruolo.
