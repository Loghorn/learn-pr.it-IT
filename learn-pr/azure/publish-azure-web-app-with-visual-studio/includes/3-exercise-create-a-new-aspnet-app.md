<span data-ttu-id="e6982-101">Ora che l'app è pronta e in esecuzione nel computer locale, è possibile pubblicarla in Azure.</span><span class="sxs-lookup"><span data-stu-id="e6982-101">Now that you've got your app up and running on your local machine, it's time to get it published to Azure.</span></span> 

<span data-ttu-id="e6982-102">In questa unità verrà creata, compilata ed eseguita una nuova applicazione Web ASP.NET nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="e6982-102">In this unit, you will create, build, and run a new ASP.NET web application on your local machine.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="e6982-103">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="e6982-103">Create a new project</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="e6982-104">Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="e6982-104">Visual Studio for Windows</span></span>

<span data-ttu-id="e6982-105">Il primo passaggio è avviare Visual Studio e creare un'applicazione Web ASP.NET Core locale.</span><span class="sxs-lookup"><span data-stu-id="e6982-105">The first step is to start Visual Studio and create a local ASP.NET Core web application.</span></span>

1. <span data-ttu-id="e6982-106">Nella pagina iniziale di Visual Studio selezionare **File**, quindi fare clic su **Nuovo** e su **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="e6982-106">On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.</span></span>

1. <span data-ttu-id="e6982-107">Nella finestra di dialogo **Nuovo progetto**, nel riquadro di sinistra, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="e6982-107">In the **New Project** dialog box, on the left-hand pane, select **Web**.</span></span>

1. <span data-ttu-id="e6982-108">Nel riquadro centrale fare clic su **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e6982-108">In the center pane, click **ASP.NET Core Web Application**.</span></span>

1. <span data-ttu-id="e6982-109">Nella parte inferiore della finestra di dialogo, nel campo **Nome**, immettere **Alpine Ski House**.</span><span class="sxs-lookup"><span data-stu-id="e6982-109">At the bottom of the dialog box, in the **Name** field, enter **Alpine Ski House**.</span></span>

1. <span data-ttu-id="e6982-110">Selezionare una **Posizione** per la nuova soluzione.</span><span class="sxs-lookup"><span data-stu-id="e6982-110">Select a **Location** for your new solution.</span></span>

1. <span data-ttu-id="e6982-111">Fare clic sul pulsante **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="e6982-111">Click the **OK** button to create your project.</span></span>

1. <span data-ttu-id="e6982-112">Nella finestra di dialogo **Nuova applicazione Web di ASP.NET Core** verrà visualizzata una selezione di modelli di partenza.</span><span class="sxs-lookup"><span data-stu-id="e6982-112">In the **New ASP.NET Core Web Application** dialog box, you will see a selection of starting templates.</span></span> <span data-ttu-id="e6982-113">Per questo esercizio, selezionare **Applicazione Web** e quindi fare clic su **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="e6982-113">For this exercise, select **Web Application**, and then click **OK** to create your project.</span></span>

    ![Finestra di dialogo Nuovo progetto](../media-draft/3-aspnet-templates.png)

    > [!NOTE]
    > <span data-ttu-id="e6982-115">È anche possibile selezionare modelli di partenza diversi nella finestra di dialogo in base alle esigenze di sviluppo Web.</span><span class="sxs-lookup"><span data-stu-id="e6982-115">You can also select different starting templates in this dialog box depending on your web development requirements.</span></span> <span data-ttu-id="e6982-116">Nella parte superiore della finestra di dialogo, è anche possibile selezionare la versione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6982-116">At the top of the dialog box, you are also able to select the version of ASP.NET Core.</span></span> <span data-ttu-id="e6982-117">È consigliabile selezionare ASP.NET Core 2.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="e6982-117">You should select ASP.NET Core 2.0 or later.</span></span>

1. <span data-ttu-id="e6982-118">Dovrebbe essere ora disponibile la nuova soluzione per un'applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6982-118">You should now have your new ASP.NET Core web application solution.</span></span>

    ![Finestra di dialogo Nuovo progetto](../media-draft/3-new-solution.png)

### <a name="visual-studio-mac"></a><span data-ttu-id="e6982-120">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e6982-120">Visual Studio Mac</span></span>

1. <span data-ttu-id="e6982-121">Nella pagina iniziale di Visual Studio selezionare **File**, quindi fare clic su **Nuovo** e su **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="e6982-121">On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.</span></span>

