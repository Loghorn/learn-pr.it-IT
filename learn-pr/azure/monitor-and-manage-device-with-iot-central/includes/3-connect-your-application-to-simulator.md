<span data-ttu-id="0c224-101">In uno scenario reale, si presuppone che si connetterà Azure IoT Central a un dispositivo reale o, in questo caso, una macchina per il caffè reale.</span><span class="sxs-lookup"><span data-stu-id="0c224-101">In real life, it's assumed that you will connect Azure IoT Central to a real device or in this case, a real coffee machine.</span></span> <span data-ttu-id="0c224-102">Ai fini di questa unità si connette un'applicazione Node.js, che rappresenta una macchina per il caffè fisica/reale, all'applicazione Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="0c224-102">But for the purpose of this unit, you connect a Node.js application, representing a physical/real coffee machine, to the Azure IoT Central application.</span></span> <span data-ttu-id="0c224-103">In seguito a questa connessione, le misure di telemetria della macchina per il caffè vengono inviati a IoT Central per il monitoraggio e l'analisi.</span><span class="sxs-lookup"><span data-stu-id="0c224-103">As a result of this connection, telemetry measurements from the coffee machine is sent to IoT Central for monitoring and analysis.</span></span>

![Macchina per il caffè](../images/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a><span data-ttu-id="0c224-105">Aggiungere la macchina per il caffè in IoT Central</span><span class="sxs-lookup"><span data-stu-id="0c224-105">Add the coffee machine in IoT Central</span></span> 
<span data-ttu-id="0c224-106">Per aggiungere la macchina per il caffè all'applicazione, usare il modello di dispositivo **Connected Coffee Maker** creato nell'unità precedente.</span><span class="sxs-lookup"><span data-stu-id="0c224-106">To add your coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous unit.</span></span>

1. <span data-ttu-id="0c224-107">Per aggiungere un nuovo dispositivo, scegliere **Device Explorer** nel menu di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="0c224-107">To add a new device, choose **Device Explorer** in the left navigation menu.</span></span>

    <span data-ttu-id="0c224-108">Per avviare la connessione della machina per il caffè, scegliere **+ Nuovo** e quindi **Reale**.</span><span class="sxs-lookup"><span data-stu-id="0c224-108">To start connecting your coffee machine, choose **+ New**, then **Real**.</span></span> <span data-ttu-id="0c224-109">Al termine verrà visualizzato un elenco di dispositivi creati mediante lo stesso modello Connected Coffee Maker.</span><span class="sxs-lookup"><span data-stu-id="0c224-109">When you're finished, you see a list of devices you've created using the same Connected Coffee Maker template.</span></span>
   
    *   <span data-ttu-id="0c224-110">Il dispositivo Connected Coffee Maker viene aggiunto all'elenco quando si sceglie + Nuovo e quindi Reale.</span><span class="sxs-lookup"><span data-stu-id="0c224-110">The Connected Coffee Maker is added to the list when you choose + New, then Real.</span></span> 
    *   <span data-ttu-id="0c224-111">Il dispositivo Connected Coffee Maker (Simulate) viene creato automaticamente da IoT Central per finalità di test.</span><span class="sxs-lookup"><span data-stu-id="0c224-111">The Connected Coffee Maker (Simulate) is automatically created by IoT Central for testing purposes.</span></span> 

1.  <span data-ttu-id="0c224-112">È facoltativamente possibile differenziare la macchina per il caffè appena aggiunta specificando la parola "Real" alla fine del nome.</span><span class="sxs-lookup"><span data-stu-id="0c224-112">Optionally, you can differentiate the newly added coffee machine by appending the word “Real” in its name.</span></span> <span data-ttu-id="0c224-113">Per rinominare il dispositivo, scegliere il dispositivo e modificare il nome nel campo corrispondente.</span><span class="sxs-lookup"><span data-stu-id="0c224-113">To rename your new device, choose the device and edit the name in the name field.</span></span> 

    ![Macchina per il caffè](../images/3-connect-device-a.png) 

    <span data-ttu-id="0c224-115">Annotare la posizione di **Connect this device** (Connetti questo dispositivo) per la connessione della macchina per il caffè nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="0c224-115">Note the location of **Connect this device** for connecting your coffee machine in the next section.</span></span> <span data-ttu-id="0c224-116">Per il momento la schermata visualizza un messaggio relativo ai dati mancanti, perché non è stata stabilita la connessione alla macchina per il caffè.</span><span class="sxs-lookup"><span data-stu-id="0c224-116">For now the screen displays "Missing Data", this is because you haven't connected to the coffee machine.</span></span> <span data-ttu-id="0c224-117">I dati effettivi di telemetria iniziano a popolare lo schermo quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="0c224-117">The real telemetry begins to populate the screen once the connection is made.</span></span> 
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="0c224-118">Ottenere la stringa di connessione per la macchina per il caffè dall'applicazione</span><span class="sxs-lookup"><span data-stu-id="0c224-118">Get connection string for the coffee machine from your application</span></span>
<span data-ttu-id="0c224-119">La stringa di connessione per la machina per il caffè reale viene incorporata nel codice eseguito nel dispositivo.</span><span class="sxs-lookup"><span data-stu-id="0c224-119">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="0c224-120">La stringa di connessione consente alla macchina per il caffè di connettersi in modo sicuro all'applicazione Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="0c224-120">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="0c224-121">Ogni istanza del dispositivo ha una stringa di connessione univoca.</span><span class="sxs-lookup"><span data-stu-id="0c224-121">Every device instance has a unique connection string.</span></span> <span data-ttu-id="0c224-122">Ecco come trovare la stringa di connessione per un'istanza di un dispositivo nell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="0c224-122">Here is how to find the connection string for a device instance in your application:</span></span>

1.  <span data-ttu-id="0c224-123">Nella schermata del dispositivo per Connected Coffee Machine Real scegliere **Connect this device** (Connetti questo dispositivo).</span><span class="sxs-lookup"><span data-stu-id="0c224-123">On the device screen for your Connected Coffee Machine Real, choose **Connect this device**.</span></span>

1.  <span data-ttu-id="0c224-124">Nella pagina **Connetti**, copiare la **stringa di connessione primaria** e salvarla.</span><span class="sxs-lookup"><span data-stu-id="0c224-124">On the **Connect** page, copy the **Primary connection string**, and save it.</span></span> <span data-ttu-id="0c224-125">Questo valore verrà usato nell'applicazione client eseguita nel dispositivo.</span><span class="sxs-lookup"><span data-stu-id="0c224-125">You use this value in the client application that runs on the device.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="0c224-126">Creare un'applicazione Node.js</span><span class="sxs-lookup"><span data-stu-id="0c224-126">Create a Node.js application</span></span>
<span data-ttu-id="0c224-127">I passaggi seguenti illustrano come creare un'applicazione client che implementa la macchina per il caffè aggiunta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0c224-127">The following steps show you how to create a client application that implements the coffee machine you added to the application.</span></span>
1. <span data-ttu-id="0c224-128">Installare [Node.js](https://nodejs.org/) versione 4.0.x o successiva nel computer.</span><span class="sxs-lookup"><span data-stu-id="0c224-128">Install [Node.js](https://nodejs.org/) version 4.0.x or later on your machine.</span></span> <span data-ttu-id="0c224-129">Node.js è disponibile per un'ampia gamma di sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="0c224-129">Node.js is available for a wide variety of operating systems.</span></span>

1. <span data-ttu-id="0c224-130">Nel computer creare una cartella denominata coffee-maker.</span><span class="sxs-lookup"><span data-stu-id="0c224-130">Create a folder called coffee-maker on your machine.</span></span> <span data-ttu-id="0c224-131">Aprire tale cartella nell'ambiente della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="0c224-131">Navigate to that folder in your command-line environment.</span></span>

1. <span data-ttu-id="0c224-132">Per inizializzare il progetto di Node.js, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0c224-132">To initialize your Node.js project, run the following commands:</span></span>
    ```cmd/sh
    npm init
    ```
    > [!NOTE]
    > <span data-ttu-id="0c224-133">Lo script init richiede l'immissione delle proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="0c224-133">The init script prompts you to enter project properties.</span></span> <span data-ttu-id="0c224-134">Per questo esercizio l'uso dei valori predefiniti è sufficiente e consigliato.</span><span class="sxs-lookup"><span data-stu-id="0c224-134">For this exercise, using the default values is sufficient and recommended.</span></span> 
1. <span data-ttu-id="0c224-135">Per installare i pacchetti necessari, eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="0c224-135">To install the necessary packages, run the following command:</span></span>
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="0c224-136">Usando un editor di testo, creare un file denominato coffeeMaker.js nella cartella coffee-maker.</span><span class="sxs-lookup"><span data-stu-id="0c224-136">Using a text editor, create a file called coffeeMaker.js in the coffee-maker folder.</span></span>

1. <span data-ttu-id="0c224-137">Copiare e incollare il codice nel file coffeeMaker.js e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="0c224-137">Copy and paste the code into your coffeeMaker.js file and **Save**.</span></span>

    <span data-ttu-id="0c224-138">Il codice che rappresenta la macchina per il caffè reale in questa unità è scritto in Node.js.</span><span class="sxs-lookup"><span data-stu-id="0c224-138">The code, representing the real coffee machine in this unit, is written in Node.js.</span></span> <span data-ttu-id="0c224-139">Iniziare stabilendo una connessione con l'applicazione, inviare le proprietà iniziali ad Azure IoT Central, sincronizzare le impostazioni, registrare due gestori di comandi per la manutenzione e la preparazione e infine avviare il timer per inviare le informazioni di telemetria ogni secondo.</span><span class="sxs-lookup"><span data-stu-id="0c224-139">You begin by establishing connection with the application, then you send initial properties to Azure IoT Central, synchronize the settings, register two command handlers for Maintenance and Brewing, and finally start the timer for sending the telemetry information every second.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0c224-140">Aggiornare il segnaposto `{your device connection string}`.</span><span class="sxs-lookup"><span data-stu-id="0c224-140">Update the placeholder `{your device connection string}`.</span></span> 


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
        
        // Cup timer - every 20 second randomly decide if the cup is present or not
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
            
            // Setup device command callbacks
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
                
                    // Send device properties once on device start up
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

1.  <span data-ttu-id="0c224-141">Aggiornare il segnaposto {your device connection string} con la stringa di connessione del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="0c224-141">Update the placeholder {your device connection string} with your device connection string.</span></span> <span data-ttu-id="0c224-142">Questo valore è stato copiato dalla pagina dei dettagli della connessione quando è stato aggiunto il dispositivo reale.</span><span class="sxs-lookup"><span data-stu-id="0c224-142">You copied this value from the connection details page when you added your real device.</span></span> 

## <a name="run-your-nodejs-application"></a><span data-ttu-id="0c224-143">Eseguire l'applicazione Node.js</span><span class="sxs-lookup"><span data-stu-id="0c224-143">Run your Node.js application</span></span>
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a><span data-ttu-id="0c224-144">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="0c224-144">Summary</span></span>
<span data-ttu-id="0c224-145">In questa unità la macchina per il caffè è stata connessa ad Azure IoT Central ed è stato avviato l'invio dei dati per il monitoraggio e l'analisi.</span><span class="sxs-lookup"><span data-stu-id="0c224-145">In this unit, you connected your coffee machine to Azure IoT Central and began sending data for monitoring and analysis.</span></span> <span data-ttu-id="0c224-146">La connettività è stata ottenuta acquisendo prima di tutto la stringa di connessione da IoT Central e quindi configurando la stringa nella macchina per il caffè.</span><span class="sxs-lookup"><span data-stu-id="0c224-146">You achieved the connectivity by first acquiring a connection string from IoT Central, followed by configuring the string in the coffee machine.</span></span>
