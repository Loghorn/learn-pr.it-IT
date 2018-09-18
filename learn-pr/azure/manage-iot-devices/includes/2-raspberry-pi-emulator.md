Recentemente, le schede Raspberry Pi hanno sollevato un notevole interesse per testare teorie o la fattibilità di dispositivi all'avanguardia. Sebbene il costo di queste schede sia relativamente basso, alcuni preferiscono testare le funzionalità della scheda Raspberry Pi prima di acquistarne una.

Microsoft ha creato un simulatore di Raspberry Pi online che permette agli utenti di controllare l'hardware emulato tramite il codice. L'emulatore è rappresentato dall'immagine grafica di una scheda Raspberry Pi connessa a un sensore di temperatura, pressione e umidità e da un LED rosso tramite una breadboard che consente ai circuiti di essere collegati insieme. Il riquadro laterale visualizzato permette agli utenti di immettere codice Javascript in Node.js per controllare il LED e raccogliere dati fittizi dal sensore simulato.

Alla prima esecuzione, il simulatore esegue un programma di acquisizione della temperatura di esempio che è visualizzato tramite la riga di comando. La stessa applicazione di esempio può anche essere eseguita su una scheda Pi reale, in quanto il simulatore è progettato per consentire il test del codice prima del trasferimento in un dispositivo reale.

## <a name="web-simulator"></a>Simulatore Web

Esistono tre aree nel simulatore Web:

1.  Area dell'assembly: nel circuito predefinito la scheda Pi si connette a un sensore BME280 e a un LED. L'area è bloccata nella versione di anteprima, pertanto non è attualmente possibile eseguire la personalizzazione.

2.  Area della codifica: un editor di codice online da usare con Raspberry Pi. L'applicazione di esempio predefinita consente di raccogliere i dati del sensore BME280 e di inviarli all'hub IoT di Azure. L'applicazione è completamente compatibile con dispositivi Pi effettivi.

3.  Finestra della console integrata: mostra l'output del codice. Nella parte superiore di questa finestra sono disponibili tre pulsanti.

    -   **Esegui**: eseguire l'applicazione nell'area di codifica.

    -   **Reimposta**: reimposta l'area di codifica sull'applicazione di esempio predefinita.

    -   **Fold/Expand** (Comprimi/Espandi): sul lato destro è disponibile un pulsante per comprimere/espandere la finestra della console.

[Immagine della panoramica del simulatore Pi online](./../media-draft/image1.png)

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>

-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->

