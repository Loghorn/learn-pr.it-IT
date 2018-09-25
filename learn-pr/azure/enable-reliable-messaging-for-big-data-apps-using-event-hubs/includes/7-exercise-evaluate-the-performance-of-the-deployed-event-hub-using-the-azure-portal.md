<span data-ttu-id="e9b1d-101">In questa unità si userà il portale di Azure per verificare che l'hub eventi funzioni con prestazioni corrispondenti alle aspettative.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-101">In this unit, you'll use the Azure portal to verify your Event Hub is working and performing according to the desired expectations.</span></span> <span data-ttu-id="e9b1d-102">Verrà inoltre testato il funzionamento della messaggistica dell'hub eventi quando è temporaneamente non disponibile e si useranno le metriche dell'hub eventi per controllarne le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-102">You'll also test how Event Hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your Event Hub.</span></span>

## <a name="view-event-hub-activity"></a><span data-ttu-id="e9b1d-103">Visualizzare l'attività dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="e9b1d-103">View Event Hub activity</span></span>

1. <span data-ttu-id="e9b1d-104">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="e9b1d-105">Trovare l'hub eventi usando la barra di ricerca e aprirlo.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-105">Find your Event Hub, using the Search bar, and open it.</span></span>

1. <span data-ttu-id="e9b1d-106">Nella pagina Panoramica visualizzare il numero di messaggi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-106">On the Overview page, view the message counts.</span></span>

    ![Screenshot del portale di Azure che mostra lo spazio dei nomi dell'hub eventi con il numero di messaggi](../media/6-view-messages.png)

1. <span data-ttu-id="e9b1d-108">Le applicazioni SimpleSend ed EventProcessorSample sono configurate per l'invio e la ricezione di 100 messaggi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-108">The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages.</span></span> <span data-ttu-id="e9b1d-109">Si noterà che l'hub eventi ha elaborato 100 messaggi dall'applicazione SimpleSend e ha trasmesso 100 messaggi all'applicazione EventProcessorSample.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-109">You'll see that the Event Hub has processed 100 messages from the SimpleSend application and has transmitted 100 messages to the EventProcessorSample application.</span></span>

## <a name="test-event-hub-resilience"></a><span data-ttu-id="e9b1d-110">Testare la resilienza dell'hub eventi</span><span class="sxs-lookup"><span data-stu-id="e9b1d-110">Test Event Hub resilience</span></span>

<span data-ttu-id="e9b1d-111">Usare la procedura seguente per vedere cosa accade quando un'applicazione invia messaggi a un hub eventi temporaneamente non disponibile.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-111">Use the following steps to see what happens when an application sends messages to an Event Hub while it's temporarily unavailable.</span></span>

1. <span data-ttu-id="e9b1d-112">Inviare di nuovo i messaggi all'hub eventi usando l'applicazione SimpleSend.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-112">Resend messages to the Event Hub using the SimpleSend application.</span></span> <span data-ttu-id="e9b1d-113">Usare il comando di seguito:</span><span class="sxs-lookup"><span data-stu-id="e9b1d-113">Use the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="e9b1d-114">Quando viene visualizzato il messaggio **Invio completato...**, premere <kbd>INVIO</kbd>.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-114">When you see **Send Complete...**, press <kbd>ENTER</kbd>.</span></span>

1. <span data-ttu-id="e9b1d-115">Selezionare l'hub eventi nella schermata **Panoramica**; verranno visualizzati i dettagli specifici dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-115">Select your Event Hub in the **Overview** screen - this will show details specific to the Event Hub.</span></span> <span data-ttu-id="e9b1d-116">È possibile accedere a questa schermata anche tramite la voce **Hub eventi** dalla pagina dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-116">You can also get to this screen through the **Event Hubs** entry from the namespace page.</span></span>

1. <span data-ttu-id="e9b1d-117">Selezionare **Impostazioni** > **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-117">Select **Settings** > **Properties**.</span></span>

1. <span data-ttu-id="e9b1d-118">In Stato hub eventi fare clic su **Disabilitato**.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-118">Under Event Hub state, click **Disabled**.</span></span> <span data-ttu-id="e9b1d-119">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-119">Save the changes.</span></span>

    ![Disabilitare l'hub eventi](../media/7-disable-event-hub.png)

    <span data-ttu-id="e9b1d-121">**Attendere almeno cinque minuti.**</span><span class="sxs-lookup"><span data-stu-id="e9b1d-121">**Wait for a minimum of five minutes.**</span></span>

1. <span data-ttu-id="e9b1d-122">Fare clic su **Attivo** in Stato hub eventi per abilitare nuovamente l'hub eventi e salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-122">Click **Active** under Event Hub state to re-enable your Event Hub and save your changes.</span></span>

1. <span data-ttu-id="e9b1d-123">Eseguire di nuovo l'applicazione EventProcessorSample per ricevere i messaggi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-123">Rerun the EventProcessorSample application to receive messages.</span></span> <span data-ttu-id="e9b1d-124">Usare il comando di seguito.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-124">Use the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="e9b1d-125">Quando non vengono più visualizzati messaggi nella console, premere <kbd>INVIO</kbd>.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-125">When messages stop being displayed to the console, press <kbd>ENTER</kbd>.</span></span>

1. <span data-ttu-id="e9b1d-126">Tornando al portale di Azure, passare nuovamente allo spazio dei nomi dell'hub eventi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-126">Back in the Azure portal, go back to your Event Hub Namespace.</span></span> <span data-ttu-id="e9b1d-127">Se ancora ci si trova nella pagina dell'hub eventi, è possibile utilizzare la barra di navigazione nella parte superiore della schermata per tornare indietro</span><span class="sxs-lookup"><span data-stu-id="e9b1d-127">If you are still on the Event Hub page, you can use the breadcrumb on the top of the screen to go backwards.</span></span> <span data-ttu-id="e9b1d-128">oppure cercare lo spazio dei nomi e selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-128">Or you can search for the namespace and select it.</span></span>

1. <span data-ttu-id="e9b1d-129">Fare clic su **MONITORAGGIO** > **Metriche (anteprima)**.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-129">Click **MONITORING** > **Metrics (preview)**.</span></span>

    ![Screenshot che mostra le metriche dell'hub eventi con la visualizzazione del numero di messaggi in arrivo e in uscita.](../media/7-event-hub-metrics.png)

1. <span data-ttu-id="e9b1d-131">Nell'elenco **Metrica** selezionare **Messaggi in arrivo** e fare clic su **Aggiungi metrica**.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-131">From the **Metric** list, select **Incoming Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="e9b1d-132">Nell'elenco **Metrica** selezionare **Messaggi in uscita** e fare clic su **Aggiungi metrica**.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-132">From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="e9b1d-133">Nella parte superiore del grafico fare clic su **Ultime 24 ore (automatico)** per modificare il periodo di tempo in **Ultimi 30 minuti** ed espandere il grafico dei dati.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-133">At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes** to expand the data graph.</span></span>

<span data-ttu-id="e9b1d-134">Si noterà che, anche se i messaggi sono stati inviati prima che l'hub eventi fosse portato offline per un periodo di tempo, tutti i 100 messaggi sono stati trasmessi correttamente.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-134">You'll see that though the messages were sent before the Event Hub was taken offline for a period, all 100 messages were successfully transmitted.</span></span>

## <a name="summary"></a><span data-ttu-id="e9b1d-135">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e9b1d-135">Summary</span></span>

<span data-ttu-id="e9b1d-136">In questa unità sono state usate le metriche dell'hub eventi per verificare che l'hub eventi abbia completato correttamente l'elaborazione dell'invio e della ricezione di messaggi.</span><span class="sxs-lookup"><span data-stu-id="e9b1d-136">In this unit, you used the Event Hubs metrics to test that your Event Hub is successfully processing the sending and receiving messages.</span></span>
