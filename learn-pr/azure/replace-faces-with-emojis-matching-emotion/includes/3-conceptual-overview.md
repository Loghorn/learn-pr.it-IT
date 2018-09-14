Prima è approfondire le implementazioni, è possibile rispondere prima di tutto alcune domande di livello superiore.

## <a name="how-to-calculate-the-emotion-of-a-face"></a>Come per la quale calcolare l'emozione di un viso?

Il calcolo delle emozioni sono una delle parti più accessibile dell'applicazione. Usiamo il [FaceAPI](https://azure.microsoft.com/services/cognitive-services/face/), parte dell'offerta di servizi cognitivi di Azure.

il FaceAPI accetta come input un'immagine e restituisce informazioni sull'immagine, tra cui se rilevata qualsiasi visi, le posizioni dei volti nell'immagine e se richiesto, consente di calcolare e restituire le emozioni delle facce, nel modo seguente:

```json
{
  "anger": 0.572,
  "contempt": 0.025,
  "disgust": 0.242,
  "fear": 0.001,
  "happiness": 0.014,
  "neutral": 0.111,
  "sadness": 0.033,
  "surprise": 0.003
}
```

Consideriamo ad esempio questa immagine:

![Esempio viso](/media-drafts/example-face.jpg)

Per elaborare questa immagine, si potrebbe eseguire una richiesta POST a un endpoint API simile al seguente:

https://xxxxxxxxx.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion

Offriamo l'immagine nel corpo come illustrato di seguito:

```json
{
  "url": "<path-to-image>"
}
```

> **Nota**
>
> L'API per impostazione predefinita non restituisce le emozioni, è necessario specificare in modo esplicito il parametro di query `returnFaceAttributes=emotion`

L'uso di una chiave privata autentica dell'API. è necessario inviare questa chiave con l'intestazione.

```
Ocp-Apim-Subscription-Key: <your-subscription-key>
```

L'API con i parametri di query precedente restituisce un oggetto JSON come illustrato di seguito:

```json
[
  {
    "faceRectangle": {
      "top": 207,
      "left": 198,
      "width": 229,
      "height": 229
    },
    "faceAttributes": {
      "emotion": {
        "anger": 0.001,
        "contempt": 0.014,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.306,
        "neutral": 0.675,
        "sadness": 0.003,
        "surprise": 0.001
      }
    }
  }
]
```

Restituisce una matrice di risultati, uno per ogni volto rilevato nell'immagine. Per ogni viso, restituisce il dimensione o il percorso del volto come `faceRectangle` e le emozioni rappresentate sotto forma di numero da 0 a 1 come `faceAttributes`.

> **Suggerimento**
>
> Iniziare a esplorare le funzionalità con servizi cognitivi persino _senza_ disporre di un account Azure, passare alla [questa pagina](https://azure.microsoft.com/try/cognitive-services/?api=face-api&WT.mc_id=mojifier-sandbox-ashussai) e immettere l'indirizzo di posta elettronica per ottenere l'accesso gratuito.

## <a name="how-to-map-an-emotion-to-an-emoji"></a>Come eseguire il mapping di un'emozione per un emoji?

Si immagini che solo due emozioni, paura e felicità, con valori compresi tra 0 e 1. Quindi ogni viso potrebbe essere tracciato in un 2D _emotivo spazio_ base le emozioni dell'utente, come illustrato di seguito:

![Distanza euclidea 1](/media-drafts/graph-1.jpg)

Si supponga quindi che abbiamo anche capito il punto per ogni emoji emotivo e tracciata quelli presenti anche lo spazio 2D emotivo. Quindi, se si calcola la distanza tra i visi e tutti gli altri emoji nello spazio 2D emotivo è possibile scoprire l'emoji più vicina per l'emozione, come illustrato di seguito:

![Distanza euclidea 2](/media-drafts/graph-2.png)

Questo calcolo viene chiamato il `euclidian distance`, e questo è ciò che è stato usato, ma non in 2D emotivo spazio in uno spazio emotivo 8d con (rabbia, biasimo, disgusto, paura, felicità, neutro, tristezza, sorpresa).

> **Suggerimento**
>
> Per rendere più semplice è usato il pacchetto npm chiamato distanza euclidea <https://www.npmjs.com/package/euclidean-distance>.

## <a name="shared-code"></a>Codice condiviso

Il progetto iniziale di esempio include una cartella condivisa con il codice che gestisce già molti i casi di utilizzo precedenti.

### <a name="emotivepoint"></a>EmotivePoint

Se si esamina con attenzione il `EmotivePoint` classe `shared/emmotive-point.ts` si noteranno alcune cose.

Il costruttore accetta come input un oggetto contenente informazioni emotive e archivi come variabili membro locale, nel modo seguente:

```typescript
 constructor({
    anger,
    contempt,
    disgust,
    fear,
    happiness,
    neutral,
    sadness,
    surprise
  }) {
    this.anger = anger;
    this.contempt = contempt;
    this.disgust = disgust;
    this.fear = fear;
    this.happiness = happiness;
    this.neutral = neutral;
    this.sadness = sadness;
    this.surprise = surprise;
  }
```

Include anche una funzione denominata distanza che è possibile usare per calcolare la distanza euclidea tra due punti emotive, come illustrato di seguito:

```typescript
  distance(other) {
    let myPoint = this.toArray();
    let otherPoint = other.toArray();
    return distance(myPoint, otherPoint);
  }
```

Pertanto, possiamo creare due punti emotive e calcolare quanto in questo modo:

```typescript
let a = new EmotivePoint({
  /* ... */
});
let b = new EmotivePoint({
  /* ... */
});
let distance = a.distance(b);
```

### <a name="face"></a>Viso

Un'altra classe helper è il `Face` classe; ciò combina alcune proprietà diverse tra cui la `EmotivePoint` di un viso, nonché il rettangolo che definisce il viso nell'immagine; si usa il `Rect` la classe.

Se si esamina con attenzione il `Face` costruttore di classe in `shared/face.ts` qui noterete la seguente riga di codice:

```typescript
this.moji = this.chooseMoji(this.emotivePoint);
```

`emotivePoint` è il punto emotive della faccia di se stesso.
`chooseMoji` Restituisce un appropriato emoji basata emotivePoint della faccia.

```typescript
  chooseMoji(point) {
    let closestMoji = null;
    let closestDistance = Number.MAX_VALUE;
    for (let moji of MOJIS) {
      let emoPoint = moji.emotiveValues;
      let distance = emoPoint.distance(point);
      if (distance < closestDistance) {
        closestMoji = moji;
        closestDistance = distance;
      }
    }
    return closestMoji;
  }
```

`MOJIS` è l'elenco di punti di emotive per tutti gli emoji, illustreremo come generare questi nella lezione successiva.

Il `chooseMoji` funzione calcola la distanza tra il viso e tutti gli emoji, restituendo il più vicino.

# <a name="summary"></a>Riepilogo

Per ogni emoji viene calcolato un punto _emotivo_ spazio, detta _calibrazione_ e questo aspetto illustrato nel capitolo successivo.

Usando l'API viso di Azure, si ottiene un elenco di visi nelle immagini con il punto emotiva di ciascuna faccia. Quindi con l'algoritmo di distanza euclidea per trovare l'emoji più vicino a ogni faccia.
