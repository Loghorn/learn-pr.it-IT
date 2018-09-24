Recentemente, le schede Raspberry Pi hanno sollevato un notevole interesse per testare teorie o la fattibilità di dispositivi all'avanguardia. Sebbene il costo di queste schede sia relativamente basso, alcuni preferiscono testare le funzionalità della scheda Raspberry Pi prima di acquistarne una.

Microsoft ha creato un [simulatore Raspberry Pi di Azure IoT](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) online che permette agli utenti di controllare l'hardware emulato tramite il codice. L'emulatore è rappresentato dall'immagine grafica di una scheda Raspberry Pi connessa a un sensore di temperatura, pressione e umidità e da un LED rosso tramite una breadboard che consente ai circuiti di essere collegati insieme. Il pannello laterale visualizzato permette agli utenti di immettere codice JavaScript in Node.js per controllare il LED e raccogliere dati fittizi dal sensore simulato.

![Simulatore Raspberry Pi](../media/RaspberryPiSimulator.png)

## <a name="raspberry-pi-azure-iot-online-simulator"></a>Simulatore online Raspberry Pi di Azure IoT

Alla prima esecuzione, il simulatore esegue un programma di acquisizione della temperatura di esempio che è visualizzato tramite la riga di comando. La stessa applicazione di esempio può anche essere eseguita su una scheda Pi reale, in quanto il simulatore è progettato per consentire il test del codice prima del trasferimento in un dispositivo reale.

Esistono tre aree nel simulatore Web:

1. **Area dell'assembly**. In quest'area è possibile visualizzare lo stato del dispositivo. Per impostazione predefinita, si tratta di un dispositivo Pi che si connette con un sensore BME280 e una luce LED. Questa configurazione non è attualmente personalizzabile.
2. **Area della codifica**. Editor di codice online per creare un'app in Raspberry Pi con Node.js. L'applicazione di esempio predefinita consente di raccogliere i dati del sensore BME280 e di inviarli all'hub IoT di Azure.
3. **Finestra della console integrata**. In questa finestra è possibile visualizzare l'output dell'app. Nella console sono disponibili tre funzioni:
    - `run`: esegue il codice di esempio. Quando è in esecuzione l'esempio, il codice è di sola lettura.
    - `Stop`: arresta l'esecuzione del codice di esempio.
    - `Reset`: reimposta il codice.

Dopo aver visto una panoramica del simulatore Raspberry Pi, si esaminerà l'hub IoT in Azure in cui si creerà una nuova risorsa per acquisire i dati dal simulatore.

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>
-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->