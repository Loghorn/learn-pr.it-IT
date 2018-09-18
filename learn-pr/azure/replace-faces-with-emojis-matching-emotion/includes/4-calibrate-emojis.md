Non è possibile passare un'immagine dell'emoji all'API Viso per ottenerne l'emozione, in quanto non è un'immagine umana. Di conseguenza, per ogni emoji è stato necessario un proxy umano, ovvero l'autore stesso di questo articolo.

L'autore ha scattato immagini di se stesso in modo _accurato_, simulando ogni emoji, e ha usato il _punto emotivo_ per l'immagine come proxy per l'emoji. Per rendere ancora più interessante l'esperimento, l'autore ha scelto anche persone del suo team, associando anche loro a emoji, come mostrato qui:

![Emoji del team](/media-drafts/team.jpg)

Per l'emoji degli occhi innamorati (😍), l'autore ha scelto un'immagine della moglie ❤️. In memoria di [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking), è stata selezionata un'immagine dello scienziato per rappresentare 🤔.

L'elenco delle immagini proxy per ogni emoji è disponibile nella cartella `bin/proxy-images` nel codice di esempio associato a questa esercitazione.

In questo capitolo si genererà una chiave in modo da poter usare l'API Viso di Azure per calibrare tutte le emoji usando immagini proxy dell'autore.

## <a name="generate-an-azure-face-api-key"></a>Generare una chiave API Viso di Azure

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

Per usare l'API Viso di Azure, è necessaria una chiave di autenticazione speciale. Vedere https://azure.microsoft.com/try/cognitive-services/ e registrarsi per ottenere la versione di valutazione dell'API Viso.

![Emoji del team](/media-drafts/4.calibrating-emojis.get-face-api.png)

> TODO: Trovare i comandi az per creare l'API Viso e recuperare le chiavi

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## <a name="setup-the-environment-variables"></a>Configurare le variabili di ambiente

Lo script di calibrazione deve avere l'URL e la chiave dell'API Viso per poter effettuare le chiamate corrette. Invece di impostarli come hardcoded nello script, verranno usate variabili di ambiente. Eseguire questi comandi nel terminale in cui si prevede di eseguire l'applicazione:

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## <a name="create-some-proxy-images-for-emojis"></a>Creare alcune immagini proxy per le emoji

Benché le immagini proxy vengano già fornite qui, è possibile generarne di personalizzate.

Per ogni emoji nella cartella `bin/proxy-images`, acquisire un'immagine di se stessi simulando l'emoji e sostituire l'immagine con la propria.

## <a name="try-it-out"></a>Provare questa operazione

Questa è la parte più divertente! Ognuna delle immagini verrà eseguita in `bin/proxy-images` tramite l'API Viso per calcolare un punto emotivo per l'emoji nello _spazio emotivo_. A questo scopo, eseguire:

```bash
node bin/calibrate.js
```

L'output di questo comando sarà simile al seguente:

```json
...
Processing 🤓
Processing 🤔
Processing 🦄
Processing 😃
Processing 😆
...
{
    emotiveValues: new EmotivePoint({
        anger: 0,
        contempt: 0.005,
        disgust: 0.001,
        fear: 0,
        happiness: 0,
        neutral: 0.922,
        sadness: 0.071,
        surprise: 0
    }),
    emojiIcon: "😴"
}
```

Verrà innanzitutto stampata l'emoji elaborata e infine verrà stampata nella console una matrice che definisce i valori della classe `EmotivePoint` di tutte l'emoji. Questa matrice ha lo stesso formato di quella in `shared/mojis.ts`.

Se alcune delle immagini proxy sono state modificate, copiare l'output di questo script nella sezione pertinente di `mojis.ts`
