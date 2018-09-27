<span data-ttu-id="aaced-101">È stata usata l'applicazione Azure IoT Central e la macchina per il caffè è stata connessa ad Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="aaced-101">You’ve now worked with the Azure IoT Central application and connected the coffee machine to Azure IoT Central.</span></span> <span data-ttu-id="aaced-102">È quasi tutto pronto per iniziare a monitorare e gestire la macchina per il caffè remota.</span><span class="sxs-lookup"><span data-stu-id="aaced-102">You are well on your way to begin to monitor and manage your remote coffee machine.</span></span> <span data-ttu-id="aaced-103">In questa unità verrà eseguita la convalida della configurazione e della connessione mediante il modello Connected Coffee Maker definito in precedenza.</span><span class="sxs-lookup"><span data-stu-id="aaced-103">In this unit, you take a moment to validate your setup and connection by using the Connected Coffee Maker template that you defined earlier.</span></span> <span data-ttu-id="aaced-104">Verrà aggiornata la temperatura ottimale nelle impostazioni, verranno eseguiti comandi per verificare lo stato della macchina e verrà visualizzata la macchina per il caffè connessa nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="aaced-104">You update the optimal temperature in settings, run commands to check for the state of your machine, and view your connected coffee machine in the dashboard.</span></span> 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a><span data-ttu-id="aaced-105">Aggiornare le impostazioni per sincronizzare l'applicazione con la macchina per il caffè</span><span class="sxs-lookup"><span data-stu-id="aaced-105">Update settings to sync your application with the coffee machine</span></span>

<span data-ttu-id="aaced-106">Nella pagina **Settings** (Impostazioni) inviare i dati di configurazione alla macchina per il caffè dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aaced-106">On the **Settings** page, you send configuration data to the coffee machine from your application.</span></span> 

<span data-ttu-id="aaced-107">In questo scenario modificare la temperatura ottimale e scegliere **Update** (Aggiorna).</span><span class="sxs-lookup"><span data-stu-id="aaced-107">In this scenario, change the optimal temperature and choose **Update**.</span></span> <span data-ttu-id="aaced-108">Quando si modifica un'impostazione, questa viene contrassegnata come in sospeso nell'interfaccia utente finché la macchina per il caffè non riconosce la modifica.</span><span class="sxs-lookup"><span data-stu-id="aaced-108">When the setting is changed, the setting is marked as pending in the UI until the coffee machine acknowledges that it has responded to the setting change.</span></span> 

> [!NOTE]
> <span data-ttu-id="aaced-109">L'esecuzione corretta degli aggiornamenti nell'impostazione indica il flusso di dati e convalida la connessione.</span><span class="sxs-lookup"><span data-stu-id="aaced-109">Successful updates in the setting indicate data flow and validate your  connection.</span></span> <span data-ttu-id="aaced-110">Le misure di telemetria risponderanno all'aggiornamento della temperatura ottimale.</span><span class="sxs-lookup"><span data-stu-id="aaced-110">The telemetry measurements will respond to the update in optimal temperature.</span></span> <span data-ttu-id="aaced-111">È possibile osservare la modifica nella pagina **Measurements** (Misure).</span><span class="sxs-lookup"><span data-stu-id="aaced-111">You can observe the change on the **Measurements** page.</span></span> 

## <a name="run-commands-on-the-coffee-machine"></a><span data-ttu-id="aaced-112">Eseguire i comandi nella macchina per il caffè</span><span class="sxs-lookup"><span data-stu-id="aaced-112">Run commands on the coffee machine</span></span> 
<span data-ttu-id="aaced-113">Passare alla pagina **Commands** (Comandi) per l'esercizio seguente.</span><span class="sxs-lookup"><span data-stu-id="aaced-113">Navigate to the **Commands** page for the following exercise.</span></span> <span data-ttu-id="aaced-114">Per convalidare la configurazione dei comandi, eseguire i comandi in remoto nella macchina per il caffè da IoT Central.</span><span class="sxs-lookup"><span data-stu-id="aaced-114">To validate the commands setup, you remotely run commands on the coffee machine from IoT Central.</span></span> <span data-ttu-id="aaced-115">Se l'esito è positivo, dalla macchina per il caffè vengono inviati messaggi di conferma.</span><span class="sxs-lookup"><span data-stu-id="aaced-115">If the commands are successful, confirmation messages are sent from the coffee machine.</span></span>

