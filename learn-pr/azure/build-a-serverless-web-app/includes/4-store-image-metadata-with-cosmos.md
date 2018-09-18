Azure Cosmos DB è il database di Microsoft multimodello, serverless, distribuito a livello globale. In questo modulo si vedrà come usare Funzioni di Azure per memorizzare e recuperare i metadati delle immagini come documenti JSON in Azure Cosmos DB.

## <a name="create-an-azure-cosmos-db-account-database-and-collection"></a>Creare un account, un database e una raccolta Azure Cosmos DB

Un account Azure Cosmos DB è una risorsa di Azure che contiene database Azure Cosmos DB.

1. Verificare di essere ancora connessi a Cloud Shell. In caso contrario, selezionare **Enter focus mode** (Accedi a modalità messa a fuoco) per aprire una finestra di Cloud Shell. 

1. Creare un account Azure Cosmos DB con un nome univoco nello stesso gruppo di risorse delle altre risorse di questa esercitazione.

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. Dopo avere creato l'account Azure Cosmos DB, creare un nuovo database denominato **imagesdb** nell'account.

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. Dopo la creazione del database, creare una nuova raccolta denominata **images** nel database con una velocità effettiva di 400 unità richiesta (UR).

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-azure-cosmos-db-when-a-thumbnail-is-created"></a>Salvare un documento in Azure Cosmos DB quando viene creata un'anteprima

L'associazione di output di Azure Cosmos DB consente di creare documenti in una raccolta Azure Cosmos DB da Funzioni di Azure. Nei passaggi seguenti viene configurata un'associazione di output di Azure Cosmos DB nella funzione **ResizeImage** e la funzione viene modificata in modo che restituisca un documento (oggetto) da salvare.

1. Aprire l'app per le funzioni nel [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Nel riquadro di spostamento a sinistra espandere la funzione **ResizeImage** e selezionare **Integrazione**.

1. Sotto **Output** fare clic su **Nuovo output**.

1. Individuare l'elemento **Azure Cosmos DB** e selezionarlo. Fare clic su **Seleziona**.

    ![Selezionare Nuovo output](../media/4-new-output.jpg)

1. Compilare i campi sotto **Azure Cosmos DB output** (Output Azure Cosmos DB) con i valori seguenti.

    | Impostazione      |  Valore consigliato   | Descrizione                                        |
    | --- | --- | ---|
    | **Nome del parametro del documento** | Selezionare **Usa valore restituito della funzione**. | Il valore della casella di testo viene impostato automaticamente su **$return**. |
    | **Nome database** | imagesdb | Usare il nome del database creato in precedenza. |
    | **Nome raccolta** | images | Usare il nome della raccolta creata in precedenza. |

