First Up Consultants esamina le modifiche trimestrali al controllo degli accessi in base al ruolo (RBAC) per scopi di controllo e risoluzione dei problemi. È noto che le modifiche vengono registrate nel [Log attività di Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Il gestore ha chiesto se è possibile generare un report delle assegnazione di ruolo e delle modifiche di ruolo personalizzate per il mese scorso.

## <a name="view-activity-logs"></a>Visualizzare i log attività

Il modo più semplice per iniziare è visualizzare i log attività con il portale di Azure.

1. Fare clic su **Tutti i servizi** e quindi trovare **Log attività**.

    ![Log attività tramite il portale](../media-draft/7-all-services-activity-log.png)

1. Fare clic su **Log attività**.

    ![Log attività tramite il portale](../media-draft/7-activity-log-portal.png)

1. Impostare il **Timespan** e filtrare in base a **Mese scorso**.

1. Impostare la **Categoria di eventi** e filtrare in base a **Amministrativo**.

1. Nel filtro **Operazione**, digitare **Ruolo** per filtrare l'elenco.

1. Selezionare le operazioni di RBAC seguenti:

    - Creare assegnazione del ruolo (roleAssignments)
    - Eliminare assegnazione del ruolo (roleAssignments)
    - Creare o aggiornare la definizione del ruolo personalizzata (roleDefinitions)
    - Eliminare la definizione del ruolo personalizzata (roleDefinitions)

    ![Filtro di operazione](../media-draft/7-operation-filter.png)

1. Fare clic su **Applica** per applicare i filtri.

    Verranno visualizzate tutte le operazioni di definizione e assegnazione del ruolo nell'ultimo mese. Include un collegamento per scaricare i log attività in un file in formato CSV.

    ![Log attività di controllo degli accessi in base al ruolo](../media-draft/7-activity-log-portal-filter.png)

## <a name="end-lab"></a>Fine del laboratorio

1. Per terminare il laboratorio, fare clic sul menu Hamburger nell'angolo superiore destro di questa finestra e quindi fare clic su **Fine**.

1. Fare clic su **Sì, terminare il laboratorio**.

## <a name="summary"></a>Riepilogo

In questa unità è stato illustrato come usare il log attività di Azure per elencare le modifiche apportate al controllo degli accessi in base al ruolo nel portale e come generare un report semplice.
