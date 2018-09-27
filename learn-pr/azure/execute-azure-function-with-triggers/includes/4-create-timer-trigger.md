<span data-ttu-id="afc1c-101">In questa unità viene creata un'app per le funzioni di Azure che verrà richiamata ogni 20 secondi con un trigger timer.</span><span class="sxs-lookup"><span data-stu-id="afc1c-101">In this unit, we create an Azure function app that's invoked every 20 seconds using a timer trigger.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="afc1c-102">Creare un'app per le funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="afc1c-102">Create an Azure function app</span></span>

<span data-ttu-id="afc1c-103">Per iniziare, creare un'app per le Funzioni di Azure nel portale.</span><span class="sxs-lookup"><span data-stu-id="afc1c-103">Let’s start by creating an Azure Function app in the portal.</span></span>

1. <span data-ttu-id="afc1c-104">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="afc1c-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="afc1c-105">Nel riquadro di spostamento a sinistra selezionare **Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="afc1c-106">Selezionare **Calcolo**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-106">Select **Compute**.</span></span>

1. <span data-ttu-id="afc1c-107">Individuare e selezionare **App per le funzioni**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-107">Locate and select **Function App**.</span></span> <span data-ttu-id="afc1c-108">Facoltativamente, è anche possibile usare la barra di ricerca per individuare il modello.</span><span class="sxs-lookup"><span data-stu-id="afc1c-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Screenshot del portale di Azure che illustra il pannello Crea una risorsa con App per le funzioni evidenziato.](../media/4-click-function-app.png)

1. <span data-ttu-id="afc1c-110">Scegliere un **Nome dell'app** globalmente univoco.</span><span class="sxs-lookup"><span data-stu-id="afc1c-110">Enter a globally unique **App name**.</span></span>

1. <span data-ttu-id="afc1c-111">Selezionare una **Sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="afc1c-112">Selezionare il **Gruppo di risorse** esistente <rgn>[nome gruppo di risorse sandbox]</rgn>.</span><span class="sxs-lookup"><span data-stu-id="afc1c-112">Select the existing **Resource group** <rgn>[sandbox resource group name]</rgn>.</span></span>

1. <span data-ttu-id="afc1c-113">Scegliere **Windows** come **sistema operativo**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="afc1c-114">Scegliere **Piano A consumo** in **Piano di hosting**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="afc1c-115">Verranno addebitati costi per ogni esecuzione della funzione.</span><span class="sxs-lookup"><span data-stu-id="afc1c-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="afc1c-116">Le risorse verranno allocate automaticamente in base al carico di lavoro dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="afc1c-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="afc1c-117">Selezionare una **Posizione** dall'elenco disponibile di seguito.</span><span class="sxs-lookup"><span data-stu-id="afc1c-117">Select a **Location** from the available list below.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="afc1c-118">Creare un nuovo account di **Archiviazione**. Per impostazione predefinita, il nome sarà una variante del nome dell'app, ma è possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="afc1c-118">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name.</span></span>

1. <span data-ttu-id="afc1c-119">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-119">Select **Create**.</span></span> <span data-ttu-id="afc1c-120">Al completamento della distribuzione dell'app per le funzioni, accedere al portale e selezionare **Tutte le risorse**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-120">Once the function app is deployed, go to **All resources** in the portal.</span></span> <span data-ttu-id="afc1c-121">L'app per le funzioni sarà inclusa nell'elenco con il tipo **Servizio app** e il nome che le è stato assegnato.</span><span class="sxs-lookup"><span data-stu-id="afc1c-121">The function app will be listed with type **App Service** and has the name you gave it.</span></span>

## <a name="create-a-timer-trigger"></a><span data-ttu-id="afc1c-122">Creare un trigger timer</span><span class="sxs-lookup"><span data-stu-id="afc1c-122">Create a timer trigger</span></span>

<span data-ttu-id="afc1c-123">Verrà ora creato un trigger timer all'interno della funzione.</span><span class="sxs-lookup"><span data-stu-id="afc1c-123">Now we're going to create a timer trigger inside our function.</span></span>

