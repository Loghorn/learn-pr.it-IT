[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="cc1b2-101">In pratica, ad Azure IoT Central sarà connesso un dispositivo fisico, ad esempio una macchina per il caffè abilitata per IoT.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-101">In practice, you will connect Azure IoT Central to a physical device, i.e. an IoT enabled coffee machine.</span></span> <span data-ttu-id="cc1b2-102">In questo caso, si simulerà un dispositivo con un'applicazione Node.jse e lo si connetterà all'applicazione Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-102">Here, you'll simualate a device with a Node.js applicatio and connect it to the Azure IoT Central application.</span></span> <span data-ttu-id="cc1b2-103">Le misure di telemetria della macchina per il caffè vengono inviate a IoT Central per eseguire il monitoraggio e l'analisi.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-103">Telemetry measurements from the simulated coffee machine are sent to IoT Central for monitoring and analysis.</span></span>

![Immagine di una macchina per il caffè.](../media/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a><span data-ttu-id="cc1b2-105">Aggiungere la macchina per il caffè in IoT Central</span><span class="sxs-lookup"><span data-stu-id="cc1b2-105">Add the coffee machine in IoT Central</span></span> 
<span data-ttu-id="cc1b2-106">Per aggiungere la macchina per il caffè all'applicazione, usare il modello di dispositivo **Connected Coffee Maker** creato nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-106">To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.</span></span>

1. <span data-ttu-id="cc1b2-107">Per aggiungere un nuovo dispositivo, scegliere **Device Explorer** nel menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-107">To add a new device, choose **Device Explorer** in the left navigation menu.</span></span>

    <span data-ttu-id="cc1b2-108">Per avviare la connessione della machina per il caffè, scegliere **+ New** (+Nuovo), **Real** (Reale), quindi **Create** (Crea).</span><span class="sxs-lookup"><span data-stu-id="cc1b2-108">To start connecting your coffee machine, choose **+ New**, **Real**, and then **Create**.</span></span> <span data-ttu-id="cc1b2-109">Al termine verrà visualizzato un elenco dei dispositivi creati con lo stesso modello Connected Coffee Maker.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-109">When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.</span></span>
   
    *   <span data-ttu-id="cc1b2-110">Il modello Connected Coffee Maker viene aggiunto all'elenco dopo aver scelto **+ New** (+Nuovo), **Real** (Reale).</span><span class="sxs-lookup"><span data-stu-id="cc1b2-110">The Connected Coffee Maker is added to the list when you choose **+ New** and then **Real**.</span></span> 
    *   <span data-ttu-id="cc1b2-111">Il modello Connected Coffee Maker (Simulate) viene creato automaticamente da IoT Central per finalità di test.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-111">The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes.</span></span> 

1.  <span data-ttu-id="cc1b2-112">È facoltativamente possibile differenziare la macchina per il caffè appena aggiunta specificando la parola "Real" alla fine del nome.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-112">Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name.</span></span> <span data-ttu-id="cc1b2-113">Per rinominare il dispositivo, scegliere il dispositivo e modificare il nome nel campo corrispondente.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-113">To rename your new device, choose the device and edit the name in the name field.</span></span> 

    ![Macchina per il caffè](../media/3-connect-device-a.png) 

    <span data-ttu-id="cc1b2-115">Annotare la posizione di **Connect this device** (Connetti questo dispositivo) per potere connettere la macchina per il caffè nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-115">Note the location of **Connect this device** for connecting your coffee machine in the next section.</span></span> <span data-ttu-id="cc1b2-116">Per il momento la schermata visualizza il messaggio "Missing Data" (Dati mancanti) perché la macchina per il caffè non è stata connessa.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-116">For now, the screen displays "Missing Data" because you haven't connected to the coffee machine.</span></span> <span data-ttu-id="cc1b2-117">I dati effettivi di telemetria iniziano a popolare lo schermo quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-117">The real telemetry begins to populate the screen once the connection is made.</span></span> 
 
## <a name="generate-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="cc1b2-118">Generare la stringa di connessione per la macchina per il caffè dall'applicazione</span><span class="sxs-lookup"><span data-stu-id="cc1b2-118">Generate connection string for the coffee machine from your application</span></span>
<span data-ttu-id="cc1b2-119">La stringa di connessione per la machina per il caffè reale viene incorporata nel codice eseguito nel dispositivo.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-119">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="cc1b2-120">La stringa di connessione consente alla macchina per il caffè di connettersi in modo sicuro all'applicazione Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-120">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="cc1b2-121">Ogni istanza del dispositivo ha una stringa di connessione univoca.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-121">Every device instance has a unique connection string.</span></span> <span data-ttu-id="cc1b2-122">Nei passaggi successivi la stringa di connessione viene generata durante la creazione dell'applicazione Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-122">In the next steps, you generate the connection string as part of creating your node.js application.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="cc1b2-123">Creare un'applicazione Node.js</span><span class="sxs-lookup"><span data-stu-id="cc1b2-123">Create a Node.js application</span></span>
<span data-ttu-id="cc1b2-124">I passaggi seguenti illustrano come creare un'applicazione client che implementa la macchina per il caffè aggiunta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-124">The following steps show you how to create a client application that implements the coffee machine you added to the application.</span></span>

> [!TIP]
> <span data-ttu-id="cc1b2-125">In questo esercizio si creerà l'app in Azure Cloud Shell in modo che non sia necessario installare altro nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-125">In this exercise, we'll create the app in the Azure Cloud Shell so that you don't have to install anything on your local machine.</span></span> 

1. <span data-ttu-id="cc1b2-126">Eseguire il comando seguente in Azure Cloud Shell per creare una cartella `coffee-maker` e accedervi:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-126">Execute the following command in the Azure Cloud Shell to create a `coffee-maker` folder and navigate to it:</span></span>

    ```azurecli
    mkdir ~/coffee-maker
    cd ~/coffee-maker
    ```

1. <span data-ttu-id="cc1b2-127">Dalla cartella `coffee-maker` in Cloud Shell, eseguire il comando seguente per installare il generatore di chiavi del servizio DPS:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-127">From the `coffee-maker` folder in Cloud Shell, execute the following command to install the DPS key generator:</span></span> 

   ```azurecli
    npm install dps-keygen
   ```
    <span data-ttu-id="cc1b2-128">Questo comando installa il pacchetto dps-keygen nella cartella `coffee-maker` locale.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-128">This command installs the dps-keygen package to our local folder, `coffee-maker`.</span></span> <span data-ttu-id="cc1b2-129">È stata tralasciata l'opzione `-g` perché non si hanno le autorizzazioni per installare un pacchetto globale.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-129">We leave out the `-g` option because we don't have permissions to install as a global package.</span></span>

    <!-- TODO: Add more information about the DPS key generator and what it's used for -->
 
1. <span data-ttu-id="cc1b2-130">Eseguire il comando seguente in Cloud Shell per scaricare l'utilità della stringa di connessione DPS da GitHub:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-130">Execute the following command in the Cloud Shell to download the DPS connection string utility from GitHub:</span></span> 

    ```azurecli
    wget https://github.com/Azure/dps-keygen/blob/master/bin/linux/dps_cstr?raw=true -O dps_cstr 
    ```

    > [!NOTE]
    > <span data-ttu-id="cc1b2-131">È stata scaricata la versione Linux di **dps_cstr** perché viene eseguita in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-131">We downloaded the Linux version of **dps_cstr** because we're running in the Cloud Shell.</span></span>

1. <span data-ttu-id="cc1b2-132">Eseguire il comando seguente per concedere le autorizzazioni di esecuzione `dps_cstr`:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-132">Execute the following command to give `dps_cstr` execute permissions:</span></span>

    ```azurecli
    chmod +x dps_cstr
    ```

    <span data-ttu-id="cc1b2-133">Per generare una stringa di connessione per il dispositivo, sono necessarie tre informazioni che si ottengono dal portale di Azure IoT Central:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-133">To generate a connection string for our device, we need three pieces of information from the Azure IoT Central portal:</span></span>
    - <span data-ttu-id="cc1b2-134">**ID Ambito**</span><span class="sxs-lookup"><span data-stu-id="cc1b2-134">**Scope ID**</span></span>
    - <span data-ttu-id="cc1b2-135">**ID dispositivo**</span><span class="sxs-lookup"><span data-stu-id="cc1b2-135">**Device ID**</span></span>
    - <span data-ttu-id="cc1b2-136">**Chiave primaria**</span><span class="sxs-lookup"><span data-stu-id="cc1b2-136">**Primary Key**</span></span>

1. <span data-ttu-id="cc1b2-137">Tornare al portale IoT Central.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-137">Return to the IoT Central portal.</span></span> <span data-ttu-id="cc1b2-138">Nella schermata del dispositivo *Connected Coffee Maker - Real* selezionare **Connect** (Connetti) nella parte superiore destra della schermata.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-138">On the device screen for the *Connected Coffee Maker - Real* device, select **Connect** in the top right of the screen.</span></span>

1. <span data-ttu-id="cc1b2-139">Nella finestra di dialogo Device Connection (Connessione dispositivo) visualizzata salvare i valori **Scope ID** (ID ambito), **Device ID** (ID dispositivo) e **Primary Key** (Chiave primaria) che saranno usati più avanti in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-139">On the Device Connection dialog that opens, save the values **Scope ID**, **Device ID** and **Primary Key** somewhere, because we'll use them later in this exercise.</span></span> 

1. <span data-ttu-id="cc1b2-140">Eseguire il comando seguente in Cloud Shell, sostituendo **< scope_id >**, **< device_id >** e **< primary_key >** con i valori salvati nell'ultimo passaggio.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-140">Execute the following command in the Cloud Shell, replacing **<scope_id>**, **<device_id>**, and **<primary_key>** with the values you saved in the last step.</span></span> 

   ```azurecli
   ./dps_cstr [scope_id] [device_id] [primary_key] > connection.txt
   ```
  
    <span data-ttu-id="cc1b2-141">Questo comando genera una stringa di connessione in base ai valori immessi e li scrive in un file denominato **connection.txt**.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-141">This command generates a connection string based on the values you gave it and writes them to a file that we've named **connection.txt**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cc1b2-142">Il comando `dps_cstr` non è presente nel percorso della shell.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-142">The command `dps_cstr` is not in your PATH in the shell.</span></span> <span data-ttu-id="cc1b2-143">Assicurarsi quindi di chiamarlo con `./dps_cstr`</span><span class="sxs-lookup"><span data-stu-id="cc1b2-143">So, make sure to call it with `./dps_cstr`</span></span>

1. <span data-ttu-id="cc1b2-144">Per aprire l'editor di codice integrato in Cloud Shell, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-144">Open the integrated code editor in the Cloud Shell by running the following command:</span></span> 

    ```azurecli
    code
    ```
1. <span data-ttu-id="cc1b2-145">Selezionare **connection.txt** dall'elenco di file nel menu **FILE** dell'editor.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-145">Select **connection.txt** from the list of files in the **FILES** menu of the editor.</span></span>

1. <span data-ttu-id="cc1b2-146">Verificare che il file **connection.txt** contenga una stringa di connessione che inizia con ``HostName=``.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-146">Verify that **connection.txt** contains a connection string that starts with ``HostName=``.</span></span>

1. <span data-ttu-id="cc1b2-147">Chiudere l'editor selezionando **Chiudi editor** dal menu (*...* ) nella parte superiore destra dell'editor.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-147">Close the editor by selecting **Close Editor** from the menu (*...*) at the top right of the editor.</span></span> 

1. <span data-ttu-id="cc1b2-148">Eseguire il comando seguente in Cloud Shell per inizializzare un progetto Node.js nella cartella `coffee-maker`:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-148">Execute the following command in the Cloud Shell to initialize a Node.js project in our `coffee-maker` folder:</span></span>

    ```azurecli
    npm init
    ```
    > [!NOTE]
    > <span data-ttu-id="cc1b2-149">Lo script init richiede l'immissione delle proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-149">The init script prompts you to enter project properties.</span></span> <span data-ttu-id="cc1b2-150">Per questo esercizio, premere INVIO per usare tutti i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-150">For this exercise, press ENTER to use all default values.</span></span>

1. <span data-ttu-id="cc1b2-151">Per installare i pacchetti necessari, eseguire questo comando nella cartella `coffee-maker`:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-151">To install the necessary packages, run the following command in our `coffee-maker` folder:</span></span>

    ```azurecli
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="cc1b2-152">Eseguire questo comando per creare un nuovo file in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-152">Execute the following command to create a new file in the Cloud Shell:</span></span>

    ```azurecli
    touch coffeeMaker.js
    ```
1. <span data-ttu-id="cc1b2-153">Per aprire l'editor di codice integrato, eseguire il comando seguente dalla riga di comando in Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-153">Open the integrated code editor by executing the following at the command line in the Cloud Shell:</span></span> 

     ```azurecli
    code
    ```
    
1. <span data-ttu-id="cc1b2-154">All'apertura dell'editor di codice, selezionare il pulsante di aggiornamento nell'elenco **FILE** e selezionare il nuovo file **coffeeMaker.js**.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-154">When the code editor opens, select the refresh button on the **FILES** list and select our new file **coffeeMaker.js**.</span></span> 

1. <span data-ttu-id="cc1b2-155">Copiare e incollare il codice seguente nella finestra vuota dell'editor:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-155">Copy and paste the following code into the empty editor window:</span></span>

    ```js
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;

    // Connection string (TODO: CHANGE HERE)
    const connectionString = `{your device connection string}`;

    // Global variables
    var client = clientFromConnectionString(connectionString);
    var optimalTemperature = 96;
    var cupState = true;
    var brewingState = false;
    var cupTimer = 20;
    var brewingTimer = 0;
    var maintenanceState = false;
    var warrantyState = Math.random() < 0.5?0:1;

    // Helper function to produce nice numbers (##.#)
    function niceNumber(value) 
    {
        var number = (Math.round(value * 10.0)).toString();
        return number.substr(0, 2) + '.' + number.substr(2, 1);
    }

    // Send device simulated telemetry measurements
    function sendTelemetry() 
    {   
        // Simulate the telemetry values
        var temperature = optimalTemperature + (Math.random() * 4) - 2;
        var humidity = 20 + (Math.random() * 80);
        
        // Cup timer - every 20 seconds randomly decide if the cup is present or not
        cupTimer--;
        
        if (cupTimer == 0)
        {
            cupTimer = 20;
            cupState = Math.random() < 0.5?false:true;
        }
        
        // Brewing timer
        if (brewingTimer > 0)
        {
            brewingTimer--;
            
            // Finished brewing
            if (brewingTimer == 0)
            {
                brewingState = false;
            }
        }
    
        // Create the data JSON package
        var data = JSON.stringify(
        { 
            waterTemperature: temperature, 
            airHumidity: humidity, 
        });

        // Create the message with the above defined data
        var message = new Message(data);
        
        // Set the state flags
        message.properties.add('stateCupDetected', cupState);
        message.properties.add('stateBrewing', brewingState);
        
        // Show the information in console
        var infoTemperature = niceNumber(temperature);
        var infoHumidity = niceNumber(humidity);
        var infoCup = cupState ? 'Y' :'N';
        var infoBrewing = brewingState ? 'Y':'N';
        var infoMaintenance = maintenanceState ? 'Y':'N';
        
        console.log('Telemetry send: Temperature: ' + infoTemperature + 
                    ' Humidity: ' + infoHumidity + '%' + 
                    ' Cup Detected: ' + infoCup + 
                    ' Brewing: ' + infoBrewing + 
                    ' Maintenance Mode: ' + infoMaintenance);
        
        // Send the message
        client.sendEvent(message, function (errorMessage) 
        {
            // Error
            if (errorMessage) 
            {
                console.log('Failed to send message to Azure IoT Hub: ${err.toString()}');
            }
        });
    }

    // Send device properties
    function sendDeviceProperties(deviceTwin) 
    {
        var properties = 
        {
            propertyWarrantyExpired: warrantyState
        };
        
        console.log(' * Property - Warranty State: ' + warrantyState.toString());
        
        deviceTwin.properties.reported.update(properties, (errorMessage) => 
            console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }

    // Optimal temperature setting
    var settings = 
    {
        'setTemperature': (newValue, callback) => 
        {
            setTimeout(() => 
            {
                optimalTemperature = newValue;
                callback(optimalTemperature, 'completed');
            }, 1000);
        }
    };

    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(deviceTwin) 
    {
        deviceTwin.on('properties.desired', function (desiredChange) 
        {
            // Iterate all settings looking for the defined one
            for (let setting in desiredChange) 
            {
                // Setting we defined
                if (settings[setting]) 
                {
                    // Console info
                    console.log(` * Received setting: ${setting}: ${desiredChange[setting].value}`);
                    
                    // Update 
                    settings[setting](desiredChange[setting].value, (newValue, status, message) => 
                    {
                        var patch = 
                        {
                            [setting]: 
                            {
                                value: newValue,
                                status: status,
                                desiredVersion: desiredChange.$version,
                                message: message
                            }
                        }
                        deviceTwin.properties.reported.update(patch, (err) => console.log(` * Sent setting update for ${setting} ` +
                        (err ? `error: ${err.toString()}` : `(success)`)));
                    });
                }
            }
        });
    }

    // Maintenance mode command
    function onCommandMaintenance(request, response) 
    {
        // Display console info
        console.log(' * Maintenance command received');

        // Console warning
        if (maintenanceState)
        {
            console.log(' - Warning: The device is already in the maintenance mode.');
        }
        
        // Set state
        maintenanceState = true;

        // Respond
        response.send(200, 'Success', function (errorMessage) 
        {
            // Failure
            if (errorMessage) 
            {
                console.error('[IoT hub Client] Failed sending a method response:\n' + errorMessage.message);
            }
        });
    }

    function onCommandStartBrewing(request, response) 
    {
        // Display console info
        console.log(' * Brewing command received');

        // Console warning
        if (brewingState == true)
        {
            console.log(' - Warning: The device is already brewing.');
        }
        
        if (cupState == false)
        {
            console.log(' - Warning: The cup has not been detected.');
        }
        
        if (maintenanceState == true)
        {
            console.log(' - Warning: The device is in maintenance state.');
        }
        
        // Set state - brew for 30 seconds
        if ((cupState == true) && (brewingState == false) && (maintenanceState == false))
        {
            brewingState = true;
            brewingTimer = 30;
        }
        
        // Respond
        response.send(200, 'Success', function (errorMessage) 
        {
            // Failure
            if (errorMessage) 
            {
                console.error('[IoT hub Client] Failed sending a method response:\n' + errorMessage.message);
            }
        });
    }

    // Handle device connection to Azure IoT Central
    var connectCallback = (errorMessage) => 
    {
        // Connection error
        if (errorMessage) 
        {
            console.log(`Device could not connect to Azure IoT Central: ${errorMessage.toString()}`);
        } 
        // Successfully connected
        else 
        {
            // Notify the user
            console.log('Device successfully connected to Azure IoT Central');

            // Send telemetry measurements to Azure IoT Central every 1 second.
            setInterval(sendTelemetry, 1000);
            
            // Set up device command callbacks
            client.onDeviceMethod('cmdSetMaintenance', onCommandMaintenance);
            client.onDeviceMethod('cmdStartBrewing', onCommandStartBrewing);
            
            // Get device twin from Azure IoT Central
            client.getTwin((errorMessage, deviceTwin) => 
            {
                // Failed to retrieve device twin
                if (errorMessage) 
                {
                    console.log(`Error getting device twin: ${errorMessage.toString()}`);
                } 
                // Success
                else 
                {
                    // Notify the user
                    console.log('Device Twin successfully retrieved from Azure IoT Central');
                
                    // Send device properties once on device startup
                    sendDeviceProperties(deviceTwin);
                    
                    // Apply device settings and handle changes to device settings
                    handleSettings(deviceTwin);
                }
            });
        }
    };

    // Start the device (connect it to Azure IoT Central)
    client.open(connectCallback);

    ```

    <span data-ttu-id="cc1b2-156">La macchina per il caffè è scritta in Node.js.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-156">Our coffee machine is written in Node.js.</span></span> <span data-ttu-id="cc1b2-157">Prima si connette ad Azure IoT Central,</span><span class="sxs-lookup"><span data-stu-id="cc1b2-157">It first connects to Azure IoT Central.</span></span> <span data-ttu-id="cc1b2-158">poi l'app invia le proprietà iniziali ad Azure IoT Central, sincronizza le impostazioni, registra due gestori di comandi per la manutenzione e la preparazione del caffè e infine avvia il timer per inviare le informazioni di telemetria ogni secondo.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-158">Then the app sends initial properties to Azure IoT Central, synchronizes settings, registers two command handlers for maintenance and brewing, and finally starts the timer for sending the telemetry information every second.</span></span>

1.  <span data-ttu-id="cc1b2-159">Aggiornare il segnaposto `{your device connection string}` all'inizio del codice con la stringa di connessione creata in precedenza e salvata in **connection.txt**.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-159">Update the placeholder `{your device connection string}` at the top of this code with the connection string you created earlier and saved in **connection.txt**.</span></span> <span data-ttu-id="cc1b2-160">La stringa di connessione inizia con `HostName=`.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-160">The connection string begins with `HostName=`.</span></span>

1. <span data-ttu-id="cc1b2-161">Selezionare i tre puntini `...` nella parte superiore destra dell'editor per espandere il menu dell'editor.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-161">Select the three dots `...` to the top right of the editor to expand the editor menu.</span></span> <span data-ttu-id="cc1b2-162">Quindi selezionare **Salva** per salvare le modifiche apportate a "coffeeMaker.js".</span><span class="sxs-lookup"><span data-stu-id="cc1b2-162">Then select **Save** to save the edits we made to \`coffeeMaker.js'</span></span>

1. <span data-ttu-id="cc1b2-163">Eseguire il comando seguente nella finestra di Cloud Shell per avviare l'app:</span><span class="sxs-lookup"><span data-stu-id="cc1b2-163">Execute the following command in the Cloud Shell window to start the app:</span></span>

    ```azurecli
    node coffeeMaker.js
    ```
1. <span data-ttu-id="cc1b2-164">Verificare che l'app venga avviata nella finestra di Cloud Shell e che vengano visualizzati i messaggi *Device successfully connected to Azure IoT Central* (Dispositivo connesso ad Azure IoT Central) e *Telemetry send:* (Invio dati di telemetria).</span><span class="sxs-lookup"><span data-stu-id="cc1b2-164">Verify that the app starts in the Cloud Shell window with the message *Device successfully connected to Azure IoT Central* along with *Telemetry send:* messages.</span></span> <span data-ttu-id="cc1b2-165">La procedura è stata completata.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-165">Congratulations!</span></span> <span data-ttu-id="cc1b2-166">L'app è attiva e in esecuzione e comunica con IoT Central.</span><span class="sxs-lookup"><span data-stu-id="cc1b2-166">Your app is up and running and communicating with IoT Central!</span></span>