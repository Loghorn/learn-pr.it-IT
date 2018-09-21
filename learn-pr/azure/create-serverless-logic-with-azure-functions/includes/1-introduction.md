Si supponga di lavorare per un'azienda di installazione di scale mobili che ha investito in tecnologia IoT per monitorare i prodotti nel luogo di installazione. L'utente controlla l'elaborazione dei dati dei sensori di temperatura degli ingranaggi di trasmissione delle scale mobili. Esegue il monitoraggio dei dati relativi alla temperatura e aggiunge un flag di dati per indicare quando gli ingranaggi hanno una temperatura troppo elevata. Nei sistemi a valle, questi dati consentono di determinare quando è necessaria la manutenzione.

La società riceve i dati dei sensori da diverse località e da diversi modelli di scale mobili. I dati arrivano in formati diversi, inclusi caricamenti di file batch, estrazioni di database pianificate, messaggi in una coda o dati in ingresso da un hub eventi. Si intende sviluppare un servizio riutilizzabile in grado di elaborare i dati di temperatura da tutte queste origini.

Quando si progetta un servizio di questo tipo con strategie di architettura aziendale tradizionali, è necessario prendere in considerazione l'infrastruttura server e la manutenzione fin dall'inizio: predisporre l'hardware necessario, pianificarne l'installazione, coordinare il personale IT per la gestione e così via. Un'alternativa a questo scenario è l'**elaborazione serverless**. Con l'elaborazione serverless il provider di servizi cloud gestisce il provisioning e la manutenzione dell'infrastruttura, consentendo agli sviluppatori di concentrarsi completamente sulla creazione del codice dell'app. Funzioni di Azure è un componente fondamentale dell'offerta di elaborazione serverless di Azure e consente di eseguire nel cloud frammenti di codice o *funzioni* scritte nel linguaggio di programmazione preferito.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Decidere se l'elaborazione serverless è adatta alle esigenze aziendali.
- Creare un'app per le funzioni di Azure nel portale di Azure.
- Eseguire una funzione con i trigger.
- Monitorare e testare la funzione di Azure nel portale di Azure.
