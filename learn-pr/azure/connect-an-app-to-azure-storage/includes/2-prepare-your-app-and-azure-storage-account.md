<span data-ttu-id="c44bb-101">L'applicazione di condivisione foto richiede l'archiviazione di diversi tipi di dati.</span><span class="sxs-lookup"><span data-stu-id="c44bb-101">Your photo sharing application requires storing different types of data.</span></span> <span data-ttu-id="c44bb-102">È necessario archiviare dati binari come una foto e dati testuali come un nome utente o un indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="c44bb-102">You need to store binary data like a photo, and textual data like a username or email address.</span></span> <span data-ttu-id="c44bb-103">Ogni sviluppatore abile deve aver tenuto conto di considerazioni sulla progettazione appropriate nello scegliere la forma di archiviazione da usare.</span><span class="sxs-lookup"><span data-stu-id="c44bb-103">Like any good developer, you need to ensure you have made the right design considerations when choosing what form of storage to use.</span></span>

<span data-ttu-id="c44bb-104">Qui vengono fornite alcune considerazioni legate alla progettazione di cui tenere conto prima di scegliere una strategia di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c44bb-104">Here, you'll look at some design considerations to keep in mind before choosing a storage strategy.</span></span> <span data-ttu-id="c44bb-105">Sarà necessario creare una soluzione semplice da usare, valida per il linguaggio scelto, affidabile e veloce.</span><span class="sxs-lookup"><span data-stu-id="c44bb-105">You want something that is easy to use, applicable to your choice of language, reliable, and fast.</span></span>

## <a name="design-considerations"></a><span data-ttu-id="c44bb-106">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="c44bb-106">Design considerations</span></span>

<span data-ttu-id="c44bb-107">Archiviazione di Azure espone funzionalità usando un endpoint HTTP tramite Internet.</span><span class="sxs-lookup"><span data-stu-id="c44bb-107">Azure Storage exposes functionality using an HTTP endpoint over the internet.</span></span> <span data-ttu-id="c44bb-108">Che cosa succede in caso di problemi di connettività Internet?</span><span class="sxs-lookup"><span data-stu-id="c44bb-108">What if that internet connectivity has problems?</span></span> <span data-ttu-id="c44bb-109">È necessario scrivere codice per creare una richiesta, comunicare con l'endpoint e analizzare qualsiasi risposta?</span><span class="sxs-lookup"><span data-stu-id="c44bb-109">In addition, do I need to write code to create a request, communicate to the endpoint, and parse any response?</span></span>

## <a name="connectivity-and-networking-issues"></a><span data-ttu-id="c44bb-110">Problemi di connettività e di rete</span><span class="sxs-lookup"><span data-stu-id="c44bb-110">Connectivity and networking issues</span></span>

<span data-ttu-id="c44bb-111">In un mondo ideale una rete è disponibile al 100% del tempo ed è sempre affidabile. Tuttavia, questa non è una prospettiva realistica.</span><span class="sxs-lookup"><span data-stu-id="c44bb-111">In an ideal world, a network is available 100% of the time and is always reliable, however this is not a realistic expectation.</span></span> <span data-ttu-id="c44bb-112">Poiché l'archiviazione è una componente di quasi tutte le applicazioni e le API di Archiviazione di Azure vengono esposte in rete, è necessario considerare l'impatto di un'interruzione di rete.</span><span class="sxs-lookup"><span data-stu-id="c44bb-112">Since storage is a part of almost all applications, and Azure Storage APIs are exposed over the network, you'll need to consider the impact of a network outage.</span></span> <span data-ttu-id="c44bb-113">Di seguito sono riportate alcune considerazioni di cui tenere conto nel determinare se Archiviazione di Azure sia la scelta corretta per le proprie esigenze:</span><span class="sxs-lookup"><span data-stu-id="c44bb-113">Following are some of the considerations to make when deciding on whether Azure Storage is well suited to your needs:</span></span>

