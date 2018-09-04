<span data-ttu-id="54fc7-101">Sono stati illustrati argomenti quali la creazione di stime dei costi per gli ambienti che si intende creare, la descrizione di alcuni strumenti per ottenere dettagli sugli elementi soggetti a spese e le proiezioni delle spese future.</span><span class="sxs-lookup"><span data-stu-id="54fc7-101">We have seen how to create cost estimates for environments you'd like to build, walked through some tools to get details on where we're spending money, and projected future expenses.</span></span> <span data-ttu-id="54fc7-102">Verrà ora spiegato come ridurre i costi di infrastruttura.</span><span class="sxs-lookup"><span data-stu-id="54fc7-102">Our next challenge is to look at how to reduce those infrastructure costs.</span></span>

## <a name="use-reserved-instances"></a><span data-ttu-id="54fc7-103">Usare le istanze riservate</span><span class="sxs-lookup"><span data-stu-id="54fc7-103">Use reserved instances</span></span>

<span data-ttu-id="54fc7-104">In presenza di carichi di lavoro di VM statici e prevedibili per natura, in particolare quelli in esecuzione tutto l'anno, 24 ore su 24, 7 giorni su 7, l'uso delle istanze riservate rappresenta il metodo ideale per conseguire un potenziale risparmio fino al 70%, a seconda delle dimensioni della VM.</span><span class="sxs-lookup"><span data-stu-id="54fc7-104">If you have VM workloads that are static and predictable in nature, particularly ones that run 24x7x365, using reserved instances is a fantastic way to potentially save up to 70 percent, depending on the VM size.</span></span>

![Risparmi associati alle istanze riservate](../images/savings-coins.png)

<span data-ttu-id="54fc7-106">Le istanze riservate vengono acquistate per periodi di uno o tre anni, con pagamento anticipato per l'intero periodo.</span><span class="sxs-lookup"><span data-stu-id="54fc7-106">Reserved instances are purchased in one-year or three-year terms, with payment required for the full term up front.</span></span> <span data-ttu-id="54fc7-107">Dopo l'acquisto, Microsoft associa la prenotazione alle istanze in esecuzione e riduce le ore dalla prenotazione.</span><span class="sxs-lookup"><span data-stu-id="54fc7-107">After it's purchased, Microsoft matches up the reservation to running instances and decrements the hours from your reservation.</span></span> <span data-ttu-id="54fc7-108">Le prenotazioni possono essere acquistate tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="54fc7-108">Reservations can be purchased through the Azure portal.</span></span> <span data-ttu-id="54fc7-109">E poiché le istanze riservate rappresentano uno sconto di ore di elaborazione, sono disponibili per le VM di Windows e Linux.</span><span class="sxs-lookup"><span data-stu-id="54fc7-109">And because reserved instances are a compute discount, they are available for both Windows and Linux VMs.</span></span>

## <a name="right-size-underutilized-virtual-machines"></a><span data-ttu-id="54fc7-110">Ridimensionare le macchine virtuali sottoutilizzate</span><span class="sxs-lookup"><span data-stu-id="54fc7-110">Right-size underutilized virtual machines</span></span>

<span data-ttu-id="54fc7-111">Come indicato in precedenza, Gestione costi di Azure e Azure Advisor potrebbero consigliare di ridimensionare o arrestare le VM.</span><span class="sxs-lookup"><span data-stu-id="54fc7-111">Recall from our previous discussion that Azure Cost Management and Azure Advisor might recommend right-sizing or shutting down VMs.</span></span> <span data-ttu-id="54fc7-112">Il ridimensionamento di una macchina virtuale è il processo che consente di portarla a una dimensione appropriata.</span><span class="sxs-lookup"><span data-stu-id="54fc7-112">Right-sizing a virtual machine is the process of resizing it to a proper size.</span></span> <span data-ttu-id="54fc7-113">Si supponga di avere un server in esecuzione come controller di dominio con le dimensioni di uno **Standard_D4sv3**, ma che la VM sia inattiva al 90% nella maggior parte dei casi.</span><span class="sxs-lookup"><span data-stu-id="54fc7-113">Let's imagine you have a server running as a domain controller that is sized as a **Standard_D4sv3**, but your VM is sitting at 90 percent idle the vast majority of the time.</span></span> <span data-ttu-id="54fc7-114">Ridimensionando la VM a uno **Standard_D2s_v3**, i costi di elaborazione si riducono del 50%.</span><span class="sxs-lookup"><span data-stu-id="54fc7-114">By resizing this VM to a **Standard_D2s_v3**, you reduce your compute cost by 50 percent.</span></span> <span data-ttu-id="54fc7-115">I costi sono lineari e raddoppiati per ogni dimensione maggiore nella stessa serie.</span><span class="sxs-lookup"><span data-stu-id="54fc7-115">Costs are linear and double for each size larger in the same series.</span></span> <span data-ttu-id="54fc7-116">In questo caso, potrebbe addirittura risultare vantaggioso modificare la serie di istanze in una serie di VM meno costosa.</span><span class="sxs-lookup"><span data-stu-id="54fc7-116">In this case, you might even benefit from changing the instance series to go to a less expensive VM series.</span></span>

