Lavagne di raspberry Pi sono ottenute molto interesse di ritardo per teorie o eventi interessanti che effettua il test. Mentre il costo questo lavagne sono abbastanza basso, alcuni possono risultare utili durante i test la funzionalità di Raspberry Pi prima di investire in uno.

Microsoft ha creato una linea [il simulatore Raspberry Pi Azure IoT](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) consentendo agli utenti di controllare l'hardware emulato tramite codice. L'emulatore è rappresentato da un'immagine di un dispositivo Raspberry Pi connessi a una temperatura, umidità, sensore di pressione e un LED rosso tramite basetta sperimentale che consente di circuiti per essere collegati. Il pannello visualizzato sul lato consente agli utenti di immettere il codice JavaScript di Node. js per controllare il LED e raccogliere dati fittizi dai sensori simulati.

![Simulatore raspberry Pi](../media-draft/RaspberryPiSimulator.png)

## <a name="raspberry-pi-azure-iot-online-simulator"></a>Simulatore Online Azure IoT raspberry Pi

Alla prima esecuzione, il simulatore viene eseguito un programma di acquisizione della temperatura di esempio che viene visualizzato tramite la riga di comando. La stessa applicazione di esempio anche possibile eseguibili in un reale Pi poiché l'applicazione client simulator è progettato per consentire agli utenti di testare il codice prima che venga trasferito a un dispositivo reale.

Esistono tre aree nel simulatore web:

1. **Area dell'assembly**. Si tratta di cui è possibile visualizzare lo stato del dispositivo. Per impostazione predefinita, si tratta di un connessione con un sensore BME280 e un LED light di Pi. Questa configurazione non è personalizzabile in questo momento.
2. **Area di codifica**. Un editor di codice online per poter rendere un'app su Raspberry Pi con Node. js. L'applicazione di esempio predefinita consente di raccogliere i dati dei sensori dal sensore BME280 e lo invia all'hub IoT di Azure.
3. **Finestra della console integrata**. Si tratta in cui è possibile visualizzare l'output dell'app. All'interno della console sono disponibili tre funzioni:
    - `run` : Esegue il codice di esempio (durante l'esecuzione, esempio di codice è di sola lettura).
    - `Stop` -Interrompe il codice di esempio in esecuzione.
    - `Reset` -Viene reimpostato il codice.

Ora che disponibile una panoramica del simulatore Raspberry Pi, esploreremo l'IoT Hub in Azure in cui si creerà una nuova risorsa per acquisire i dati dal simulatore.

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>
-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->