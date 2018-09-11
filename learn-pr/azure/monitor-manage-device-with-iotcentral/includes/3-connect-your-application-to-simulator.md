In uno scenario reale, si presuppone che si connetterà Azure IoT Central a un dispositivo reale o, in questo caso, una macchina per il caffè reale. Ai fini di questo modulo, tuttavia, si connette l'applicazione Azure IoT Central a un'applicazione Node.js generica che rappresenta una macchina per il caffè fisica. In seguito a questa connessione, i dati dei sensori della macchina per il caffè vengono inviati a IoT Central per il monitoraggio e l'analisi.

![Macchina per il caffè](../images/3-coffee-machine.png) 

## <a name="add-the-simulated-coffee-machine-in-iot-central"></a>Aggiungere la macchina per il caffè simulata in IoT Central 
Per aggiungere la macchina per il caffè simulata all'applicazione, usare il modello di dispositivo **Connected Coffee Maker** creato nel modulo precedente.

1. Per aggiungere un nuovo dispositivo in qualità di operatore, scegliere **Device Explorer** nel menu di spostamento a sinistra:

    **Device Explorer** mostra il modello di dispositivo **Connected Coffee Maker** e il simulatore di dispositivo creato automaticamente al momento della creazione del modello di dispositivo da parte del generatore.

1.  Per avviare la connessione della machina per il caffè simulata, scegliere **Nuovo** e quindi **Reale**.

1.  Facoltativamente, è possibile rinominare il nuovo dispositivo scegliendo il nome del dispositivo e modificandone il valore:
 
## <a name="get-connection-string-for-the-coffee-machine-from-your-application"></a>Ottenere la stringa di connessione per la macchina per il caffè dall'applicazione
La stringa di connessione per la machina per il caffè reale viene incorporata nel codice eseguito nel dispositivo. La stringa di connessione consente alla macchina per il caffè di connettersi in modo sicuro all'applicazione Azure IoT Central. Ogni istanza del dispositivo ha una stringa di connessione univoca. Ecco come trovare la stringa di connessione per un'istanza di un dispositivo nell'applicazione:

1.  Nella schermata del dispositivo per la macchina per il caffè reale connessa scegliere **Connect this device** (Connetti questo dispositivo).

1.  Nella pagina **Connetti** copiare il valore di **Stringa di connessione primaria** e salvarlo. Questo valore verrà usato nell'applicazione client eseguita nel dispositivo.

## <a name="create-a-nodejs-application"></a>Creare un'applicazione Node.js
I passaggi seguenti illustrano come creare un'applicazione client che implementa la macchina per il caffè aggiunta all'applicazione.
1. Installare [Node.js](https://nodejs.org/) versione 4.0.x o successiva nella macchina. Node.js è disponibile per un'ampia gamma di sistemi operativi.

1. Nel computer creare una cartella denominata coffee-maker. Passare alla cartella nell'ambiente della riga di comando.

1. Per inizializzare il progetto Node.js, eseguire i comandi seguenti:
    ```cmd/sh
    npm init
    ```

1. Per installare i pacchetti necessari, eseguire il comando seguente:
    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Usando un editor di testo, creare un file denominato coffeeMaker.js nella cartella coffee-maker.

1. Copiare e incollare il codice nel file coffeeMaker.js e fare clic su **Salva**.

    Il codice che rappresenta la macchina per il caffè reale in questo modulo è scritto in Node.js. Iniziare stabilendo una connessione con l'applicazione, inviare le proprietà iniziali ad Azure IoT Central, sincronizzare le impostazioni, registrare due gestori di comandi per la manutenzione e la preparazione e infine avviare il timer per inviare le informazioni di telemetria ogni secondo.

    > [!NOTE]
    > Ricordarsi di aggiornare il segnaposto `{your device connection string}`. 


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

1.  Aggiornare il segnaposto {your device connection string} con la stringa di connessione del dispositivo. Questo valore è stato copiato dalla pagina dei dettagli della connessione quando è stato aggiunto il dispositivo reale. 

## <a name="run-your-nodejs-application"></a>Eseguire l'applicazione Node.js
```cmd/sh
node coffeeMaker.js
```

## <a name="summary"></a>Riepilogo
In questo modulo la macchina per il caffè simulata è stata connessa ad Azure IoT Central ed è stato avviato l'invio dei dati per il monitoraggio e l'analisi. La connettività è stata ottenuta acquisendo prima di tutto la stringa di connessione da IoT Central e quindi configurando la stringa nella macchina per il caffè.