1. Accanto a **Connessione all'account Azure Cosmos DB** fare clic su **nuova**. Selezionare l'account Azure Cosmos DB creato in precedenza.

    ![Immettere le impostazioni per l'associazione di output di Azure Cosmos DB](../media/4-cosmos-db-output.png)

1. Fare clic su **Salva** per creare l'associazione di output di Azure Cosmos DB.

1. Fare clic sul nome della funzione **ResizeImage** a sinistra per aprire la funzione.

::: zone pivot="csharp"
1. (C#) Cambiare il tipo restituito dalla funzione da **void** a **object**.

1. (C#) Alla fine della funzione, aggiungere il blocco di codice seguente per restituire il documento da salvare:

    ```csharp
    return new {
        id = name,
        imgPath = "/images/" + name,
        thumbnailPath = "/thumbnails/" + name
    };
    ```

    ![run.csx per la funzione ResizeImages dopo le modifiche](../media/4-update-function.png)

::: zone-end

::: zone pivot="javascript"
1. (JavaScript) Modificare l'istruzione `context.done()` nella clausola `else` in modo da restituire il documento da salvare in Azure Cosmos DB.

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

::: zone-end

1. Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.

1. Fare clic su **Salva**. Verificare nel pannello dei log che la funzione sia stata salvata correttamente e che non vi siano errori.


## <a name="create-a-function-to-list-images-from-azure-cosmos-db"></a>Creare una funzione per elencare le immagini da Azure Cosmos DB

L'applicazione Web richiede un'API per recuperare i metadati delle immagini da Azure Cosmos DB. Con la procedura seguente verrà creata una funzione attivata da HTTP che usa un'associazione di input di Azure Cosmos DB per eseguire una query sulla raccolta di database.

1. Nell'app Funzioni passare il puntatore del mouse su **Funzioni** a sinistra e fare clic sul segno + per creare una nuova funzione.

1. Individuare il modello **HttpTrigger** e selezionarlo.

1. Usare questi valori per creare una funzione che genera un URL per ottenere le immagini:

    | Impostazione      |  Valore consigliato   | Descrizione                                        |
    | --- | --- | ---|
    | **Assegnare un nome alla funzione** | GetImages | Immettere questo nome esattamente come illustrato in modo che l'applicazione possa individuare la funzione. |
    | **Livello di autorizzazione** | Anonimo | Consente l'accesso pubblico alla funzione. |

1. Fare clic su **Crea**.

1. Dopo la creazione della nuova funzione, fare clic su **Integrazione** sotto il nome della funzione nel riquadro di spostamento di sinistra.

1. Fare clic su **Nuovo input** e selezionare **Azure Cosmos DB**. 

    ![Selezionare Nuovo input](../media/4-new-input.jpg)

1. Fare clic su **Seleziona**.

1. Inserire i valori seguenti:

    | Impostazione      |  Valore consigliato   | Descrizione                                        |
    | --- | --- | ---|
    | **Nome del parametro del documento** | documents | Corrisponde al nome del parametro nella funzione. |
    | **Nome database** | imagesdb |  |
    | **Nome raccolta** | images |  |
    | **SQL query** | select * from c order by c._ts desc | Ottiene i documenti, a cominciare dal più recente. |
    | **Connessione all'account Azure Cosmos DB** | Selezionare la stringa di connessione esistente. |  |

1. Fare clic su **Salva** per creare l'associazione di input.

::: zone pivot="csharp"
1. Fare clic sul nome della funzione per aprire la finestra del codice. Sostituire tutto il file **run.csx** con il contenuto del file [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).

::: zone-end

::: zone pivot="javascript"
1. Fare clic sul nome della funzione per aprire la finestra del codice. Sostituire tutto il file **index.js** con il contenuto del file [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).

::: zone-end

1. Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.

1. Fare clic su **Salva**. Verificare nel pannello dei log che la funzione sia stata salvata correttamente e che non vi siano errori.

## <a name="test-the-application"></a>Test dell'applicazione

1. Aprire l'applicazione in un browser. Selezionare un file di immagine e caricarlo.

1. Dopo alcuni secondi, nella pagina verrà visualizzata l'anteprima della nuova immagine.

1. Nel portale di Azure usare la casella **Cerca** per cercare l'account Azure Cosmos DB in base al nome. Fare clic sul nome per aprire l'account.

1. Fare clic su **Esplora dati** a sinistra per sfogliare le raccolte e i documenti.

1. Sotto il database **imagesdb** selezionare la raccolta **images**.

1. Verificare che sia stato creato un documento per l'immagine caricata.

    ![Esplora dati con un documento per un'immagine caricata](../media/4-data-explorer.png)

## <a name="summary"></a>Riepilogo

In questa unità è stato descritto come creare un account, un database e una raccolta di Azure Cosmos DB. Si è anche osservato come usare le associazioni di Azure Cosmos DB per salvare e recuperare i metadati delle immagini nella raccolta di Azure Cosmos DB. Ora si apprenderà come generare automaticamente una didascalia per ogni immagine caricata usando Servizi cognitivi Microsoft.