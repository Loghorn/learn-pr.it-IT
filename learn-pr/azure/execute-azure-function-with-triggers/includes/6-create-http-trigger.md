<span data-ttu-id="8dc1b-101">In questa unità verrà creata una funzione di Azure che accetta una richiesta HTTP con un'unica stringa.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-101">In this unit, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="8dc1b-102">La funzione restituisce una stringa al chiamante per indicare l'esito positivo o negativo.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-102">The function returns a string back to the caller to represent success or failure.</span></span> <span data-ttu-id="8dc1b-103">Continueremo a usare le funzioni partendo dall'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-103">We'll continue working on the function from the previous exercise.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="8dc1b-104">Creare un trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="8dc1b-104">Create an HTTP trigger</span></span>

<span data-ttu-id="8dc1b-105">Continuare a usare l'applicazione Funzioni di Azure esistente e aggiungere un trigger HTTP.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="8dc1b-106">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-106">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="8dc1b-107">Passare alla schermata **Tutte le risorse** e selezionare l'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-107">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="8dc1b-108">Selezionare **Funzioni** e quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="8dc1b-108">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="8dc1b-109">Selezionare **HTTP trigger** (Trigger HTTP).</span><span class="sxs-lookup"><span data-stu-id="8dc1b-109">Select **HTTP trigger**.</span></span>

1. <span data-ttu-id="8dc1b-110">Selezionare il linguaggio di programmazione **C#**.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-110">Select **C#** as the language.</span></span>

1. <span data-ttu-id="8dc1b-111">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-111">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="8dc1b-112">Impostare il **livello di autorizzazione** su **Anonimo**.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-112">Set the **Authorization level** to **Anonymous**.</span></span>

1. <span data-ttu-id="8dc1b-113">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-113">Select **Create**.</span></span>

1. <span data-ttu-id="8dc1b-114">Verificare rapidamente il codice generato automaticamente per capirne i contenuti.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-114">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="8dc1b-115">Il parametro *req* rappresenta la richiesta in ingresso e contiene un parametro *name*.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-115">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="8dc1b-116">È possibile controllare se *name* ha un valore.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-116">We check to see if *name* has a value.</span></span> <span data-ttu-id="8dc1b-117">In caso affermativo, viene restituito un messaggio di saluto.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-117">If it does, we return a greeting.</span></span> <span data-ttu-id="8dc1b-118">In caso contrario, viene restituito un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-118">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="8dc1b-119">Ottenere l'URL della funzione</span><span class="sxs-lookup"><span data-stu-id="8dc1b-119">Get your function URL</span></span>

<span data-ttu-id="8dc1b-120">Dopo aver creato il trigger HTTP, è possibile ottenere l'URL della funzione così da poter iniziare a effettuare una richiesta.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-120">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="8dc1b-121">Selezionare il trigger HTTP per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-121">Select your HTTP trigger to open the code screen.</span></span>

1. <span data-ttu-id="8dc1b-122">A destra di **Esegui** selezionare **Ottieni URL della funzione**.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-122">To the right of **Run**, select **Get function URL**.</span></span>

1. <span data-ttu-id="8dc1b-123">Selezionare **Copia** e quindi chiudere la finestra popup URL della funzione.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-123">Select **Copy**, then close the function URL popup.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="8dc1b-124">Inviare una richiesta GET al trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="8dc1b-124">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="8dc1b-125">L'URL della funzione è stato copiato negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-125">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="8dc1b-126">È possibile inviare una richiesta GET per vedere se si ottiene una risposta.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-126">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="8dc1b-127">Aprire una nuova scheda nel Web browser.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-127">Open a new tab in your web browser.</span></span>

1. <span data-ttu-id="8dc1b-128">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-128">Paste the URL into the address bar.</span></span>

1. <span data-ttu-id="8dc1b-129">Aggiungere un parametro della stringa di query denominato *name* con il nome, ad esempio `.../api/HttpTriggerCSharp1?name=Jesse`</span><span class="sxs-lookup"><span data-stu-id="8dc1b-129">Add a query string parameter called *name* with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`</span></span>

1. <span data-ttu-id="8dc1b-130">Premere <kbd>INVIO</kbd> per inviare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="8dc1b-130">Press <kbd>ENTER</kbd> to submit the request.</span></span>
