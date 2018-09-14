<span data-ttu-id="30bdd-101">First Up Consultants esamina le modifiche trimestrali al controllo degli accessi in base al ruolo (RBAC) per scopi di controllo e risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="30bdd-101">First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes.</span></span> <span data-ttu-id="30bdd-102">È noto che le modifiche vengono registrate nel [Log attività di Azure](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span><span class="sxs-lookup"><span data-stu-id="30bdd-102">You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span></span> <span data-ttu-id="30bdd-103">Il gestore ha chiesto se è possibile generare un report delle assegnazione di ruolo e delle modifiche di ruolo personalizzate per il mese scorso.</span><span class="sxs-lookup"><span data-stu-id="30bdd-103">Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.</span></span>

## <a name="view-activity-logs"></a><span data-ttu-id="30bdd-104">Visualizzare i log attività</span><span class="sxs-lookup"><span data-stu-id="30bdd-104">View activity logs</span></span>

<span data-ttu-id="30bdd-105">Il modo più semplice per iniziare è visualizzare i log attività con il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="30bdd-105">The easiest way to get started is to view the activity logs with the Azure portal.</span></span>

1. <span data-ttu-id="30bdd-106">Fare clic su **Tutti i servizi** e quindi trovare **Log attività**.</span><span class="sxs-lookup"><span data-stu-id="30bdd-106">Click **All services** and then find **Activity log**.</span></span>

    ![Log attività tramite il portale](../media-draft/7-all-services-activity-log.png)

1. <span data-ttu-id="30bdd-108">Fare clic su **Log attività**.</span><span class="sxs-lookup"><span data-stu-id="30bdd-108">Click **Activity log**.</span></span>

    ![Log attività tramite il portale](../media-draft/7-activity-log-portal.png)

1. <span data-ttu-id="30bdd-110">Impostare il **Timespan** e filtrare in base a **Mese scorso**.</span><span class="sxs-lookup"><span data-stu-id="30bdd-110">Set the **Timespan** filter to **Last month**.</span></span>

1. <span data-ttu-id="30bdd-111">Impostare la **Categoria di eventi** e filtrare in base a **Amministrativo**.</span><span class="sxs-lookup"><span data-stu-id="30bdd-111">Set the **Event category** filter to **Administrative**.</span></span>

1. <span data-ttu-id="30bdd-112">Nel filtro **Operazione**, digitare **Ruolo** per filtrare l'elenco.</span><span class="sxs-lookup"><span data-stu-id="30bdd-112">In the **Operation** filter, type **role** to filter the list.</span></span>

1. <span data-ttu-id="30bdd-113">Selezionare le operazioni di RBAC seguenti:</span><span class="sxs-lookup"><span data-stu-id="30bdd-113">Select the following RBAC operations:</span></span>

    - <span data-ttu-id="30bdd-114">Creare assegnazione del ruolo (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="30bdd-114">Create role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="30bdd-115">Eliminare assegnazione del ruolo (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="30bdd-115">Delete role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="30bdd-116">Creare o aggiornare la definizione del ruolo personalizzata (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="30bdd-116">Create or update custom role definition (roleDefinitions)</span></span>
    - <span data-ttu-id="30bdd-117">Eliminare la definizione del ruolo personalizzata (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="30bdd-117">Delete custom role definition (roleDefinitions)</span></span>

    ![Filtro di operazione](../media-draft/7-operation-filter.png)

1. <span data-ttu-id="30bdd-119">Fare clic su **Applica** per applicare i filtri.</span><span class="sxs-lookup"><span data-stu-id="30bdd-119">Click **Apply** to apply your filters.</span></span>

    <span data-ttu-id="30bdd-120">Verranno visualizzate tutte le operazioni di definizione e assegnazione del ruolo nell'ultimo mese.</span><span class="sxs-lookup"><span data-stu-id="30bdd-120">You'll see all the role assignment and role definition operations for the last month.</span></span> <span data-ttu-id="30bdd-121">Include un collegamento per scaricare i log attività in un file in formato CSV.</span><span class="sxs-lookup"><span data-stu-id="30bdd-121">It also includes a link to download the activity log as a CSV file.</span></span>

    ![Log attività di controllo degli accessi in base al ruolo](../media-draft/7-activity-log-portal-filter.png)

## <a name="end-lab"></a><span data-ttu-id="30bdd-123">Fine del laboratorio</span><span class="sxs-lookup"><span data-stu-id="30bdd-123">End lab</span></span>

1. <span data-ttu-id="30bdd-124">Per terminare il laboratorio, fare clic sul menu Hamburger nell'angolo superiore destro di questa finestra e quindi fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="30bdd-124">To end the lab, click the hamburger menu in the upper-right corner of this window and then click **End**.</span></span>

1. <span data-ttu-id="30bdd-125">Fare clic su **Sì, terminare il laboratorio**.</span><span class="sxs-lookup"><span data-stu-id="30bdd-125">Click **Yes, end my lab**.</span></span>

## <a name="summary"></a><span data-ttu-id="30bdd-126">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="30bdd-126">Summary</span></span>

<span data-ttu-id="30bdd-127">In questa unità è stato illustrato come usare il log attività di Azure per elencare le modifiche apportate al controllo degli accessi in base al ruolo nel portale e come generare un report semplice.</span><span class="sxs-lookup"><span data-stu-id="30bdd-127">In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.</span></span>
