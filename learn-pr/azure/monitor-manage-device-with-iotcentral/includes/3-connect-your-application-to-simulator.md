<span data-ttu-id="91d70-101">In uno scenario reale, si presuppone che si connetterà Azure IoT Central a un dispositivo reale o, in questo caso, una macchina per il caffè reale.</span><span class="sxs-lookup"><span data-stu-id="91d70-101">In real life, it's assumed that you will connect Azure IoT Central to a real device or in this case, a real coffee machine.</span></span> <span data-ttu-id="91d70-102">Ai fini di questo modulo, tuttavia, si connette l'applicazione Azure IoT Central a un'applicazione Node.js generica che rappresenta una macchina per il caffè fisica.</span><span class="sxs-lookup"><span data-stu-id="91d70-102">But for the purpose of this module, you connect the Azure IoT Central application to a generic Node.js application representing a physical coffee machine.</span></span> <span data-ttu-id="91d70-103">In seguito a questa connessione, i dati dei sensori della macchina per il caffè vengono inviati a IoT Central per il monitoraggio e l'analisi.</span><span class="sxs-lookup"><span data-stu-id="91d70-103">As a result of this connection, sensory data from the coffee machine is sent to IoT Central for monitoring and analysis.</span></span>

![Macchina per il caffè](../images/3-coffee-machine.png) 

## <a name="add-the-simulated-coffee-machine-in-iot-central"></a><span data-ttu-id="91d70-105">Aggiungere la macchina per il caffè simulata in IoT Central</span><span class="sxs-lookup"><span data-stu-id="91d70-105">Add the simulated coffee machine in IoT Central</span></span> 
<span data-ttu-id="91d70-106">Per aggiungere la macchina per il caffè simulata all'applicazione, usare il modello di dispositivo **Connected Coffee Maker** creato nel modulo precedente.</span><span class="sxs-lookup"><span data-stu-id="91d70-106">To add your simulated coffee machine to your application, you use the **Connected Coffee Maker** device template you created in the previous module.</span></span>

1. <span data-ttu-id="91d70-107">Per aggiungere un nuovo dispositivo in qualità di operatore, scegliere **Device Explorer** nel menu di spostamento a sinistra:</span><span class="sxs-lookup"><span data-stu-id="91d70-107">To add a new device as an operator choose **Device Explorer** in the left navigation menu:</span></span>

    <span data-ttu-id="91d70-108">**Device Explorer** mostra il modello di dispositivo **Connected Coffee Maker** e il simulatore di dispositivo creato automaticamente al momento della creazione del modello di dispositivo da parte del generatore.</span><span class="sxs-lookup"><span data-stu-id="91d70-108">The **Device Explorer** shows the **Connected Coffee Maker** device template and the device simulator that was automatically created when the builder created the device template.</span></span>

1.  <span data-ttu-id="91d70-109">Per avviare la connessione della machina per il caffè simulata, scegliere **Nuovo** e quindi **Reale**.</span><span class="sxs-lookup"><span data-stu-id="91d70-109">To start connecting your simulated coffee machine, choose **New**, then **Real**.</span></span>

1.  <span data-ttu-id="91d70-110">Facoltativamente, è possibile rinominare il nuovo dispositivo scegliendo il nome del dispositivo e modificandone il valore:</span><span class="sxs-lookup"><span data-stu-id="91d70-110">Optionally, you can rename your new device by choosing the device name and editing the value:</span></span>
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a><span data-ttu-id="91d70-111">Ottenere la stringa di connessione per la macchina per il caffè dall'applicazione</span><span class="sxs-lookup"><span data-stu-id="91d70-111">Get connection string for the coffee machine from your application</span></span>
<span data-ttu-id="91d70-112">La stringa di connessione per la machina per il caffè reale viene incorporata nel codice eseguito nel dispositivo.</span><span class="sxs-lookup"><span data-stu-id="91d70-112">You embed the connection string for your real coffee machine in the code that runs on the device.</span></span> <span data-ttu-id="91d70-113">La stringa di connessione consente alla macchina per il caffè di connettersi in modo sicuro all'applicazione Azure IoT Central.</span><span class="sxs-lookup"><span data-stu-id="91d70-113">The connection string enables the coffee machine to connect securely to your Azure IoT Central application.</span></span> <span data-ttu-id="91d70-114">Ogni istanza del dispositivo ha una stringa di connessione univoca.</span><span class="sxs-lookup"><span data-stu-id="91d70-114">Every device instance has a unique connection string.</span></span> <span data-ttu-id="91d70-115">Ecco come trovare la stringa di connessione per un'istanza di un dispositivo nell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="91d70-115">Here is how to find the connection string for a device instance in your application:</span></span>

1.  <span data-ttu-id="91d70-116">Nella schermata del dispositivo per la macchina per il caffè reale connessa scegliere **Connect this device** (Connetti questo dispositivo).</span><span class="sxs-lookup"><span data-stu-id="91d70-116">On the Device screen for your real connected coffee machine, choose **Connect this device**.</span></span>

