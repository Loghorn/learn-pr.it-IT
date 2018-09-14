Come uno sviluppatore di lead presso Contoso bevande distribuzione, ha la responsabilità per la creazione e gestione di un'app line-of-business che consente la distribuzione nel sistema dalla scansione e caricare le immagini o sugli scaffali archivio in cui essi sono re-immagazzinamento. 

Si desidera convalidare che le immagini inviate dagli utenti rispettino le regole di contenuto aziendali stabilite. L'azienda non vuole contenuti inappropriati inseriti nei siti aziendali. Dato i miglioramenti di intelligenza artificiale, si decide di usare l'API visione artificiale nell'app. 

Per iniziare, creare un account di visione artificiale nella sottoscrizione di Azure e avviare il test di **analizzare** funzionalità.

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>Chiamare l'API visione artificiale per analizzare le immagini

Il `analyze` operazione estrae un set completo di caratteristiche visive sulla base del contenuto di immagine.  L'URL della richiesta ha il formato seguente:

`https://[location].api.cognitive.microsoft.com/vision/v2.0/analyze[?visualFeatures][&details][&language] `

I parametri inviati al servizio sono `visualFeatures`, `details` e `languages`. Impostare il `details` parametro per `Landmarks` o `Celebrities` che consentono di identificare i punti di riferimento o celebrità. `visualFeatures` identifica la tipologia di informazioni che deve essere restituita dal servizio. Il `Categories` opzione verrà suddivisi in categorie il contenuto delle immagini, ad esempio alberi, edifici e altro ancora. `Faces` identifica i visi delle persone e fornisce indicazioni su sesso ed età.

Se si desidera le immagini che si impostarla per elaborare, tutte le operazioni API visione artificiale presentano le restrizioni seguenti:

- Formati di immagine supportati: JPEG, PNG, GIF, BMP. 
- Dimensioni file di immagine devono essere inferiori a 4 MB.
- Le dimensioni dell'immagine devono essere almeno 50 x 50.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>Identificare i punti di riferimento in un'immagine

Si inizierà con una chiamata a trovare i punti di riferimento in un'immagine. Si userà l'immagine seguente per questo esempio, ma è possibile ripetere lo stesso comando con gli URL per le altre immagini. 

![Immagine di un intervallo di mountain con tempo sereno blu sopra la vasta mole snow-capped.](../media/3-mountains.jpg)

1. Eseguire il comando seguente in Azure Cloud Shell:

<!-- TODO Replace image URL with one that points to an image in the sample repo -->
```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

Questa chiamata ricerca di punti di riferimento nell'immagine specificata per l'URL dell'immagine. Inoltre, la chiamata chiede al servizio di restituire le informazioni sulla categoria e una descrizione dell'immagine. La descrizione viene restituita come una frase inglese completa. Come saprete, ogni chiamata all'API richiede una chiave di accesso. È impostato in ogni chiamata con il `Ocp-Apim-Subscription-Key` intestazione. 

> [!TIP]
> Il risultato della richiesta è costituito dal file JSON non elaborato che descrive l'immagine in `url`. È stato aggiunto il ` | jq '.'` alla fine del comando al miglioramento delle JSON di output.

Restituisce la risposta JSON in questo esempio:

- Oggetto `categories` matrice Elenca tutte le categorie di immagini che sono state rilevate, insieme a un punteggio compreso tra 0 e 1 di quanta fiducia il servizio è che l'immagine appartenga nella categoria specificata.
- Oggetto `description` voce contenente una matrice di tag o parole correlate all'immagine.
- Oggetto `captions` voce con un campo di testo che descrive in inglese le novità nell'immagine. Osservare che il testo ha anche un punteggio di confidenza. Questo punteggio può aiutarti a decidere cosa fare successivamente con questa analisi.


## <a name="check-for-inappropriate-content-in-an-image"></a>Verificare la presenza di contenuti inappropriati in un'immagine

In questo esempio verrà analizzata un'immagine per il contenuto per adulti. Il punteggio di confidenza classifica le probabilità che l'immagine contiene il contenuto per adulti o spinti. 

Si userà l'immagine seguente per questo esempio, ma è possibile ripetere lo stesso comando con gli URL per le altre immagini. 

![Immagine di una famiglia sorridente.](../media/3-people.png)

1. Eseguire il comando seguente in Azure Cloud Shell:

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

In questo esempio viene impostato il `visualFeatures` a `Adult,Description`. La risposta ci offre due punteggi di confidenza, uno per il contenuto per adulti e uno per il contenuto per adulti. Usando questi punteggi e la descrizione dell'immagine e altre funzionalità di visual, è possibile iniziare a flag immagini registrate nel server.

Gli esempi si è provato in questo esercizio dare un'idea del tipo di analisi che è possibile eseguire con il `analyze` operazione. Provare le analisi con le immagini di diversi e diverse combinazioni di parametri visualFeatures, dettagli e linguaggi.

Per altre informazioni sul `analyze` operazione, vedere la [analizzare immagine](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) documentazione di riferimento.