L'acquisizione dei dati meteorologici è un'attività importante, in quanto il meteo può influire su numerosi fattori, dai modelli di traffico alla modalità di funzionamento dei sistemi di riscaldamento, ventilazione e condizionamento (HVAC) nei negozi al dettaglio. In questo esercizio si interagirà con il simulatore Raspberry Pi online illustrato nell'unità precedente per acquisire dati meteo simulati tramite l'hub IoT di Azure.

[!include[](../../../includes/azure-sandbox-activate.md)]

Mentre l'esercizio viene eseguito in un ambiente simulato, l'applicazione in esecuzione nel dispositivo simulato potrà essere trasferita a un dispositivo reale in futuro.

## <a name="create-an-iot-hub"></a>Creare un hub IoT
L'hub IoT di Azure fornisce le funzionalità e un modello di estendibilità che consentono agli sviluppatori di dispositivi e back-end di creare soluzioni di gestione affidabili per i dispositivi. I dispositivi spaziano da sensori vincolati e microcontroller a scopo singolo, a potenti gateway che instradano le comunicazioni per gruppi di dispositivi. Inoltre, i casi d'uso e i requisiti per gli operatori IoT variano notevolmente all'interno dei diversi settori. Nonostante questa variabilità, la gestione dei dispositivi con l'hub IoT fornisce le funzionalità, i modelli e le librerie di codice necessari per soddisfare un insieme eterogeneo di dispositivi e utenti finali.

Per iniziare a raccogliere i dati dal simulatore Raspberry Pi, prima è necessario creare un hub IoT.

1. Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

2. Scegliere **Crea una risorsa** nell'angolo superiore sinistro del portale di Azure.

3. Selezionare **Internet delle cose** e quindi **Hub IoT**.

![Screenshot della selezione dell'hub IoT nel portale di Azure](../media/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. Nel riquadro **Hub IoT** immettere le informazioni seguenti per l'hub IoT:
   
   - **Sottoscrizione**: usare la sottoscrizione predefinita per questo esempio.
   - **Gruppo di risorse**: usare il gruppo di risorse esistente. Inserendo tutte le risorse correlate nello stesso gruppo, è possibile gestirle tutte insieme. Ad esempio, con l'eliminazione del gruppo di risorse vengono eliminate tutte le risorse contenute in quel gruppo.
   - **Nome**: creare un nome univoco per l'hub IoT. Se il nome immesso è disponibile, viene visualizzato un segno di spunta verde.
   - **Area**: selezionare la località più vicina a quella in cui ci si trova nell'elenco seguente.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    > [!IMPORTANT]
    > L'hub IoT sarà individuabile pubblicamente come endpoint DNS, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.
    
    ![Finestra delle informazioni di base dell'hub IoT](./../media/dbb7319388673b8ee0e0b407536156c0.png)

1. Selezionare **Avanti: Dimensioni e piano** per continuare a creare l'hub IoT.
2. Scegliere un valore per **Piano tariffario e livello di scalabilità**. Per questo esempio, selezionare il livello **F1 - Gratuito**.

    ![Finestra per specificare dimensioni e il piano dell'hub IoT](../media/b506eb3293fa4aa9d4785ad498fc476c.png)

3. Selezionare **Rivedi e crea**.

4. Esaminare le informazioni sull'hub IoT e quindi fare clic su **Crea**. La creazione dell'hub IoT può richiedere alcuni minuti. È possibile monitorare lo stato di avanzamento nel riquadro **Notifiche**.

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up exercise. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a>Registrare un dispositivo
È necessario registrare un dispositivo con l'hub IoT perché questo possa connettersi.

1. Nel menu di spostamento dell'hub IoT aprire **Dispositivi IoT**, quindi fare clic su **Aggiungi** per registrare un dispositivo nell'hub IoT.

   ![Aggiungere un dispositivo a Dispositivi IoT nell'hub IoT](../media/ee5f177abcf06b86dd007fce3b8448ad.png)

2. Immettere un **ID dispositivo** per il nuovo dispositivo. Gli ID dispositivo fanno distinzione tra maiuscole e minuscole.

    > [!IMPORTANT]
    > L'ID dispositivo può essere visibile nei log raccolti per il supporto tecnico e la risoluzione dei problemi, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.
    
3. Fare clic su **Salva**.
4. Dopo la creazione del dispositivo, aprirlo nel riquadro **Dispositivi IoT**.
5. Copiare il valore di **Stringa di connessione---Chiave primaria** dell'hub IoT per usarlo in seguito.

   ![Ottenere la stringa di connessione del dispositivo](../media/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a>Inviare dati di telemetria simulati

1. Aprire il [simulatore Raspberry Pi di Azure IoT](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).
1. Sostituire il segnaposto nella riga 15 con la stringa di connessione del dispositivo dell'hub IoT di Azure copiata.
1. Fare clic sul pulsante `Run` o digitare `npm start` nella finestra della console per eseguire l'applicazione.
   
    ![Sostituire la stringa di connessione del dispositivo](../media/Line15.png)

1. Dovrebbe essere visibile l'output seguente che mostra i dati del sensore e i messaggi inviati all'hub IoT.

    ![Output - dati del sensore inviati da Raspberry Pi all'hub IoT](../media/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a>Leggere i dati di telemetria dell'hub
Che cosa sta succedendo? L'hub IoT sta ricevendo i messaggi da dispositivo a cloud inviati dal dispositivo simulato. Per verificarlo, esaminiamo rapidamente come l'hub IoT di Azure sta elaborando i dati in ingresso. Nell'hub IoT, in **Monitoraggio** selezionare **Metrica**. La visualizzazione dei dati nell'immagine richiede alcuni minuti.
   
![Metriche di hub IoT](../media/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
