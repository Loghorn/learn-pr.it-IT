Si è lavorato con l'applicazione Azure IoT Central e si è connessa la macchina per il caffè ad Azure IoT Central. È quasi tutto pronto per iniziare a monitorare e gestire la macchina per il caffè remota. In questa unità si consiglia di convalidare il programma di installazione e la connessione usando il modello collegato caffé definita in precedenza. Si aggiorna la temperatura ottima nelle impostazioni, eseguire i comandi per cercare lo stato della macchina e visualizza il machine caffè connessi nel dashboard di. 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>Aggiornare le impostazioni per eseguire la sincronizzazione dell'applicazione con la macchina di caffè

Nella pagina Impostazioni, inviare dati di configurazione per il machine caffè dall'applicazione. 

In questo scenario, modificare la temperatura ottimale e scegliere **Update** (Aggiorna). Quando si modifica un'impostazione, questa viene contrassegnata come in sospeso nell'interfaccia utente finché la macchina per il caffè non riconosce la modifica. 

> [!NOTE]
> Aggiornamenti ha esito positivo nell'impostazione di indicano il flusso di dati e convalidare la connessione. Le misurazioni di dati di telemetria risponderà all'aggiornamento della temperatura ottimale. È possibile osservare la modifica nella pagina Measurements (Misure). 

## <a name="run-commands-on-the-coffee-machine"></a>Eseguire i comandi nel computer caffè 
Passare il **comandi** pagina per l'esercizio seguente. Per convalidare la configurazione di comandi, è in modalità remota eseguire comandi nel computer coffee da IoT Central. Se ha esito positivo, vengono inviati i messaggi di conferma dal computer di caffè.

1. Avviare Brewing in remoto scegliendo **eseguiti**. 
    
    La macchina per il caffè si avvierà se vengono soddisfatte queste tre condizioni:
    - Cup detected
    - Not in maintenance
    - Not brewing already  

    > [!NOTE]
    > Quando la preparazione del caffè inizia, lo stato della macchina passa al giallo come indicato in Measurements > State (Misure > Stato). 
    
    Esaminare i messaggi di conferma nel log di console nel computer caffè. 

    ![Eseguire i comandi](../images/4-commands-brewing.png)

1. Impostare la modalità di manutenzione, scegliere **eseguiti**. Se è il machine caffè imposterà all'utilità di manutenzione *non* già in modalità manutenzione.
    
    Esaminare i messaggi di conferma nel log di console nel computer caffè. 

    > [!NOTE]
    > Come nella vita reale quando il tecnico porta offline la macchina per eseguire riparazioni necessarie prima di passare online, il machine caffè continua a rimanere in modalità manutenzione fino a quando non si riavvia il codice client.

    ![Eseguire i comandi](../images/4-commands-maintenance.png)

1. È consigliabile non eseguire l'applicazione Node. js più di 60 minuti o meno per impedire all'applicazione l'invio di notifiche, messaggi indesiderati. Arresto dell'applicazione quando non si usa l'esercitazione impedisce inoltre si esauriscano la quota giornaliera di messaggi.

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visualizzare la macchina per il caffè nel dashboard
Passare il **Dashboard** pagina in cui è possibile vedere collettivamente le informazioni rilevanti sul machine caffè. Per l'esercizio seguente, attivare **modalità di progettazione** per configurare il dashboard. Ogni volta che si è terminato, scegliere **salvare**.

1. Scegli **grafico a linee** e immettere il titolo come dati di telemetria per visualizzare le misurazioni di dati di telemetria. Scegliere **Past 30 minutes** (Ultimi 30 minuti) come **Time Range** (Intervallo di tempo).

    ![Visualizzi il dashboard](../images/4-dashboard-a.png)

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) e immettere il titolo Device Properties. In **Add/Remove** (Aggiungi/Rimuovi) scegliere Coffee Makers Max Temperature, Coffee Makers Min Temperature, Device Warranty Expired. 

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) e immettere il titolo Optimal Temperature. In **Add/Remove** (Aggiungi/Rimuovi) scegliere Optimal Temperature. 

## <a name="summary"></a>Riepilogo

In questa unità trascorso del tempo per convalidare la connessione tra machine caffè e Azure IoT Central. È ottenuta convalida aggiornando la temperatura ottima, eseguire i comandi. Infine si è configurato il dashboard per monitorare la macchina per il caffè in un'unica posizione definendo le informazioni da visualizzare. Questi passaggi di convalida sono necessari prima di procedere con altre attività nell'unità di Avanti. 