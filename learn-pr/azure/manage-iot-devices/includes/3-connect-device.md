<span data-ttu-id="cf3f7-101">L'acquisizione dei dati meteorologici è un'attività importante, in quanto il meteo può influire su numerosi fattori, dai modelli di traffico alla modalità di funzionamento dei sistemi HVAC nei negozi al dettaglio.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-101">Capturing of weather data is an important task as weather can effect everything from traffic patterns to how HVAC systems in retail stores are operated.</span></span> <span data-ttu-id="cf3f7-102">In questo esercizio si userà il simulatore Raspberry Pi online per acquisire dati meteo simulati e acquisire tali dati tramite l'hub IoT di Azure.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-102">In this exercise, you will be harnessing the online Raspberry Pi simulator to capture simulated weather data and capture said data via the Azure IoT Hub.</span></span>

<span data-ttu-id="cf3f7-103">Mentre l'esercizio viene eseguito in un ambiente simulato, l'applicazione in esecuzione nel dispositivo simulato potrà essere trasferita a un dispositivo reale in futuro.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-103">While this exercise is being conducted in a simulated environment, the application running on the simulated device can be transferred to a real device in future.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="cf3f7-104">Creare un hub IoT</span><span class="sxs-lookup"><span data-stu-id="cf3f7-104">Create an IoT hub</span></span>

