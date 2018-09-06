<span data-ttu-id="5b57b-101">In questo esercizio verrà creata una funzione di Azure che accetta una richiesta HTTP con un'unica stringa.</span><span class="sxs-lookup"><span data-stu-id="5b57b-101">In this exercise, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="5b57b-102">La funzione restituisce una stringa al chiamante per indicare l'esito positivo o negativo.</span><span class="sxs-lookup"><span data-stu-id="5b57b-102">The function returns a string back to the caller to represent success or failure.</span></span>

> [!NOTE]
> <span data-ttu-id="5b57b-103">Per completare questo esercizio, assicurarsi di essere connessi al [portale di Azure](https://portal.azure.com?azure-portal=true) con un account valido.</span><span class="sxs-lookup"><span data-stu-id="5b57b-103">To complete this exercise, make sure you're signed into the [Azure portal](https://portal.azure.com?azure-portal=true) with a valid account.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="5b57b-104">Creare un trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="5b57b-104">Create an HTTP trigger</span></span>

<span data-ttu-id="5b57b-105">È possibile continuare a usare l'applicazione Funzioni di Azure esistente e aggiungere un trigger HTTP.</span><span class="sxs-lookup"><span data-stu-id="5b57b-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="5b57b-106">Selezionare **Funzioni**, quindi l'icona del segno più (+).</span><span class="sxs-lookup"><span data-stu-id="5b57b-106">Point to **Functions** and select the plus (+) icon.</span></span>

    ![Selezionare Funzioni, quindi il segno più (+) al passaggio del mouse](../media-drafts/4-hover-function.png)

2. <span data-ttu-id="5b57b-108">Selezionare **Trigger HTTP**.</span><span class="sxs-lookup"><span data-stu-id="5b57b-108">Select **HTTP trigger**.</span></span>

3. <span data-ttu-id="5b57b-109">Selezionare il linguaggio di programmazione **C#**.</span><span class="sxs-lookup"><span data-stu-id="5b57b-109">Select **C#** as the language.</span></span> 

4. <span data-ttu-id="5b57b-110">Lasciare **Nome** impostato sul valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="5b57b-110">Leave the **Name** set to the default value.</span></span>

5. <span data-ttu-id="5b57b-111">Impostare il **livello di autorizzazione** su **Anonimo**.</span><span class="sxs-lookup"><span data-stu-id="5b57b-111">Set the **Authorization level** to **Anonymous**.</span></span>

6. <span data-ttu-id="5b57b-112">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="5b57b-112">Select **Create**.</span></span>

7. <span data-ttu-id="5b57b-113">Verificare rapidamente il codice generato automaticamente per capirne i contenuti.</span><span class="sxs-lookup"><span data-stu-id="5b57b-113">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="5b57b-114">Il parametro *req* rappresenta la richiesta in ingresso e contiene un parametro *name*.</span><span class="sxs-lookup"><span data-stu-id="5b57b-114">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="5b57b-115">È possibile controllare se *name* ha un valore.</span><span class="sxs-lookup"><span data-stu-id="5b57b-115">We check to see if *name* has a value.</span></span> <span data-ttu-id="5b57b-116">In caso affermativo, viene restituito un messaggio di saluto.</span><span class="sxs-lookup"><span data-stu-id="5b57b-116">If it does, we return a greeting.</span></span> <span data-ttu-id="5b57b-117">In caso contrario, viene restituito un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="5b57b-117">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="5b57b-118">Ottenere l'URL della funzione</span><span class="sxs-lookup"><span data-stu-id="5b57b-118">Get your function URL</span></span>

<span data-ttu-id="5b57b-119">Dopo aver creato il trigger HTTP, è possibile ottenere l'URL della funzione così da poter iniziare a effettuare una richiesta.</span><span class="sxs-lookup"><span data-stu-id="5b57b-119">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="5b57b-120">Selezionare il trigger HTTP per aprire la schermata di codice.</span><span class="sxs-lookup"><span data-stu-id="5b57b-120">Select your HTTP trigger to open the code screen.</span></span>

2. <span data-ttu-id="5b57b-121">A destra di **Esegui** selezionare **Ottieni URL della funzione**.</span><span class="sxs-lookup"><span data-stu-id="5b57b-121">To the right of **Run**, select **Get function URL**.</span></span>

3. <span data-ttu-id="5b57b-122">Selezionare **Copia**.</span><span class="sxs-lookup"><span data-stu-id="5b57b-122">Select **Copy**.</span></span>

4. <span data-ttu-id="5b57b-123">Selezionare **Esegui** per avviare la funzione.</span><span class="sxs-lookup"><span data-stu-id="5b57b-123">Select **Run** to start your function.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="5b57b-124">Inviare una richiesta GET al trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="5b57b-124">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="5b57b-125">L'URL della funzione è stato copiato negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="5b57b-125">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="5b57b-126">È possibile inviare una richiesta GET per vedere se si ottiene una risposta.</span><span class="sxs-lookup"><span data-stu-id="5b57b-126">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="5b57b-127">Aprire una nuova scheda nel Web browser.</span><span class="sxs-lookup"><span data-stu-id="5b57b-127">Open a new tab in your web browser.</span></span>

2. <span data-ttu-id="5b57b-128">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="5b57b-128">Paste the URL into the address bar.</span></span>

3. <span data-ttu-id="5b57b-129">Aggiungere un parametro della stringa di query denominato *name* con il nome, ad esempio `.../api/HttpTriggerCSharp1?name=Jesse`</span><span class="sxs-lookup"><span data-stu-id="5b57b-129">Add a query string parameter called *name* with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`</span></span>

4. <span data-ttu-id="5b57b-130">Premere INVIO per inviare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="5b57b-130">Select ENTER to submit the request.</span></span>

## <a name="clean-up"></a><span data-ttu-id="5b57b-131">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="5b57b-131">Clean up</span></span>

<span data-ttu-id="5b57b-132">Per assicurarsi che non sono previsti costi per questa funzione, selezionare **Sospendi** sopra la finestra del log.</span><span class="sxs-lookup"><span data-stu-id="5b57b-132">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![Sospendi](../media-drafts/4-pause-timer.png)