* <span data-ttu-id="c44bb-114">L'applicazione deve poter mantenere sempre la connettività Web?</span><span class="sxs-lookup"><span data-stu-id="c44bb-114">Does your application expect that it can always maintain web connectivity?</span></span>

<span data-ttu-id="c44bb-115">Se si crea un'applicazione Web, si presuppone generalmente che la connessione Web sia sempre disponibile, di certo almeno, in primo luogo, per accedere all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c44bb-115">If you are creating a web application, there is often an assumption that web connectivity is always available, certainly at least to access the application in the first place.</span></span> <span data-ttu-id="c44bb-116">La possibilità di presupporre questo aspetto semplifica certamente la scrittura di applicazioni, in quanto sono necessari meno codice e complessità per gestire gli scenari offline.</span><span class="sxs-lookup"><span data-stu-id="c44bb-116">Assuming this fact certainly makes it easier to write applications as less code and complexity is required to handle offline scenarios.</span></span> <span data-ttu-id="c44bb-117">Le applicazioni non Web, come quelle installate in un desktop o un dispositivo mobile, possono non essere in grado di dare per scontato questo aspetto, di cui è necessario tenere conto durante la progettazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c44bb-117">Non-web applications such as those installed on a desktop or mobile device, may not be able to make such assumptions and this is something to assess when designing your application.</span></span> <span data-ttu-id="c44bb-118">Le app che prevedono connettività Web costante devono essere almeno in grado di gestire occasionalmente errori di connettività, come le interruzioni di rete.</span><span class="sxs-lookup"><span data-stu-id="c44bb-118">Those apps with assumed constant web connectivity should at least be able to deal with connectivity errors from time to time, such as network dropouts.</span></span>

* <span data-ttu-id="c44bb-119">La progettazione e l'implementazione delle applicazioni supporterà un set di funzionalità ridotte se non è disponibile connettività Web?</span><span class="sxs-lookup"><span data-stu-id="c44bb-119">Will your applications design and implementation support a degraded feature set if there is no web connectivity?</span></span>

<span data-ttu-id="c44bb-120">Se l'applicazione può essere disconnessa regolarmente o almeno operare in stato disconnesso, sarà in grado di ridurre le proprie funzionalità senza arrestarsi in modo anomalo o generare un errore e diventare inutilizzabile?</span><span class="sxs-lookup"><span data-stu-id="c44bb-120">If your application can be disconnected regularly, or at least operate in a disconnected state, will the application degrade gracefully without simply crashing or reporting an error and being unusable?</span></span> <span data-ttu-id="c44bb-121">Questo è un aspetto importante da considerare, in quanto gli scenari disconnessi richiedono spesso livelli elevati di impegno e complessità.</span><span class="sxs-lookup"><span data-stu-id="c44bb-121">This is an important consideration as there is a significant amount of effort and complexity in handling disconnected scenarios.</span></span> <span data-ttu-id="c44bb-122">Potrebbe essere possibile archiviare determinate parti di dati necessari in locale sul dispositivo per permettere il funzionamento offline.</span><span class="sxs-lookup"><span data-stu-id="c44bb-122">You may be able to store certain parts of needed data locally on the device to allow offline operation.</span></span> <span data-ttu-id="c44bb-123">Alcuni elementi dell'applicazione devono essere disabilitati fino al ripristino della connettività?</span><span class="sxs-lookup"><span data-stu-id="c44bb-123">Will certain elements of the application need to be disabled until connectivity is restored?</span></span> <span data-ttu-id="c44bb-124">Queste considerazioni riguardano nello specifico l'applicazione ed è importante tenerne conto nelle prima fasi di progettazione e implementazione a causa dell'impegno richiesto per gestire questo scenario.</span><span class="sxs-lookup"><span data-stu-id="c44bb-124">These considerations are specific to the application and it is important to include this in the design and implementation early due to the effort involved in catering for this scenario.</span></span>

## <a name="communicating-with-azure-storage"></a><span data-ttu-id="c44bb-125">Comunicazione con Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="c44bb-125">Communicating with Azure Storage</span></span>

