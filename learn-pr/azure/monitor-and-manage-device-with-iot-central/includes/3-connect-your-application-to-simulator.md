[!include[](../../../includes/azure-sandbox-activate.md)]

In pratica, ad Azure IoT Central sarà connesso un dispositivo fisico, ad esempio una macchina per il caffè abilitata per IoT. In questo caso, si simulerà un dispositivo con un'applicazione Node.jse e lo si connetterà all'applicazione Azure IoT Central. Le misure di telemetria della macchina per il caffè vengono inviate a IoT Central per eseguire il monitoraggio e l'analisi.

![Immagine di una macchina per il caffè.](../media/3-coffee-machine.png) 

## <a name="add-the-coffee-machine-in-iot-central"></a>Aggiungere la macchina per il caffè in IoT Central 
Per aggiungere la macchina per il caffè all'applicazione, usare il modello di dispositivo **Connected Coffee Maker** creato nell'unità precedente.

1. Per aggiungere un nuovo dispositivo, scegliere **Device Explorer** nel menu di spostamento a sinistra.

    Per avviare la connessione della machina per il caffè, scegliere **+ New** (+Nuovo), **Real** (Reale), quindi **Create** (Crea). Al termine verrà visualizzato un elenco dei dispositivi creati con lo stesso modello Connected Coffee Maker.
   
    *   Il modello Connected Coffee Maker viene aggiunto all'elenco dopo aver scelto **+ New** (+Nuovo), **Real** (Reale). 
    *   Il modello Connected Coffee Maker (Simulate) viene creato automaticamente da IoT Central per finalità di test. 

1.  È facoltativamente possibile differenziare la macchina per il caffè appena aggiunta specificando la parola "Real" alla fine del nome. Per rinominare il dispositivo, scegliere il dispositivo e modificare il nome nel campo corrispondente. 

    ![Macchina per il caffè](../media/3-connect-device-a.png) 

    Annotare la posizione di **Connect this device** (Connetti questo dispositivo) per potere connettere la macchina per il caffè nella sezione successiva. Per il momento la schermata visualizza il messaggio "Missing Data" (Dati mancanti) perché la macchina per il caffè non è stata connessa. I dati effettivi di telemetria iniziano a popolare lo schermo quando viene stabilita la connessione. 
 
## <a name="generate-connection-string-for-the-coffee-machine-from-your-application"></a>Generare la stringa di connessione per la macchina per il caffè dall'applicazione
La stringa di connessione per la machina per il caffè reale viene incorporata nel codice eseguito nel dispositivo. La stringa di connessione consente alla macchina per il caffè di connettersi in modo sicuro all'applicazione Azure IoT Central. Ogni istanza del dispositivo ha una stringa di connessione univoca. Nei passaggi successivi la stringa di connessione viene generata durante la creazione dell'applicazione Node.js.

## <a name="create-a-nodejs-application"></a>Creare un'applicazione Node.js
I passaggi seguenti illustrano come creare un'applicazione client che implementa la macchina per il caffè aggiunta all'applicazione.

> [!TIP]
> In questo esercizio si creerà l'app in Azure Cloud Shell in modo che non sia necessario installare altro nel computer locale. 

1. Eseguire il comando seguente in Azure Cloud Shell per creare una cartella `coffee-maker` e accedervi:

    ```azurecli
    mkdir ~/coffee-maker
    cd ~/coffee-maker
    ```

1. Dalla cartella `coffee-maker` in Cloud Shell, eseguire il comando seguente per installare il generatore di chiavi del servizio DPS: 

   ```azurecli
    npm install dps-keygen
   ```
    Questo comando installa il pacchetto dps-keygen nella cartella `coffee-maker` locale. È stata tralasciata l'opzione `-g` perché non si hanno le autorizzazioni per installare un pacchetto globale.

    <!-- TODO: Add more information about the DPS key generator and what it's used for -->
 
1. Eseguire il comando seguente in Cloud Shell per scaricare l'utilità della stringa di connessione DPS da GitHub: 

    ```azurecli
    wget https://github.com/Azure/dps-keygen/blob/master/bin/linux/dps_cstr?raw=true -O dps_cstr 
    ```

    > [!NOTE]
    > È stata scaricata la versione Linux di **dps_cstr** perché viene eseguita in Cloud Shell.

