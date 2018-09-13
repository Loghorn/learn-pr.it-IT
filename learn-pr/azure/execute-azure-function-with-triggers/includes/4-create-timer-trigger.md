<span data-ttu-id="40f17-101">In questa unità si crea una funzione di Azure che verrà richiamata ogni 20 secondi con un trigger timer.</span><span class="sxs-lookup"><span data-stu-id="40f17-101">In this unit, we create an Azure function that's invoked every 20 seconds using a timer trigger.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="40f17-102">Creare una funzione di Azure</span><span class="sxs-lookup"><span data-stu-id="40f17-102">Create an Azure function</span></span>

<span data-ttu-id="40f17-103">Per iniziare, creare una funzione di Azure nel portale.</span><span class="sxs-lookup"><span data-stu-id="40f17-103">Let’s start by creating an Azure Function in the portal.</span></span>

1. <span data-ttu-id="40f17-104">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="40f17-104">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="40f17-105">Nel riquadro di spostamento a sinistra selezionare **Crea una risorsa**.</span><span class="sxs-lookup"><span data-stu-id="40f17-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="40f17-106">Selezionare **Calcolo**.</span><span class="sxs-lookup"><span data-stu-id="40f17-106">Select **Compute**.</span></span>

1. <span data-ttu-id="40f17-107">Individuare e selezionare **App per le funzioni**.</span><span class="sxs-lookup"><span data-stu-id="40f17-107">Locate and select **Function App**.</span></span> <span data-ttu-id="40f17-108">Facoltativamente, è anche possibile usare la barra di ricerca per individuare il modello.</span><span class="sxs-lookup"><span data-stu-id="40f17-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Selezionare App per le funzioni](../media-drafts/4-click-function-app.png)

1. <span data-ttu-id="40f17-110">Immettere un **nome dell'app** univoco.</span><span class="sxs-lookup"><span data-stu-id="40f17-110">Enter a unique **App name**.</span></span>

1. <span data-ttu-id="40f17-111">Selezionare una **sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="40f17-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="40f17-112">Creare un nuovo **gruppo di risorse**.</span><span class="sxs-lookup"><span data-stu-id="40f17-112">Create a new **Resource Group**.</span></span>

1. <span data-ttu-id="40f17-113">Scegliere **Windows** come **sistema operativo**.</span><span class="sxs-lookup"><span data-stu-id="40f17-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="40f17-114">Scegliere **Piano A consumo** in **Piano di hosting**.</span><span class="sxs-lookup"><span data-stu-id="40f17-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="40f17-115">Verranno addebitati costi per ogni esecuzione della funzione.</span><span class="sxs-lookup"><span data-stu-id="40f17-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="40f17-116">Le risorse verranno allocate automaticamente in base al carico di lavoro dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="40f17-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="40f17-117">Selezionare una **località**.</span><span class="sxs-lookup"><span data-stu-id="40f17-117">Select a **Location**.</span></span>

1. <span data-ttu-id="40f17-118">Creare un nuovo account di **archiviazione**. Per impostazione predefinita, il nome sarà una variante del nome dell'app, ma è possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="40f17-118">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name</span></span>

1. <span data-ttu-id="40f17-119">Disattivare **Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="40f17-119">Turn off **Application Insights**.</span></span>

1. <span data-ttu-id="40f17-120">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="40f17-120">Select **Create**.</span></span> <span data-ttu-id="40f17-121">Il completamento richiederà alcuni minuti. Al termine della creazione della risorsa, l'icona **Notifiche** nell'area della barra degli strumenti avrà un pulsante per aprirla nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="40f17-121">This will take a few minutes to complete, you can watch the **Notifications** icon in the toolbar area - once it has finished creating the resource it will have a button there to open it in the Azure Portal.</span></span>

## <a name="create-a-timer-trigger"></a><span data-ttu-id="40f17-122">Creare un trigger timer</span><span class="sxs-lookup"><span data-stu-id="40f17-122">Create a timer trigger</span></span>

<span data-ttu-id="40f17-123">Verrà ora creato un trigger timer all'interno della funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="40f17-123">Now we're going to create a timer trigger inside our Azure function.</span></span>

