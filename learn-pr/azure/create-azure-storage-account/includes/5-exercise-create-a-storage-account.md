In questo esercizio si userà il portale di Azure per creare un account di archiviazione appropriato per un'app Web fittizia di previsioni per il surf nella California del sud.

## <a name="design-goals"></a>Obiettivi di progettazione

Il sito di previsioni per il surf consente agli utenti di caricare foto e video delle condizioni delle spiagge locali. I visualizzatori useranno il contenuto per scegliere la spiaggia con le condizioni migliori per il surf. Ecco l'elenco degli obiettivi di progettazione e funzionalità:

- Il contenuto video deve caricarsi rapidamente
- Il sito deve essere in grado di gestire i picchi imprevisti nei volumi di caricamento
- Il contenuto non aggiornato deve essere rimosso man mano che cambiano le condizioni per il surf, in modo che il sito mostri sempre le condizioni attuali

Si opta per un'implementazione che memorizza nel buffer il contenuto caricato in una coda di Azure per l'elaborazione e quindi lo sposta in un BLOB di Azure per l'archiviazione. A questo scopo è necessario un account di archiviazione in grado di contenere sia le code che i BLOB fornendo al contempo un accesso con bassa latenza al contenuto.

## <a name="exercise-steps"></a>Passaggi dell'esercizio

### <a name="launch-the-blade"></a>Avviare il pannello

1. Nel Web browser passare al [portale di Azure](https://portal.azure.com?azure-portal=true) e accedere al proprio account.

1. Nella barra laterale sinistra selezionare **Crea una risorsa**.

1. Selezionare il titolo **Archiviazione** in Azure Marketplace.

1. Selezionare **Account di archiviazione**. Nel portale viene visualizzato il pannello **Crea account di archiviazione**.

### <a name="configure-the-basic-options"></a>Configurare le opzioni di base

1. Selezionare la scheda **Informazioni di base** nella parte superiore del pannello.

1. **Sottoscrizione:** selezionare una delle proprie sottoscrizioni.

1. **Gruppo di risorse**: creare un nuovo gruppo di risorse denominato **SurfReportResourceGroup**.

1. **Nome account di archiviazione**: immettere un valore univoco globale come `surfreport` + le proprie iniziali + un numero.

 1. **Posizione**: selezionare **Stati Uniti occidentali**.

    Motivazione logica: l'applicazione è destinata agli utenti residenti nella California del sud. Per ridurre al minimo la latenza durante il caricamento dei video, I BLOB devono essere ospitati nelle vicinanze di questi utenti, pertanto **Stati Uniti occidentali** è una buona scelta.

1. **Modello di distribuzione**: selezionare **Gestione risorse**.
    
    Motivazione logica: **Gestione risorse** è appropriato perché consente di usare un gruppo di risorse per gestire l'app Web, l'account di archiviazione e altri elementi per l'applicazione.

1. **Prestazioni**: selezionare **Standard**.

    Motivazione logica: non è possibile usare l'opzione **Premium** in quanto limiterebbe l'account di archiviazione ai BLOB di pagine. Sono necessari BLOB in blocchi per i video e una coda per la memorizzazione nel buffer ed entrambi sono disponibili solo nell'opzione **Standard**.

1. **Tipologia account**: selezionare **Archiviazione v2 (utilizzo generico v2)**.

    Motivazione logica: **Archiviazione v2 (utilizzo generico v2)** è la scelta giusta in questo scenario. Sono infatti necessari sia BLOB che una coda, quindi l'opzione **Archiviazione BLOB** non è adatta. La scelta di un account di tipo **Archiviazione (utilizzo generico v1)** non porterebbe alcun vantaggio a questa applicazione, poiché limiterebbe le funzionalità accessibili senza presumibilmente ridurre il costo del carico di lavoro previsto.

1. **Replica**: selezionare **Archiviazione con ridondanza locale**.

    Motivazione logica: le immagini e i video diventano rapidamente obsoleti e vengono rimossi dal sito. Non vale quindi la pena di pagare di più per la ridondanza globale. Nel caso in cui un evento catastrofico causi la perdita di dati, è possibile riavviare il sito con contenuti aggiornati caricati dagli utenti.

1. **Livello di accesso (predefinito)**: selezionare **Frequente**.
   
    Motivazione logica: si vuole che i video vengano caricati rapidamente, quindi si userà l'opzione con prestazioni elevate per i BLOB.
   
Lo screenshot seguente mostra le impostazioni compilate per la scheda **Informazioni di base**.

![Screenshot di un pannello Crea account di archiviazione con la scheda **Informazioni di base** selezionata.](../media-drafts/5-create-storage-account-basics.png)

### <a name="configure-the-advanced-options"></a>Configurare le opzioni avanzate

1. Selezionare la scheda **Avanzate** nella parte superiore del pannello.

1. **Trasferimento sicuro obbligatorio**: selezionare **Abilitato**.

    Motivazione logica: Https via cavo è generalmente considerato come la procedura consigliata.

1. **Reti virtuali**: selezionare **Disabilitato**.

    Motivazione logica: il contenuto è esposto al pubblico ed è necessario consentire l'accesso da client pubblici.

Lo screenshot seguente mostra le impostazioni compilate per la scheda **Avanzate**.

![Screenshot di un pannello Crea account di archiviazione con la scheda **Avanzate** selezionata.](../media-drafts/5-create-storage-account-advanced.png)

### <a name="create"></a>Creare

1. Fare clic sul pulsante **Rivedi e crea** nella parte inferiore del pannello.

1. Nella schermata successiva fare clic sul pulsante **Crea** nella parte inferiore del pannello.

1. Attendere che la risorsa venga creata.

### <a name="verify"></a>Verificare

1. Selezionare il collegamento **Account di archiviazione** nella barra laterale sinistra.

1. Individuare il nuovo account di archiviazione nell'elenco per verificare che la creazione sia riuscita.

### <a name="clean-up"></a>Eseguire la pulizia

1. Selezionare il collegamento **Gruppo di risorse** nella barra laterale sinistra.

1. Individuare **SurfReportResourceGroup** nell'elenco.

1. Fare clic con il pulsante destro del mouse sulla voce **SurfReportResourceGroup** e scegliere **Elimina gruppo di risorse** dal menu di scelta rapida.

1. Digitare il nome del gruppo di risorse nel campo di conferma.

1. Fare clic sul pulsante **Elimina**.

## <a name="summary"></a>Riepilogo

È stato creato un account di archiviazione con le impostazioni più appropriate per i propri requisiti aziendali. Ad esempio, è stato selezionato il data center Stati Uniti occidentali perché i clienti risiedevano principalmente nella California del sud. Il flusso descritto è un flusso tipico: prima si analizzano i dati e gli obiettivi e poi si configurano le opzioni dell'account di archiviazione più appropriate.