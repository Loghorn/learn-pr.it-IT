> [!TIP]
> <span data-ttu-id="1a894-101">Il nome utente e la password necessari per accedere alla macchina virtuale si trovano nella scheda **Risorse**.</span><span class="sxs-lookup"><span data-stu-id="1a894-101">The username and password you need to sign in to the VM are located on the **Resources** tab.</span></span>

> [!NOTE]
> <span data-ttu-id="1a894-102">Se si usa un Mac, dopo che la macchina virtuale è stata avviata, per sbloccarla può essere necessario usare l'icona del fulmine sulla barra degli strumenti o l'opzione **Ctrl+Alt+Canc** nella scheda **Risorse** accanto alle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="1a894-102">If you are using a Mac, after launching the VM you may need to use either the lightning icon on the toolbar, or the **Ctrl+Alt+Delete** option from the **Resources** tab next to the instructions to unlock the VM.</span></span>


<span data-ttu-id="1a894-103">In questo modulo si creerà un'app Xamarin.Forms multipiattaforma con un back-end serverless.</span><span class="sxs-lookup"><span data-stu-id="1a894-103">In this module, you'll create a cross-platform Xamarin.Forms app with a serverless back end.</span></span> <span data-ttu-id="1a894-104">L'app otterrà dal dispositivo di un utente la posizione in cui si trova l'utente stesso e la invierà con un elenco di numeri di telefono a una funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="1a894-104">This app will get the user's location from their device and send it with a list of phone numbers to an Azure function.</span></span> <span data-ttu-id="1a894-105">La funzione userà quindi un'associazione a un servizio di terze parti (Twilio) per inviare la posizione in un messaggio SMS a tutti i numeri di telefono elencati.</span><span class="sxs-lookup"><span data-stu-id="1a894-105">The function will then use a binding to a third-party service (Twilio) to send your location as an SMS message to all the phone numbers provided.</span></span>

<span data-ttu-id="1a894-106">Il processo include i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1a894-106">This process involves the following steps:</span></span>

1. <span data-ttu-id="1a894-107">L'app acquisisce la posizione usando Xamarin.Essentials come astrazione su API di posizione specifiche del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="1a894-107">The app captures your location using Xamarin.Essentials as an abstraction over device-specific location APIs.</span></span>

1. <span data-ttu-id="1a894-108">La posizione e i numeri di telefono vengono inseriti in un pacchetto in un payload JSON e inviati a una funzione di Azure.</span><span class="sxs-lookup"><span data-stu-id="1a894-108">The location and phone numbers are packaged up into a JSON payload and sent to an Azure function.</span></span>

1. <span data-ttu-id="1a894-109">La funzione di Azure decodifica il payload JSON e crea i messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="1a894-109">The Azure function decodes the JSON payload and creates SMS messages.</span></span>

