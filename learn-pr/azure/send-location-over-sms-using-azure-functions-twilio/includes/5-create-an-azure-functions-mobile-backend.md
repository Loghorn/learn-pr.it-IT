A questo punto, l'app sta cercando di ottenere la posizione dell'utente ed è pronta per essere inviata a una funzione di Azure. In questa unità, si compila la funzione di Azure.

## <a name="create-an-azure-functions-project"></a>Creare un progetto di Funzioni di Azure

1. Aggiungere un nuovo progetto nella soluzione `ImHere` facendo clic con il tasto destr sulla soluzione e selezionando *Aggiungi -> Nuovo progetto...*.

1. Dall'albero sul lato sinistro selezionare *Visual C#->Cloud* e quindi *Funzioni di Azure* dal pannello al centro.

1. Denominare il progetto "ImHere.Functions" e quindi fare clic su **OK**.

    ![Finestra di dialogo Aggiungi nuovo progetto](../media-drafts/5-add-new-functions-project.png)

1. Nella finestra di configurazione **Nuovo progetto**, lasciare Versione Funzioni impostata su *Funzioni di Azure v1 (.NET Framework)*. Selezionare *Trigger HTTP*, lasciare l'account di archiviazione impostato su *Emulatore di archiviazione*e impostare i diritti di accesso su *Anonimo*. Fare quindi clic su **OK**.

    ![Finestra di dialogo di configurazione del progetto Funzioni di Azure](../media-drafts/5-configure-trigger.png)

Il nuovo progetto verrà creato e includerà una funzione predefinita denominata `Function1`.

> Questa funzione è stata creata con l'accesso anonimo. Dopo la pubblicazione in Azure, qualsiasi persona che disponga dell'URL potrà chiamare la funzione. In uno scenario reale, disporrebbe di protezione tramite un qualche tipo di autenticazione, ad esempio il [servizio app di Azure](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) o [Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c).

## <a name="create-the-function"></a>Creare la funzione

Il progetto di Funzioni di Azure viene creato con una singola funzione di trigger HTTP denominata `Function1`. La funzione stessa viene implementata come un metodo statico `Run` nella classe `Function1`.

1. Rinominare il file in Esplora soluzioni da "Function1.cs" a "SendLocation.cs". Quando viene richiesto di rinominare tutti i riferimenti all'elemento di codice `Function1`, fare clic su **Sì**.

1. Rinominare il nome della funzione nell'attributo in "SendLocation".

    ```cs
    [FunctionName("SendLocation")]
    ```

1. Eliminare il contenuto della funzione, eccetto la prima riga che scrive un messaggio informativo nel logger.

    ```cs
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                       TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a>Creare una classe per condividere dati tra le app per dispositivi mobili e la funzione

Quando vengono inviati a una funzione di Azure, i dati sono in formato JSON. L'app per dispositivi mobili serializzerà i dati in JSON, mentre la funzione li deserializzerà. Per mantenere la coerenza dei dati tra l'app per dispositivi mobili e la funzione, creare un nuovo progetto che contenga una classe per mantenere i dati della posizione e del numero di telefono. L'app e la funzione faranno quindi riferimento a questo progetto.

1. Creare un nuovo progetto nella soluzione `ImHere` facendo clic con il tasto destr sulla soluzione e selezionando *Aggiungi -> Nuovo progetto...*.

1. Dall'albero sul lato sinistro selezionare *Visual C#->.NET Standard* e quindi *Libreria di classi (.NET Standard)* dal pannello al centro.

1. Denominare il progetto "ImHere.Data" e quindi fare clic su **OK**.

    ![Finestra di dialogo Aggiungi nuovo progetto](../media-drafts/5-add-new-net-standard-project.png)

1. Eliminare il file "Class1.cs" generato automaticamente.

1. Creare una nuova classe nel progetto `ImHere.Data` denominata `PostData` facendo clic con il pulsante destro del mouse sul progetto e quindi selezionando *Aggiungi -> Classe...*. Assegnare il nome "PostData" alla nuova classe e fare clic su **OK**.

1. Aggiungere le proprietà `double` per la latitudine e la longitudine e una proprietà `string[]`per indicare i numeri di telefono a cui inviare le informazioni.

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. Aggiungere un riferimento a questo progetto in entrambi i progetti `ImHere.Functions` e `ImHere` facendo clic sul progetto e selezionando *Aggiungi -> Riferimento...*. Selezionare *Progetti* nell'albero a sinistra, quindi selezionare la casella accanto a *ImHere.Data*.

    ![Configurazione dei riferimenti del progetto](../media-drafts/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a>Leggere i dati inviati alla funzione

Nella funzione di Azure, il parametro `req` contiene la richiesta HTTP eseguita. I dati all'interno della richiesta saranno un oggetto `PostData` JSON serializzato.

1. Aprire la classe `SendLocation` nel progetto `ImHere.Functions`.

1. Leggere il contenuto della richiesta HTTP in un oggetto `PostData`, aggiungendo una direttiva d'uso per lo spazio dei nomi `ImHere.Data`.

    ```cs
    PostData data = await req.Content.ReadAsAsync<PostData>();
    ```

1. Costruire un URL di Google Maps usando la latitudine e la longitudine da `PostData`.

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. Registrare l'URL.

    ```cs
    log.Info($"URL created - {url}");
    ```

1. Viene restituito un codice di stato 200 per mostrare il completamento della funzione senza errori.

    ```cs
    return req.CreateResponse(HttpStatusCode.OK);
    ```

La funzione completa viene mostrata di seguito.

```cs
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                                Route = null)]HttpRequestMessage req,
                                                    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    PostData data = await req.Content.ReadAsAsync<PostData>();
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.Info($"URL created - {url}");
    return req.CreateResponse(HttpStatusCode.OK);
}
```

## <a name="run-the-azure-function-locally"></a>Eseguire localmente la funzione di Azure

È possibile eseguire in locale le funzioni con un account di archiviazione locale e un runtime Funzioni di Azure locale. Questo runtime locale consente di testare la funzione prima di distribuirla in Azure.

1. Fare clic con il pulsante destro del mouse sul progetto `ImHere.Functions` in Esplora soluzioni, quindi scegliere *Imposta come progetto di avvio*.

1. Dal menu *Debug*, selezionare *Avvia senza eseguire debug*. Il runtime di Funzioni di Azure locale verrà avviato all'interno di una finestra della console e avvierà la funzione, in ascolto su una porta disponibile in `localhost`.

    ![Funzione di Azure eseguita localmente](../media-drafts/5-function-running-locally.png)

1. Prendere nota della porta su cui è in ascolto la funzione. Sarà necessaria nell'unità successiva per testare l'app per dispositivi mobili. Nell'immagine precedente, la funzione è in ascolto sulla porta **7071**.

    ```sh
    Listening on http://localhost:7071/
    ```

1. Lasciare la funzione in esecuzione in modo da poter testare l'app per dispositivi mobili nell'unità successiva.

## <a name="summary"></a>Summary

In questa unità si è visto come creare un progetto Funzioni di Azure in Visual Studio, si è aggiunto un progetto condiviso con un oggetto dati da condividere tra l'app per dispositivi mobili e la funzione e si è appreso come creare un'implementazione di base della funzione per deserializzare i dati al suo interno. Si è anche appreso come eseguire in locale una funzione di Azure. Nell'unità successiva si chiamerà la funzione di Azure dall'app per dispositivi mobili.