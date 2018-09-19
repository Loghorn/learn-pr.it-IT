<span data-ttu-id="b098a-101">Ora che si sono appresi i vantaggi e le funzionalità dell'archiviazione dei dati in Azure, ne verranno illustrate le differenze dall'archiviazione locale.</span><span class="sxs-lookup"><span data-stu-id="b098a-101">Now that you know about the benefits and features of Azure data storage, let's see how it differs from on-premises storage.</span></span>

<span data-ttu-id="b098a-102">Il termine "locale" si riferisce all'archiviazione e alla manutenzione dei dati presenti su hardware e server locali.</span><span class="sxs-lookup"><span data-stu-id="b098a-102">The term "on-premises" refers to the storage and maintenance of data on local hardware and servers.</span></span> <span data-ttu-id="b098a-103">Nel confronto tra l'archiviazione dei dati locale e quella in Azure occorre considerare diversi fattori.</span><span class="sxs-lookup"><span data-stu-id="b098a-103">There are several factors to consider when comparing on-premises to Azure data storage.</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="b098a-104">![Fattura cartacea e un cloud che rappresentano la convenienza](../media/4-cost-effectiveness.png)</span><span class="sxs-lookup"><span data-stu-id="b098a-104">![Paper bill and a cloud representing cost effectiveness](../media/4-cost-effectiveness.png)</span></span>
  :::column-end:::
    <span data-ttu-id="b098a-105">:::column span="3"::: **Convenienza**</span><span class="sxs-lookup"><span data-stu-id="b098a-105">:::column span="3"::: **Cost effectiveness**</span></span>

<span data-ttu-id="b098a-106">Una soluzione di archiviazione locale richiede hardware dedicato che è necessario acquistare, installare, configurare e gestire.</span><span class="sxs-lookup"><span data-stu-id="b098a-106">An on-premises storage solution requires dedicated hardware that needs to be purchased, installed, configured, and maintained.</span></span> <span data-ttu-id="b098a-107">Le spese iniziali o i costi di capitale possono essere significativi.</span><span class="sxs-lookup"><span data-stu-id="b098a-107">This can be a significant up-front expense (or capital cost).</span></span> <span data-ttu-id="b098a-108">Il cambiamento dei requisiti può rendere necessari investimenti in nuovo hardware.</span><span class="sxs-lookup"><span data-stu-id="b098a-108">Change in requirements can require investment in new hardware.</span></span> <span data-ttu-id="b098a-109">L'hardware deve essere in grado di gestire picchi di domanda e ciò comporta la possibilità che rimanga inattivo o sottoutilizzato nei periodi di minore attività.</span><span class="sxs-lookup"><span data-stu-id="b098a-109">Your hardware needs to be capable of handling peak demand which means it may sit idle or be under-utilized in off-peak times.</span></span>

<span data-ttu-id="b098a-110">L'archiviazione dati in Azure offre un modello di prezzi con pagamento in base al consumo spesso interessante per le aziende che possono considerare questi costi come spese operative senza dover sostenere costi di capitale iniziali.</span><span class="sxs-lookup"><span data-stu-id="b098a-110">Azure data storage provides a pay-as-you-go pricing model which is often appealing to businesses as an operating expense instead of an upfront capital cost.</span></span> <span data-ttu-id="b098a-111">La soluzione è anche scalabile poiché consente l'aumento delle prestazioni o del numero di istanze in base alla domanda e la relativa riduzione quando la domanda diminuisce.</span><span class="sxs-lookup"><span data-stu-id="b098a-111">It's also scalable, allowing you to scale up or scale out as demand dictates and scale back when demand is low.</span></span> <span data-ttu-id="b098a-112">Vengono addebitati solo i servizi dati usati in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="b098a-112">You are charged for data services only as you need them.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="b098a-113">![Certificato che rappresenta l'affidabilità](../media/4-reliability.png)</span><span class="sxs-lookup"><span data-stu-id="b098a-113">![A certificate representing reliability](../media/4-reliability.png)</span></span>
  :::column-end:::
    <span data-ttu-id="b098a-114">:::column span="3"::: **Affidabilità**</span><span class="sxs-lookup"><span data-stu-id="b098a-114">:::column span="3"::: **Reliability**</span></span>

<span data-ttu-id="b098a-115">L'archiviazione locale richiede strategie di backup dei dati, bilanciamento del carico e ripristino di emergenza.</span><span class="sxs-lookup"><span data-stu-id="b098a-115">On-premises storage requires data backup, load balancing, and disaster recovery strategies.</span></span> <span data-ttu-id="b098a-116">Queste strategie possono risultare complesse e costose poiché per ognuna di esse sono spesso necessari server dedicati che richiedono un investimento significativo in hardware e risorse IT.</span><span class="sxs-lookup"><span data-stu-id="b098a-116">These can be challenging and expensive as they often each need dedicated servers requiring a significant investment in both hardware and IT resources.</span></span>

