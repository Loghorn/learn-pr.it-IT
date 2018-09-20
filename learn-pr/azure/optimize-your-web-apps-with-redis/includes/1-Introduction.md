Si lavora presso una società che tiene traccia delle statistiche per sport professionistici e fornisce un'API per eseguire query sui risultati. In questo modo i tifosi possono monitorare e controllare gare e punteggi, sia in tempo reale che nei dati storici. Gli utenti possono anche richiedere statistiche di una squadra tramite una ricerca in linguaggio naturale, ad esempio "How many times has John Smith hit a home run against a left-handed pitcher?"

Durante i periodi di picco della domanda, ad esempio durante i playoff, i tempi di risposta del servizio rallentano perché il servizio back-end non ha la capacità per soddisfare la domanda. Si vogliono migliorare le prestazioni per gli utenti e ridurre il carico di lavoro nei servizi back-end e di archiviazione dei dati. Le metriche indicano che tra il 50% e l'80% dei dati restituiti riguarda valori di sola lettura o richiesti di recente. L'implementazione di una cache per i dati di uso comune potrebbe migliorare le prestazioni e ridurre la latenza.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Ottenere informazioni sulla cache Redis e sul rispettivo funzionamento per esigenze aziendali specifiche
- Creare un progetto e pianificare l'uso di Cache Redis
- Effettuare il provisioning di una Cache Redis in Azure
- Connettere un'applicazione alla cache

## <a name="prerequisites"></a>Prerequisiti

- Esperienza con lo sviluppo di app
- Esperienza con l'uso dei dati nelle app