1.  <span data-ttu-id="91d70-117">Nella pagina **Connetti** copiare il valore di **Stringa di connessione primaria** e salvarlo.</span><span class="sxs-lookup"><span data-stu-id="91d70-117">On the **Connect** page, copy the **Primary connection string**, and save it.</span></span> <span data-ttu-id="91d70-118">Questo valore verrà usato nell'applicazione client eseguita nel dispositivo.</span><span class="sxs-lookup"><span data-stu-id="91d70-118">You use this value in the client application that runs on the device.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="91d70-119">Creare un'applicazione Node.js</span><span class="sxs-lookup"><span data-stu-id="91d70-119">Create a Node.js application</span></span>
<span data-ttu-id="91d70-120">I passaggi seguenti illustrano come creare un'applicazione client che implementa la macchina per il caffè aggiunta all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="91d70-120">The following steps show how to create a client application that implements the coffee machine you added to the application.</span></span>
1. <span data-ttu-id="91d70-121">Installare [Node.js](https://nodejs.org/) versione 4.0.x o successiva nella macchina.</span><span class="sxs-lookup"><span data-stu-id="91d70-121">Install [Node.js](https://nodejs.org/) version 4.0.x or later in your machine.</span></span> <span data-ttu-id="91d70-122">Node.js è disponibile per un'ampia gamma di sistemi operativi.</span><span class="sxs-lookup"><span data-stu-id="91d70-122">Node.js is available for a wide variety of operating systems.</span></span>

1. <span data-ttu-id="91d70-123">Nel computer creare una cartella denominata coffee-maker.</span><span class="sxs-lookup"><span data-stu-id="91d70-123">Create a folder called coffee-maker on your machine.</span></span> <span data-ttu-id="91d70-124">Passare alla cartella nell'ambiente della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="91d70-124">Navigate to that folder in your command-line environment.</span></span>

1. <span data-ttu-id="91d70-125">Per inizializzare il progetto Node.js, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="91d70-125">To initialize your Node.js project, run the following commands:</span></span>
    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="91d70-126">Per installare i pacchetti necessari, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="91d70-126">To install the necessary packages, run the following command:</span></span>
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="91d70-127">Usando un editor di testo, creare un file denominato coffeeMaker.js nella cartella coffee-maker.</span><span class="sxs-lookup"><span data-stu-id="91d70-127">Using a text editor, create a file called coffeeMaker.js in the coffee-maker folder.</span></span>

1. <span data-ttu-id="91d70-128">Copiare e incollare il codice nel file coffeeMaker.js e fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="91d70-128">Copy and paste the code into your coffeeMaker.js file and **Save**.</span></span>

    <span data-ttu-id="91d70-129">Il codice che rappresenta la macchina per il caffè reale in questo modulo è scritto in Node.js.</span><span class="sxs-lookup"><span data-stu-id="91d70-129">The code representing the real coffee machine in this module is written in Node.js.</span></span> <span data-ttu-id="91d70-130">Iniziare stabilendo una connessione con l'applicazione, inviare le proprietà iniziali ad Azure IoT Central, sincronizzare le impostazioni, registrare due gestori di comandi per la manutenzione e la preparazione e infine avviare il timer per inviare le informazioni di telemetria ogni secondo.</span><span class="sxs-lookup"><span data-stu-id="91d70-130">You begin by establishing connection with the application, send initial properties to Azure IoT Central, synchronize the settings, register two command handlers for Maintenance and Brewing, and finally start the timer for sending the telemetry information every second.</span></span>

    > [!NOTE]
    > <span data-ttu-id="91d70-131">Ricordarsi di aggiornare il segnaposto `{your device connection string}`.</span><span class="sxs-lookup"><span data-stu-id="91d70-131">Remeber to update the placeholder `{your device connection string}`.</span></span> 


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
    var deviceMinTemperature = 88 + (Math.random() * 4);
    var deviceMaxTemperature = 98 + (Math.random() * 2);
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

1.  <span data-ttu-id="91d70-132">Aggiornare il segnaposto {your device connection string} con la stringa di connessione del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="91d70-132">Update the placeholder {your device connection string} with your device connection string.</span></span> <span data-ttu-id="91d70-133">Questo valore è stato copiato dalla pagina dei dettagli della connessione quando è stato aggiunto il dispositivo reale.</span><span class="sxs-lookup"><span data-stu-id="91d70-133">You copied this value from the connection details page when you added your real device.</span></span> 

## <a name="run-your-nodejs-application"></a><span data-ttu-id="91d70-134">Eseguire l'applicazione Node.js</span><span class="sxs-lookup"><span data-stu-id="91d70-134">Run your Node.js application</span></span>
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a><span data-ttu-id="91d70-135">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="91d70-135">Summary</span></span>
<span data-ttu-id="91d70-136">In questo modulo la macchina per il caffè simulata è stata connessa ad Azure IoT Central ed è stato avviato l'invio dei dati per il monitoraggio e l'analisi.</span><span class="sxs-lookup"><span data-stu-id="91d70-136">In this module, you connected your simulated coffee machine to Azure IoT Central and began sending data for monitoring and analysis.</span></span> <span data-ttu-id="91d70-137">La connettività è stata ottenuta acquisendo prima di tutto la stringa di connessione da IoT Central e quindi configurando la stringa nella macchina per il caffè.</span><span class="sxs-lookup"><span data-stu-id="91d70-137">You achieved the connectivity by first acquiring a connection string from IoT Central, followed by configuring the string in the coffee machine.</span></span>
