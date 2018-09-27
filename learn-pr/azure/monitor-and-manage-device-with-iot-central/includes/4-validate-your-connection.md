È stata usata l'applicazione Azure IoT Central e la macchina per il caffè è stata connessa ad Azure IoT Central. È quasi tutto pronto per iniziare a monitorare e gestire la macchina per il caffè remota. In questa unità verrà eseguita la convalida della configurazione e della connessione mediante il modello Connected Coffee Maker definito in precedenza. Verrà aggiornata la temperatura ottimale nelle impostazioni, verranno eseguiti comandi per verificare lo stato della macchina e verrà visualizzata la macchina per il caffè connessa nel dashboard. 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>Aggiornare le impostazioni per sincronizzare l'applicazione con la macchina per il caffè

Nella pagina **Settings** (Impostazioni) inviare i dati di configurazione alla macchina per il caffè dall'applicazione. 

In questo scenario modificare la temperatura ottimale e scegliere **Update** (Aggiorna). Quando si modifica un'impostazione, questa viene contrassegnata come in sospeso nell'interfaccia utente finché la macchina per il caffè non riconosce la modifica. 

> [!NOTE]
> L'esecuzione corretta degli aggiornamenti nell'impostazione indica il flusso di dati e convalida la connessione. Le misure di telemetria risponderanno all'aggiornamento della temperatura ottimale. È possibile osservare la modifica nella pagina **Measurements** (Misure). 

## <a name="run-commands-on-the-coffee-machine"></a>Eseguire i comandi nella macchina per il caffè 
Passare alla pagina **Commands** (Comandi) per l'esercizio seguente. Per convalidare la configurazione dei comandi, eseguire i comandi in remoto nella macchina per il caffè da IoT Central. Se l'esito è positivo, dalla macchina per il caffè vengono inviati messaggi di conferma.

1. Iniziare la preparazione del caffè in remoto scegliendo **Run** (Esegui). 
    
    La macchina per il caffè si avvierà se vengono soddisfatte le tre condizioni seguenti:
    - Cup detected
    - Not in maintenance
    - Not brewing already  

    > [!NOTE]
    > Quando inizia la preparazione del caffè, lo stato della macchina passa al giallo come indicato in **Measurements** > **State** (Misure > Stato). 
    
    Cercare i messaggi di conferma nel log della console nella macchina per il caffè simulata (applicazione Node.js). 

    ![Eseguire i comandi](../media/4-commands-brewing.png)

1. Impostare la modalità di manutenzione scegliendo **Run** (Esegui). La macchina per il caffè sarà impostata sulla modalità di manutenzione se *non* lo è già.
    
    Cercare i messaggi di conferma nel log della console nella macchina per il caffè. 

    > [!NOTE]
    > Come nel mondo reale, quando il tecnico mette offline la macchina per eseguire le riparazioni necessarie prima di riportarla online, la macchina per il caffè resta in modalità di manutenzione finché non si riavvia il codice client.

    ![Eseguire i comandi](../media/4-commands-maintenance.png)

> [!IMPORTANT]
> È consigliabile eseguire l'applicazione Node.js per al massimo 60 minuti circa, per evitare che invii notifiche/messaggi di posta elettronica indesiderati. Quando non si lavora al modulo, è consigliabile arrestare l'applicazione per evitare l'esaurimento della quota giornaliera di messaggi.

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visualizzare la macchina per il caffè nel dashboard

1. Passare alla scheda**Dashboard**.

1. Selezionare **Edit Template** (Modifica modello) per modificare il dashboard.

1. Scegliere **Line Chart** (Grafico a linee) dal menu laterale.

1. In **Configure Chart** (Configura grafico) immettere `Telemetry` nel campo **Title** (Titolo). I dati di telemetria saranno visualizzati con questo grafico. 

1. Selezionare **Past 30 minutes** (Ultimi 30 minuti) come **Time Range** (Intervallo di tempo). 

1. Nell'area **Measures** (Misure) selezionare l'icona di visibilità a destra di ogni misura per rendere visibile la misura nel grafico. 

1. Selezionare **Save** (Salva) per salvare la configurazione e visualizzare il grafico a linee. 

    ![Visualizzazione del dashboard](../media/4-dashboard-a.png)

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) dal menu a sinistra per aprire il pannello **Configure Device Details** (Configura dettagli dispositivo). 

1. Immettere `Device properties` nel campo **Title** (Titolo).

1. In **Add/Remove** (Aggiungi/Rimuovi) scegliere **Coffee Makers Max Temperature**, **Coffee Makers Min Temperature**, **Device Warranty Expired**, quindi selezionare **OK** per completare la selezione.

1. Selezionare **Save** (Salva) per creare un nuovo pannello *Device properties* (Proprietà dispositivo) nel dashboard. 

1. Scegliere di nuovo **Settings and Properties** (Impostazioni e proprietà) e immettere `Optimal Temperature` come titolo. In **Add/Remove** (Aggiungi/Rimuovi) scegliere **Optimal Temperature**.

1. Selezionare **Save** (Salva) per salvare il lavoro e visualizzare un nuovo riquadro nel dashboard. 

1. Selezionare **Done** (Fine) per uscire dalla modalità di modifica e visualizzare il nuovo dashboard. 