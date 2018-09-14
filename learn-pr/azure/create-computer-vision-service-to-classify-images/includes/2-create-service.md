
## <a name="what-is-microsoft-cognitive-services"></a>Informazioni sui Servizi cognitivi Microsoft

Servizi cognitivi Microsoft è un set di algoritmi disponibili come servizio a chiunque di usare machine learning. Invece di creare questa intelligence per le app da zero, è possibile usare questi servizi per visione, sintesi vocale, linguaggio, conoscenza e ricerca. È possibile provare gratuitamente ogni servizio. Quando si decide di integrare un servizio nell'app, iscriversi per una sottoscrizione a pagamento. Ci concentreremo l'attenzione su API visione artificiale in questo modello.

> [!TIP]
> Per visualizzare un elenco di tutti i servizi cognitivi, consultare il [Directory di servizi cognitivi](https://azure.microsoft.com/services/cognitive-services/directory/). 

## <a name="what-is-the-computer-vision-api"></a>Che cos'è l'API visione artificiale?

L'API visione artificiale fornisce gli algoritmi per l'elaborazione di immagini e restituiscono informazioni dettagliate. Ad esempio, è possibile scoprire se un'immagine ha contenuto per adulti o usarlo per trovare tutti i visi in un'immagine. Include anche altre funzionalità come la stima dei colori dominante e distinzione tra caratteri accentati, classificare il contenuto delle immagini e descrivere un'immagine con frasi complete in lingua inglese. Inoltre, può generare inoltre in modo intelligente le anteprime delle immagini per la visualizzazione di immagini di grandi dimensioni in modo efficace.

> [!TIP]
> L'API visione artificiale è disponibile in molte aree in tutto il mondo. Per trovare l'area più vicina, vedere la [prodotti disponibili in base all'area](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

È possibile usare l'API visione artificiale per:

- Analizza le immagini per l'analisi
- Estrai testo stampato dalle immagini usando il riconoscimento ottico dei caratteri (OCR).
- Riconosci il testo stampato e scritte a mano dalle immagini
- Riconosci le celebrità e punti di riferimento
- Analizza i video 
- Genera un'anteprima di un'immagine 

## <a name="how-to-call-the-computer-vision-api"></a>Come chiamare l'API visione artificiale

Visione artificiale è chiamare nell'applicazione usando direttamente le librerie client o l'API REST. In questo modulo verrà chiamato l'API REST. Per effettuare una chiamata:

1. Ottenere una chiave di accesso API

    Le chiavi di accesso è stato assegnato quando effettua l'iscrizione per un account del servizio visione artificiale. Una chiave deve essere passata nell'intestazione della **ogni** richiesta. 

1. Effettuare una chiamata POST all'API

    Formattare l'URL come indicato di seguito: **regione**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parametri]** 

    - **area geografica** -l'area in cui è stato creato l'account, ad esempio, *westus*.
    - **Resource** -si sta chiamando, ad esempio la risorsa di visione artificiale `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.

    È possibile specificare l'immagine da elaborare come URL di un'immagine o un file binario di immagine non elaborata.

    L'intestazione della richiesta deve contenere la chiave di sottoscrizione, che fornisce l'accesso a questa API.

1. Analizzare la risposta

    La risposta contiene le informazioni dettagliate che l'API visione artificiale avevo l'immagine come un payload JSON.

Si supponga che creato la sottoscrizione di visione artificiale nel `westus` area e ha ottenuto una chiave di sottoscrizione per accedere all'API. Assegnare quindi la chiave di un valore fittizio di `xxxx-xxxxx-xxxx-xxxx`. Ora si desidera generare un'anteprima per un'immagine nel percorso `http://example.com/images/test.jpg`. Usando `generateThumbnail`, la richiesta sarebbe simile al seguente.

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

In questo modulo, si eseguirà tutti gli esercizi la CLI di Azure usando Cloud Shell integrata. È possibile trovare out un po' più su questo programma di installazione.

## <a name="what-is-the-azure-cli"></a>Che cos'è la CLI di Azure

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> Attualmente sono disponibili due versioni dello strumento di interfaccia della riga di comando di Azure: 1.0 e 2.0. In questa unità si userà l'interfaccia della riga di comando di Azure 2.0, ovvero la versione più recente e consigliata, a meno che non si debbano eseguire script legacy. La versione 1.0 dell'interfaccia della riga di comando di Azure viene avviata con il comando `azure`, mentre la versione 2.0 viene avviata con il comando `az`.

## <a name="az-cognitiveservices-commands"></a>comandi AZ cognitiveservices

Il comando di Azure include il `cognitiveservices` comandi per gestire gli account di servizi cognitivi in Azure. È possibile fornire vari sottocomandi per eseguire attività specifiche. Quelli più comuni includono:

| Sottocomando | Descrizione |
|-------------|-------------|
| `list` | Elenco dei account servizi cognitivi di Azure disponibili. |
| `account show` | Ottenere i dettagli di un account servizi cognitivi di Azure. |
| `account create` | Creare un account servizi cognitivi di Azure. |
| `account delete` | Eliminare un account servizi cognitivi di Azure. |
| `keys list` | Elencare le chiavi di un account servizi cognitivi di Azure. |

Creiamo un servizi cognitivi con la CLI di Azure.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a>Creare un account Servizi cognitivi

È necessaria una chiave di accesso API per eseguire chiamate all'API visione artificiale. Per ottenere le chiavi di accesso, è necessario un account servizi cognitivi per l'API visione artificiale. Si userà `az cognitiveservices create` per creare l'account nella sottoscrizione.

 Il comando `az cognitiveservices create` viene usato per creare un account servizi cognitivi in un gruppo di risorse.  Quando si chiama questo comando, è necessario specificare i seguenti cinque parametri.

> [!Tip]
> La maggior parte dei flag per i parametri della riga di comando di Azure può essere abbreviato utilizzando un singolo carattere. Ad esempio, è possibile affermare `-l` invece di `--location`. La forma estesa viene utilizzata per maggiore chiarezza.

| Parametro | Descrizione |
|-----------|-------------|
| `resource-group` | Il gruppo di risorse che conterrà l'account servizi cognitivi. In questa sessione sandbox interattiva, si userà <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> |
| `kind` | Il nome dell'API dell'account servizi cognitivi. |
| `location` | Il percorso, o area, da cui si desidera effettuare chiamate a questa API. |
| `name` | Nome dell'account servizi cognitivi. |
| `sku` | Lo Sku dell'account servizi cognitivi.|

1. Eseguire il comando seguente in Azure Cloud Shell:

> [!Tip]
> I comandi di CLI di Azure in questo modulo vengono scritti come comandi su più righe per ridurre la quantità di scorrimento orizzontale. Si è liberi di convertire i comandi seguenti a riga singola comandi se si desidera. 

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--location westus2 \
--name ComputerVisionService \
--sku F0 \
--resource-group <rgn>[Sandbox resource group name]</rgn>
```

In questo caso viene creato un account servizi cognitivi per il **ComputerVision** API. È stato selezionato lo sku gratuito *F0* e denominate conto **ComputerVisionService**. L'account appartenga al gruppo di risorse **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>** ed è possibile chiamare l'API dal percorso impostato nel `--location` parametro. In questo esempio, è possibile effettuare chiamate da westus2. Chiamate da qualsiasi altra area verrà generato un errore.

Dopo il completamento del comando la creazione dell'account servizi cognitivi, si otterrà una risposta JSON, che include **provisioningState** impostata su **Succeeded**. Ecco un esempio di come appare una risposta con esito positivo.

<!-- TODO: find out the default location! -->

```json
{
  "endpoint": "https://westus2.api.cognitive.microsoft.com/vision/v2.0",
  "etag": "\"5c0070e5-0000-0000-0000-5b98cafa0000\"",
  "id": "/subscriptions/ae49d8aa-de86-418a-ba8c-4e9f3add2620/resourceGroups/ComputerVisionRG/providers/Microsoft.CognitiveServices/accounts/ComputerVisionService",
  "internalId": "437c9519bead47e6ba4b8e0842c1f76f",
  "kind": "ComputerVision",
  "location": "westus2",
  "name": "ComputerVisionService",
  "provisioningState": "Succeeded",
  "resourceGroup": "ComputerVisionRG",
  "sku": {
    "name": "F0",
    "tier": null
  },
  "tags": null,
  "type": "Microsoft.CognitiveServices/accounts"
}
```

> [!NOTE]
> Se la sottoscrizione di Azure già dispone di un account servizi cognitivi di visione artificiale impostato con il **F0** Sku prima di eseguire questo comando, verrà visualizzato il messaggio seguente:
>
> `Only one free account is allowed for account type 'ComputerVision'` 
>
> Si deve usare tale account per questo modulo oppure cambiare lo Sku nel `az cognitiveservices account create` al comando *S1* o un altro Sku.

## <a name="get-an-access-key"></a>Ottenere una chiave di accesso

Dopo aver ottenuto l'account è stato creato è possibile recuperare le chiavi di sottoscrizione o le chiavi, per questo account di accesso.

1. Eseguire il comando seguente in Azure Cloud Shell:

```azurecli
az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn>
```

Il comando precedente restituisce le chiavi associate all'account servizi cognitivi chiamato **ComputerVisionService**, che è appartengono al gruppo di risorse specificato. Restituisce due chiavi: una è una chiave di riserva. Le chiavi sono difficili da ricordare, in modo che la prima chiave verrà archiviato in una variabile che si userà per tutte le chiamate all'API.

2.  Eseguire il comando seguente in Azure Cloud Shell:

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--query key1 -o tsv)
```

Riga di comando Azure 2.0 usa i `--query` argomento per eseguire una query JMESPath sui risultati dei comandi. JMESPath è un linguaggio di query per JSON che offre la possibilità di selezionare e presentare i dati dell'output dell'interfaccia della riga di comando. Queste query vengono eseguite sull'output JSON prima di qualsiasi formattazione visualizzata.
L'argomento `--query` è supportato da tutti i comandi dell'interfaccia della riga di comando di Azure. 

Nel nostro esempio, viene eseguita una query di elenco di chiavi per una voce denominata "key1" e il risultato di output **tsv** formato. Questo formato consente di rimuovere le offerte di racchiudere il valore di stringa. Il risultato viene assegnato a una variabile **chiave**.

> [!IMPORTANT]
> Si userà questa chiave in tutto il modulo, quindi salvarlo in una variabile è una buona idea. Se si perde il valore o la variabile diventa non impostata, eseguire nuovamente il comando per impostarla.  

3. Per visualizzare il valore della chiave, eseguire il comando seguente in Azure Cloud Shell:

```azurecli
echo $key
```

Ora che abbiamo un account e una chiave, è possibile effettuare alcune chiamate all'API.