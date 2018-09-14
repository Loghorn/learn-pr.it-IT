Si è lavorato con l'applicazione Azure IoT Central e si è connessa la macchina per il caffè ad Azure IoT Central. È quasi tutto pronto per iniziare a monitorare e gestire la macchina per il caffè remota. In questo modulo si convalidano la configurazione e la connessione aggiornando la temperatura ottimale nelle impostazioni, eseguendo comandi per verificare lo stato della macchina e visualizzando la macchina per il caffè connessa nel dashboard. 

## <a name="edit-setting-to-see-if-the-application-syncs-with-the-coffee-machine"></a>Modificare l'impostazione per verificare se l'applicazione si sincronizza con la macchina per il caffè

Inviare i dati di configurazione alla macchina per il caffè dall'applicazione nella sezione delle impostazioni. In questo scenario, modificare la temperatura ottimale e scegliere **Update** (Aggiorna). Quando si modifica un'impostazione, questa viene contrassegnata come in sospeso nell'interfaccia utente finché la macchina per il caffè non riconosce la modifica. L'esecuzione corretta di aggiornamenti nell'impostazione indica il flusso di dati e convalida la connessione. Le misure di telemetria risponderanno all'aggiornamento della temperatura ottimale. È possibile osservare la modifica nella pagina Measurements (Misure). 

  ![Aggiornamento delle impostazioni](../images/3-settings-a.png)

## <a name="run-commands-and-receive-confirmation-messages-sent-by-the-coffee-machine"></a>Eseguire comandi e ricevere messaggi di conferma inviati dalla macchina per il caffè 
Attivare **Design Mode** (Modalità progettazione) per aggiungere **New Commands** (Nuovi comandi).

1. Avviare la preparazione del caffè da remoto. La macchina per il caffè si avvierà se vengono soddisfatte queste tre condizioni:
    - Cup detected
    - Not in maintenance
    - Not brewing already

    Quando la preparazione del caffè inizia, lo stato della macchina passa al giallo come indicato in Measurements > State (Misure > Stato). 
    ![Eseguire comandi](../images/3-commands-b.png)

    Cercare i messaggi di conferma nel log della console nella macchina per il caffè simulata. 
    ![Eseguire comandi](../images/3-commands-brewing.png)

2. Impostare la modalità di manutenzione. La macchina per il caffè verrà impostata sulla modalità di manutenzione se *non* lo è già.
    ![Eseguire comandi](../images/3-commands-c.png)
    
    Cercare i messaggi di conferma nel log della console nella macchina per il caffè simulata. 
    > [!NOTE]
    > Come nel mondo reale, quando il tecnico porta offline la macchina per eseguire le riparazioni necessarie prima di riportarla online, la macchina per il caffè simulata resta in modalità di manutenzione finché non si riavvia il codice client.
    ![Eseguire comandi](../images/3-commands-maintenance.png)

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visualizzare la macchina per il caffè nel dashboard
Nel dashboard è possibile visualizzare collettivamente le informazioni rilevanti relative alla macchina per il caffè. 

1. Scegliere **Line Chart** (Grafico a linee) e immettere il titolo Temperature per visualizzare i dati di telemetria. Scegliere **Past 30 minutes** (Ultimi 30 minuti) come **Time Range** (Intervallo di tempo).
![Visualizzazione del dashboard](../images/3-dashboard-a.png)

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) e immettere il titolo Device Properties. In **Add/Remove** (Aggiungi/Rimuovi) scegliere Coffee Makers Max Temperature, Coffee Makers Min Temperature, Device Warranty Expired. 
![Visualizzazione del dashboard](../images/3-dashboard-b.png)

1. Scegliere **Settings and Properties** (Impostazioni e proprietà) e immettere il titolo Optimal Temperature. In **Add/Remove** (Aggiungi/Rimuovi) scegliere Optimal Temperature. 
![Visualizzazione del dashboard](../images/3-dashboard-c.png)


## <a name="summary"></a>Riepilogo

In questo modulo si è trascorso del tempo a convalidare la connessione tra la macchina per il caffè simulata e Azure IoT Central. Si è ottenuta la convalida aggiornando la temperatura ottimale ed eseguendo comandi. Infine si è configurato il dashboard per monitorare la macchina per il caffè in un'unica posizione definendo le informazioni da visualizzare. Questi passaggi di convalida sono necessari prima di procedere con altre attività nel modulo successivo. 