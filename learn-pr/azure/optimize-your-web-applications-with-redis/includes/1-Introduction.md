<span data-ttu-id="5a475-101">Si lavora presso una società che tiene traccia delle statistiche per sport professionistici e fornisce app e un'API per eseguire query sui risultati.</span><span class="sxs-lookup"><span data-stu-id="5a475-101">You work at a company that tracks professional sports statistics and provides apps an API to query results.</span></span> <span data-ttu-id="5a475-102">In questo modo i tifosi possono monitorare e controllare gare e punteggi, sia in tempo reale che nei dati storici.</span><span class="sxs-lookup"><span data-stu-id="5a475-102">It helps fans track and review games and scores, both live and historical.</span></span> <span data-ttu-id="5a475-103">Gli utenti possono anche richiedere statistiche di una squadra tramite una ricerca in linguaggio naturale, ad esempio "How many times has John Smith hit a home run against a left-handed pitcher?" (Quanti home run ha fatto John Smith contro un lanciatore mancino?)</span><span class="sxs-lookup"><span data-stu-id="5a475-103">Users can also request team statistics using a natural language search, such as “How many times has John Smith hit a home run against a left-handed pitcher?”</span></span>

<span data-ttu-id="5a475-104">Durante i periodi di picco della domanda, ad esempio durante i playoff, i tempi di risposta del servizio rallentano perché il servizio back-end non ha la capacità per soddisfare la domanda.</span><span class="sxs-lookup"><span data-stu-id="5a475-104">During times of peak demand, such as during playoffs, your response time of your service slows down because your back-end service doesn't have the capacity to meet demand.</span></span> <span data-ttu-id="5a475-105">Si vogliono migliorare le prestazioni per gli utenti e ridurre il carico di lavoro nei servizi back-end e di archiviazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="5a475-105">You want to improve performance for your users and reduce the workload on your back-end and data storage services.</span></span> <span data-ttu-id="5a475-106">Le metriche indicano che tra il 50% e l'80% dei dati restituiti riguarda valori di sola lettura o richiesti di recente.</span><span class="sxs-lookup"><span data-stu-id="5a475-106">Your metrics show that 50% to 80% of the data returned is for read-only or recently requested values.</span></span> <span data-ttu-id="5a475-107">L'implementazione di una cache per i dati di uso comune potrebbe migliorare le prestazioni e ridurre la latenza.</span><span class="sxs-lookup"><span data-stu-id="5a475-107">Implementing a cache of commonly used data could improve performance and reduce latency.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="5a475-108">Obiettivi di apprendimento</span><span class="sxs-lookup"><span data-stu-id="5a475-108">Learning objectives</span></span>

<span data-ttu-id="5a475-109">In questo modulo verrà descritto come:</span><span class="sxs-lookup"><span data-stu-id="5a475-109">In this module, you will:</span></span>

- <span data-ttu-id="5a475-110">Descrivere cosa è una cache Redis e i relativi usi.</span><span class="sxs-lookup"><span data-stu-id="5a475-110">Describe what a Redis cache is, and what you can use it for.</span></span>
- <span data-ttu-id="5a475-111">Creare un progetto e pianificare l'uso di Cache Redis.</span><span class="sxs-lookup"><span data-stu-id="5a475-111">Create a design and plan to use a Redis cache.</span></span>
- <span data-ttu-id="5a475-112">Effettuare il provisioning di una Cache Redis in Azure.</span><span class="sxs-lookup"><span data-stu-id="5a475-112">Provision a Redis cache in Azure.</span></span>
- <span data-ttu-id="5a475-113">Connettere un'app Web alla cache.</span><span class="sxs-lookup"><span data-stu-id="5a475-113">Connect a web app to the cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a475-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5a475-114">Prerequisites</span></span>

- <span data-ttu-id="5a475-115">Esperienza con lo sviluppo di app</span><span class="sxs-lookup"><span data-stu-id="5a475-115">Experience with app development</span></span>
- <span data-ttu-id="5a475-116">Esperienza con l'uso dei dati nelle app</span><span class="sxs-lookup"><span data-stu-id="5a475-116">Experience using data in apps</span></span>