1. <span data-ttu-id="cf3f7-105">Accedere al [portale di Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cf3f7-105">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

1. <span data-ttu-id="cf3f7-106">Selezionare **Crea una risorsa** \> **Internet delle cose**\>**Hub IoT**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-106">Select **Create a resource** \> **Internet of Things** \> **IoT Hub**.</span></span>

![Screenshot della selezione dell'hub IoT nel portale di Azure](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. <span data-ttu-id="cf3f7-108">Nel riquadro **Hub IoT** immettere le informazioni seguenti per l'hub IoT:</span><span class="sxs-lookup"><span data-stu-id="cf3f7-108">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

 - <span data-ttu-id="cf3f7-109">**Sottoscrizione**: scegliere la sottoscrizione da usare per creare questo hub IoT.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-109">**Subscription**: Choose the subscription that you want to use to create this IoT hub.</span></span>
 - <span data-ttu-id="cf3f7-110">**Gruppo di risorse**: creare un gruppo di risorse per ospitare l'hub IoT o usarne uno esistente.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-110">**Resource group**: Create a resource group to host the IoT hub or use an existing one.</span></span> <span data-ttu-id="cf3f7-111">Per altre informazioni, vedere [Usare i gruppi di risorse per gestire le risorse di Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).</span><span class="sxs-lookup"><span data-stu-id="cf3f7-111">For more information, see [Use resource groups to manage your Azure resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).</span></span>
 - <span data-ttu-id="cf3f7-112">**Area**: selezionare la località più vicina.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-112">**Region**: Select the closest location to you.</span></span>
 - <span data-ttu-id="cf3f7-113">**Nome**: creare un nome per l'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-113">**Name**: Create a name for your IoT hub.</span></span> <span data-ttu-id="cf3f7-114">Se il nome immesso è disponibile, viene visualizzato un segno di spunta verde.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-114">If the name you enter is available, a green check mark appears.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cf3f7-115">L'hub IoT sarà individuabile pubblicamente come endpoint DNS, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-115">The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.</span></span>

   ![Finestra delle informazioni di base dell'hub IoT](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  <span data-ttu-id="cf3f7-117">Selezionare **Avanti: Dimensioni e piano** per continuare a creare l'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-117">Select **Next: Size and scale** to continue creating your IoT hub.</span></span>

1.  <span data-ttu-id="cf3f7-118">Scegliere un valore per **Piano tariffario e livello di scalabilità**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-118">Choose your **Pricing and scale tier**.</span></span> <span data-ttu-id="cf3f7-119">Per questo articolo selezionare il livello **F1 - Gratuito** se ancora disponibile nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-119">For this article, select the **F1 - Free** tier if it's still available on your subscription.</span></span> <span data-ttu-id="cf3f7-120">Per altre informazioni, vedere [Piano tariffario e livello di scalabilità](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="cf3f7-120">For more information, see the [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

   ![Finestra per specificare dimensioni e il piano dell'hub IoT](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  <span data-ttu-id="cf3f7-122">Selezionare **Rivedi e crea**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-122">Select **Review + create**.</span></span>

1.  <span data-ttu-id="cf3f7-123">Esaminare le informazioni sull'hub IoT e quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-123">Review your IoT hub information, then click **Create**.</span></span> <span data-ttu-id="cf3f7-124">La creazione dell'hub IoT può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-124">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="cf3f7-125">È possibile monitorare lo stato di avanzamento nel riquadro **Notifiche**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-125">You can monitor the progress in the **Notifications** pane.</span></span>

<span data-ttu-id="cf3f7-126">Ora che è stato creato un hub IoT, individuare le informazioni importanti che consentono di connettere dispositivi e applicazioni all'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-126">Now that you have created an IoT hub, locate the important information that you use to connect devices and applications to your IoT hub.</span></span>

<span data-ttu-id="cf3f7-127">Nel menu di spostamento dell'hub IoT, aprire **Criteri di accesso condiviso**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-127">In your IoT hub navigation menu, open **Shared access policies**.</span></span> <span data-ttu-id="cf3f7-128">Selezionare il criterio **iothubowner**, quindi copiare la **stringa di connessione---chiave primaria** dell'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-128">Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub.</span></span> <span data-ttu-id="cf3f7-129">Per altre informazioni, vedere [Controllare l'accesso all'hub IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).</span><span class="sxs-lookup"><span data-stu-id="cf3f7-129">For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).</span></span>

> [!NOTE]
> <span data-ttu-id="cf3f7-130">Per questa esercitazione di configurazione non è necessaria la stringa di connessione iothubowner.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-130">You do not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="cf3f7-131">Potrebbe essere tuttavia necessaria per alcune delle esercitazioni o per altri scenari IoT, dopo aver completato questa configurazione.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-131">However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.</span></span>

![Ottenere la stringa di connessione dell'hub IoT](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a><span data-ttu-id="cf3f7-133">Registrare un dispositivo nell'hub IoT per il dispositivo</span><span class="sxs-lookup"><span data-stu-id="cf3f7-133">Register a device in the IoT hub for your device</span></span>
------------------------------------------------

1.  <span data-ttu-id="cf3f7-134">Nel menu di spostamento dell'hub IoT, aprire **Dispositivi IoT**, quindi fare clic su **Aggiungi** per registrare un dispositivo nell'hub IoT.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-134">In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.</span></span>

   ![Aggiungere un dispositivo a Dispositivi IoT nell'hub IoT](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  <span data-ttu-id="cf3f7-136">Immettere un **ID dispositivo** per il nuovo dispositivo.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-136">Enter a **Device ID** for the new device.</span></span> <span data-ttu-id="cf3f7-137">Gli ID dispositivo fanno distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-137">Device IDs are case sensitive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cf3f7-138">L'ID dispositivo può essere visibile nei log raccolti per il supporto tecnico e la risoluzione dei problemi, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-138">The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.</span></span>

1.  <span data-ttu-id="cf3f7-139">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-139">Click **Save**.</span></span>

1.  <span data-ttu-id="cf3f7-140">Dopo la creazione del dispositivo, aprirlo nel riquadro **Dispositivi IoT**.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-140">After the device is created, open the device from the list in the **IoT devices** pane.</span></span>

1.  <span data-ttu-id="cf3f7-141">Copiare il valore di **Stringa di connessione---Chiave primaria** dell'hub IoT per usarlo in seguito.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-141">Copy the **Connection string---primary key** to use later.</span></span>

   ![Ottenere la stringa di connessione del dispositivo](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="cf3f7-143">Eseguire un'applicazione di esempio nel simulatore Web Pi</span><span class="sxs-lookup"><span data-stu-id="cf3f7-143">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="cf3f7-144">Nell'area di codifica assicurarsi di lavorare nell'applicazione di esempio predefinita.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-144">In coding area, make sure you are working on the default sample application.</span></span> <span data-ttu-id="cf3f7-145">Sostituire il segnaposto nella riga 15 con la stringa di connessione del dispositivo dell'hub IoT di Azure.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-145">Replace the placeholder in Line 15 with the Azure IoT hub device connection string.</span></span>

    ![Sostituire la stringa di connessione del dispositivo](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  <span data-ttu-id="cf3f7-147">Fare clic su **Esegui** o digitare npm per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cf3f7-147">Click **Run** or type npm start to run the application.</span></span>

<span data-ttu-id="cf3f7-148">Verrà visualizzato l'output seguente che mostra i dati del sensore e i messaggi inviati all'hub IoT</span><span class="sxs-lookup"><span data-stu-id="cf3f7-148">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub</span></span>

   ![Output - dati del sensore inviati da Raspberry Pi all'hub IoT](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
