Si immagini di lavorare per un'azienda di scale mobili con grandi investimenti nella tecnologia IoT. Il compito è quello di controllare l'elaborazione dei dati dei sensori di temperatura negli ingranaggi conduttori delle scale mobili. È necessario monitorare i dati relativi alla temperatura e aggiungere un flag di dati per indicare quando gli ingranaggi hanno una temperatura troppo elevata. Nei sistemi a valle, ciò consentirà di determinare quando è necessaria la manutenzione.

La società riceve i dati del sensore da diverse posizioni e da diversi modelli di scale mobili. I dati arrivano in formati diversi, inclusi caricamenti di file batch, pool di database pianificati, messaggi in una coda o dati in ingresso da un Hub eventi. Si desidera sviluppare un servizio riutilizzabile in grado di elaborare i dati di temperatura come richiesto da tutte queste origini.

Sviluppare e ospitare un servizio come questo consente di richiedere i server e l'infrastruttura. Con Funzioni di Azure è possibile ottenere il calcolo senza server; viene codificata la logica nel linguaggio di programmazione preferito e viene eseguita la logica quando necessario, senza provisioning o server di gestione.

È possibile concentrarsi sulla creazione di applicazioni eccellenti, non sul provisioning o sui server di manutenzione.

## <a name="learning-objectives"></a>Obiettivi di apprendimento
> [!div class="checklist"]
> * Decidere se l'elaborazione senza server è adatta alla propria azienda
> * Creare un'app per le funzioni di Azure nel portale di Azure
> * Eseguire una funzione con i trigger
> * Monitorare e testare la funzione di Azure nel portale di Azure 
