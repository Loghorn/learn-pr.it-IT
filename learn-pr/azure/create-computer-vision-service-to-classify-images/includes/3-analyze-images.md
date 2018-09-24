In qualità di sviluppatore responsabile presso Contoso Beverage Distribution, l'utente è incaricato della compilazione e gestione di un'app line-of-business che consente ai distributori di scansionare e caricare le immagini degli scaffali del magazzino nei quali si stanno rinnovando le scorte. 

È necessario verificare che le immagini inviate dagli utenti rispettino le regole aziendali stabilite per i contenuti. L'azienda non vuole che nei siti aziendali vengano inviati contenuti inappropriati. Dati i miglioramenti in materia di intelligenza artificiale, si decide di usare l'API Visione artificiale nell'app. 

Per iniziare, creare un account di Visione artificiale nella sottoscrizione di Azure e avviare il test della funzionalità di **analisi**.

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>Chiamata dell'API Visione artificiale per analizzare le immagini

L'operazione `analyze` estrae una vasta gamma di caratteristiche visive in base al contenuto delle immagini. L'URL della richiesta si presenta nel formato seguente:

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

I parametri inviati al servizio sono `visualFeatures`, `details` e `languages`. Impostare il parametro `details` su `Landmarks` o `Celebrities` per identificare luoghi di interesse o celebrità. `visualFeatures` identifica la tipologia di informazioni che deve essere restituita dal servizio. L'opzione `Categories` classifica il contenuto delle immagini, ad esempio alberi, edifici e così via. `Faces` identifica i visi delle persone e offre indicazioni su sesso ed età.

Tutte le operazioni nell'API Visione artificiale presentano le restrizioni seguenti relative alle immagini di cui si richiede l'elaborazione:

- Formati di immagine supportati: JPEG, PNG, GIF, BMP. 
- Le dimensioni del file di immagine devono essere minori di 4 MB.
- Le dimensioni dell'immagine devono essere almeno 50 x 50.

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>Identificare i punti di riferimento in un'immagine

Si inizierà con una chiamata che consente di trovare i punti di riferimento in un'immagine. Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con URL ad altre immagini. 

![Immagine di una catena montuosa con cielo blu sopra alle cime innevate.](../media/3-mountains.jpg)

Eseguire il comando seguente in Azure Cloud Shell. Sostituire `<region>` nel comando con l'area dell'account di Servizi cognitivi.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- Questa chiamata ricerca punti di riferimento nell'immagine specificata dall'URL dell'immagine. Per questo esercizio, l'immagine analizzata viene archiviata in un repository di GitHub. 
- La chiamata chiede anche al servizio di restituire le informazioni sulla categoria e una descrizione dell'immagine. La descrizione viene restituita come una frase inglese completa. 
- Come saprete, ogni chiamata all'API richiede una chiave di accesso, che viene impostata nell'intestazione `Ocp-Apim-Subscription-Key` della richiesta. 

> [!TIP]
> Il risultato della richiesta è costituito dal file JSON non elaborato che descrive l'immagine in `url`. È stato aggiunto ` | jq '.'` alla fine del comando per migliorare l'output del file JSON.

La risposta JSON da questa chiamata restituisce quanto segue:

- Una matrice `categories` in cui sono elencate tutte le categorie di immagini rilevate, insieme a un punteggio compreso tra 0 e 1 che indica il livello di affidabilità dell'appartenenza dell'immagine alla categoria specificata.
- Una voce `description` contenente una matrice di tag o parole correlate all'immagine.
- Una voce `captions` con un campo di testo che descrive in inglese il contenuto dell'immagine. Si noti che il testo ha anche un punteggio di certezza. Questo punteggio può essere utile per decidere come procedere con l'analisi.


## <a name="check-for-inappropriate-content-in-an-image"></a>Verificare la presenza di contenuti inappropriati in un'immagine

In questo esempio verrà analizzata un'immagine con contenuto per adulti. Il punteggio di attendibilità classifica le probabilità che nell'immagine sia presente contenuto per adulti o spinto. 

Per questo esempio si userà l'immagine seguente, ma è possibile provare lo stesso comando con URL ad altre immagini. 

![Immagine di una famiglia sorridente.](../media/3-people.png)

1. Eseguire il comando seguente in Azure Cloud Shell, sostituendo `<region>` nell'URL.

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

In questo esempio `visualFeatures` è stato impostato su `Adult,Description`. 

La risposta offre due punteggi di attendibilità, uno per il contenuto spinto e uno per il contenuto per adulti. Usando questi punteggi insieme alla descrizione dell'immagine e altre caratteristiche visive è possibile iniziare a contrassegnare le immagini inviate nel server.

Gli esempi contenuti in questo esercizio danno un'idea del tipo di analisi che è possibile eseguire con l'operazione `analyze`. Provare le analisi con immagini di diverse e diverse combinazioni di parametri visualFeatures, dettagli e linguaggi.

Per altre informazioni sull'operazione `analyze`, vedere la documentazione di riferimento relativa all'[analisi dell'immagine](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).