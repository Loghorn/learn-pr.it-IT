L'applicazione che si sta creando è una raccolta di foto che usa JavaScript lato client per chiamare API per caricare e visualizzare immagini. In questa unità si creerà un'API usando una funzione serverless che genera un URL temporaneo per caricare un'immagine. L'applicazione Web usa l'URL generato per caricare un'immagine nell'archiviazione BLOB usando l'[API REST dell'archiviazione BLOB](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

## <a name="create-a-blob-storage-container-for-images"></a>Creare un contenitore di archiviazione BLOB per le immagini

L'applicazione richiede un contenitore di archiviazione separato per caricare e ospitare immagini.

1. Verificare di essere ancora connessi a Azure Cloud Shell (Bash). In caso contrario, selezionare **Enter focus mode** (Accedi a modalità messa a fuoco) per aprire una finestra di Cloud Shell.

1.  Creare un nuovo contenitore denominato **images** nell'account di archiviazione con accesso pubblico a tutti i BLOB.

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-functions-app"></a>Creare un'app Funzioni di Azure

Funzioni di Azure è un servizio per l'esecuzione di funzioni serverless. Una funzione serverless può essere attivata (chiamata) da eventi come una richiesta HTTP o la creazione di un BLOB in un contenitore di archiviazione.

Un'app Funzioni di Azure è un contenitore per una o più funzioni serverless.

- Creare una nuova app Funzioni con un nome univoco nel gruppo di risorse **first-serverless-app** creato precedentemente. Le app Funzioni richiedono un account di archiviazione. In questa unità si userà l'account di archiviazione esistente creato nell'ultima unità.

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```

## <a name="create-an-http-triggered-serverless-function"></a>Creare una funzione serverless attivata da HTTP

Per caricare un'immagine in modo sicuro nell'archiviazione BLOB, l'applicazione Web della raccolta foto invia alla funzione serverless una richiesta HTTP di generazione di un URL temporaneo. La funzione viene attivata da una richiesta HTTP e usa Azure Storage SDK per generare e restituire l'URL protetto.

1. Dopo che è stata creata, cercare l'app Funzioni di Azure nel [portale di Azure](https://portal.azure.com/?azure-portal=true) usando la casella **Cerca**. Fare clic sull'app per aprirla.

    ![Aprire l'app Funzioni](../media/2-search-function-app.png)

1. Nella finestra dell'app Funzioni nel riquadro di spostamento a sinistra scegliere **Funzioni** e fare clic sul segno più (+) per creare una nuova funzione serverless.

    ![Creare una nuova funzione](../media/2-new-function.png)

1. Fare clic su **Funzione personalizzata** per visualizzare un elenco di modelli di funzione.

1. Individuare il modello **HttpTrigger** e fare clic su C# o JavaScript.

1. Usare i valori seguenti per creare una funzione che genera un URL di caricamento BLOB:

    | Impostazione      |  Valore consigliato   | Descrizione                                        |
    | --- | --- | ---|
    | **Linguaggio** | C# o JavaScript | Selezionare il linguaggio che si vuole usare. |
    | **Assegnare un nome alla funzione** | GetUploadUrl | Immettere questo nome esattamente come illustrato in modo che l'applicazione possa individuare la funzione. |
    | **Livello di autorizzazione** | Anonimo | Permette l'accesso pubblico alla funzione. |

    ![Immettere le impostazioni per una nuova funzione attivata da HTTP](../media/2-new-function-httptrigger.png)

1. Fare clic su **Crea** per creare la funzione.

::: zone pivot="csharp"
1. (C#) Quando viene visualizzato il codice sorgente della funzione, sostituire l'intero contenuto del file **run.csx** con il contenuto del file [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).

::: zone-end

::: zone pivot="javascript"
1. (JavaScript) Questa funzione richiede il pacchetto `azure-storage` di npm. Il pacchetto genera il token di firma di accesso condiviso (SAS) necessario per compilare l'URL protetto. Per installare il pacchetto npm, fare clic sull'app Funzioni nel riquadro di spostamento di sinistra e fare clic su **Funzionalità della piattaforma**.

1. (JavaScript) Fare clic su **Console** per visualizzare una finestra della console.

    ![Aprire una finestra della console](../media/2-open-console.jpg)

1. (JavaScript) Assicurarsi che la directory corrente sia **d:\home\site\wwwroot** eseguendo il comando `cd d:\home\site\wwwroot`.

1. (JavaScript) Eseguire il comando `npm init -y` per creare un file **package.json** vuoto.

1. (JavaScript) Eseguire il comando `npm install --save azure-storage` nella console per installare il pacchetto. Salvare il pacchetto come **package.json**. Il completamento dell'operazione potrebbe richiedere alcuni minuti.

1. (JavaScript) Fare clic sulla funzione (**GetUploadUrl**) nel riquadro di spostamento a sinistra per visualizzare la funzione. Sostituire tutto il contenuto del file **index.js** con il contenuto del file [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).

    ![Contenuto di index.js dopo l'aggiornamento](../media/2-paste-js.jpg)

::: zone-end

1. Fare clic su **Log** sotto la finestra del codice per espandere il pannello dei log.

1. Fare clic su **Salva**. Verificare nel pannello dei log che la funzione sia stata compilata correttamente.

La funzione genera un URL di firma di accesso condiviso, usato per caricare un file nell'archiviazione BLOB. L'URL di firma di accesso condiviso è valido per un breve periodo di tempo e permette il caricamento di un solo file. Vedere la documentazione dell'archiviazione BLOB per altre informazioni sull'[uso di firme di accesso condiviso](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>Aggiungere una variabile di ambiente per la stringa di connessione di archiviazione

La funzione appena creata richiede una stringa di connessione per l'account di archiviazione in modo da poter generare l'URL SAS. Invece di codificare la stringa di connessione come hardcoded nel corpo della funzione, è possibile memorizzarla come impostazione dell'applicazione. Le impostazioni dell'applicazione sono accessibili come variabili di ambiente da tutte le funzioni nell'app Funzioni.

1. In Cloud Shell eseguire una query sulla stringa di connessione dell'account di archiviazione e salvarla in una variabile Bash denominata **STORAGE_CONNECTION_STRING**.

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    Verificare che la variabile sia impostata correttamente.

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. Creare una nuova impostazione dell'applicazione denominata **AZURE_STORAGE_CONNECTION_STRING** usando il valore salvato nel passaggio precedente.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    Verificare che l'output del comando contenga la nuova impostazione dell'applicazione con il valore corretto.


## <a name="test-the-serverless-function"></a>Testare la funzione serverless

Oltre alla creazione e alla modifica delle funzioni, il portale di Azure offre uno strumento predefinito anche per il test delle funzioni.

1. Per testare la funzione HTTP serverless, fare clic sulla scheda **Test** sulla destra della finestra del codice per espandere il pannello di test.

1. Modificare **Metodo HTTP** in **GET**.

1. In **Query** fare clic su **Aggiungi parametro** e aggiungere il parametro seguente:

    | Nome      |  Valore   | 
    | --- | --- |
    | **filename** | image1.jpg |

1. Fare clic su **Esegui** nel pannello di test per inviare una richiesta HTTP alla funzione.

1. La funzione restituisce un URL di caricamento nell'output. L'esecuzione della funzione viene visualizzata nel pannello dei log.

    ![Log che indicano l'esecuzione corretta della funzione](../media/2-test-function.png)


## <a name="configure-cors-in-the-functions-app"></a>Configurare CORS nell'app Funzioni

Poiché il front-end della funzione è ospitato nell'archiviazione BLOB, ha un nome di dominio diverso rispetto all'app Funzioni di Azure. Per consentire al codice JavaScript lato client di chiamare correttamente la funzione appena creata, l'app Funzioni deve essere configurata per la condivisione di risorse tra le origini (CORS).

1. Nella barra di spostamento di sinistra della finestra dell'app Funzioni fare clic sul nome dell'app Funzioni.

1. Fare clic su **Funzionalità della piattaforma** per visualizzare un elenco di funzionalità avanzate.

1. Sotto **API** fare clic su **CORS**.

    ![Selezionare CORS](../media/2-open-cors.jpg)

1. Aggiungere un'origine consentita per l'URL dell'applicazione dal modulo precedente, omettendo la barra finale (/). Ad esempio: `https://firstserverlessweb.z4.web.core.windows.net`.

    ![Impostazioni CORS che mostrano l'aggiunta dell'URL dell'app Web serverless](../media/2-add-cors.png)

1. Fare clic su **Salva**.

::: zone pivot="csharp"
1. (C#) Tornare alla funzione `GetUploadUrl` e selezionare la scheda **Integrazione**.

1. (C#) In **Metodi HTTP selezionati** selezionare **OPTIONS**.

    **GET**, **POST** e **OPTIONS** devono essere tutti selezionati. CORS usa il metodo **OPTIONS**, che non è selezionato per impostazione predefinita per le funzioni C#.  

1. (C#) Fare clic su **Salva**.

::: zone-end

1. Sempre nel portale di Azure, passare all'app Funzioni. Selezionare la scheda **Panoramica**. Fare clic su **Riavvia** per assicurarsi che le modifiche per CORS abbiano effetto.

## <a name="configure-cors-in-the-storage-account"></a>Configurare CORS nell'account di archiviazione

Poiché l'applicazione Funzioni esegue anche chiamate JavaScript lato client all'archiviazione BLOB per caricare i file, è necessario configurare anche l'account di archiviazione per CORS.

- Eseguire il comando seguente per consentire a tutte le origini di caricare file nell'account di archiviazione:

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Modificare l'app Web per caricare immagini

L'app Web recupera le impostazioni da un file denominato **settings.js**. Nei passaggi seguenti viene creato il file usando Cloud Shell. Impostare `window.apiBaseUrl` sull'URL dell'app Funzioni e `window.blobBaseUrl` sull'URL dell'endpoint di archiviazione BLOB di Azure.

1. In Cloud Shell assicurarsi che la directory corrente sia la cartella **www/dist**.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Eseguire una query sull'URL dell'app Funzioni e memorizzarlo in una variabile Bash denominata **FUNCTION_APP_URL**.

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    Verificare che la variabile sia impostata correttamente.

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. Per impostare l'URI base delle chiamate dell'API alle app Funzioni, creare il file **settings.js**. Aggiungere l'URL dell'app Funzioni, come nell'esempio seguente:

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    È possibile apportare la modifica eseguendo il comando seguente o usando un editor da riga di comando come VIM.

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    Verificare che il file sia stato scritto correttamente.

    ```azurecli
    cat settings.js
    ```

1. Eseguire una query sull'URL di base di archiviazione BLOB e memorizzarlo in una variabile Bash denominata **BLOB_BASE_URL**.

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    Verificare che la variabile sia impostata correttamente.

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. Per impostare l'URI di base delle chiamate API all'app Funzioni, aggiungere l'URL di archiviazione BLOB al file **settings.js** come nell'esempio seguente:

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    È possibile apportare la modifica eseguendo il comando seguente o usando un editor da riga di comando come VIM.

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    Verificare che il file sia stato scritto correttamente e che ora contenga due righe.

    ```azurecli
    cat settings.js
    ```

1. Caricare il file nell'archiviazione BLOB.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a>Testare l'applicazione Web

A questo punto, l'applicazione della raccolta può caricare un'immagine nell'archiviazione BLOB, ma non può ancora visualizzare le immagini. Tenterà di chiamare una funzione `GetImages` che non esiste ancora perché verrà creata in un modulo successivo. La chiamata avrà esito negativo e la pagina Web apparirà bloccata su "Analisi", ma l'immagine selezionata verrà caricata correttamente.

È possibile verificare che un'immagine sia stata caricata correttamente controllando il contenuto del contenitore **images** nel portale di Azure.

1. In una finestra del browser passare all'applicazione. Selezionare un file di immagine e caricarlo. Il file viene caricato ma, poiché non è stata ancora aggiunta la possibilità di visualizzare le immagini, la foto caricata non viene visualizzata nell'app. La pagina Web appare bloccata su "Analyzing image..." (Analisi immagine in corso...). Questo problema verrà risolto in seguito.

1. In Cloud Shell verificare che l'immagine sia stata caricata nel contenitore **images**.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Prima di passare all'esercitazione successiva, eliminare tutti i file nel contenitore **images**.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```

## <a name="summary"></a>Riepilogo

In questa unità è stata creata un'app Funzioni di Azure ed è stato descritto come usare una funzione serverless per consentire a un'applicazione Web di caricare immagini nell'archiviazione BLOB. Si osserverà ora come creare anteprime per le immagini caricate usando una funzione serverless attivata dal BLOB.