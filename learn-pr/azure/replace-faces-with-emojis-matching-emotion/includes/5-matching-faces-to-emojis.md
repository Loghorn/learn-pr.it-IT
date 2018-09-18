Nel capitolo precedente si è visto che il file `shared/mojis.ts` include un elenco di emoji e dei rispettivi punti emotivi.

In questo capitolo verrà presentato il resto del codice da usare per associare un viso a un'emoji.

Verrà descritto come:

1. Creare uno script in cui passare un URL di un'immagine di un viso.
2. Calcolare il punto emotivo di qualsiasi viso nell'immagine.
3. Associare i visi nell'immagine alle emoji più vicine

Questa funzionalità verrà infine implementata come comando di Slack, ma per il momento si inizierà con un semplice script Node che è possibile eseguire nella riga di comando, come questo:

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript"></a>Debug di codice TypeScript

Il codice viene scritto come TypeScript, ma eseguito come JavaScript. Questo approccio complica il debug, perché è necessario impostare punti di interruzione ed eseguire il debug nei file JavaScript transcompilati, che possono risultare difficili da leggere.

Idealmente, è preferibile scrivere il codice _ed_ eseguirne il debug in TypeScript.

La buona notizia è che questo è possibile con Visual Studio Code con una configurazione minima.

Aprire il file `launch.json` usando il riquadro comandi `Ctrl+P > Debug: Open launch.json`

> TODO: Immagine

Aggiungere un'opzione di configurazione come questa:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mojify",
  "env": {
    "DEBUG": "*"
  },
  "args": [
    "https://pbs.twmedia-drafts.com/profile_images/833970306339446784/83MO53R9_400x400.jpg"
  ],
  "sourceMaps": true,
  "stopOnEntry": false,
  "console": "integratedTerminal",
  "cwd": "${workspaceRoot}",
  "program": "${workspaceRoot}/bin/mojify.js",
  "outFiles": ["${workspaceRoot}/bin/mojify.js"]
}
```

Nel menu Debug verrà ora visualizzata un'opzione denominata `Mojify` che esegue lo script mojify passando un URL come argomento. Sarà possibile impostare punti di interruzione nel file TypeScript e controllare le variabili direttamente da qui.

## <a name="open-up-binmojists"></a>Aprire `bin/mojis.ts`

È già stato eseguito lo scaffolding del file con tutte le importazioni necessarie, come qui:

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` è un pacchetto helper che carica il contenuto di un file ENV nella radice del progetto come variabili di ambiente, utili per lo sviluppo.

`node-fetch` viene usato per effettuare richieste HTTP all'API Viso di Azure.

`EmotivePoint` è una classe helper che descrive un punto nello _spazio emotivo_. Questo argomento verrà approfondito di seguito.

`Rect` è una classe helper per rappresentare una forma rettangolare usata per descrivere la posizione di un viso in un'immagine.

`Face` è una classe di utilità helper che combina le informazioni su rettangolo e punto emotivo di un viso in un'immagine.

A metà del file è possibile osservare alcune funzioni stub, come in questo esempio:

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

Si tratta delle funzioni che verranno usate in questa lezione e nella successiva

Alla fine del file è possibile osservare questo codice

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

Si chiamerà ora l'API Viso e di conseguenza è necessario usare le chiavi segrete e gli URL generati prima, aggiungendoli all'inizio del file sotto le importazioni:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a>Chiamare l'API Viso con l'immagine fornita e ottenere una risposta

Per effettuare una richiesta all'API Viso, è necessario aggiungere questo codice all'inizio della funzione `getFaces`

```typescript
let response = await fetch(API_URL, {
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

Il codice precedente usa il comando `fetch` per inviare una richiesta `POST` all'API Viso.

Viene passato `API_KEY` nell'intestazione in modo che l'API Viso sia in grado di determinare che la richiesta proviene dal mittente corretto, altrimenti viene rifiutata.

Passare `imageUrl` che l'API Viso deve analizzare nel corpo della richiesta.

Viene quindi ottenuta la risposta dall'API Viso, che viene stampata.

Se ora si esegue lo script con

```bash
node bin/mojify.js <path-to-image>
```

Verrà stampata la risposta JSON restituita dal passaggio dell'immagine all'API Viso.

## <a name="parse-the-responce"></a>Analizzare la risposta

Per calcolare le emoji, è necessario convertire ogni viso restituito nella risposta dell'API in un'istanza di una classe `Face`.

Aggiungere questo codice subito dopo il codice necessario per chiamare l'API nella funzione `getFaces`:

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

- Eseguire il ciclo di ogni viso restituito nella risposta.
- Generare gli oggetti `EmotivePoint`, `Rect` e `Face` dal codice JSON restituito.
- La creazione di un'istanza di `Face` associa il viso a un'emoji
- Per esaminare le emoji associate, stampare l'output di `mojiicon`.

## <a name="try-it-out"></a>Provare questa operazione

Se ora si esegue lo script, ecco cosa succederà:

- L'immagine fornita verrà passata tramite l'API Viso calcolando l'emozione.
- Verranno associate emozioni a emoji.
- Verranno stampate le emoji nel terminale.

Ecco un esempio:

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
