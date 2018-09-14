Si lavora presso una società che tiene traccia delle statistiche di sport professionali e fornisce un'API per i risultati della query. In questo modo i tifosi possono monitorare e controllare gare e punteggi, sia in tempo reale che nei dati storici. Gli utenti possono anche richiedere le statistiche del team tramite una ricerca in linguaggio naturale, ad esempio, "quante volte ha raggiunto un'esecuzione home in un lanciatore mancino John Smith?"

Durante i periodi di picco della domanda, ad esempio durante playoff, tempo di risposta del servizio rallenta perché il servizio back-end non è in grado di soddisfare la domanda. Si vogliono migliorare le prestazioni per gli utenti e ridurre il carico di lavoro nei servizi back-end e di archiviazione dei dati. Le metriche indicano che tra il 50% e l'80% dei dati restituiti riguarda valori di sola lettura o richiesti di recente. L'implementazione di una cache per i dati di uso comune potrebbe migliorare le prestazioni e ridurre la latenza.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Vengono descritte quali un Redis cache è e come è possibile usarlo per le esigenze aziendali
- Creare una progettazione e prevede di usare una cache Redis
- Effettuare il provisioning di una cache Redis di Azure
- Connettere un'applicazione nella cache

## <a name="prerequisites"></a>Prerequisiti

- Esperienza con lo sviluppo di app
- Esperienza con l'uso dei dati nelle app
