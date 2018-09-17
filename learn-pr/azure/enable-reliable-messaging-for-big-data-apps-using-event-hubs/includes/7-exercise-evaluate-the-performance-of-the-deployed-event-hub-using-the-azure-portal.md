<span data-ttu-id="9d8b6-101">In questa unità si userà il portale di Azure per verificare che l'hub eventi funzioni con prestazioni corrispondenti alle aspettative.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-101">In this unit, you'll use the Azure portal to verify your event hub is working and performing according to the desired expectations.</span></span> <span data-ttu-id="9d8b6-102">Verrà inoltre testato il funzionamento della messaggistica dell'hub eventi quando è temporaneamente non disponibile e si useranno le metriche dell'hub eventi per controllare le prestazioni dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-102">You'll also test how event hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your event hub.</span></span>

## <a name="view-event-hub-activity"></a><span data-ttu-id="9d8b6-103">Visualizzare l'attività dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="9d8b6-103">View event hub activity</span></span>

1. <span data-ttu-id="9d8b6-104">Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="9d8b6-104">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="9d8b6-105">Trovare l'hub eventi usando la barra di ricerca e aprirlo.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-105">Find your event hub, using the Search bar, and open it.</span></span>

1. <span data-ttu-id="9d8b6-106">Nella pagina Panoramica visualizzare i conteggi dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-106">On the Overview page, view the message counts.</span></span>

    ![Visualizzare i messaggi dell'hub eventi](../media-draft/6-view-messages.png)

1. <span data-ttu-id="9d8b6-108">Le applicazioni SimpleSend ed EventProcessorSample sono configurate per l'invio e la ricezione di 100 messaggi.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-108">The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages.</span></span> <span data-ttu-id="9d8b6-109">Si noterà che l'hub eventi ha elaborato 100 messaggi dall'applicazione SimpleSend e ha trasmesso 100 messaggi all'applicazione EventProcessorSample.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-109">You'll see that the event hub has processed 100 messages from the SimpleSend application and has transmitted 100 messages to the EventProcessorSample application.</span></span>

## <a name="test-event-hub-resilience"></a><span data-ttu-id="9d8b6-110">Testare la resilienza dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="9d8b6-110">Test event hub resilience</span></span>

<span data-ttu-id="9d8b6-111">Usare la procedura seguente per vedere cosa accade quando un'applicazione invia messaggi a un hub eventi, mentre è temporaneamente non disponibile.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-111">Use the following steps to see what happens when an application sends messages to an event hub while it's temporarily unavailable.</span></span>

1. <span data-ttu-id="9d8b6-112">Inviare di nuovo i messaggi all'hub eventi usando l'applicazione SimpleSend.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-112">Resend messages to the event hub using the SimpleSend application.</span></span> <span data-ttu-id="9d8b6-113">Usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9d8b6-113">Use the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="9d8b6-114">Quando viene visualizzato il messaggio **Invio completato...** premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-114">When you see **Send Complete...**, press ENTER.</span></span>

1. <span data-ttu-id="9d8b6-115">Nel portale di Azure fare clic su **Istanza di hub eventi** > **Impostazioni** > **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-115">In the Azure portal, click **Event Hubs Instance** > **SETTINGS** > **Properties**.</span></span>

1. <span data-ttu-id="9d8b6-116">In Stato hub eventi fare clic su **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-116">Under Event Hub state, click **Disabled**.</span></span>

    ![Disabilitare l'hub eventi](../media-draft/7-disable-event-hub.png)

<span data-ttu-id="9d8b6-118">Attendere almeno cinque minuti.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-118">Wait for a minimum of five minutes.</span></span>

1. <span data-ttu-id="9d8b6-119">Fare clic su **Attivo** in Stato hub eventi per abilitare di nuovo l'hub eventi e salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-119">Click **Active** under Event Hub state to re-enable your event hub and save your changes.</span></span>

1. <span data-ttu-id="9d8b6-120">Eseguire di nuovo l'applicazione EventProcessorSample per ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-120">Rerun the EventProcessorSample application to receive messages.</span></span> <span data-ttu-id="9d8b6-121">Usare il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-121">Use the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="9d8b6-122">Quando non vengono più visualizzati messaggi nella console, premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-122">When messages stop being displayed to the console, press ENTER.</span></span>

1. <span data-ttu-id="9d8b6-123">Nel portale di Azure individuare lo **_spazio dei nomi_** dell'hub eventi e aprirlo.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-123">In the Azure portal, find your event hub **_namespace_** and open it.</span></span> 

1. <span data-ttu-id="9d8b6-124">Fare clic su **Spazio dei nomi hub eventi** > **Monitoraggio** > **Metriche (anteprima)**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-124">Click **Event Hubs Namespace** > **MONITORING** > **Metrics (preview)**.</span></span>

    ![Usare le metriche dell'hub eventi](../media-draft/7-event-hub-metrics.png)

1. <span data-ttu-id="9d8b6-126">Nell'elenco **Metrica** selezionare **Messaggi in arrivo** e fare clic su **Aggiungi metrica**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-126">From the **Metric** list, select **Incoming Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="9d8b6-127">Nell'elenco **Metrica** selezionare **Messaggi in uscita** e fare clic su **Aggiungi metrica**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-127">From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="9d8b6-128">Nella parte superiore del grafico fare clic su **Ultime 24 ore (automatico)** per modificare il periodo di tempo in **Ultimi 30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-128">At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes**.</span></span>

1. <span data-ttu-id="9d8b6-129">Fare clic su **Applica**.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-129">Click **Apply**.</span></span>

<span data-ttu-id="9d8b6-130">Si noterà che, anche se i messaggi sono stati inviati prima che l'hub eventi fosse portato offline per un periodo di tempo, tutti i 100 messaggi sono stati trasmessi correttamente.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-130">You'll see that though the messages were sent before the event hub was taken offline for a period, all 100 messages were successfully transmitted.</span></span>

## <a name="summary"></a><span data-ttu-id="9d8b6-131">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="9d8b6-131">Summary</span></span>

<span data-ttu-id="9d8b6-132">In questa unità sono state usate le metriche di hub eventi per verificare che l'hub eventi abbia completato correttamente l'elaborazione dell'invio e della ricezione di messaggi.</span><span class="sxs-lookup"><span data-stu-id="9d8b6-132">In this unit, you used the Event Hubs metrics to test that your event hub is successfully processing the sending and receiving messages.</span></span>
