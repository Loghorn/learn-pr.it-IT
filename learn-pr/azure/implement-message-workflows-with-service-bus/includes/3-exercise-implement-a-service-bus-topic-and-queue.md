<span data-ttu-id="0e5ec-101">Si supponga di avere un'applicazione per il team di vendita di un'azienda globale.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-101">Suppose you have an application for the sales team in your global company.</span></span> <span data-ttu-id="0e5ec-102">Ogni membro del team ha un telefono cellulare in cui verrà installata l'app.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-102">Each team member has a mobile phone where your app will be installed.</span></span> <span data-ttu-id="0e5ec-103">Un servizio Web ospitato in Azure implementa la logica di business per l'applicazione e archivia le informazioni nel database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-103">A web service hosted in Azure implements the business logic for your application and stores information in Azure SQL Database.</span></span> <span data-ttu-id="0e5ec-104">C'è un'istanza del servizio Web per ogni area geografica.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-104">There is one instance of the web service for each geographical region.</span></span> <span data-ttu-id="0e5ec-105">Sono stati identificati gli scopi seguenti per l'invio di messaggi tra l'app per dispositivi mobili e il servizio Web:</span><span class="sxs-lookup"><span data-stu-id="0e5ec-105">You have identified the following purposes for sending messages between the mobile app and the web service:</span></span>

- <span data-ttu-id="0e5ec-106">I messaggi relativi a singole vendite devono essere inviati solo all'istanza del servizio Web nell'area dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-106">Messages that relate to individual sales must be sent only to the web service instance in the user's region.</span></span>
- <span data-ttu-id="0e5ec-107">I messaggi relativi alle prestazioni di vendita devono essere inviati a tutte le istanze del servizio Web.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-107">Messages that relate to sales performance must be sent to all instances of the web service.</span></span>

<span data-ttu-id="0e5ec-108">Si è deciso di implementare una coda del bus di servizio per il primo caso d'uso e l'argomento del bus di servizio per il secondo caso d'uso.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-108">You have decided to implement a Service Bus queue for the first use case and the Service Bus topic for the second use case.</span></span>

<span data-ttu-id="0e5ec-109">In questo esercizio si creerà uno spazio dei nomi del bus di servizio, che conterrà sia una coda che un argomento con sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-109">In this exercise, you will create a Service Bus namespace, which will contain both a queue and a topic with subscriptions.</span></span>

## <a name="create-a-service-bus-namespace"></a><span data-ttu-id="0e5ec-110">Creare uno spazio dei nomi del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="0e5ec-110">Create a Service Bus namespace</span></span>

<span data-ttu-id="0e5ec-111">Nel bus di servizio di Azure uno spazio dei nomi è un contenitore, con un nome di dominio completo univoco, per code, argomenti e inoltri.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-111">In Azure Service Bus, a namespace is a container, with a unique fully qualified domain name, for queues, topics, and relays.</span></span> <span data-ttu-id="0e5ec-112">È necessario iniziare creando lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-112">You must start by creating the namespace.</span></span>

<span data-ttu-id="0e5ec-113">Ogni spazio dei nomi include anche le chiavi di crittografia di firma di accesso condiviso (SAS, Shared Access Signature) primaria e secondaria.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-113">Each namespace also has primary and secondary Shared Access Signature (SAS) encryption keys.</span></span> <span data-ttu-id="0e5ec-114">Un componente mittente o destinatario deve fornire queste chiavi quando si connette per ottenere l'accesso agli oggetti nello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-114">A sending or receiving component must provide these keys when it connects to gain access to the objects within the namespace.</span></span>

<span data-ttu-id="0e5ec-115">Per creare uno spazio dei nomi del bus di servizio usando il portale di Azure, eseguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="0e5ec-115">To create a Service Bus namespace by using the Azure portal, following these steps:</span></span>