1. <span data-ttu-id="1a894-110">I messaggi SMS vengono inviati tramite [Twilio](https://www.twilio.com/?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="1a894-110">The SMS messages are sent via [Twilio](https://www.twilio.com/?azure-portal=true).</span></span>

<span data-ttu-id="1a894-111">L'illustrazione seguente mostra una panoramica di questo processo.</span><span class="sxs-lookup"><span data-stu-id="1a894-111">The following illustration shows an overview of this process.</span></span>

![Illustrazione che mostra un'architettura di alto livello del processo di condivisione percorso tramite messaggio di testo.](../media/1-architecture.png)

## <a name="create-a-twilio-account"></a><span data-ttu-id="1a894-113">Creare un account Twilio</span><span class="sxs-lookup"><span data-stu-id="1a894-113">Create a Twilio account</span></span>

<span data-ttu-id="1a894-114">Per poter inviare messaggi SMS da una funzione di Azure, è necessario disporre di un account Twilio.</span><span class="sxs-lookup"><span data-stu-id="1a894-114">To be able to send SMS messages from an Azure function, you'll need a Twilio account.</span></span> <span data-ttu-id="1a894-115">La versione gratuita dell'account è più che sufficiente per iniziare.</span><span class="sxs-lookup"><span data-stu-id="1a894-115">The free account is more than enough to get started.</span></span>

1. <span data-ttu-id="1a894-116">Accedere a [twilio.com](https://www.twilio.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="1a894-116">Head to [twilio.com](https://www.twilio.com?azure-portal=true).</span></span>

1. <span data-ttu-id="1a894-117">Fare clic sul pulsante rosso **Sign Up** (Registrazione) nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="1a894-117">Click the red **Sign Up** button in the top-right corner.</span></span>

1. <span data-ttu-id="1a894-118">Immettere i propri dati personali e fare clic su **Get Started** (Per iniziare).</span><span class="sxs-lookup"><span data-stu-id="1a894-118">Fill in your details and click **Get Started**.</span></span>

1. <span data-ttu-id="1a894-119">Sarà necessario verificare il proprio numero di telefono.</span><span class="sxs-lookup"><span data-stu-id="1a894-119">You'll need to verify your phone number.</span></span> <span data-ttu-id="1a894-120">Gli account Twilio gratuiti consentono di inviare messaggi solo a numeri di telefono verificati al fine di evitare che vengano usati per inviare posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="1a894-120">Twilio free accounts let you send messages only to verified phone numbers to stop them being used for spam.</span></span> <span data-ttu-id="1a894-121">Twilio invierà un codice di verifica da immettere per verificare il telefono.</span><span class="sxs-lookup"><span data-stu-id="1a894-121">Twilio will send you a verification code that you need to enter to verify your phone.</span></span>

1. <span data-ttu-id="1a894-122">Selezionare la scheda **Products** (Prodotti), fare clic su **Programmable SMS** (SMS programmabile) e quindi su **Continue** (Continua).</span><span class="sxs-lookup"><span data-stu-id="1a894-122">Select the **Products** tab, and click **Programmable SMS**, then click **Continue**.</span></span>

1. <span data-ttu-id="1a894-123">Immettere un nome per il primo progetto, ad esempio "I'm here" e quindi fare clic su **Continue** (Continua).</span><span class="sxs-lookup"><span data-stu-id="1a894-123">Provide a name for your first project, such as "I'm here", then click **Continue**.</span></span>

1. <span data-ttu-id="1a894-124">Ignorare il passaggio relativo all'invito di un membro del team.</span><span class="sxs-lookup"><span data-stu-id="1a894-124">Skip the step to invite a team mate.</span></span>

1. <span data-ttu-id="1a894-125">Dal dashboard di messaggistica di Twilio espandere il pannello **Project Info** (Informazioni progetto).</span><span class="sxs-lookup"><span data-stu-id="1a894-125">From the Twilio messaging dashboard, expand the **Project Info** panel.</span></span>

1. <span data-ttu-id="1a894-126">Prendere nota dei valori **ACCOUNT SID** e **AUTH TOKEN**, perché saranno necessari in seguito.</span><span class="sxs-lookup"><span data-stu-id="1a894-126">Note your **ACCOUNT SID** and **AUTH TOKEN** because you will need these values later.</span></span>

    <span data-ttu-id="1a894-127">Quando si crea un account Twilio, viene assegnato un numero di telefono da cui è possibile inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="1a894-127">When you create a Twilio account, you are assigned a phone number that you can send messages from.</span></span> <span data-ttu-id="1a894-128">È possibile trovare questo numero di telefono nel dashboard **Numeri di telefono** di Twilio.</span><span class="sxs-lookup"><span data-stu-id="1a894-128">You can find this phone number on the Twilio **Phone Numbers** dashboard.</span></span>

1. <span data-ttu-id="1a894-129">Dal sito di Twilio, selezionare i puntini di sospensione nella parte inferiore del menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="1a894-129">From the Twilio site, select the ellipses at the bottom of the left-hand menu.</span></span> <span data-ttu-id="1a894-130">Quindi, selezionare *SUPER NETWORK -> Numeri di telefono*.</span><span class="sxs-lookup"><span data-stu-id="1a894-130">Then, select *SUPER NETWORK->Phone Numbers*.</span></span> <span data-ttu-id="1a894-131">È possibile aggiungere questo dashboard al menu a sinistra mediante l'icona della puntina.</span><span class="sxs-lookup"><span data-stu-id="1a894-131">You can pin this dashboard to the left-hand menu using the pin icon.</span></span> <span data-ttu-id="1a894-132">Il numero Twilio si troverà in *Manage Numbers->Active Numbers* (Gestisci numeri -> Numeri attivi).</span><span class="sxs-lookup"><span data-stu-id="1a894-132">Your Twilio number will be under *Manage Numbers->Active Numbers*.</span></span>

    ![Ricerca del numero Twilio](../media/7-twilio-find-number.png)

    > [!TIP]
    > <span data-ttu-id="1a894-134">Se non si ha ancora un numero attivo, selezionare **Get Started** (Inizia) nella pagina Active Numbers (Numeri attivi) per iniziare il processo di creazione di un numero.</span><span class="sxs-lookup"><span data-stu-id="1a894-134">If you don't have an active number yet, select **Get Started** in the Active Numbers page to begin the process of creating a number.</span></span>

1. <span data-ttu-id="1a894-135">Prendere nota del numero di telefono attivo.</span><span class="sxs-lookup"><span data-stu-id="1a894-135">Note your active phone number.</span></span> <span data-ttu-id="1a894-136">Verrà usato più avanti in questo modulo.</span><span class="sxs-lookup"><span data-stu-id="1a894-136">It will be used later in this module.</span></span>


> [!NOTE]
> <span data-ttu-id="1a894-137">Nella fase di iscrizione verrà assegnato un numero di telefono Twilio che verrà usato per inviare i messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="1a894-137">When you sign up, you will be assigned a Twilio phone number that will be used to send SMS messages.</span></span> <span data-ttu-id="1a894-138">In alcuni paesi questi numeri potrebbero non essere in grado di inviare i messaggi SMS.</span><span class="sxs-lookup"><span data-stu-id="1a894-138">In some countries, these numbers may not be able to send SMS messages.</span></span> <span data-ttu-id="1a894-139">Nella documentazione di Twilio sono elencati i [paesi che prevedono restrizioni](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true)e vengono descritti i modi per inviare messaggi SMS con un [numero internazionale oppure un Id alfanumerico del mittente](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="1a894-139">The Twilio documentation lists [which countries have restrictions](https://support.twilio.com/hc/articles/223183068-Twilio-international-phone-number-availability-and-their-capabilities?azure-portal=true), and shows ways to send SMS messages using an [international number or AlphaNumeric sender Id](https://support.twilio.com/hc/articles/226690868-Using-Twilio-when-SMS-numbers-are-unavailable-in-your-country?azure-portal=true).</span></span>

## <a name="launch-visual-studio"></a><span data-ttu-id="1a894-140">Avviare Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a894-140">Launch Visual Studio</span></span>

<span data-ttu-id="1a894-141">Per questo modulo si svilupperà l'app per dispositivi mobili e l'app di Funzioni di Azure usando Visual Studio 2017, disponibile tramite una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="1a894-141">For this module, you'll develop the mobile app and Azure Functions app using Visual Studio 2017, available via a virtual machine.</span></span> <span data-ttu-id="1a894-142">Sebbene sia possibile creare app Xamarin.Forms eseguibili in iOS, Android e Universal Windows Platform (UWP), questo modulo si concentrerà solo sulla piattaforma UWP, per consentirne il funzionamento nella macchina virtuale del lab.</span><span class="sxs-lookup"><span data-stu-id="1a894-142">Although Xamarin.Forms apps can be built to run on iOS, Android, and Universal Windows Platform (UWP), this module will just focus on UWP so that it works inside the lab virtual machine.</span></span>

<span data-ttu-id="1a894-143">Avviare Visual Studio 2017 dal menu Start della macchina virtuale o dal collegamento sul desktop.</span><span class="sxs-lookup"><span data-stu-id="1a894-143">Launch Visual Studio 2017 from the VM's Start Menu, or from the desktop shortcut.</span></span>

## <a name="summary"></a><span data-ttu-id="1a894-144">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1a894-144">Summary</span></span>

<span data-ttu-id="1a894-145">In questa unità è stato creato un account Twilio da usare per inviare messaggi SMS ed è stato avviato Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a894-145">In this unit, you created a Twilio account to use to send SMS messages and launched Visual Studio.</span></span> <span data-ttu-id="1a894-146">Nella prossima unità si apprenderà come creare un'app Xamarin.Forms e come aggiungere il pacchetto NuGet Xamarin.Essentials.</span><span class="sxs-lookup"><span data-stu-id="1a894-146">Next, you learn how to create a Xamarin.Forms app and add the Xamarin.Essentials NuGet package.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a894-147">Prendere nota dei valori **ACCOUNT SID** e **AUTH TOKEN** Twilio e del **numero di telefono attivo** presentati in questa unità, perché serviranno in seguito.</span><span class="sxs-lookup"><span data-stu-id="1a894-147">Keep note of the Twilio  **ACCOUNT SID** and **AUTH TOKEN** and **Active Phone Number** values that you gathered in this unit, because you'll need these values later.</span></span>
