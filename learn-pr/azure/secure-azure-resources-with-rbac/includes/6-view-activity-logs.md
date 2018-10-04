First Up Consultants esamina le modifiche trimestrali al controllo degli accessi in base al ruolo (RBAC) per scopi di controllo e risoluzione dei problemi. È noto che le modifiche vengono registrate nel [Log attività di Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Il gestore ha chiesto se è possibile generare un report delle assegnazione di ruolo e delle modifiche di ruolo personalizzate per il mese scorso.

## <a name="view-activity-logs"></a>Visualizzare i log attività

Il modo più semplice per iniziare è visualizzare i log attività con il portale di Azure.

1. Fare clic su **Tutti i servizi** e quindi trovare **Log attività**.

    ![Log attività tramite il portale](../media/6-all-services-activity-log.png)

1. Fare clic su **Log attività** per aprire il log attività.

    ![Log attività tramite il portale](../media/6-activity-log-portal.png)

1. Impostare il filtro **Intervallo di tempo** su **Mese scorso**.

1. Aggiungere un filtro **Operazione** e digitare **Ruolo** per filtrare l'elenco.

1. Selezionare le operazioni di controllo degli accessi in base al ruolo seguenti:

    - Creare assegnazione del ruolo (roleAssignments)
    - Eliminare assegnazione del ruolo (roleAssignments)
    - Creare o aggiornare la definizione del ruolo personalizzata (roleDefinitions)
    - Eliminare la definizione del ruolo personalizzata (roleDefinitions)

    ![Filtro operazioni](../media/6-operation-filter.png)

    Dopo qualche momento verranno visualizzate tutte le operazioni di definizione e assegnazione del ruolo avvenute nell'ultimo mese. Include un collegamento per scaricare i log attività in un file in formato CSV.

In questa unità si è imparato come usare il log attività di Azure per elencare le modifiche apportate al controllo degli accessi in base al ruolo nel portale e come generare un report semplice.