1. <span data-ttu-id="0e5ec-116">In un browser passare al [portale di Azure](https://portal.azure.com/) e accedere con le proprie credenziali dell'account di Azure.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-116">In a browser, navigate the to [Azure portal](https://portal.azure.com/) and log in with your usual Azure account credentials.</span></span>

1. <span data-ttu-id="0e5ec-117">Nel riquadro di spostamento a sinistra fare clic su **Tutti i servizi**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-117">In the navigation on the left, click **All services**.</span></span>

1. <span data-ttu-id="0e5ec-118">Nel pannello **Tutti i servizi** scorrere verso il basso fino alla sezione **INTEGRAZIONE** e quindi fare clic su **Bus di servizio**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-118">In the **All Services** blade, scroll down to the **INTEGRATION** section and then click **Service Bus**.</span></span>

    ![Creare uno spazio dei nomi del bus di servizio](../media-draft/3-create-namespace-1.png)

1. <span data-ttu-id="0e5ec-120">In alto a sinistra nel pannello **Bus di servizio** fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-120">In the top left of the **Service Bus** blade, click **Add**.</span></span>

1. <span data-ttu-id="0e5ec-121">Nella casella di testo **Nome** digitare un nome univoco per lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-121">In the **Name** textbox, type a unique name for the namespace.</span></span> <span data-ttu-id="0e5ec-122">Ad esempio "salesteamapp" + *iniziali del nome* + *data corrente*</span><span class="sxs-lookup"><span data-stu-id="0e5ec-122">For example "salesteamapp" + *your initials* + *current date*</span></span>

1. <span data-ttu-id="0e5ec-123">Nell'elenco a discesa **Piano tariffario** selezionare **Standard**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-123">In the **Pricing tier** drop-down list, select **Standard**.</span></span>

1. <span data-ttu-id="0e5ec-124">Nell'elenco a discesa **Sottoscrizione** selezionare la sottoscrizione in uso.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-124">In the **Subscription** drop-down list, select your subscription.</span></span>

1. <span data-ttu-id="0e5ec-125">In **Gruppo di risorse** selezionare **Crea nuovo** e quindi digitare **SalesTeamAppRG**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-125">Under **Resource group** select **Create new**, and then type **SalesTeamAppRG**.</span></span>

1. <span data-ttu-id="0e5ec-126">Nell'elenco a discesa **Località** selezionare una località nelle vicinanze e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-126">In the **Location** drop-down list, select a location near you and then click **Create**.</span></span> <span data-ttu-id="0e5ec-127">Azure creerà il nuovo spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-127">Azure creates the new Service Bus namespace.</span></span>

    ![Creare uno spazio dei nomi del bus di servizio](../media-draft/3-create-namespace-2.png)

## <a name="create-a-service-bus-queue"></a><span data-ttu-id="0e5ec-129">Creare una coda del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="0e5ec-129">Create a Service Bus queue</span></span>

<span data-ttu-id="0e5ec-130">Dopo aver creato uno spazio dei nomi, è possibile creare una coda per i messaggi relativi alle singole vendite.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-130">Now that you have a namespace, you can create a queue for messages about individual sales.</span></span> <span data-ttu-id="0e5ec-131">A questo scopo, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="0e5ec-131">To do this, follow these steps:</span></span>

1. <span data-ttu-id="0e5ec-132">Nel pannello **Bus di servizio** fare clic su **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-132">In the **Service Bus** blade, click **Refresh**.</span></span> <span data-ttu-id="0e5ec-133">Verrà visualizzato lo spazio dei nomi appena creato.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-133">The namespace you just created is displayed.</span></span>

1. <span data-ttu-id="0e5ec-134">Fare clic sullo spazio dei nomi appena creato.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-134">Click the namespace you just created.</span></span>

1. <span data-ttu-id="0e5ec-135">In alto a sinistra nel pannello dello spazio dei nomi fare clic su **Coda**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-135">In the top left of the namespace blade, click **+ Queue**.</span></span>

1. <span data-ttu-id="0e5ec-136">Nel pannello **Crea coda**, nella casella di testo **Nome** digitare **salesmessages** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-136">In the **Create queue** blade, in the **Name** textbox type **salesmessages**, and then click **Create**.</span></span> <span data-ttu-id="0e5ec-137">Azure creerà la coda nello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-137">Azure creates the queue in your namespace.</span></span>

    ![Creazione di una coda](../media-draft/3-create-queue.png)

## <a name="create-a-service-bus-topic-and-subscriptions"></a><span data-ttu-id="0e5ec-139">Creare un argomento del bus di servizio e le sottoscrizioni</span><span class="sxs-lookup"><span data-stu-id="0e5ec-139">Create a Service Bus topic and subscriptions</span></span>

<span data-ttu-id="0e5ec-140">Si vuole creare anche un argomento che verrà usato per i messaggi relativi alle prestazioni di vendita.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-140">You also want to create a topic that will be used for messages that relate to sales performance.</span></span> <span data-ttu-id="0e5ec-141">Più istanze del servizio Web per la logica di business eseguiranno la sottoscrizione di questo argomento da paesi diversi.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-141">Multiple instances of the business logic web service will subscribe to this topic from different countries.</span></span> <span data-ttu-id="0e5ec-142">Ogni messaggio verrà recapitato a più istanze.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-142">Each message will be delivered to multiple instances.</span></span>

<span data-ttu-id="0e5ec-143">Seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="0e5ec-143">Follow these steps:</span></span>

1. <span data-ttu-id="0e5ec-144">Nel pannello **Spazio dei nomi del bus di servizio** fare clic su **+ Argomento**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-144">In the **Service Bus Namespace** blade, click **+ Topic**.</span></span>

1. <span data-ttu-id="0e5ec-145">Nel pannello **Crea argomento**, nella casella di testo **Nome** digitare **salesperformancemessages** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-145">In the **Create topic** blade, in the **Name** textbox type **salesperformancemessages**, and then click **Create**.</span></span> <span data-ttu-id="0e5ec-146">Azure creerà l'argomento nello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-146">Azure creates the topic in your namespace.</span></span>

    ![Creazione di un argomento](../media-draft/3-create-topic.png)

1. <span data-ttu-id="0e5ec-148">Quando l'argomento è stato creato, nel pannello **Spazio dei nomi del bus di servizio** fare clic su **Argomenti** in **Entità**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-148">When the topic has been created, in the **Service Bus Namespace** blade, under **Entities** click **Topics**.</span></span>

1. <span data-ttu-id="0e5ec-149">Nell'elenco di argomenti fare clic su **salesperformancemessages** e quindi fare clic su **+ Sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-149">In the list of topics, click **salesperformancemessages** and then click **+ Subscription**.</span></span>

1. <span data-ttu-id="0e5ec-150">Nella casella di testo **Nome** digitare **Americas** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-150">In the **Name** textbox, type **Americas** and then click **Create**.</span></span>

1. <span data-ttu-id="0e5ec-151">Fare clic su **+ Sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-151">Click **+ Subscription**.</span></span>

1. <span data-ttu-id="0e5ec-152">Nella casella di testo **Nome** digitare **EuropeAndAfrica** e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-152">In the **Name** textbox, type **EuropeAndAfrica** and then click **Create**.</span></span>

<span data-ttu-id="0e5ec-153">È stata creata l'infrastruttura necessaria per usare il bus di servizio per aumentare la resilienza dell'applicazione distribuita per la forza vendita.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-153">You have built the infrastructure required to use Service Bus to increase the resilience of your sales force distributed application.</span></span> <span data-ttu-id="0e5ec-154">Sono stati creati una coda per i messaggi relativi alle singole vendite e un argomento per i messaggi relativi alle prestazioni di vendita.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-154">You have created a queue for messages about individual sales and a topic for messages about sales performance.</span></span> <span data-ttu-id="0e5ec-155">L'argomento include più sottoscrizioni perché i messaggi inviati all'argomento possono essere recapitati a più servizi Web di destinazione in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="0e5ec-155">The topic includes multiple subscriptions because messages sent to that topic can be delivered to multiple recipient web services around the world.</span></span>