<span data-ttu-id="b098a-117">L'archiviazione dei dati in Azure offre backup dei dati, bilanciamento del carico, ripristino di emergenza e replica dei dati come servizi per garantire sicurezza dei dati e disponibilità elevata.</span><span class="sxs-lookup"><span data-stu-id="b098a-117">Azure data storage provides data backup, load balancing, disaster recovery, and data replication as services to ensure data safety and high availability.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="b098a-118">![Edificio di forma particolare che rappresenta i diversi tipi di archiviazione](../media/4-storage-types.png)</span><span class="sxs-lookup"><span data-stu-id="b098a-118">![A uniquely shaped building representing different storage types](../media/4-storage-types.png)</span></span>
  :::column-end:::
    <span data-ttu-id="b098a-119">:::column span="3"::: **Tipi di archiviazione**</span><span class="sxs-lookup"><span data-stu-id="b098a-119">:::column span="3"::: **Storage types**</span></span>

<span data-ttu-id="b098a-120">Per una soluzione a volte sono necessari più tipi di archiviazione diversi, ad esempio archiviazione su file e database.</span><span class="sxs-lookup"><span data-stu-id="b098a-120">Sometimes multiple different storage types are required for a solution, such as file and database storage.</span></span> <span data-ttu-id="b098a-121">Un approccio locale spesso richiede numerosi server e strumenti di amministrazione per ogni tipo di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="b098a-121">An on-premises approach often requires numerous servers and administrative tools for each storage type.</span></span>

<span data-ttu-id="b098a-122">L'archiviazione dei dati in Azure offre diverse opzioni di archiviazione, tra cui l'accesso distribuito e l'archiviazione a livelli.</span><span class="sxs-lookup"><span data-stu-id="b098a-122">Azure data storage provides a variety of different storage options including distributed access and tiered storage.</span></span> <span data-ttu-id="b098a-123">Ciò consente di integrare una combinazione di tecnologie di archiviazione con la possibilità di scegliere il tipo di archiviazione migliore per ogni parte della soluzione.</span><span class="sxs-lookup"><span data-stu-id="b098a-123">This makes it possible to integrate a combination of storage technologies providing the best storage choice for each part of your solution.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="b098a-124">![Playbook sportivo che rappresenta la flessibilità](../media/4-agility.png)</span><span class="sxs-lookup"><span data-stu-id="b098a-124">![A sports playbook representing agility](../media/4-agility.png)</span></span>
  :::column-end:::
    <span data-ttu-id="b098a-125">:::column span="3"::: **Flessibilità**</span><span class="sxs-lookup"><span data-stu-id="b098a-125">:::column span="3"::: **Agility**</span></span>

<span data-ttu-id="b098a-126">I requisiti e le tecnologie cambiano.</span><span class="sxs-lookup"><span data-stu-id="b098a-126">Requirements and technologies change.</span></span> <span data-ttu-id="b098a-127">In una distribuzione locale ciò può comportare la necessità di lunghe e costose attività di provisioning e distribuzione di nuovi server e parti dell'infrastruttura.</span><span class="sxs-lookup"><span data-stu-id="b098a-127">For an on-premises deployment this may mean provisioning and deploying new servers and infrastructure pieces, which is a time consuming and expensive activity.</span></span>

<span data-ttu-id="b098a-128">L'archiviazione dei dati in Azure offre la flessibilità necessaria per creare nuovi servizi in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="b098a-128">Azure data storage gives you the flexibility to create new services in minutes.</span></span> <span data-ttu-id="b098a-129">Grazie a questa flessibilità, è possibile cambiare rapidamente i back-end di archiviazione senza la necessità di un investimento significativo in hardware.</span><span class="sxs-lookup"><span data-stu-id="b098a-129">This flexibility allows you to change storage back-ends quickly without needing a significant hardware investment.</span></span>

<span data-ttu-id="b098a-130">La figura seguente illustra le differenze tra l'archiviazione locale e l'archiviazione dei dati in Azure.</span><span class="sxs-lookup"><span data-stu-id="b098a-130">The following illustration shows differences between on-premise storage and Azure data storage.</span></span>

![Figura che illustra il confronto tra l'archiviazione locale e l'archiviazione dei dati in Azure per diverse esigenze aziendali comuni.](../media/4-Comparison.png)

  :::column-end:::
:::row-end:::