1. <span data-ttu-id="aaced-116">Iniziare la preparazione del caffè in remoto scegliendo **Run** (Esegui).</span><span class="sxs-lookup"><span data-stu-id="aaced-116">Start brewing remotely by choosing **Run**.</span></span> 
    
    <span data-ttu-id="aaced-117">La macchina per il caffè si avvierà se vengono soddisfatte le tre condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="aaced-117">The coffee machine will start if these three conditions are satisfied:</span></span>
    - <span data-ttu-id="aaced-118">Cup detected</span><span class="sxs-lookup"><span data-stu-id="aaced-118">Cup detected</span></span>
    - <span data-ttu-id="aaced-119">Not in maintenance</span><span class="sxs-lookup"><span data-stu-id="aaced-119">Not in maintenance</span></span>
    - <span data-ttu-id="aaced-120">Not brewing already</span><span class="sxs-lookup"><span data-stu-id="aaced-120">Not brewing already</span></span>  

    > [!NOTE]
    > <span data-ttu-id="aaced-121">Quando inizia la preparazione del caffè, lo stato della macchina passa al giallo come indicato in **Measurements** > **State** (Misure > Stato).</span><span class="sxs-lookup"><span data-stu-id="aaced-121">When you've successfully started brewing, the state of the machine changes to yellow as indicated in **Measurements** > **State**.</span></span> 
    
    <span data-ttu-id="aaced-122">Cercare i messaggi di conferma nel log della console nella macchina per il caffè simulata (applicazione Node.js).</span><span class="sxs-lookup"><span data-stu-id="aaced-122">Look for confirmation messages in the console log on the node.js simulated coffee machine.</span></span> 

    ![Eseguire i comandi](../media/4-commands-brewing.png)

1. <span data-ttu-id="aaced-124">Impostare la modalità di manutenzione scegliendo **Run** (Esegui).</span><span class="sxs-lookup"><span data-stu-id="aaced-124">Set maintenance mode by choosing **Run**.</span></span> <span data-ttu-id="aaced-125">La macchina per il caffè sarà impostata sulla modalità di manutenzione se *non* lo è già.</span><span class="sxs-lookup"><span data-stu-id="aaced-125">The coffee machine will set to maintenance if it's *not* already in maintenance.</span></span>
    
    <span data-ttu-id="aaced-126">Cercare i messaggi di conferma nel log della console nella macchina per il caffè.</span><span class="sxs-lookup"><span data-stu-id="aaced-126">Look for confirmation messages in the console log on the coffee machine.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="aaced-127">Come nel mondo reale, quando il tecnico mette offline la macchina per eseguire le riparazioni necessarie prima di riportarla online, la macchina per il caffè resta in modalità di manutenzione finché non si riavvia il codice client.</span><span class="sxs-lookup"><span data-stu-id="aaced-127">As in real life, when the technician takes the machine offline to perform necessary repairs before switching it back online, the coffee machine continues to stay in the maintenance mode until you reboot the client code.</span></span>

    ![Eseguire i comandi](../media/4-commands-maintenance.png)

> [!IMPORTANT]
> <span data-ttu-id="aaced-129">È consigliabile eseguire l'applicazione Node.js per al massimo 60 minuti circa, per evitare che invii notifiche/messaggi di posta elettronica indesiderati.</span><span class="sxs-lookup"><span data-stu-id="aaced-129">It's recommended that you run the Node.js application no more than 60 minutes or so to prevent the application from sending you unwanted notifications/emails.</span></span> <span data-ttu-id="aaced-130">Quando non si lavora al modulo, è consigliabile arrestare l'applicazione per evitare l'esaurimento della quota giornaliera di messaggi.</span><span class="sxs-lookup"><span data-stu-id="aaced-130">Stopping the application when you're not working on the module also prevents you from exhausting the daily message quota.</span></span>

## <a name="view-the-coffee-machine-in-the-dashboard"></a><span data-ttu-id="aaced-131">Visualizzare la macchina per il caffè nel dashboard</span><span class="sxs-lookup"><span data-stu-id="aaced-131">View the coffee machine in the dashboard</span></span>

1. <span data-ttu-id="aaced-132">Passare alla scheda**Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="aaced-132">Navigate to the **Dashboard** tab.</span></span>

1. <span data-ttu-id="aaced-133">Selezionare **Edit Template** (Modifica modello) per modificare il dashboard.</span><span class="sxs-lookup"><span data-stu-id="aaced-133">Select **Edit Template** to edit the dashboard.</span></span>

