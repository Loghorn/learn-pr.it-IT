Nel capitolo precedente, abbiamo appreso che `shared/mojis.ts` file include un elenco di emoji e i relativi punti emotivo.

In questo capitolo contiene informazioni sul resto del codice che utilizziamo per eseguire il mapping di un viso per un emoji.

Si apprenderà come:

1. Creare uno script dove si passano in un URL di un'immagine di un viso.
2. Calcolare il punto emotivo di qualsiasi volti nell'immagine.
3. Eseguire il mapping di volti nell'immagine per gli emoji più vicino

Alla fine si implementa questo a livello funzionale di un comando Slack, ma per ora, si userà per iniziare con uno script semplice che è possibile eseguire nella riga di comando, come segue:

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript-in-vs-code"></a>Debug TypeScript in Visual Studio Code

Viene scritta `TypeScript` ma l'esecuzione `JavaScript`; questo semplifica il debug di difficoltà che presentava è stato necessario impostare punti di interruzione ed eseguire il debug nel convertito tramite transpile in file JavaScript che può essere difficile da leggere.

In teoria è scrivere _e_ eseguire il debug in TypeScript.

La buona notizia è che è possibile eseguire con Visual Studio code con solo un po' di configurazione.

Aprire il `launch.json` file, utilizzando il comando palete <kbd>Ctrl</kbd>+<kbd>P</kbd> e sono digitando `Debug: Open launch.json`

![Aprire avvio Json](/media-drafts/5.open-debug-launch.json.png)

Verrà visualizzata la `launch.json` file di configurazione. Il file viene modificato per aggiungere e rimuovere le configurazioni di debug.

Aggiungere la seguente opzione di configurazione del debug nella matrice di impostazioni:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mojify",
  "env": {
    "DEBUG": "*"
  },
  "args": [
    "https://pbs.twimg.com/profile_images/833970306339446784/83MO53R9_400x400.jpg"
  ],
  "sourceMaps": true,
  "stopOnEntry": false,
  "console": "integratedTerminal",
  "cwd": "${workspaceRoot}",
  "program": "${workspaceRoot}/bin/mojify.js",
  "outFiles": ["${workspaceRoot}/bin/mojify.js"]
}
```

A questo punto nel menu debug, viene visualizzata un'opzione denominata `Mojify` ciò viene eseguito lo script mojify passando in un URL come argomento. È possibile impostare i punti di interruzione nel `TypeScript` file e ispezionare le variabili direttamente da questa posizione.

## <a name="open-up-binmojists"></a>Aprite `bin/mojis.ts`

Il file è già sottoposto a scaffolding con tutte le importazioni necessarie, come illustrato di seguito:

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` è un pacchetto di supporto che carica il contenuto di un file con estensione env nella radice del progetto come variabili di ambiente, utile per lo sviluppo.

`node-fetch` viene usato per effettuare richieste HTTP all'API viso di Azure.

`EmotivePoint` è una classe helper che descrive un punto nel _emotivo spazio_ -questo argomento viene illustrato in modo più dettagliato riportato di seguito.

`Rect` è una classe helper per descrivere una forma di rettangolo, useremo queste per descrivere la posizione di un viso in un'immagine.

`Face` è una classe di utilità helper che combina il rettangolo e punto emotive informazioni su un viso in un'immagine.

Al centro il file verrà visualizzato alcune funzioni stub, come illustrato di seguito:

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

Queste sono le funzioni che è essere fleshing out in questa lezione e quella successiva.

Alla fine del file, si dovrebbe visualizzare il codice:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a>Aggiungere le variabili di ambiente

Si procede a chiamare l'API viso, pertanto è necessario utilizzare tali URL viene generati prima e chiavi private, aggiungere quanto segue la parte superiore del file sotto le importazioni:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a>Chiamare l'API viso con l'immagine fornita e ottenere una risposta

Per effettuare una richiesta all'API viso, aggiungiamo questo codice all'inizio del `getFaces` (funzione)

```typescript
const fullUrl =
  API_URL +
  "/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion";

let response = await fetch(fullUrl, {
  headers: {
    "Ocp-Apim-Subscription-Key": API_KEY,
    "Content-Type": "application/json"
  },
  method: "POST",
  body: JSON.stringify({ url: imageUrl })
});
let resp = await response.json();
console.log(resp);
```

Il codice precedente Usa la `fetch` comando per inviare un `POST` richiesta all'API viso.

> **NOTA**
>
> È necessario aggiungere `/detect` e alcuni parametri di query per il API_URL per fare in modo che rileva i visi e restituire anche le emozioni.

Passiamo la `API_KEY` nell'intestazione, in modo che l'API viso possa la richiesta proviene da Microsoft; in caso contrario, la richiesta viene rifiutata.

Passiamo la `imageUrl` vogliamo l'API viso per analizzare nel corpo.

Abbiamo quindi ottenere la risposta della richiesta di API e stamparlo.

Se ora si esegue lo script con

```bash
node bin/mojify.js <path-to-image>
```

Vengono stampate le la risposta JSON restituita dal passaggio di tale immagine per l'API viso.

## <a name="parse-the-response"></a>Analizzare la risposta

Per calcolare gli emoji ee necessaria convertire ogni viso restituito nella risposta dell'API a un'istanza di un `Face` classe.

Aggiungiamo questo codice subito dopo il codice per chiamare l'API nel `getFaces` funzione:

```typescript
let faces = [];
for (let f of resp) {
  let scores = new EmotivePoint(f.faceAttributes.emotion);
  let faceRectangle = new Rect(f.faceRectangle);
  let face = new Face(scores, faceRectangle);
  console.log(face.mojiIcon);
  faces.push(face);
}
return faces;
```

- Esaminati in ciclo ogni viso restituito nella risposta.
- È possibile generare una `EmotivePoint`, una `Rect` e un `Face` dal codice json restituito.
- Creazione di `Face` istanza corrisponde la faccia di un emoji
- Per vedere quali emoji è stata trovata corrispondenza si stampa il `mojiicon`.

## <a name="try-it-out"></a>Provare il servizio

Se si esegue lo script che dovrebbe essere ora:

1. Passare l'immagine fornita tramite l'API viso e calcolare le emozioni.
2. Corrisponde a emozioni per emoji.
3. Stampare gli emoji sul terminale.

In questo modo:

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
