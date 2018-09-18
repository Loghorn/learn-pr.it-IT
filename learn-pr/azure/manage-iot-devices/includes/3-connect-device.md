L'acquisizione dei dati meteorologici è un'attività importante, in quanto il meteo può influire su numerosi fattori, dai modelli di traffico alla modalità di funzionamento dei sistemi HVAC nei negozi al dettaglio. In questo esercizio si userà il simulatore Raspberry Pi online per acquisire dati meteo simulati e acquisire tali dati tramite l'hub IoT di Azure.

Mentre l'esercizio viene eseguito in un ambiente simulato, l'applicazione in esecuzione nel dispositivo simulato potrà essere trasferita a un dispositivo reale in futuro.

## <a name="create-an-iot-hub"></a>Creare un hub IoT

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Selezionare **Crea una risorsa** \> **Internet delle cose**\>**Hub IoT**.

![Screenshot della selezione dell'hub IoT nel portale di Azure](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. Nel riquadro **Hub IoT** immettere le informazioni seguenti per l'hub IoT:

 - **Sottoscrizione**: scegliere la sottoscrizione da usare per creare questo hub IoT.
 - **Gruppo di risorse**: creare un gruppo di risorse per ospitare l'hub IoT o usarne uno esistente. Per altre informazioni, vedere [Usare i gruppi di risorse per gestire le risorse di Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
 - **Area**: selezionare la località più vicina.
 - **Nome**: creare un nome per l'hub IoT. Se il nome immesso è disponibile, viene visualizzato un segno di spunta verde.

> [!IMPORTANT]
> L'hub IoT sarà individuabile pubblicamente come endpoint DNS, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.

   ![Finestra delle informazioni di base dell'hub IoT](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  Selezionare **Avanti: Dimensioni e piano** per continuare a creare l'hub IoT.

1.  Scegliere un valore per **Piano tariffario e livello di scalabilità**. Per questo articolo selezionare il livello **F1 - Gratuito** se ancora disponibile nella sottoscrizione. Per altre informazioni, vedere [Piano tariffario e livello di scalabilità](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Finestra per specificare dimensioni e il piano dell'hub IoT](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  Selezionare **Rivedi e crea**.

1.  Esaminare le informazioni sull'hub IoT e quindi fare clic su **Crea**. La creazione dell'hub IoT può richiedere alcuni minuti. È possibile monitorare lo stato di avanzamento nel riquadro **Notifiche**.

Ora che è stato creato un hub IoT, individuare le informazioni importanti che consentono di connettere dispositivi e applicazioni all'hub IoT.

Nel menu di spostamento dell'hub IoT, aprire **Criteri di accesso condiviso**. Selezionare il criterio **iothubowner**, quindi copiare la **stringa di connessione---chiave primaria** dell'hub IoT. Per altre informazioni, vedere [Controllare l'accesso all'hub IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Per questa esercitazione di configurazione non è necessaria la stringa di connessione iothubowner. Potrebbe essere tuttavia necessaria per alcune delle esercitazioni o per altri scenari IoT, dopo aver completato questa configurazione.

![Ottenere la stringa di connessione dell'hub IoT](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Registrare un dispositivo nell'hub IoT per il dispositivo
------------------------------------------------

1.  Nel menu di spostamento dell'hub IoT, aprire **Dispositivi IoT**, quindi fare clic su **Aggiungi** per registrare un dispositivo nell'hub IoT.

   ![Aggiungere un dispositivo a Dispositivi IoT nell'hub IoT](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  Immettere un **ID dispositivo** per il nuovo dispositivo. Gli ID dispositivo fanno distinzione tra maiuscole e minuscole.

> [!IMPORTANT]
> L'ID dispositivo può essere visibile nei log raccolti per il supporto tecnico e la risoluzione dei problemi, quindi evitare di indicare informazioni riservate nell'assegnazione del nome.

1.  Fare clic su **Salva**.

1.  Dopo la creazione del dispositivo, aprirlo nel riquadro **Dispositivi IoT**.

1.  Copiare il valore di **Stringa di connessione---Chiave primaria** dell'hub IoT per usarlo in seguito.

   ![Ottenere la stringa di connessione del dispositivo](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="run-a-sample-application-on-pi-web-simulator"></a>Eseguire un'applicazione di esempio nel simulatore Web Pi

1. Nell'area di codifica assicurarsi di lavorare nell'applicazione di esempio predefinita. Sostituire il segnaposto nella riga 15 con la stringa di connessione del dispositivo dell'hub IoT di Azure.

    ![Sostituire la stringa di connessione del dispositivo](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  Fare clic su **Esegui** o digitare npm per eseguire l'applicazione.

Verrà visualizzato l'output seguente che mostra i dati del sensore e i messaggi inviati all'hub IoT

   ![Output - dati del sensore inviati da Raspberry Pi all'hub IoT](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
