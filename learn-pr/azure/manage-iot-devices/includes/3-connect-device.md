<span data-ttu-id="fe3ae-101">L'acquisizione dei dati meteorologici è un'attività importante, in quanto il meteo può influire su numerosi fattori, dai modelli di traffico alla modalità di funzionamento dei sistemi di riscaldamento, ventilazione e condizionamento (HVAC) nei negozi al dettaglio.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-101">Capturing of weather data is an important task as weather can effect everything from traffic patterns to how heating, ventilation, and air conditioning (HVAC) systems in retail stores are operated.</span></span> <span data-ttu-id="fe3ae-102">In questo esercizio si interagirà con il simulatore Raspberry Pi online illustrato nell'unità precedente per acquisire dati meteo simulati tramite l'hub IoT di Azure.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-102">In this exercise, you will be interacting with the online Raspberry Pi simulator you learned about in the previous unit to capture simulated weather data and via the Azure IoT Hub.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="fe3ae-103">Mentre l'esercizio viene eseguito in un ambiente simulato, l'applicazione in esecuzione nel dispositivo simulato potrà essere trasferita a un dispositivo reale in futuro.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-103">While this exercise is being conducted in a simulated environment, the application running on the simulated device can be transferred to a real device in the future.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="fe3ae-104">Creare un hub IoT</span><span class="sxs-lookup"><span data-stu-id="fe3ae-104">Create an IoT hub</span></span>
<span data-ttu-id="fe3ae-105">L'hub IoT di Azure fornisce le funzionalità e un modello di estendibilità che consentono agli sviluppatori di dispositivi e back-end di creare soluzioni di gestione affidabili per i dispositivi.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-105">Azure IoT Hub provides the features and an extensibility model that enable device and back-end developers to build robust device management solutions.</span></span> <span data-ttu-id="fe3ae-106">I dispositivi spaziano da sensori vincolati e microcontroller a scopo singolo, a potenti gateway che instradano le comunicazioni per gruppi di dispositivi.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-106">Devices range from constrained sensors and single purpose microcontrollers, to powerful gateways that route communications for groups of devices.</span></span> <span data-ttu-id="fe3ae-107">Inoltre, i casi d'uso e i requisiti per gli operatori IoT variano notevolmente all'interno dei diversi settori.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-107">In addition, the use cases and requirements for IoT operators vary significantly across industries.</span></span> <span data-ttu-id="fe3ae-108">Nonostante questa variabilità, la gestione dei dispositivi con l'hub IoT fornisce le funzionalità, i modelli e le librerie di codice necessari per soddisfare un insieme eterogeneo di dispositivi e utenti finali.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-108">Despite this variation, device management with IoT Hub provides the capabilities, patterns, and code libraries to cater to a diverse set of devices and end users.</span></span>

<span data-ttu-id="fe3ae-109">Per iniziare a raccogliere i dati dal simulatore Raspberry Pi, prima è necessario creare un hub IoT.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-109">In order to start collecting the data from the Raspberry Pi simulator, you need to first create an IoT hub.</span></span>

1. <span data-ttu-id="fe3ae-110">Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-110">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

2. <span data-ttu-id="fe3ae-111">Scegliere **Crea una risorsa** nell'angolo superiore sinistro del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-111">Choose **Create a resource** in the upper left-hand corner of the Azure portal.</span></span>

3. <span data-ttu-id="fe3ae-112">Selezionare **Internet delle cose** e quindi **Hub IoT**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-112">Select **Internet of Things**, and then select **IoT Hub**.</span></span>