![Ridimensionare una VM](../images/vm-resize.png)

<span data-ttu-id="54fc7-118">Le macchine virtuali sovradimensionate costituiscono una spesa superflua comune in Azure, tuttavia facilmente risolvibile.</span><span class="sxs-lookup"><span data-stu-id="54fc7-118">Oversized virtual machines are a common unnecessary expense on Azure and one that is easily fixed.</span></span> <span data-ttu-id="54fc7-119">È possibile modificare la dimensione di una VM mediante il portale di Azure, Azure PowerShell o l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="54fc7-119">You can change the size of a VM through the Azure portal, Azure PowerShell, or the Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="54fc7-120">Per ridimensionare una VM, è necessario arrestarla, ridimensionarla e riavviarla.</span><span class="sxs-lookup"><span data-stu-id="54fc7-120">Resizing a VM requires that it be stopped, resized, and then restarted.</span></span> <span data-ttu-id="54fc7-121">Questa operazione potrebbe richiedere qualche minuto, a seconda del volume della modifica.</span><span class="sxs-lookup"><span data-stu-id="54fc7-121">This may take a few minutes depending on how significant the size change is.</span></span> <span data-ttu-id="54fc7-122">Pianificare un'interruzione del servizio o spostare il traffico in un'altra istanza mentre si esegue questa attività.</span><span class="sxs-lookup"><span data-stu-id="54fc7-122">Plan for an outage, or shift your traffic to another instance while you perform this task.</span></span>

## <a name="deallocate-virtual-machines-in-off-hours"></a><span data-ttu-id="54fc7-123">Deallocare le macchine virtuali durante gli orari di minore attività</span><span class="sxs-lookup"><span data-stu-id="54fc7-123">Deallocate virtual machines in off hours</span></span>

<span data-ttu-id="54fc7-124">L'uso di carichi di lavoro di macchine virtuali solo in determinati momenti, ma in esecuzione ogni ora di ogni giorno, determina uno spreco di denaro.</span><span class="sxs-lookup"><span data-stu-id="54fc7-124">If you have virtual machine workloads that are only used during certain periods of time, but you're running them every hour of every day, you're wasting money.</span></span> <span data-ttu-id="54fc7-125">Queste VM rappresentano candidati ideali da arrestare se non in uso e riavviare in base a una pianificazione, consentendo di risparmiare costi di calcolo mentre la VM è deallocata.</span><span class="sxs-lookup"><span data-stu-id="54fc7-125">These VMs are great candidates to shut down when not in use and start back up on a schedule, saving you compute costs while the VM is deallocated.</span></span>

