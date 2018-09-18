Si è lavorato con l'applicazione Azure IoT Central e si è connessa la macchina per il caffè ad Azure IoT Central. È quasi tutto pronto per iniziare a monitorare e gestire la macchina per il caffè remota. In questa unità verrà eseguita la convalida della configurazione e della connessione mediante il modello Connected Coffee Maker definito in precedenza. Verrà aggiornata la temperatura ottimale nelle impostazioni, verranno eseguiti comandi per verificare lo stato della macchina e verrà visualizzata la macchina per il caffè connessa nel dashboard. 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>Aggiornare le impostazioni per sincronizzare l'applicazione con la macchina per il caffè

Nella pagina Impostazioni inviare i dati di configurazione alla macchina per il caffè dall'applicazione. 

In questo scenario modificare la temperatura ottimale e scegliere **Update** (Aggiorna). Quando si modifica un'impostazione, questa viene contrassegnata come in sospeso nell'interfaccia utente finché la macchina per il caffè non riconosce la modifica. 

> [!NOTE]
> L'esecuzione corretta di aggiornamenti nell'impostazione indica il flusso di dati e convalida la connessione. Le misure di telemetria risponderanno all'aggiornamento della temperatura ottimale. È possibile osservare la modifica nella pagina Measurements (Misure). 

## <a name="run-commands-on-the-coffee-machine"></a>Eseguire comandi nella macchina per il caffè 
Passare alla pagina **Commands** (Comandi) per l'esercizio seguente. Per convalidare la configurazione dei comandi, eseguire i comandi in modalità remota nella macchina per il caffè da IoT Central. Se l'esito è positivo, dalla macchina per il caffè vengono inviati messaggi di conferma.

1. Iniziare a preparare il caffè in modalità remota scegliendo **Run** (Esegui). 
    
    La macchina per il caffè si avvierà se vengono soddisfatte queste tre condizioni:
    - Cup detected
    - Not in maintenance
    - Not brewing already  

    > [!NOTE]
    > Quando la preparazione del caffè inizia, lo stato della macchina passa al giallo come indicato in Measurements > State (Misure > Stato). 
    
    Cercare i messaggi di conferma nel log della console nella macchina per il caffè. 

    ![Eseguire comandi](../images/4-commands-brewing.png)

1. Impostare la modalità di manutenzione scegliendo **Run** (Esegui). La macchina per il caffè viene impostata sulla modalità di manutenzione se *non* lo è già.
    
    Cercare i messaggi di conferma nel log della console nella macchina per il caffè. 

    > [!NOTE]
    > Come nel mondo reale, quando il tecnico porta offline la macchina per eseguire le riparazioni necessarie prima di riportarla online, la macchina per il caffè resta in modalità di manutenzione finché non si riavvia il codice client.

    ![Eseguire comandi](../images/4-commands-maintenance.png)

1. È consigliabile eseguire l'applicazione Node.js per al massimo 60 minuti circa, per evitare che invii notifiche/messaggi di posta elettronica indesiderati. L'arresto dell'applicazione quando non si lavora all'esercitazione evita anche l'esaurimento della quota giornaliera di messaggi.

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visualizzare la macchina per il caffè nel dashboard
Passare alla pagina **Dashboard**, in cui è possibile visualizzare collettivamente le informazioni rilevanti relative alla macchina per il caffè. Per l'esercizio seguente attivare l'opzione **Design Mode** (Modalità progettazione) per configurare il dashboard. Al termine, scegliere **Save** (Salva).

1. Scegliere **Line Chart** (Grafico a linee) e immettere il titolo Telemetry per visualizzare le misure di telemetria. Scegliere **Past 30 minutes** (Ultimi 30 minuti) come **Time Range** (Intervallo di tempo).

    ![Visualizzazione del dashboard](../images/4-dashboard-a.png)

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) e immettere il titolo Device Properties. In **Add/Remove** (Aggiungi/Rimuovi) scegliere Coffee Makers Max Temperature, Coffee Makers Min Temperature, Device Warranty Expired. 

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) e immettere il titolo Optimal Temperature. In **Add/Remove** (Aggiungi/Rimuovi) scegliere Optimal Temperature. 

## <a name="summary"></a>Riepilogo

In questa unità si è dedicato tempo alla convalida della connessione tra la macchina per il caffè e Azure IoT Central. Si è ottenuta la convalida aggiornando la temperatura ottimale ed eseguendo comandi. Infine si è configurato il dashboard per monitorare la macchina per il caffè in un'unica posizione definendo le informazioni da visualizzare. Questi passaggi di convalida sono necessari prima di procedere con altre attività nell'unità successiva. 