![Screenshot della selezione dell'hub IoT nel portale di Azure](../media/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. <span data-ttu-id="fe3ae-114">Nel riquadro **Hub IoT** immettere le informazioni seguenti per l'hub IoT:</span><span class="sxs-lookup"><span data-stu-id="fe3ae-114">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

   - <span data-ttu-id="fe3ae-115">**Sottoscrizione**: usare la sottoscrizione predefinita per questo esempio.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-115">**Subscription**: Use the default subscription for this example.</span></span>
   - <span data-ttu-id="fe3ae-116">**Gruppo di risorse**: usare il gruppo di risorse esistente.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-116">**Resource group**: Use the existing resource group.</span></span> <span data-ttu-id="fe3ae-117">Inserendo tutte le risorse correlate nello stesso gruppo, è possibile gestirle tutte insieme.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-117">By putting all related resources in the same group you can manage them all together.</span></span> <span data-ttu-id="fe3ae-118">Ad esempio, con l'eliminazione del gruppo di risorse vengono eliminate tutte le risorse contenute in quel gruppo.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-118">For example, deleting the resource group deletes all resources contained in that group.</span></span>
   - <span data-ttu-id="fe3ae-119">**Nome**: creare un nome univoco per l'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-119">**Name**: Create a unique name for your IoT hub.</span></span> <span data-ttu-id="fe3ae-120">Se il nome immesso è disponibile, viene visualizzato un segno di spunta verde.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-120">If the name you enter is available, a green check mark appears.</span></span>
   - <span data-ttu-id="fe3ae-121">**Area**: selezionare la località più vicina a quella in cui ci si trova nell'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-121">**Region**: Select the closest location to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    > [!IMPORTANT]
    > <span data-ttu-id="fe3ae-122">L'hub IoT sarà individuabile pubblicamente come endpoint DNS, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-122">The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.</span></span>

    ![Finestra delle informazioni di base dell'hub IoT](./../media/dbb7319388673b8ee0e0b407536156c0.png)

1. <span data-ttu-id="fe3ae-124">Selezionare **Avanti: Dimensioni e piano** per continuare a creare l'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-124">Select **Next: Size and scale** to continue creating your IoT hub.</span></span>
2. <span data-ttu-id="fe3ae-125">Scegliere un valore per **Piano tariffario e livello di scalabilità**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-125">Choose your **Pricing and scale tier**.</span></span> <span data-ttu-id="fe3ae-126">Per questo esempio, selezionare il livello **F1 - Gratuito**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-126">For this example, select the **F1 - Free** tier.</span></span>

    ![Finestra per specificare dimensioni e il piano dell'hub IoT](../media/b506eb3293fa4aa9d4785ad498fc476c.png)

3. <span data-ttu-id="fe3ae-128">Selezionare **Rivedi e crea**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-128">Select **Review + create**.</span></span>

4. <span data-ttu-id="fe3ae-129">Esaminare le informazioni sull'hub IoT e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-129">Review your IoT hub information, then click **Create**.</span></span> <span data-ttu-id="fe3ae-130">La creazione dell'hub IoT può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-130">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="fe3ae-131">È possibile monitorare lo stato di avanzamento nel riquadro **Notifiche**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-131">You can monitor the progress in the **Notifications** pane.</span></span>

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up exercise. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a><span data-ttu-id="fe3ae-132">Registrare un dispositivo</span><span class="sxs-lookup"><span data-stu-id="fe3ae-132">Register a device</span></span>
<span data-ttu-id="fe3ae-133">È necessario registrare un dispositivo con l'hub IoT perché questo possa connettersi.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-133">A device must be registered with your IoT hub before it can connect.</span></span>

1. <span data-ttu-id="fe3ae-134">Nel menu di spostamento dell'hub IoT aprire **Dispositivi IoT**, quindi fare clic su **Aggiungi** per registrare un dispositivo nell'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-134">In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.</span></span>

   ![Aggiungere un dispositivo a Dispositivi IoT nell'hub IoT](../media/ee5f177abcf06b86dd007fce3b8448ad.png)

2. <span data-ttu-id="fe3ae-136">Immettere un **ID dispositivo** per il nuovo dispositivo.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-136">Enter a **Device ID** for the new device.</span></span> <span data-ttu-id="fe3ae-137">Gli ID dispositivo fanno distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-137">Device IDs are case sensitive.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fe3ae-138">L'ID dispositivo può essere visibile nei log raccolti per il supporto tecnico e la risoluzione dei problemi, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-138">The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.</span></span>

3. <span data-ttu-id="fe3ae-139">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-139">Click **Save**.</span></span>
4. <span data-ttu-id="fe3ae-140">Dopo la creazione del dispositivo, aprirlo nel riquadro **Dispositivi IoT**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-140">After the device is created, open the device from the list in the **IoT devices** pane.</span></span>
5. <span data-ttu-id="fe3ae-141">Copiare il valore di **Stringa di connessione---Chiave primaria** dell'hub IoT per usarlo in seguito.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-141">Copy the **Connection string---primary key** to use later.</span></span>

   ![Ottenere la stringa di connessione del dispositivo](../media/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a><span data-ttu-id="fe3ae-143">Inviare dati di telemetria simulati</span><span class="sxs-lookup"><span data-stu-id="fe3ae-143">Send simulated telemetry</span></span>

1. <span data-ttu-id="fe3ae-144">Aprire il [simulatore Raspberry Pi di Azure IoT](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).</span><span class="sxs-lookup"><span data-stu-id="fe3ae-144">Open the [Raspberry Pi Azure IoT Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).</span></span>
1. <span data-ttu-id="fe3ae-145">Sostituire il segnaposto nella riga 15 con la stringa di connessione del dispositivo dell'hub IoT di Azure copiata.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-145">Replace the placeholder in Line 15 with the Azure IoT hub device connection string you just copied.</span></span>
1. <span data-ttu-id="fe3ae-146">Fare clic sul pulsante `Run` o digitare `npm start` nella finestra della console per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-146">Click the `Run` button or type `npm start` in the console window to run the application.</span></span>

    ![Sostituire la stringa di connessione del dispositivo](../media/Line15.png)

1. <span data-ttu-id="fe3ae-148">Dovrebbe essere visibile l'output seguente che mostra i dati del sensore e i messaggi inviati all'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-148">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

    ![Output - dati del sensore inviati da Raspberry Pi all'hub IoT](../media/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a><span data-ttu-id="fe3ae-150">Leggere i dati di telemetria dell'hub</span><span class="sxs-lookup"><span data-stu-id="fe3ae-150">Read the telemetry from your hub</span></span>
<span data-ttu-id="fe3ae-151">Che cosa sta succedendo?</span><span class="sxs-lookup"><span data-stu-id="fe3ae-151">So what's happening?</span></span> <span data-ttu-id="fe3ae-152">L'hub IoT sta ricevendo i messaggi da dispositivo a cloud inviati dal dispositivo simulato.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-152">IoT hub is receiving the device-to-cloud messages sent from the simulated device.</span></span> <span data-ttu-id="fe3ae-153">Per verificarlo, esaminiamo rapidamente come l'hub IoT di Azure sta elaborando i dati in ingresso.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-153">In order to see that, let's take a quick look at how Azure IoT Hub is processing the incoming data.</span></span> <span data-ttu-id="fe3ae-154">Nell'hub IoT, in **Monitoraggio** selezionare **Metrica**.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-154">In your IoT Hub, under **Monitoring**, select **Metrics**.</span></span> <span data-ttu-id="fe3ae-155">La visualizzazione dei dati nell'immagine richiede alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="fe3ae-155">Give it a few minutes as you wait for the data to come into the picture.</span></span>

![Metriche di hub IoT](../media/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
