<span data-ttu-id="e1efc-101">L'applicazione in fase di compilazione è un'app per dispositivi mobili multipiattaforma che comunica con una funzione di Azure per condividere il percorso.</span><span class="sxs-lookup"><span data-stu-id="e1efc-101">The application you're building is a cross-platform mobile app that talks to an Azure function to share your location.</span></span> <span data-ttu-id="e1efc-102">In questa unità si creerà l'app per dispositivi mobili vuota mediante Visual Studio e si installerà un pacchetto NuGet con un'API per ottenere la posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="e1efc-102">In this unit, you create the blank mobile app using Visual Studio and install a NuGet package that has an API for getting the user's location.</span></span>

## <a name="create-the-xamarinforms-project"></a><span data-ttu-id="e1efc-103">Creare il progetto Xamarin.Forms</span><span class="sxs-lookup"><span data-stu-id="e1efc-103">Create the Xamarin.Forms project</span></span>

1. <span data-ttu-id="e1efc-104">In Visual Studio selezionare *File->Nuovo->Progetto...*.</span><span class="sxs-lookup"><span data-stu-id="e1efc-104">From Visual Studio, select *File->New->Project...*.</span></span>

1. <span data-ttu-id="e1efc-105">Dall'albero sul lato sinistro selezionare *Visual C#->Multipiattaforma* e quindi *App per dispositivi mobili (Xamarin.Forms)* dal pannello al centro.</span><span class="sxs-lookup"><span data-stu-id="e1efc-105">From the tree on the left-hand side, select *Visual C#->Cross-Platform* and then select *Mobile App (Xamarin.Forms)* from the panel in the center.</span></span>

1. <span data-ttu-id="e1efc-106">Denominare la soluzione "ImHere".</span><span class="sxs-lookup"><span data-stu-id="e1efc-106">Name the solution "ImHere".</span></span>

1. <span data-ttu-id="e1efc-107">Scegliere un percorso appropriato per la soluzione.</span><span class="sxs-lookup"><span data-stu-id="e1efc-107">Choose an appropriate location for the solution.</span></span>

1. <span data-ttu-id="e1efc-108">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1efc-108">Click **OK**.</span></span>

    ![Finestra di dialogo Nuova soluzione](../media/2-new-solution-dialog.png)

1. <span data-ttu-id="e1efc-110">Nella finestra di dialogo **Nuova app multipiattaforma** selezionare il modello *App vuota*.</span><span class="sxs-lookup"><span data-stu-id="e1efc-110">From the **New Cross Platform App** dialog, select the *Blank App* template.</span></span>

1. <span data-ttu-id="e1efc-111">Per questo modulo verrà compilata un'app UWP: deselezionare iOS e Android e lasciare UWP selezionato.</span><span class="sxs-lookup"><span data-stu-id="e1efc-111">For this module you will build a UWP app, so uncheck iOS and Android and leave UWP checked.</span></span>

1. <span data-ttu-id="e1efc-112">Per *Strategia di condivisione codice* selezionare **.NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="e1efc-112">For the *Code Sharing Strategy*, select **.NET Standard**.</span></span>

1. <span data-ttu-id="e1efc-113">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e1efc-113">Click **OK**.</span></span>

    ![Finestra di dialogo Configura la nuova soluzione](../media/2-configure-solution-dialog.png)

<span data-ttu-id="e1efc-115">Visual Studio creerà due progetti: un'app UWP denominata `ImHere.UWP` e una libreria .NET Standard, `ImHere`.</span><span class="sxs-lookup"><span data-stu-id="e1efc-115">Visual Studio will create two projects for you - a UWP app called `ImHere.UWP` and a .NET Standard library, `ImHere`.</span></span> <span data-ttu-id="e1efc-116">Le app Xamarin.Forms sono costituite da due parti: uno o più progetti di app specifici della piattaforma e una o più librerie .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e1efc-116">Xamarin.Forms apps are made up of two parts - one or more platform-specific app projects and one (or more) .NET Standard libraries.</span></span> <span data-ttu-id="e1efc-117">I progetti di app specifici della piattaforma contengono il codice specifico della piattaforma necessario per eseguire un'app nella piattaforma pertinente.</span><span class="sxs-lookup"><span data-stu-id="e1efc-117">The platform-specific app projects contain the platform-specific code needed to run an app on the relevant platform.</span></span> <span data-ttu-id="e1efc-118">Questi progetti avviano quindi un'app Xamarin.Forms definita in una libreria .NET Standard multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="e1efc-118">These projects then launch a Xamarin.Forms app that is defined in a cross-platform .NET Standard library.</span></span> <span data-ttu-id="e1efc-119">L'app viene compilata in codice multipiattaforma e, in fase di esecuzione, eventuali interfacce utente create verranno convertite nei componenti dell'interfaccia utente specifici della piattaforma pertinente.</span><span class="sxs-lookup"><span data-stu-id="e1efc-119">You build your app in cross-platform code and, at runtime, any user interfaces you create are translated into the relevant platform-specific UI components.</span></span>

## <a name="adding-xamarinessentials"></a><span data-ttu-id="e1efc-120">Aggiunta di Xamarin.Essentials</span><span class="sxs-lookup"><span data-stu-id="e1efc-120">Adding Xamarin.Essentials</span></span>