1. <span data-ttu-id="40f17-124">Al termine della creazione della funzione di Azure, selezionare **Tutte le risorse** nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="40f17-124">After the Azure function is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="40f17-125">Individuare e selezionare la funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="40f17-125">Locate and select your Azure function.</span></span>

1. <span data-ttu-id="40f17-126">Nel nuovo pannello scegliere **Funzioni** e selezionare l'icona con il segno più (+).</span><span class="sxs-lookup"><span data-stu-id="40f17-126">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Selezionare Funzioni, quindi il segno più](../media-drafts/4-hover-function.png)

1. <span data-ttu-id="40f17-128">Selezionare **Timer**.</span><span class="sxs-lookup"><span data-stu-id="40f17-128">Select **Timer**.</span></span>

1. <span data-ttu-id="40f17-129">Selezionare **CSharp** come linguaggio.</span><span class="sxs-lookup"><span data-stu-id="40f17-129">Select **CSharp** as the language.</span></span>

1. <span data-ttu-id="40f17-130">Selezionare **Creare questa funzione**.</span><span class="sxs-lookup"><span data-stu-id="40f17-130">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="40f17-131">Configurare il trigger timer</span><span class="sxs-lookup"><span data-stu-id="40f17-131">Configure the timer trigger</span></span>

<span data-ttu-id="40f17-132">Si ha una funzione di Azure la cui logica stampa un messaggio nella finestra di log.</span><span class="sxs-lookup"><span data-stu-id="40f17-132">We have an Azure function with logic to print a message to the log window.</span></span> <span data-ttu-id="40f17-133">Si imposterà la pianificazione del timer per l'esecuzione ogni 20 secondi.</span><span class="sxs-lookup"><span data-stu-id="40f17-133">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="40f17-134">Selezionare **Integrazione**.</span><span class="sxs-lookup"><span data-stu-id="40f17-134">Select **Integrate**.</span></span>

1. <span data-ttu-id="40f17-135">Immettere il valore seguente nella casella **Pianifica**:</span><span class="sxs-lookup"><span data-stu-id="40f17-135">Enter the following value into the **Schedule** box:</span></span>

    ```
    */20 * * * * *
    ```

1. <span data-ttu-id="40f17-136">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="40f17-136">Select **Save**.</span></span>

## <a name="start-the-timer"></a><span data-ttu-id="40f17-137">Avviare il timer</span><span class="sxs-lookup"><span data-stu-id="40f17-137">Start the timer</span></span>

<span data-ttu-id="40f17-138">Dopo aver configurato il timer, è possibile avviarlo.</span><span class="sxs-lookup"><span data-stu-id="40f17-138">Now that we've configured the timer, we're ready to start it.</span></span>

1. <span data-ttu-id="40f17-139">Selezionare **TimerTriggerCSharp1**.</span><span class="sxs-lookup"><span data-stu-id="40f17-139">Select **TimerTriggerCSharp1**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="40f17-140">**TimerTriggerCSharp1** è un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="40f17-140">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="40f17-141">Viene selezionato automaticamente quando si crea il trigger.</span><span class="sxs-lookup"><span data-stu-id="40f17-141">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="40f17-142">Selezionare **Esegui**.</span><span class="sxs-lookup"><span data-stu-id="40f17-142">Select **Run**.</span></span> 

<span data-ttu-id="40f17-143">A questo punto, nella finestra di log dovrebbe essere visualizzato un messaggio ogni 20 secondi.</span><span class="sxs-lookup"><span data-stu-id="40f17-143">At this point, you should see a message every 20 seconds in the log window.</span></span>

## <a name="clean-up"></a><span data-ttu-id="40f17-144">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="40f17-144">Clean up</span></span>

<span data-ttu-id="40f17-145">Per evitare che vengano addebitati costi per questa funzione, selezionare **Sospendi** sopra la finestra di log per arrestare il timer.</span><span class="sxs-lookup"><span data-stu-id="40f17-145">To ensure that you aren't charged for this function, above the log window, select **Pause** to stop the timer.</span></span>

![Sospensione](../media-drafts/4-pause-timer.png)
