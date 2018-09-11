Il team di sviluppo dell'app per le statistiche sportive ha deciso che la memorizzazione nella cache potrebbe migliorare notevolmente le prestazioni per i dati richiesti di recente. Il passaggio successivo consiste nel creare e configurare un'istanza di Cache Redis di Azure.

### <a name="create-and-configure-the-azure-redis-cache-instance"></a>Creare e configurare l'istanza di Cache Redis di Azure

1. È possibile aggiungere un'istanza di Cache Redis di Azure dalle risorse del database nel portale di Azure.

1. Specificare un nome globalmente univoco. Il nome deve essere una stringa contenente da 1 a 63 caratteri che possono includere solo numeri, lettere e il carattere '-'. Il nome della cache non può iniziare o terminare con il carattere '-' e più caratteri '-' consecutivi non sono validi.

1. Specificare la sottoscrizione in cui viene creata questa nuova istanza di Cache Redis di Azure.

1. Assegnare un nome al gruppo di risorse in cui creare la cache. Inserendo tutte le risorse per un'app in un gruppo è possibile gestirle insieme.

1. Scegliere una località vicina al consumer di dati.

    > [!NOTE]
    > È molto più importante che la cache sia vicino al consumer di dati piuttosto che all'archivio dati.

1. Selezionare il piano tariffario. Tenere presente che per sfruttare i vantaggi della persistenza dei dati, sarà necessario scegliere un piano tariffario Premium.

    - Il livello Premium consente di rendere persistenti i dati in uno di due modi per tenere conto del ripristino di emergenza:

        - Con la persistenza RDB viene creato uno snapshot periodico ed è possibile ricostruire la cache usando lo snapshot in caso di errore.

            ![Screenshot del portale di Azure che illustra le opzioni di persistenza RDB per una nuova istanza di Cache Redis.](../media/3-redis-persistence-1.png)

        - La persistenza AOF salva ogni operazione di scrittura in un log che viene salvato almeno una volta al secondo. Vengono così creati file di dimensioni maggiori rispetto a RDB ma con minore perdita di dati.

            ![Screenshot del portale di Azure che illustra le opzioni di persistenza AOF per una nuova istanza di Cache Redis.](../media/3-redis-persistence-2.png)

    - Proteggere la cache con una rete virtuale.
      Se è disponibile una cache Redis di livello Premium, è possibile distribuirla in una rete virtuale nel cloud. La cache sarà disponibile solo per altre macchine virtuali e applicazioni nella stessa rete virtuale.

    - Distribuire la cache con il clustering.
      Con una cache Redis di livello Premium, è possibile implementare il clustering per suddividere automaticamente i set di dati tra più nodi. Per implementare il clustering, specificare il numero di partizioni per un massimo di 10. Il costo è il costo del nodo originale moltiplicato per il numero di partizioni.

    - Eseguire la migrazione dal Servizio cache gestita di Azure.
      A seconda delle funzionalità del Servizio cache gestita usate, la migrazione delle applicazioni che usano il Servizio cache gestita di Azure in Cache Redis di Azure può essere eseguita con modifiche minime all'applicazione. Anche se le API non sono esattamente uguali, sono simili. Gran parte del codice esistente che accede alla cache con il Servizio cache gestita può essere riutilizzata con modifiche minime.

### <a name="configure-your-client"></a>Configurare il client

Le applicazioni client devono essere configurate per l'uso di Cache Redis. Un client Redis ad alte prestazioni diffuso per il linguaggio .NET è **StackExchange.Redis**. Per aggiungere il client Redis si usa **Gestione pacchetti NuGet** in Visual Studio con il comando **Install-Package**.

È quindi necessario configurare il client per connettersi alla cache. Creare un file di testo con estensione **config** che contiene le impostazioni per il nome host e la chiave primaria o secondaria. Il file deve avere la struttura seguente:

```XML
    <appSettings>
        <add key="CacheConnection" value="HOST-NAME.redis.cache.windows.net,abortConnect=false,ssl=true,password=PRIMARY-KEY"/>
    </appSettings>
```

Il file **App.config** verrà quindi modificato e aggiornato per includere un attributo di file **appSettings** che fa riferimento al file config appena creato.

### <a name="what-are-access-keys"></a>Cosa sono le chiavi di accesso?

Per connettersi a un'istanza di Cache Redis di Azure, è necessario specificare il nome host, le porte e una chiave per la cache. Queste informazioni possono essere recuperate nel portale di Azure.

### <a name="connection-settings"></a>Impostazioni di connessione

Per connettersi a Cache Redis di Azure, saranno necessari il nome host e la chiave di accesso. Il nome host è l'indirizzo Internet della cache, ad esempio *sportsresults.redis.cache.windows.net*. La chiave di accesso funge da password per la cache. Sono disponibili due chiavi di accesso, una primaria e una secondaria. È possibile usare una delle due chiavi, ma ne vengono fornite due nel caso diventi necessario modificare la chiave primaria. È possibile passare tutti i client alla chiave secondaria e quindi apportare la modifica alla chiave primaria, evitando così la perdita del servizio.