1. Eseguire il comando seguente per concedere le autorizzazioni di esecuzione `dps_cstr`:

    ```azurecli
    chmod +x dps_cstr
    ```

    Per generare una stringa di connessione per il dispositivo, sono necessarie tre informazioni che si ottengono dal portale di Azure IoT Central:
    - **ID Ambito**
    - **ID dispositivo**
    - **Chiave primaria**

1. Tornare al portale IoT Central. Nella schermata del dispositivo *Connected Coffee Maker - Real* selezionare **Connect** (Connetti) nella parte superiore destra della schermata.

1. Nella finestra di dialogo Device Connection (Connessione dispositivo) visualizzata salvare i valori **Scope ID** (ID ambito), **Device ID** (ID dispositivo) e **Primary Key** (Chiave primaria) che saranno usati più avanti in questo esercizio. 

1. Eseguire il comando seguente in Cloud Shell, sostituendo **< scope_id >**, **< device_id >** e **< primary_key >** con i valori salvati nell'ultimo passaggio. 

   ```azurecli
   ./dps_cstr [scope_id] [device_id] [primary_key] > connection.txt
   ```
  
    Questo comando genera una stringa di connessione in base ai valori immessi e li scrive in un file denominato **connection.txt**.

    > [!IMPORTANT]
    > Il comando `dps_cstr` non è presente nel percorso della shell. Assicurarsi quindi di chiamarlo con `./dps_cstr`

1. Per aprire l'editor di codice integrato in Cloud Shell, eseguire il comando seguente: 

    ```azurecli
    code
    ```
1. Selezionare **connection.txt** dall'elenco di file nel menu **FILE** dell'editor.

1. Verificare che il file **connection.txt** contenga una stringa di connessione che inizia con ``HostName=``.

1. Chiudere l'editor selezionando **Chiudi editor** dal menu (*...* ) nella parte superiore destra dell'editor. 

1. Eseguire il comando seguente in Cloud Shell per inizializzare un progetto Node.js nella cartella `coffee-maker`:

    ```azurecli
    npm init
    ```
    > [!NOTE]
    > Lo script init richiede l'immissione delle proprietà del progetto. Per questo esercizio, premere INVIO per usare tutti i valori predefiniti.

1. Per installare i pacchetti necessari, eseguire questo comando nella cartella `coffee-maker`:

    ```azurecli
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Eseguire questo comando per creare un nuovo file in Cloud Shell:

    ```azurecli
    touch coffeeMaker.js
    ```
1. Per aprire l'editor di codice integrato, eseguire il comando seguente dalla riga di comando in Cloud Shell: 

     ```azurecli
    code
    ```
    
1. All'apertura dell'editor di codice, selezionare il pulsante di aggiornamento nell'elenco **FILE** e selezionare il nuovo file **coffeeMaker.js**. 

1. Copiare e incollare il codice seguente nella finestra vuota dell'editor:

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

    La macchina per il caffè è scritta in Node.js. Prima si connette ad Azure IoT Central, poi l'app invia le proprietà iniziali ad Azure IoT Central, sincronizza le impostazioni, registra due gestori di comandi per la manutenzione e la preparazione del caffè e infine avvia il timer per inviare le informazioni di telemetria ogni secondo.

1.  Aggiornare il segnaposto `{your device connection string}` all'inizio del codice con la stringa di connessione creata in precedenza e salvata in **connection.txt**. La stringa di connessione inizia con `HostName=`.

1. Selezionare i tre puntini `...` nella parte superiore destra dell'editor per espandere il menu dell'editor. Quindi selezionare **Salva** per salvare le modifiche apportate a "coffeeMaker.js".

1. Eseguire il comando seguente nella finestra di Cloud Shell per avviare l'app:

    ```azurecli
    node coffeeMaker.js
    ```
1. Verificare che l'app venga avviata nella finestra di Cloud Shell e che vengano visualizzati i messaggi *Device successfully connected to Azure IoT Central* (Dispositivo connesso ad Azure IoT Central) e *Telemetry send:* (Invio dati di telemetria). La procedura è stata completata. L'app è attiva e in esecuzione e comunica con IoT Central.