
## <a name="what-is-microsoft-cognitive-services"></a>Informazioni sui Servizi cognitivi Microsoft

Servizi cognitivi Microsoft è un set di algoritmi di apprendimento automatico disponibile come servizio che chiunque può usare. Invece di creare da zero questa funzionalità di intelligence per le app, è possibile usare questi servizi per visione artificiale, sintesi vocale, linguaggio, conoscenza e ricerca. È possibile provare gratuitamente ogni servizio. Quando si decide di integrare un servizio nelle proprie app, è necessario registrarsi per una sottoscrizione a pagamento. In questo modello si approfondirà l'API Visione artificiale.

> [!TIP]
> Per visualizzare un elenco di tutti i Servizi cognitivi, vedere la [Directory di Servizi cognitivi](https://azure.microsoft.com/services/cognitive-services/directory/). 

## <a name="what-is-the-computer-vision-api"></a>Che cos'è l'API Visione artificiale?

L'API Visione artificiale fornisce algoritmi per elaborare le immagini e restituire informazioni dettagliate. È ad esempio possibile usarla per verificare se un'immagine include contenuto per soli adulti o per trovare tutti i visi in un'immagine. Offre anche altre funzionalità, ad esempio l'identificazione del colore dominante e di quelli in primo piano, la classificazione del contenuto delle immagini e la descrizione di un'immagine con frasi complete in lingua inglese. Può anche generare anteprime intelligenti delle immagini per visualizzare le immagini di grandi dimensioni in modo efficace.

> [!TIP]
> L'API Visione artificiale è disponibile in molte aree in tutto il mondo. Per trovare l'area più vicina, vedere [Prodotti disponibili in base all'area](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

È possibile usare l'API Visione artificiale per:

- Analizzare le immagini per ottenere informazioni dettagliate
- Estrarre il testo stampato dalle immagini usando il riconoscimento ottico dei caratteri (OCR).
- Riconoscere il testo stampato e scritto a mano nelle immagini
- Riconoscere celebrità e luoghi di interesse
- Analizzare video 
- Generare un'anteprima di un'immagine 

## <a name="how-to-call-the-computer-vision-api"></a>Come chiamare l'API Visione artificiale

Per chiamare Visione artificiale nell'applicazione, usare direttamente le librerie client dell'API REST. In questo modulo si chiamerà l'API REST. Per effettuare una chiamata:

1. Ottenere una chiave di accesso all'API

    Le chiavi di accesso vengono assegnate quando ci si iscrive a un account del servizio Visione artificiale. Una chiave deve essere passata nell'intestazione di **ogni** richiesta. 

1. Effettuare una chiamata POST all'API

    Formattare l'URL come segue: **area**.api.cognitive.microsoft.com/vision/v2.0/**risorsa**/**[parametri]** 

    - **area**: area in cui è stato creato l'account, ad esempio *westus*.
    - **risorsa**: risorsa di Visione artificiale da chiamare, ad esempio `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.

    È possibile specificare l'immagine da elaborare come file binario dell'immagine RAW o come URL dell'immagine.

    L'intestazione della richiesta deve contenere la chiave di sottoscrizione, che fornisce l'accesso a questa API.

1. Analizzare la risposta

    La risposta contiene le informazioni dettagliate che l'API Visione artificiale ha raccolto sull'immagine come payload JSON.

Si supponga di aver creato la sottoscrizione di Visione artificiale nell'area `westus` e di aver ottenuto una chiave di sottoscrizione per accedere all'API. Assegnare alla chiave il valore fittizio `xxxx-xxxxx-xxxx-xxxx`. Si vuole ora generare un'anteprima per un'immagine che si trova in `http://example.com/images/test.jpg`. Usando `generateThumbnail`, la richiesta sarà simile alla seguente.

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

In questo modulo si eseguiranno tutti gli esercizi nell'interfaccia della riga di comando di Azure usando Cloud Shell integrato. Ecco alcune informazioni aggiuntive su questa configurazione.

## <a name="what-is-the-azure-cli"></a>Che cos'è l'interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> Sono attualmente disponibili due versioni dell'interfaccia della riga di comando di Azure: 1.0 e 2.0. In questa unità si userà l'interfaccia della riga di comando di Azure 2.0, ovvero la versione più recente e consigliata, a meno che non si debbano eseguire script legacy. La versione 1.0 dell'interfaccia della riga di comando di Azure viene avviata con il comando `azure`, mentre la versione 2.0 viene avviata con il comando `az`.

## <a name="az-cognitiveservices-commands"></a>Comandi az cognitiveservices

L'interfaccia della riga di comando di Azure include il comando `cognitiveservices` per gestire gli account di Servizi cognitivi in Azure. È possibile fornire vari sottocomandi per eseguire attività specifiche. Quelli più comuni includono:

| Sottocomando | Descrizione |
|-------------|-------------|
| `list` | Elenca gli account di Servizi cognitivi di Azure disponibili. |
| `account show` | Ottiene i dettagli di un account di Servizi cognitivi di Azure. |
| `account create` | Crea un account di Servizi cognitivi di Azure. |
| `account delete` | Elimina un account di Servizi cognitivi di Azure. |
| `keys list` | Elenca le chiavi di un account di Servizi cognitivi di Azure. |

Verrà ora creata un'istanza di Servizi cognitivi con l'interfaccia della riga di comando di Azure.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Creare un account di Servizi cognitivi

È necessaria una chiave di accesso all'API per effettuare le chiamate all'API Visione artificiale. Per ottenere le chiavi di accesso, è necessario un account di Servizi cognitivi per l'API Visione artificiale. Si userà `az cognitiveservices create` per creare l'account nella sottoscrizione.

 Il comando `az cognitiveservices create` viene usato per creare un account di Servizi cognitivi in un gruppo di risorse.  Quando si chiama questo comando, devono essere specificati i cinque parametri seguenti.

> [!Tip]
> La maggior parte dei flag per i parametri dell'interfaccia della riga di comando di Azure può essere abbreviata con un carattere singolo. È ad esempio possibile usare `-l` invece di `--location`. La forma estesa viene usata per maggiore chiarezza.

| Parametro | Descrizione |
|-----------|-------------|
| `resource-group` | Gruppo di risorse che sarà il proprietario dell'account di Servizi Cognitivi. In questa sessione sandbox interattiva si userà <rgn>[Nome gruppo di risorse sandbox]</rgn> |
| `kind` | Nome dell'API dell'account di Servizi cognitivi. |
| `name` | Nome dell'account di Servizi cognitivi. |
| `sku` | SKU dell'account di Servizi cognitivi.|
| `location` | Località, ovvero area, da cui effettuare le chiamate a questa API. Selezionare una delle località nell'elenco seguente. |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

Eseguire il comando seguente in Azure Cloud Shell. Assicurarsi di sostituire `[location]` con una località nelle vicinanze.

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> Ricordare la località selezionata. Tutte le chiamate all'API verranno effettuate da tale area.

È stato creato un account di Servizi cognitivi per l'API **Visione artificiale**. È stato selezionato lo SKU *S1* e l'account è stato denominato **ComputerVisionService**. L'account è di proprietà del gruppo di risorse **<rgn>[Nome gruppo di risorse sandbox]</rgn>** e l'API verrà chiamata dalla località impostata nel parametro `--location`. 

Dopo che il comando ha completato la creazione dell'account di Servizi Cognitivi, si otterrà una risposta JSON, che include la proprietà **provisioningState** impostata su **Succeeded**.

## <a name="get-an-access-key"></a>Ottenere una chiave di accesso

Dopo aver creato l'account, è possibile recuperare le chiavi della sottoscrizione, o chiavi di accesso, per questo account.

1. Eseguire il comando seguente in Azure Cloud Shell:

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn>
    ```
    
    Il comando precedente restituisce le chiavi associate all'account Servizi Cognitivi denominato **ComputerVisionService**, di proprietà del gruppo di risorse specificato. Restituisce due chiavi, una delle quali è una chiave di riserva. Poiché è difficile ricordare le chiavi, la prima chiave verrà archiviata in una variabile da usare per tutte le chiamate all'API.

2.  Eseguire il comando seguente in Azure Cloud Shell:

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    L'interfaccia della riga di comando di Azure 2.0 usa l'argomento `--query` per eseguire una query JMESPath sui risultati dei comandi. JMESPath è un linguaggio di query per JSON che offre la possibilità di selezionare e presentare i dati dell'output dell'interfaccia della riga di comando. Le query vengono eseguite sull'output JSON prima di qualsiasi formattazione per la visualizzazione.
    L'argomento `--query` è supportato da tutti i comandi dell'interfaccia della riga di comando di Azure. 
    
    In questo esempio viene eseguita una query sull'elenco di chiavi per cercare una voce denominata "key1" e il risultato viene visualizzato in formato **tsv**. Questo formato rimuove le virgolette che racchiudono il valore della stringa. Il risultato viene assegnato a una variabile **key**.
    
    > [!IMPORTANT]
    > Poiché questa chiave verrà usata in tutto il modulo, è consigliabile salvarla in una variabile. Se si perde il valore o se la variabile viene annullata, eseguire nuovamente il comando per impostarla.  

3. Per visualizzare il valore della chiave, eseguire il comando seguente in Azure Cloud Shell:

    ```azurecli
    echo $key
    ```

Ora che sono stati creati un account e una chiave, è possibile effettuare le chiamate all'API.