1. <span data-ttu-id="e6982-122">In **.NET Core** selezionare **App Web ASP.NET Core** e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e6982-122">Under .**NET Core**, select an **ASP.NET Core Web App**, and then click **Next**.</span></span>

1. <span data-ttu-id="e6982-123">In **Nome progetto** digitare **AlpineSkiHouse**.</span><span class="sxs-lookup"><span data-stu-id="e6982-123">For the **Project Name**, type **AlpineSkiHouse**.</span></span> <span data-ttu-id="e6982-124">In questo modo viene popolato automaticamente anche il nome della soluzione.</span><span class="sxs-lookup"><span data-stu-id="e6982-124">This should also auto-populate the solution name.</span></span>

1. <span data-ttu-id="e6982-125">Selezionare una **posizione** nel computer locale per il progetto.</span><span class="sxs-lookup"><span data-stu-id="e6982-125">Select a **location** on your local machine for the project.</span></span>

1. <span data-ttu-id="e6982-126">Fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e6982-126">Click **Create**.</span></span>

## <a name="build-and-test-on-your-local-machine"></a><span data-ttu-id="e6982-127">Compilare e testare nel computer locale</span><span class="sxs-lookup"><span data-stu-id="e6982-127">Build and test on your local machine</span></span>

<span data-ttu-id="e6982-128">A questo punto, è possibile compilare e testare il nuovo progetto nel computer locale per assicurarsi di compilarlo e distribuirlo in locale prima della distribuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="e6982-128">Now, let's build and test your new project on your local machine to ensure it builds and deploys locally before deploying to Azure.</span></span>

1. <span data-ttu-id="e6982-129">Premere **F5** per eseguire l'app in modalità di debug oppure **CTRL+F5** per eseguirla senza collegare il debugger.</span><span class="sxs-lookup"><span data-stu-id="e6982-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span>

    ![Finestra di dialogo Nuovo progetto](../media-draft/3-webapp-launch.png)

<span data-ttu-id="e6982-131">Visual Studio avvia IIS Express ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="e6982-131">Visual Studio starts IIS Express and runs the app.</span></span> <span data-ttu-id="e6982-132">Quando Visual Studio crea un progetto Web, viene usata una porta casuale per il server Web.</span><span class="sxs-lookup"><span data-stu-id="e6982-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e6982-133">Nell'immagine precedente il numero di porta è 44381.</span><span class="sxs-lookup"><span data-stu-id="e6982-133">In the preceding image, the port number is 44381.</span></span> <span data-ttu-id="e6982-134">Quando si esegue l'app, si noterà probabilmente un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="e6982-134">When you run the app, you'll likely see a different port number.</span></span>

> [!TIP]
> <span data-ttu-id="e6982-135">L'avvio dell'app con **CTRL+F5** (modalità non di debug) consente di apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche al codice.</span><span class="sxs-lookup"><span data-stu-id="e6982-135">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e6982-136">Molti sviluppatori preferiscono usare la modalità non di debug per avviare l'app rapidamente e visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e6982-136">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e6982-137">È possibile notare la sezione nella parte superiore della pagina Web, che offre una posizione per la privacy e i criteri di uso dei cookie.</span><span class="sxs-lookup"><span data-stu-id="e6982-137">You might notice the section at the top of the web page that provides a place for your privacy and cookie use policy.</span></span> <span data-ttu-id="e6982-138">Selezionare **Accetto** per acconsentire al rilevamento.</span><span class="sxs-lookup"><span data-stu-id="e6982-138">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e6982-139">Questa app non tiene traccia delle informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="e6982-139">This app doesn't track personal information.</span></span> <span data-ttu-id="e6982-140">Il codice generato dal modello include asset che consentono di soddisfare i requisiti del Regolamento generale sulla protezione dei dati (GDPR).</span><span class="sxs-lookup"><span data-stu-id="e6982-140">The template-generated code includes assets to help meet General Data Protection Regulation (GDPR).</span></span>

## <a name="summary"></a><span data-ttu-id="e6982-141">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e6982-141">Summary</span></span>

<span data-ttu-id="e6982-142">Il primo passaggio per rendere operativo il sito ASP.NET consiste nel crearlo ed eseguirlo in locale.</span><span class="sxs-lookup"><span data-stu-id="e6982-142">The first step to getting your ASP.NET site up and running is to create it and run it locally.</span></span> <span data-ttu-id="e6982-143">Ora che il sito è stato creato, si è pronti per distribuirlo in Azure.</span><span class="sxs-lookup"><span data-stu-id="e6982-143">Now that your site is created, you are ready to deploy it to Azure.</span></span>
