Finora si è connessa la macchina per il caffè all'applicazione Azure IoT Central, abilitando lo scambio di dati che consente di monitorare e gestire la macchina per il caffè. In questa unità vengono create regole che attivano azioni quando la temperatura dell'acqua della macchina per il caffè non rientra nell'intervallo normale. 

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>Creare regole in IoT Central con invio di messaggio di posta elettronica come azione

Azure IoT Central include funzionalità native di posta elettronica per l'invio di notifiche. In questo scenario se la macchina per il caffè non rientra nell'intervallo di temperatura ottimale e non è coperta dalla garanzia, IoT Central invia un messaggio di posta elettronica al reparto di manutenzione del cliente.

1. Passare alla pagina **Regole** per gli esercizi di questa unità e avviare la modalità di modifica selezionando **Modifica modello** a destra. 
1. Selezionare **+ Nuova regola** e quindi **Telemetria**. 

1. Immettere il nome `Coffee Maker Water Too Cold (Expired)`

1. Aggiungere le seguenti condizioni alla regola selezionando il segno più (**+**) a destra di **Condizioni**, quindi fare clic su **Salva**:      
    - Water Temperature is less than Coffee Maker's Min Temperature
    - Device Warranty Expired equals 1

    ![Uso della regola](../media/5-flow-a.png)

1. Scorrere verso il basso fino al pannello **Configure Telemetry Rule** (Configura regola di telemetria) e scegliere l'icona **+** accanto ad **Azioni** e quindi scegliere **Posta elettronica**.

1. Immettere l'indirizzo di posta elettronica usato per accedere all'applicazione IoT Central e aggiungere la nota `Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.`

1. Scegliere **Salva**. La regola viene elencata nella pagina **Regole**.

Questi passaggi si possono ripetere nel caso in cui l'acqua sia troppo calda. 

1. Selezionare **+ Nuova regola** e quindi **Telemetria**.

1. Aggiungere una nuova regola e assegnarle il nome `Coffee Maker Water Too Hot (Expired)`

1. Aggiungere le seguenti condizioni alla regola selezionando il segno più (**+**) a destra di **Condizioni**, quindi fare clic su **Salva**:      
    - Water Temperature is greater than Coffee Makers Max Temperature
    - Device Warranty Expired equals 1

1. Scorrere verso il basso fino al pannello **Configure Telemetry Rule** (Configura regola di telemetria) e scegliere l'icona **+** accanto ad **Azioni** e quindi scegliere **Posta elettronica**.

1. Immettere l'indirizzo di posta elettronica usato per accedere all'applicazione IoT Central e aggiungere la nota `Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.`

1. Scegliere **Salva**. La regola viene elencata nella pagina **Regole**.

Per attivare la regola, configurare la temperatura ottimale in **Impostazioni** come esterna all'intervallo specificato in **Proprietà**. Al termine della convalida, disattivare le regole per evitare di ricevere troppi messaggi nella posta in arrivo.