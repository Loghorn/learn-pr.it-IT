Un collaboratore di First Up Consultants di nome Alain ha bisogno di creare e gestire macchine virtuali per un progetto a cui sta lavorando. Si è ricevuto dal proprio manager l'incarico di gestire questa richiesta. Seguendo la procedura consigliata secondo la quale occorre concedere agli utenti esclusivamente i privilegi che servono per svolgere il proprio lavoro, si decide di assegnare ad Alain il ruolo Collaboratore Macchina virtuale per un gruppo di risorse.

## <a name="grant-access"></a>Concedere l'accesso

Seguire questa procedura per assegnare il ruolo Collaboratore Macchina virtuale a un utente nell'ambito del gruppo di risorse.

1. Nell'elenco di spostamento fare clic su **Gruppi di risorse**.

1. Trovare e selezionare il gruppo di risorse **FirstUpConsultantsRG1-_XXXXXXX_**.

1. Fare clic su **Controllo di accesso (IAM)** per visualizzare l'elenco corrente delle assegnazioni di ruoli.

   ![Controllo di accesso - Assegnazione di ruolo per il gruppo di risorse](../media/5-resource-group-role-assignment.png)

1. Nella parte superiore fare clic su **Aggiungi** per aprire il riquadro **Aggiungi autorizzazioni**.

   ![Riquadro Aggiungi autorizzazioni](../media/5-add-permissions.png)

1. Nell'elenco a discesa **Ruolo** selezionare **Collaboratore Macchina virtuale**.

1. Nell'elenco **Seleziona** selezionare **LabUser-_XXXXXXX_**.

    Il nome utente è indicato nella scheda **Risorse** accanto alle istruzioni.

   ![Riquadro Aggiungi autorizzazioni completato](../media/5-add-permissions-save.png)

1. Fare clic su **Salva** per creare l'assegnazione di ruolo.

   Dopo alcuni istanti, all'utente **LabUser-_XXXXXXX_** viene assegnato il ruolo Collaboratore Macchina virtuale nell'ambito del gruppo di risorse **FirstUpConsultantsRG1-_XXXXXXX_**. L'utente può ora creare e gestire macchine virtuali solo all'interno di questo gruppo di risorse.

   ![Assegnazione del ruolo Collaboratore Macchina virtuale](../media/5-vm-contributor-assignment.png)

## <a name="remove-access"></a>Rimuovere l'accesso

Per rimuovere un accesso mediante il controllo degli accessi in base al ruolo, si rimuove un'assegnazione di ruolo.

1. Nell'elenco delle assegnazioni di ruolo selezionare l'utente **LabUser-_XXXXXXX_** con il ruolo Collaboratore Macchina virtuale.

1. Fare clic su **Rimuovi**.

   ![Messaggio di rimozione assegnazione di ruolo](../media/5-remove-role-assignment.png)

1. Nella finestra di messaggio **Rimuovi assegnazioni di ruolo** fare clic su **Sì**.

In questa unità si è appreso come concedere a un utente le autorizzazioni di accesso per creare e gestire macchine virtuali in un gruppo di risorse mediante il portale di Azure.