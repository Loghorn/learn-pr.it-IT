> [!NOTE]
> Dopo l'avvio del lab, il nome utente e la password necessari sono disponibili nella scheda **Risorse** accanto alle istruzioni.

Presso la First Up Consultants, all'utente è stato concesso l'accesso a un gruppo di risorse per il team di marketing. Si desidera acquisire familiarità con il portale di Azure e visualizzare i ruoli attualmente assegnati.

## <a name="list-role-assignments-for-yourself"></a>Elencare i ruoli assegnati all’utente stesso

Seguire questa procedura per vedere quali sono i ruoli attualmente assegnati all'utente.

1. Nell'angolo superiore destro del portale di Azure fare clic sul nome utente per aprire il menu.

    ![Menu Autorizzazioni personali](../media/4-my-permissions-menu.png)

1. Fare clic su **Autorizzazioni personali** per aprire il riquadro Autorizzazioni personali.

    ![Riquadro Autorizzazioni personali](../media/4-my-permissions-pane.png)

    Nel riquadro Autorizzazioni personali l'utente può vedere l'elenco dei ruoli che gli sono stati assegnati e l'ambito. L'elenco avrà un aspetto diverso.

## <a name="list-role-assignments-for-a-resource-group"></a>Elencare le assegnazioni di ruolo per un gruppo di risorse

Seguire questa procedura per visualizzare quali ruoli vengono assegnati nell'ambito del gruppo di risorse.

1. Nell'elenco di spostamento fare clic su **Gruppi di risorse**.

   ![Gruppi di risorse](../media/4-resource-groups.png)

1. Individuare il gruppo di risorse denominato **FirstUpConsultantsRG1-_XXXXXXX_** e farci clic sopra.

1. Fare clic su **Controllo di accesso (IAM)**.

   ![Controllo di accesso per il gruppo di risorse](../media/4-resource-group-access-control.png)

    È possibile vedere chi ha accesso al gruppo di risorse. Si noterà che l'ambito di alcuni ruoli è **Questa risorsa**, mentre quello di altri è **(Ereditato)** da un altro ambito.

   ![Controllo di accesso - Assegnazione di ruolo per il gruppo di risorse](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a>Elencare i ruoli

Come illustrato nell'unità di precedente, un ruolo è una raccolta di autorizzazioni. Azure dispone di più di 70 ruoli predefiniti che è possibile usare nelle assegnazioni di ruolo. Seguire questi passaggi per elencare i ruoli.

- Nella parte superiore del riquadro fare clic su **Ruoli** per vedere l'elenco di tutti i ruoli predefiniti e personalizzati.

   È possibile visualizzare il numero di utenti e gruppi assegnati a ogni ruolo.

   ![Elenco dei ruoli](../media/4-roles-list.png)

In questa unità si è appreso come elencare le assegnazioni di ruolo per se stessi nel portale di Azure. Inoltre, si è appreso come elencare le assegnazioni di ruolo per un gruppo di risorse.