<span data-ttu-id="c44bb-126">Quando si scrive un'applicazione, in genere si preferisce evitare di occuparsi della scrittura del codice alla base di elementi di basso livello come la formattazione dei dati da inviare ai servizi per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="c44bb-126">When writing your application, you typically do not want to overly concern yourself with writing code that deals with low-level elements such as formatting data to send to services for processing.</span></span> <span data-ttu-id="c44bb-127">Spesso, vengono create librerie per la comunicazione dei client con servizi come Archiviazione di Azure per semplificare notevolmente questa attività.</span><span class="sxs-lookup"><span data-stu-id="c44bb-127">Often, libraries for clients to communicate with services such as Azure Storage are created to make this task considerably easier.</span></span> <span data-ttu-id="c44bb-128">Tuttavia, non sono disponibili librerie client per tutti i linguaggi a questo scopo.</span><span class="sxs-lookup"><span data-stu-id="c44bb-128">However, not all languages have client libraries to support this.</span></span>

* <span data-ttu-id="c44bb-129">È disponibile una libreria client per lo stack di tecnologie scelto?</span><span class="sxs-lookup"><span data-stu-id="c44bb-129">Is there an available client library for your technology stack of choice?</span></span>

<span data-ttu-id="c44bb-130">In caso contrario, si è disposti a creare metodi di accesso agli endpoint personalizzati, che in genere richiedono un impegno maggiore?</span><span class="sxs-lookup"><span data-stu-id="c44bb-130">If not, are you content to create your own service endpoint access methods, which usually involve more effort?</span></span>

<span data-ttu-id="c44bb-131">Se sono disponibili librerie, assicurarsi che ve ne sia una per lo stack di tecnologie selezionato.</span><span class="sxs-lookup"><span data-stu-id="c44bb-131">If libraries are available, ensure that there is one available for your technology stack.</span></span> <span data-ttu-id="c44bb-132">Se si usano stack di tecnologie tra i più diffusi, come .NET o Java, sono in genere disponibili librerie client, mentre se si usano stack meno diffusi come Haskel o Scala, questo potrebbe non avvenire.</span><span class="sxs-lookup"><span data-stu-id="c44bb-132">If using more popular technology stacks such as .Net or Java, usually client libraries are available however if using less mainstream technology stacks such as Haskel or Scala this may not be the case.</span></span> <span data-ttu-id="c44bb-133">L'entità dell'impegno aggiuntivo richiesto quando non si usa una libreria client predefinita può spesso essere significativa e deve essere tenuta in considerazione nella progettazione e nell'implementazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c44bb-133">The amount of extra effort required when not using a provided client library can often be significant and needs to be factored into design and implementation of your application.</span></span>

<span data-ttu-id="c44bb-134">Ai fini del modulo e per motivi di semplicità, si presuppone di creare un'applicazione console .NET Core che avrà sempre connettività Internet e che non è progettata per ridurre uniformemente le proprie funzionalità in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="c44bb-134">For the purposes of the module and to aid in simplicity, we will assume that we are creating a .NET Core console application that will always have internet connectivity and is not designed for graceful degradation of functionality without it.</span></span>

## <a name="summary"></a><span data-ttu-id="c44bb-135">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c44bb-135">Summary</span></span>

<span data-ttu-id="c44bb-136">Archiviazione di Azure è un meccanismo di archiviazione flessibile e potente che espone le proprie funzionalità tramite una serie di endpoint API HTTP.</span><span class="sxs-lookup"><span data-stu-id="c44bb-136">Azure Storage is a flexible and powerful storage mechanism that exposes its functionality via a series of HTTP API endpoints.</span></span> <span data-ttu-id="c44bb-137">È importante tenere conto dell'impatto di eventuali problemi di connettività di rete e di disponibilità delle librerie client nel processo di progettazione e implementazione dell'applicazione per garantire che Archiviazione di Azure sia la scelta ideale per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c44bb-137">It is important consider the impact of network connectivity issues as well as availability of client libraries on the design and implementation effort for your application to ensure that Azure Storage is the right choice for your application.</span></span>