1. <span data-ttu-id="afc1c-124">Una volta creata la funzione, selezionare **Tutte le risorse** nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="afc1c-124">After the function is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="afc1c-125">Trovare l'app per le funzioni nell'elenco e selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="afc1c-125">Find your function app in the list and select it.</span></span>

1. <span data-ttu-id="afc1c-126">Nel nuovo pannello scegliere **Funzioni** e selezionare l'icona con il segno più (+).</span><span class="sxs-lookup"><span data-stu-id="afc1c-126">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Screenshot del portale di Azure che illustra un pannello App per le funzioni con il pulsante (+) del sottomenu Funzioni evidenziato.](../media/4-hover-function.png)

1. <span data-ttu-id="afc1c-128">Selezionare **Timer**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-128">Select **Timer**.</span></span>

1. <span data-ttu-id="afc1c-129">Selezionare **CSharp** come linguaggio.</span><span class="sxs-lookup"><span data-stu-id="afc1c-129">Select **CSharp** as the language.</span></span>

1. <span data-ttu-id="afc1c-130">Selezionare **Creare questa funzione**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-130">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="afc1c-131">Configurare il trigger timer</span><span class="sxs-lookup"><span data-stu-id="afc1c-131">Configure the timer trigger</span></span>

<span data-ttu-id="afc1c-132">Si ha un'app per le funzioni di Azure la cui logica stampa un messaggio nella finestra di log.</span><span class="sxs-lookup"><span data-stu-id="afc1c-132">We have an Azure function app with logic to print a message to the log window.</span></span> <span data-ttu-id="afc1c-133">Ora verrà impostata la pianificazione del timer per l'esecuzione ogni 20 secondi.</span><span class="sxs-lookup"><span data-stu-id="afc1c-133">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="afc1c-134">Selezionare **Integrazione**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-134">Select **Integrate**.</span></span>

1. <span data-ttu-id="afc1c-135">Immettere il valore seguente nella casella **Pianifica**:</span><span class="sxs-lookup"><span data-stu-id="afc1c-135">Enter the following value into the **Schedule** box:</span></span>

    ```log
    */20 * * * * *
    ```

1. <span data-ttu-id="afc1c-136">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-136">Select **Save**.</span></span>

## <a name="test-the-timer"></a><span data-ttu-id="afc1c-137">Testare il timer</span><span class="sxs-lookup"><span data-stu-id="afc1c-137">Test the timer</span></span>

<span data-ttu-id="afc1c-138">Ora che è stato configurato, il timer richiama la funzione nell'intervallo definito.</span><span class="sxs-lookup"><span data-stu-id="afc1c-138">Now that we've configured the timer, it will invoke the function on the interval we defined.</span></span>

1. <span data-ttu-id="afc1c-139">Selezionare **TimerTriggerCSharp1**.</span><span class="sxs-lookup"><span data-stu-id="afc1c-139">Select **TimerTriggerCSharp1**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="afc1c-140">**TimerTriggerCSharp1** è un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="afc1c-140">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="afc1c-141">Viene selezionato automaticamente quando si crea il trigger.</span><span class="sxs-lookup"><span data-stu-id="afc1c-141">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="afc1c-142">Aprire il pannello **Log** nella parte inferiore della schermata.</span><span class="sxs-lookup"><span data-stu-id="afc1c-142">Open the **Logs** panel at the bottom of the screen.</span></span>

1. <span data-ttu-id="afc1c-143">Osservare che i nuovi messaggi arrivano ogni 20 secondi nella finestra del log.</span><span class="sxs-lookup"><span data-stu-id="afc1c-143">Observe new messages arrive every 20 seconds in the log window.</span></span>

1. <span data-ttu-id="afc1c-144">Per interrompere l'esecuzione della funzione, selezionare **Gestisci** e quindi impostare **Stato funzione** su *Disabilitato*.</span><span class="sxs-lookup"><span data-stu-id="afc1c-144">To stop the function from running, select **Manage** and then switch **Function State** to *Disabled*.</span></span>
