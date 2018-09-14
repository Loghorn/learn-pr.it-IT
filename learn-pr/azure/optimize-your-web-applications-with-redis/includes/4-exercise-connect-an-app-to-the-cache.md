In questa unità si creerà un'istanza di Cache Redis di Azure per archiviare e restituire valori usati comunemente.

## <a name="create-a-cache"></a>Creare una cache

1. Accedere al portale di Azure. Fare quindi clic su **Crea una risorsa**, su **Database** e quindi su **Cache Redis**.

    Lo screenshot seguente mostra la posizione di Cache Redis all'interno delle varie opzioni per le risorse di database nel portale di Azure.

    ![Screenshot che mostra le opzioni per i database del portale di Azure, con le opzioni Crea una risorsa, Database e Cache Redis evidenziate.](../media/4-create-a-cache-1.png)

## <a name="configure-your-cache"></a>Configurare la cache

1. **Nome DNS:** creare un nome globalmente univoco, ad esempio **ContosoSportsApp1028**.

1. **Sottoscrizione:** selezionare la sottoscrizione.

1. **Gruppo di risorse:** creare un nuovo gruppo di risorse e assegnare al gruppo un nome.

1. **Area:** dato che la maggior parte dei clienti si trova nell'area di New York, è necessario selezionare **Stati Uniti orientali**.

1. **Piano tariffario:** selezionare **Basic C0**.

1. Fare clic su **Crea**.

    Lo screenshot seguente mostra una configurazione rappresentativa usata per creare una nuova risorsa di Cache Redis.

    ![Screenshot che mostra il pannello del portale di Azure quando si crea una nuova risorsa di Cache Redis, popolato con un nome DNS della configurazione di esempio, sottoscrizione, nuovo gruppo di risorse, area e piano tariffario.](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> Si dovrà attendere che la cache venga distribuita prima di continuare. Questo processo può richiedere del tempo.

## <a name="retrieve-the-access-keys-and-host-name"></a>Recuperare le chiavi di accesso e il nome host

1. Nel portale di Azure passare alla cache e selezionare **Impostazioni** > **Chiavi di accesso**. Prendere nota della **Stringa di connessione primaria (StackExchange.Redis)**.

    Questa chiave include la chiave primaria e il nome host in una stringa di connessione completa per l'uso all'interno delle impostazioni dell'applicazione per il pacchetto StackExchange.Redis usato.

1. Avviare il Blocco note o un altro editor di testo nel computer.

1. Aggiungere il contenuto seguente:

    Sostituire `<connection-string>` con la stringa di connessione primaria della cache di cui si è preso nota in precedenza.

    ```xml
    <appSettings>
        <add key="CacheConnection" value="<connection-string>"/>
    </appSettings>
    ```

1. Salvare il file in una posizione in cui non verrà incluso nel codice sorgente dell'applicazione di esempio. Per questa esercitazione, il file verrà salvato come `C:\AppSecrets\CacheSecrets.config`.

## <a name="create-a-console-app"></a>Creare un'app console

1. Avviare Visual Studio e fare clic sulla voce di menu **File** > **Nuovo** > **Progetto**.

1. Selezionare il modello **Visual C#** > **Windows Desktop** > **App console**.

1. Assegnare un nome all'app e fare clic su **OK** per creare una nuova applicazione console.

## <a name="configure-the-cache-client"></a>Configurare il client della cache

In questa sezione, si configurerà l'applicazione console per usare il client **StackExchange.Redis** per .NET.

1. In Visual Studio fare clic su **Strumenti**, su **Gestione pacchetti NuGet**, **Console di Gestione pacchetti** ed eseguire il comando seguente dalla finestra Console di Gestione pacchetti:

    ```powershell
    Install-Package StackExchange.Redis
    ```

Una volta completata l'installazione, il client della cache StackExchange.Redis è disponibile per l'uso con il progetto.

## <a name="connect-to-the-cache"></a>Connettersi alla cache

1. In Visual Studio aprire il file **App.config** in **Esplora soluzioni** e aggiornarlo per includere un attributo di file **appSettings** che fa riferimento al file **CacheSecrets.config**.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
        </startup>

        <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>

    </configuration>
    ```

1. In Esplora soluzioni fare clic con il pulsante destro del mouse su **Riferimenti** e scegliere **Aggiungi riferimento**. Selezionare **System.Configuration** nell'elenco **Assembly** > **Framework** e fare clic su **OK**.

1. Aggiungere le istruzioni `using` seguenti a **Program.cs**:

    ```csharp
    using StackExchange.Redis;
    using System.Configuration;
    ```

La connessione a Cache Redis di Azure è gestita dalla classe ConnectionMultiplexer. Questa classe deve essere condivisa e riutilizzata in tutta l'applicazione client. Non creare una nuova connessione per ogni operazione.

Non archiviare mai le credenziali nel codice sorgente. Per semplificare questo esempio, viene usato solo un file di configurazione dei segreti esterno. Un approccio migliore potrebbe consistere nell'usare Azure Key Vault con certificati.

1. In **Program.cs** aggiungere i membri seguenti alla classe Program dell'applicazione console:

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

Questo approccio per la condivisione di un'istanza di ConnectionMultiplexer nell'applicazione usa una proprietà statica che restituisce un'istanza connessa. Il codice offre un modo thread-safe per inizializzare solo una singola istanza di ConnectionMultiplexer connessa. La proprietà **abortConnect** è impostata su false, a indicare che la chiamata riuscirà anche se non viene stabilita una connessione a Cache Redis di Azure. Una delle funzionalità principali di ConnectionMultiplexer è il ripristino automatico della connettività alla cache non appena l'errore di rete o eventuali altri problemi vengono risolti.

Il valore dell'appSetting **CacheConnection** viene usato per fare riferimento alla stringa di connessione della cache dal portale di Azure come parametro password.