1. <span data-ttu-id="aaced-134">Scegliere **Line Chart** (Grafico a linee) dal menu laterale.</span><span class="sxs-lookup"><span data-stu-id="aaced-134">Choose **Line Chart** from the side menu.</span></span>

1. <span data-ttu-id="aaced-135">In **Configure Chart** (Configura grafico) immettere `Telemetry` nel campo **Title** (Titolo).</span><span class="sxs-lookup"><span data-stu-id="aaced-135">In **Configure Chart**  enter `Telemetry` in the **Title** field.</span></span> <span data-ttu-id="aaced-136">I dati di telemetria saranno visualizzati con questo grafico.</span><span class="sxs-lookup"><span data-stu-id="aaced-136">We'll view our telemetry data with this chart.</span></span> 

1. <span data-ttu-id="aaced-137">Selezionare **Past 30 minutes** (Ultimi 30 minuti) come **Time Range** (Intervallo di tempo).</span><span class="sxs-lookup"><span data-stu-id="aaced-137">Select **Past 30 minutes** for **Time Range**.</span></span> 

1. <span data-ttu-id="aaced-138">Nell'area **Measures** (Misure) selezionare l'icona di visibilità a destra di ogni misura per rendere visibile la misura nel grafico.</span><span class="sxs-lookup"><span data-stu-id="aaced-138">In the **Measures** area, select the visibility icon to the right of each measurement to make that measurement visible for our chart.</span></span> 

1. <span data-ttu-id="aaced-139">Selezionare **Save** (Salva) per salvare la configurazione e visualizzare il grafico a linee.</span><span class="sxs-lookup"><span data-stu-id="aaced-139">Select **Save** to save our configuration and display the line chart.</span></span> 

    ![Visualizzazione del dashboard](../media/4-dashboard-a.png)

1. <span data-ttu-id="aaced-141">Scegliere **Settings and Properties** (Impostazioni e proprietà) dal menu a sinistra per aprire il pannello **Configure Device Details** (Configura dettagli dispositivo).</span><span class="sxs-lookup"><span data-stu-id="aaced-141">Choose **Settings and Properties** in the left-hand menu to open the **Configure Device Details** panel.</span></span> 

1. <span data-ttu-id="aaced-142">Immettere `Device properties` nel campo **Title** (Titolo).</span><span class="sxs-lookup"><span data-stu-id="aaced-142">Enter `Device properties` in the **Title** filed.</span></span>

1. <span data-ttu-id="aaced-143">In **Add/Remove** (Aggiungi/Rimuovi) scegliere **Coffee Makers Max Temperature**, **Coffee Makers Min Temperature**, **Device Warranty Expired**, quindi selezionare **OK** per completare la selezione.</span><span class="sxs-lookup"><span data-stu-id="aaced-143">In **Add/Remove**, choose **Coffee Makers Max Temperature**, **Coffee Makers Min Temperature**, **Device Warranty Expired** and then select **OK** to complete the selection.</span></span>

1. <span data-ttu-id="aaced-144">Selezionare **Save** (Salva) per creare un nuovo pannello *Device properties* (Proprietà dispositivo) nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="aaced-144">Select **Save** to create a new *Device properties* panel in our dashboard.</span></span> 

1. <span data-ttu-id="aaced-145">Scegliere di nuovo **Settings and Properties** (Impostazioni e proprietà) e immettere `Optimal Temperature` come titolo.</span><span class="sxs-lookup"><span data-stu-id="aaced-145">Choose **Settings and Properties** again,  and enter `Optimal Temperature` as the title.</span></span> <span data-ttu-id="aaced-146">In **Add/Remove** (Aggiungi/Rimuovi) scegliere **Optimal Temperature**.</span><span class="sxs-lookup"><span data-stu-id="aaced-146">In **Add/Remove**, choose **Optimal  Temperature**.</span></span>

1. <span data-ttu-id="aaced-147">Selezionare **Save** (Salva) per salvare il lavoro e visualizzare un nuovo riquadro nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="aaced-147">Select **Save** to save your work and display a new pane in our dashboard.</span></span> 

1. <span data-ttu-id="aaced-148">Selezionare **Done** (Fine) per uscire dalla modalità di modifica e visualizzare il nuovo dashboard.</span><span class="sxs-lookup"><span data-stu-id="aaced-148">Select **Done** to exit edit mode and display the new dashboard.</span></span> 