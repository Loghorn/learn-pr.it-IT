L'acquisizione dei dati meteo è un'importante attività come meteo può influire tutti i dati da modelli di traffico al modo in cui si opera riscaldamento, ventilazione e aria condizionata (HVAC) sistemi nei punti vendita al dettaglio. In questo esercizio, verrà interagisce con il simulatore online Raspberry Pi che apprese nelle unità precedente per i dati meteo di acquisizione simulati e tramite l'IoT Hub di Azure.

Mentre questo esercizio viene effettuato in un ambiente simulato, l'applicazione in esecuzione sul dispositivo simulato può essere trasferito in futuro a un dispositivo reale.

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

## <a name="create-an-iot-hub"></a>Creare un hub IoT
L'hub IoT di Azure fornisce le funzionalità e un modello di estendibilità che consentono agli sviluppatori di dispositivi e sistemi di back-end di creare soluzioni affidabili per la gestione dei dispositivi. I dispositivi spaziano da sensori vincolati e microcontroller a scopo singolo, a potenti gateway che instradano le comunicazioni per gruppi di dispositivi. Inoltre, i casi d'uso e i requisiti per gli operatori IoT variano notevolmente all'interno dei diversi settori. Nonostante questa variabilità, la gestione dei dispositivi con l'hub IoT fornisce le funzionalità, i modelli e le librerie di codice necessari per soddisfare un insieme eterogeneo di dispositivi e utenti finali.

Per iniziare a raccogliere i dati dal simulatore Raspberry Pi, è necessario innanzitutto creare un hub IoT.

1. Aprire il [portale di Azure](https://portal.azure.com?azure-portal=true).
2. Scegliere **Crea una risorsa** nell'angolo superiore sinistro del portale di Azure.
3. Selezionare **Internet delle cose**, quindi selezionare **dell'IoT Hub**.

![Screenshot del passaggio a Hub IoT nel portale di Azure](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. Nel riquadro **Hub IoT** immettere le informazioni seguenti per l'hub IoT:
   
   **Sottoscrizione**: usare la sottoscrizione predefinita per questo esempio.
   **Gruppo di risorse**: creare un gruppo di risorse per ospitare l'hub IoT o usarne uno esistente. Inserendo tutte le risorse correlate in un gruppo, ad esempio *TestResources*, è possibile gestirle tutte insieme. Ad esempio, con l'eliminazione del gruppo di risorse vengono eliminate tutte le risorse contenute in quel gruppo.
   **Area**: selezionare la località più vicina.
   **Nome**: creare un nome univoco per l'hub IoT. Se il nome immesso è disponibile, viene visualizzato un segno di spunta verde.

> [!IMPORTANT]
> L'hub IoT sarà individuabile pubblicamente come endpoint DNS, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.

   ![Finestra delle informazioni di base hub IoT](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

5. Selezionare **Next: Size and scale** (Avanti: Dimensioni e scalabilità) per continuare a creare l'hub IoT.
6. Scegliere un valore per **Piano tariffario e livello di scalabilità**. Per questo esempio, selezionare la **F1 gratuito** livello.

   ![Finestra per specificare dimensioni e scalabilità dell'hub IoT](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

7. Selezionare **Rivedi e crea**.

8. Esaminare le informazioni sull'hub IoT e quindi fare clic su **Crea**. La creazione dell'hub IoT può richiedere alcuni minuti. È possibile monitorare lo stato di avanzamento nel riquadro **Notifiche**.

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up tutorial. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a>Registrare un dispositivo
È necessario registrare un dispositivo con l'hub IoT perché questo possa connettersi.

1. Nel menu di spostamento dell'hub IoT, aprire **Dispositivi IoT**, quindi fare clic su **Aggiungi** per registrare un dispositivo nell'hub IoT.

   ![Aggiungere un dispositivo a Dispositivi IoT nell'hub IoT](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

2. Immettere un **ID dispositivo** per il nuovo dispositivo. Gli ID dispositivo fanno distinzione tra maiuscole e minuscole.

> [!IMPORTANT]
> L'ID dispositivo può essere visibile nei log raccolti per il supporto tecnico e la risoluzione dei problemi, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.

3. Fare clic su **Salva**.
4. Dopo la creazione del dispositivo, aprirlo nel riquadro **Dispositivi IoT**.
5. Copiare il valore di **Stringa di connessione---Chiave primaria** dell'hub IoT per usarlo in seguito.

   ![Ottenere la stringa di connessione del dispositivo](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a>Inviare dati di telemetria simulati

1. Aprire il [simulatore Raspberry Pi IoT Azure](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).
2. Sostituire il segnaposto nella riga 15 con la stringa di connessione del dispositivo hub IoT di Azure che appena copiato.
3. Scegliere il `Run` pulsante o un tipo `npm start` nella finestra della console per eseguire l'applicazione.
   
   ![Sostituire la stringa di connessione](../media-draft/Line15.png)

Si verrà visualizzato l'output seguente che mostra i dati del sensore e i messaggi inviati all'hub IoT

![Output - dati del sensore inviati da Raspberry Pi all'hub IoT](../media-draft/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a>Leggere i dati di telemetria dell'hub
 Quindi, cosa sta succedendo? L'hub IoT riceve i messaggi da dispositivo a cloud inviati dal dispositivo simulato. Per vedere che, verrà ora diamo un'occhiata veloce all'Hub IoT di Azure sta elaborando i dati in ingresso. Nell'IoT Hub, sotto **Monitoring**, selezionare **metriche**. Assegnare alcuni minuti perché in attesa per i dati entrano in immagine.
   
   ![Metriche dell'Hub IoT](../media-draft/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
