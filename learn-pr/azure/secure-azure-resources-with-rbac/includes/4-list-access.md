Presso la First Up Consultants, all'utente è stato concesso l'accesso alla sottoscrizione di Azure per il team di marketing. Si desidera acquisire familiarità con il portale di Azure e visualizzare i ruoli attualmente assegnati.

## <a name="list-role-assignments-for-yourself"></a>Elencare i ruoli assegnati all’utente stesso

Seguire questa procedura per vedere quali sono i ruoli attualmente assegnati all'utente.

1. Nell'elenco di spostamento fare clic su **Azure Active Directory**.

1. Fare clic su **Utenti** e aprire **Tutti gli utenti**.

    ![Utenti di Azure Active Directory](../media-draft/4-aad-all-users.png)

1. Trovare e selezionare il nome utente **LabAdmin -_XXXXXXX_**.

    ![Utenti lab di Azure Active Directory](../media-draft/4-aad-all-users-lab.png)

1. Nella sezione **Gestisci** fare clic su **Risorse di Azure**.

    ![Risorse di Azure](../media-draft/4-aad-user-azure-resources.png)

    Nel pannello delle risorse di Azure, è possibile visualizzare le risorse e ruoli a cui si ha accesso. L'elenco avrà un aspetto diverso.

## <a name="list-role-assignments-for-a-resource-group"></a>Elencare le assegnazioni di ruolo per un gruppo di risorse

Seguire questa procedura per visualizzare quali ruoli vengono assegnati nell'ambito del gruppo di risorse.

1. Nell'elenco di spostamento fare clic su **Gruppi di risorse**.

   ![Gruppi di risorse](../media-draft/4-resource-groups.png)

1. Fare clic sul gruppo di risorse denominato **FirstUpConsultantsRG1-_XXXXXXX_**.

1. Fare clic su **Controllo di accesso (IAM)**.

   Nel pannello Controllo di accesso (IAM) è possibile vedere chi ha accesso al gruppo di risorse. Si noterà che l'ambito di alcuni ruoli è **Questa risorsa**, mentre quello di altri è **(Ereditato)** da un altro ambito.

   ![Controllo di accesso (IAM) per il gruppo di risorse](../media-draft/4-resource-group-access-control.png)

## <a name="list-roles"></a>Elenco dei ruoli

Come illustrato nell'unità di precedente, un ruolo è una raccolta di autorizzazioni. Azure dispone di più di 70 ruoli predefiniti che è possibile usare nelle assegnazioni di ruolo. Per elencare i ruoli, eseguire questa procedura.

- Nella parte superiore del pannello Controllo di accesso (IAM), fare clic su **Ruoli** per visualizzare un elenco di tutti i ruoli predefiniti e personalizzati.

   ![Opzione Ruoli](../media-draft/4-roles-option.png)

   È possibile visualizzare il numero di utenti e gruppi assegnati a ogni ruolo.

   ![Elenco dei ruoli](../media-draft/4-roles-list.png)

## <a name="summary"></a>Riepilogo

In questa unità viene illustrato come elencare le assegnazioni di ruolo per se stessi nel portale di Azure. Inoltre, sarà illustrato come elencare le assegnazioni di ruolo in ambiti diversi nel portale. Nell'unità successiva, verrà illustrato il passaggio successivo e come usare RBAC (Controllo degli accessi in base al ruolo) per concedere l'accesso a un utente.