<span data-ttu-id="54fc7-126">Questo approccio costituisce una strategia ottimale per ambienti di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="54fc7-126">This approach is a great strategy for development environments.</span></span> <span data-ttu-id="54fc7-127">Spesso le attività di sviluppo possono verificarsi solo durante le ore lavorative, offrendo la flessibilità necessaria per deallocare questi sistemi durante gli orari di minore attività e impedendo l'accumulo dei costi di calcolo.</span><span class="sxs-lookup"><span data-stu-id="54fc7-127">It's often the case that development may happen only during business hours, giving you the flexibility to deallocate these systems in the off hours and stopping your compute costs from accruing.</span></span> <span data-ttu-id="54fc7-128">Azure offre ora una [soluzione di automazione](https://docs.microsoft.com/azure/automation/automation-solution-vm-management) completamente disponibile per l'ambiente in uso.</span><span class="sxs-lookup"><span data-stu-id="54fc7-128">Azure now has an [automation solution](https://docs.microsoft.com/azure/automation/automation-solution-vm-management) fully available for you to leverage in your environment.</span></span>

<span data-ttu-id="54fc7-129">È anche possibile usare la funzionalità di arresto automatico in una macchina virtuale per pianificare arresti automatizzati.</span><span class="sxs-lookup"><span data-stu-id="54fc7-129">You can also use the auto-shutdown feature on a virtual machine to schedule automated shutdowns.</span></span>

![Arresto automatico](../images/vm-auto-shutdown.png)

## <a name="delete-unused-virtual-machines"></a><span data-ttu-id="54fc7-131">Eliminare le macchine virtuali inutilizzate</span><span class="sxs-lookup"><span data-stu-id="54fc7-131">Delete unused virtual machines</span></span> 

 <span data-ttu-id="54fc7-132">Può sembrare ovvio ma, se un servizio non viene usato, è consigliabile arrestarlo.</span><span class="sxs-lookup"><span data-stu-id="54fc7-132">This advice may sound obvious, but if you aren't using a service, you should shut it down.</span></span> <span data-ttu-id="54fc7-133">Non è insolito avere sistemi non di produzione o per un modello di verifica ancora attivi su un progetto non più necessario.</span><span class="sxs-lookup"><span data-stu-id="54fc7-133">It's not uncommon to find non-production or proof-of-concept systems left around following a project that is no longer needed.</span></span> <span data-ttu-id="54fc7-134">Esaminare periodicamente l'ambiente e il lavoro per identificare questi sistemi.</span><span class="sxs-lookup"><span data-stu-id="54fc7-134">Regularly review your environment and work to identify these systems.</span></span> <span data-ttu-id="54fc7-135">L'arresto di questi sistemi può comportare vantaggi di vario tipo, garantendo un risparmio dei costi di infrastruttura, ma anche un risparmio potenziale sulla gestione delle licenze e sulle operazioni.</span><span class="sxs-lookup"><span data-stu-id="54fc7-135">Shutting down these systems can have a multifaceted benefit by saving you not only on infrastructure costs but also potential savings on licensing and operations.</span></span>

## <a name="migrate-to-paas-or-saas-services"></a><span data-ttu-id="54fc7-136">Eseguire la migrazione a servizi PaaS o SaaS</span><span class="sxs-lookup"><span data-stu-id="54fc7-136">Migrate to PaaS or SaaS services</span></span> 

<span data-ttu-id="54fc7-137">Se si decide di spostare i carichi di lavoro nel cloud, un'evoluzione naturale è quella di iniziare con i servizi IaaS (Infrastructure-as-a-Service, infrastruttura distribuita come servizio) e quindi spostarli in un modello PaaS (Platform-as-a-Service, piattaforma distribuita come servizio) a seconda delle esigenze, in un processo iterativo.</span><span class="sxs-lookup"><span data-stu-id="54fc7-137">Lastly, as you move workloads to the cloud, a natural evolution is to start with infrastructure-as-a-service (IaaS) services and then move them to platform-as-a-service (PaaS) as appropriate, in an iterative process.</span></span>

<span data-ttu-id="54fc7-138">I servizi PaaS offrono in genere risparmi notevoli sui costi operativi e delle risorse.</span><span class="sxs-lookup"><span data-stu-id="54fc7-138">PaaS services typically provide substantial savings in both resource and operational costs.</span></span> <span data-ttu-id="54fc7-139">Il problema è che, a seconda del tipo di servizio, saranno necessari livelli diversi di impegno per passare a questi servizi in termini di tempo e risorse.</span><span class="sxs-lookup"><span data-stu-id="54fc7-139">The challenge is that depending on the type of service, varying levels of effort will be required to move to these services from both a time and resource perspective.</span></span> <span data-ttu-id="54fc7-140">Potrebbe essere semplice spostare un database SQL Server in un database SQL di Azure e molto più impegnativo spostare un'applicazione a più livelli in un contenitore o un'architettura serverless.</span><span class="sxs-lookup"><span data-stu-id="54fc7-140">You might be able to easily move a SQL Server database to Azure SQL Database, but it might take substantially more effort to move your multitier application to a container or serverless-based architecture.</span></span> <span data-ttu-id="54fc7-141">È consigliabile valutare costantemente l'architettura delle applicazioni per determinare eventuali efficienze conseguibili mediante i servizi PaaS.</span><span class="sxs-lookup"><span data-stu-id="54fc7-141">It's a good practice to continuously evaluate the architecture of your applications to determine if there are efficiencies to be gained through PaaS services.</span></span>  

<span data-ttu-id="54fc7-142">Grazie ad Azure è semplice testare questi servizi con un livello di rischio minimo, offrendo la possibilità di provare nuovi modelli di architettura senza troppe difficoltà.</span><span class="sxs-lookup"><span data-stu-id="54fc7-142">Azure makes it easy to test these services with little risk, giving you the ability to try out new architecture patterns relatively easily.</span></span> <span data-ttu-id="54fc7-143">Detto questo, il percorso è in genere più lungo e potrebbe non essere d'aiuto nell'immediato, se si intende ottenere risparmi a breve termine in termini di costi.</span><span class="sxs-lookup"><span data-stu-id="54fc7-143">That said, it's typically a longer journey and might not be of immediate help if you're looking for quick wins from a cost-savings perspective.</span></span> <span data-ttu-id="54fc7-144">Il Centro architetture Azure è il luogo ideale in cui ricavare idee per trasformare l'applicazione, nonché procedure consigliate in un'ampia gamma di architetture e servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="54fc7-144">The Azure Architecture Center is a great place to get ideas for transforming your application, as well as best practices across a wide array of architectures and Azure services.</span></span> 