<span data-ttu-id="e1efc-121">Le piattaforme iOS, Android e UWP forniscono numerose funzionalità simili che sfruttano il sistema operativo e l'hardware.</span><span class="sxs-lookup"><span data-stu-id="e1efc-121">The UWP, Android, and iOS platforms provide numerous similar capabilities that take advantage of the operating system and hardware.</span></span> <span data-ttu-id="e1efc-122">Nonostante queste analogie, le API sono molto diverse.</span><span class="sxs-lookup"><span data-stu-id="e1efc-122">Despite these similarities, the APIs are very different.</span></span> <span data-ttu-id="e1efc-123">L'uso di queste API dal codice multipiattaforma richiede la scrittura di codice specifico della piattaforma nei progetti di app esposti alle librerie .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e1efc-123">Using these APIs from cross-platform code requires writing platform-specific code in your app projects that you expose to your .NET Standard libraries.</span></span> <span data-ttu-id="e1efc-124">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) è un pacchetto NuGet che fornisce un'astrazione multipiattaforma per numerose API di questo tipo, pertanto non è necessario scrivere codice specifico della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="e1efc-124">[Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) is a NuGet package that provides a cross-platform abstraction over a number of these APIs so that you don't need to write platform-specific code.</span></span> <span data-ttu-id="e1efc-125">Ciò include le API di georilevazione API che verranno usate nell'app per ottenere la posizione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="e1efc-125">This includes the geolocation APIs that you will use in your app to get the user's location.</span></span>

1. <span data-ttu-id="e1efc-126">In Esplora soluzioni di Visual Studio fare clic con il pulsante destro del mouse sulla soluzione `ImHere` (la soluzione più in alto, non il progetto .NET Standard `ImHere`) e scegliere *Gestisci pacchetti NuGet per la soluzione...*.</span><span class="sxs-lookup"><span data-stu-id="e1efc-126">Right-click on the `ImHere` solution (the top level solution, not the `ImHere` .NET Standard project) in the Visual Studio Solution Explorer and select *Manage NuGet Packages for Solution...*.</span></span>

1. <span data-ttu-id="e1efc-127">Selezionare la scheda **Sfoglia** e cercare "Xamarin.Essentials".</span><span class="sxs-lookup"><span data-stu-id="e1efc-127">Select the **Browse** tab and search for "Xamarin.Essentials".</span></span> <span data-ttu-id="e1efc-128">Questo pacchetto è attualmente disponibile come versione preliminare del pacchetto NuGet, quindi selezionare la casella *Includi versione preliminare*.</span><span class="sxs-lookup"><span data-stu-id="e1efc-128">This package is currently available as a prerelease NuGet package, so check the *include prelease* box.</span></span>

    > [!TIP]
    > <span data-ttu-id="e1efc-129">Se non viene visualizzato il pacchetto Xamarin.Essentials NuGet, controllare che sia selezionata la casella *Includi versione preliminare*.</span><span class="sxs-lookup"><span data-stu-id="e1efc-129">If you do not see the Xamarin.Essentials NuGet package, double check that *include prelease* is checked.</span></span> 

1. <span data-ttu-id="e1efc-130">Selezionare il pacchetto NuGet **Xamarin.Essentials**.</span><span class="sxs-lookup"><span data-stu-id="e1efc-130">Select the **Xamarin.Essentials** NuGet package.</span></span>

1. <span data-ttu-id="e1efc-131">Selezionare tutti i progetti nell'elenco sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="e1efc-131">Check all your projects in the project list on the right-hand side.</span></span>

1. <span data-ttu-id="e1efc-132">Fare clic sul pulsante **Installa** per installare il pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="e1efc-132">Click the **Install** button to install the NuGet package.</span></span> <span data-ttu-id="e1efc-133">È necessario accettare le condizioni di licenza per continuare.</span><span class="sxs-lookup"><span data-stu-id="e1efc-133">You'll need to accept the license to continue.</span></span>

    ![Aggiunta del pacchetto NuGet Xamarin.Essentials a tutti i progetti nella soluzione](../media/2-add-essentials-nuget.png)

## <a name="building-and-running-the-app"></a><span data-ttu-id="e1efc-135">Compilazione ed esecuzione dell'app</span><span class="sxs-lookup"><span data-stu-id="e1efc-135">Building and running the app</span></span>

1. <span data-ttu-id="e1efc-136">Fare clic con il pulsante destro del mouse sul progetto `ImHere.UWP` in Esplora soluzioni, quindi scegliere *Imposta come progetto di avvio*.</span><span class="sxs-lookup"><span data-stu-id="e1efc-136">Right-click on the `ImHere.UWP` project in Solution Explorer and select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="e1efc-137">Impostare la configurazione della compilazione su **Debug**, la piattaforma su **x86**e il dispositivo di esecuzione su **Computer locale**.</span><span class="sxs-lookup"><span data-stu-id="e1efc-137">Set the build configuration to **Debug**, the platform to **x86**, and the device to run on to **Local Machine**.</span></span>

    ![Impostazione della configurazione di Debug x86 per l'esecuzione nel dispositivo locale](../media/2-debug-configuration.png)

1. <span data-ttu-id="e1efc-139">Avviare il debug dell'app.</span><span class="sxs-lookup"><span data-stu-id="e1efc-139">Start debugging the app.</span></span>

    ![L'app in esecuzione](../media/2-debuging-app.png)

## <a name="summary"></a><span data-ttu-id="e1efc-141">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e1efc-141">Summary</span></span>

<span data-ttu-id="e1efc-142">In questa unità è stata creata una nuova app per dispositivi mobili multipiattaforma Xamarin.Forms ed è stato aggiunto il pacchetto NuGet Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="e1efc-142">In this unit, you created a new Xamarin.Forms cross-platform mobile app and added the Xamarin.Essentials NuGet package.</span></span> <span data-ttu-id="e1efc-143">Verrà ora illustrato come creare l'interfaccia utente e la logica dell'app per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="e1efc-143">Next, you learn how to build up the mobile app UI and logic.</span></span>