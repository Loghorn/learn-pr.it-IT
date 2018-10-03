<span data-ttu-id="a78fc-101">First Up Consultants esamina le modifiche trimestrali al controllo degli accessi in base al ruolo (RBAC) per scopi di controllo e risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="a78fc-101">First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes.</span></span> <span data-ttu-id="a78fc-102">È noto che le modifiche vengono registrate nel [Log attività di Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span><span class="sxs-lookup"><span data-stu-id="a78fc-102">You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span></span> <span data-ttu-id="a78fc-103">Il gestore ha chiesto se è possibile generare un report delle assegnazione di ruolo e delle modifiche di ruolo personalizzate per il mese scorso.</span><span class="sxs-lookup"><span data-stu-id="a78fc-103">Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.</span></span>

## <a name="view-activity-logs"></a><span data-ttu-id="a78fc-104">Visualizzare i log attività</span><span class="sxs-lookup"><span data-stu-id="a78fc-104">View activity logs</span></span>

<span data-ttu-id="a78fc-105">Il modo più semplice per iniziare è visualizzare i log attività con il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="a78fc-105">The easiest way to get started is to view the activity logs with the Azure portal.</span></span>

1. <span data-ttu-id="a78fc-106">Fare clic su **Tutti i servizi** e quindi trovare **Log attività**.</span><span class="sxs-lookup"><span data-stu-id="a78fc-106">Click **All services** and then find **Activity log**.</span></span>

    ![Log attività tramite il portale](../media/6-all-services-activity-log.png)

1. <span data-ttu-id="a78fc-108">Fare clic su **Log attività** per aprire il log attività.</span><span class="sxs-lookup"><span data-stu-id="a78fc-108">Click **Activity log** to open the activity log.</span></span>

    ![Log attività tramite il portale](../media/6-activity-log-portal.png)

1. <span data-ttu-id="a78fc-110">Impostare il filtro **Intervallo di tempo** su **Mese scorso**.</span><span class="sxs-lookup"><span data-stu-id="a78fc-110">Set the **Timespan** filter to **Last month**.</span></span>

1. <span data-ttu-id="a78fc-111">Aggiungere un filtro **Operazione** e digitare **Ruolo** per filtrare l'elenco.</span><span class="sxs-lookup"><span data-stu-id="a78fc-111">Add an **Operation** filter and type **role** to filter the list.</span></span>

1. <span data-ttu-id="a78fc-112">Selezionare le operazioni di controllo degli accessi in base al ruolo seguenti:</span><span class="sxs-lookup"><span data-stu-id="a78fc-112">Select the following RBAC operations:</span></span>

    - <span data-ttu-id="a78fc-113">Creare assegnazione del ruolo (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="a78fc-113">Create role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="a78fc-114">Eliminare assegnazione del ruolo (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="a78fc-114">Delete role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="a78fc-115">Creare o aggiornare la definizione del ruolo personalizzata (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="a78fc-115">Create or update custom role definition (roleDefinitions)</span></span>
    - <span data-ttu-id="a78fc-116">Eliminare la definizione del ruolo personalizzata (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="a78fc-116">Delete custom role definition (roleDefinitions)</span></span>

    ![Filtro operazioni](../media/6-operation-filter.png)

    <span data-ttu-id="a78fc-118">Dopo qualche momento verranno visualizzate tutte le operazioni di definizione e assegnazione del ruolo avvenute nell'ultimo mese.</span><span class="sxs-lookup"><span data-stu-id="a78fc-118">After a few moments, you'll see all the role assignment and role definition operations for the last month.</span></span> <span data-ttu-id="a78fc-119">Include un collegamento per scaricare i log attività in un file in formato CSV.</span><span class="sxs-lookup"><span data-stu-id="a78fc-119">It also includes a link to download the activity log as a CSV file.</span></span>

<span data-ttu-id="a78fc-120">In questa unità si è imparato come usare il log attività di Azure per elencare le modifiche apportate al controllo degli accessi in base al ruolo nel portale e come generare un report semplice.</span><span class="sxs-lookup"><span data-stu-id="a78fc-120">In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.</span></span>