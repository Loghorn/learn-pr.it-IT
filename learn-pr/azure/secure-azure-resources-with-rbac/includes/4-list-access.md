Al primo backup ai consulenti IT, è stato concesso l'accesso a un gruppo di risorse per il team di marketing. Si desidera acquisire familiarità con il portale di Azure e visualizzare i ruoli attualmente assegnati.

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a>Avvia laboratorio e accedere al portale di Azure

1. Fare clic sul collegamento precedente per avviare il lab.

1. Accedere al portale di Azure come **LabAdmin -_XXXXXXX_@_xxxxxxxxxxxx_. onmicrosoft.com**. Il nome utente e la password sono disponibili nella scheda **Risorse** nella parte superiore di questa finestra.

## <a name="list-role-assignments-for-yourself"></a>Elencare i ruoli assegnati all’utente stesso

Seguire questa procedura per vedere quali sono i ruoli attualmente assegnati all'utente.

1. Nell'angolo superiore destro del portale di Azure, fare clic sul nome utente per aprire il menu di scelta.

    ![Il menu di scelta delle autorizzazioni](../media/4-my-permissions-menu.png)

1. Fare clic su **autorizzazioni personali** per aprire il riquadro di lavoro delle autorizzazioni.

    ![Il riquadro delle autorizzazioni](../media/4-my-permissions-pane.png)

    Nel mio riquadro autorizzazioni, è possibile visualizzare un elenco dei ruoli di cui è stata assegnata e l'ambito. L'elenco avrà un aspetto diverso.

## <a name="list-role-assignments-for-a-resource-group"></a>Elencare le assegnazioni di ruolo per un gruppo di risorse

Seguire questa procedura per visualizzare quali ruoli vengono assegnati nell'ambito del gruppo di risorse.

1. Nell'elenco di spostamento fare clic su **Gruppi di risorse**.

   ![Gruppi di risorse](../media/4-resource-groups.png)

1. Trovare e fare clic sul gruppo di risorse denominato **FirstUpConsultantsRG1 -_XXXXXXX_**.

1. Fare clic su **Controllo di accesso (IAM)**.

   ![Controllo di accesso per il gruppo di risorse](../media/4-resource-group-access-control.png)

1. Fare clic su **assegnazione di ruolo**.

    È possibile visualizzare chi ha accesso al gruppo di risorse. Si noterà che l'ambito di alcuni ruoli è **Questa risorsa**, mentre quello di altri è **(Ereditato)** da un altro ambito.

   ![Controllo di accesso - assegnazione di ruolo per il gruppo di risorse](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a>Elenco dei ruoli

Come illustrato nell'unità di precedente, un ruolo è una raccolta di autorizzazioni. Azure dispone di più di 70 ruoli predefiniti che è possibile usare nelle assegnazioni di ruolo. Seguire questo passaggio per elencare i ruoli.

- In assegnazione di ruolo, fare clic su **ruoli** per visualizzare un elenco di tutti i ruoli predefiniti e personalizzati.

   È possibile visualizzare il numero di utenti e gruppi assegnati a ogni ruolo.

   ![Elenco dei ruoli](../media/4-roles-list.png)

## <a name="summary"></a>Riepilogo

In questa unità viene illustrato come elencare le assegnazioni di ruolo per se stessi nel portale di Azure. Inoltre appreso come elencare le assegnazioni di ruolo per un gruppo di risorse. Nell'unità successiva, verrà illustrato il passaggio successivo e come usare RBAC (Controllo degli accessi in base al ruolo) per concedere l'accesso a un utente.
