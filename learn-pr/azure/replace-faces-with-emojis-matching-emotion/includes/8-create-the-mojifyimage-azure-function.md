L'obiettivo di questa lezione consiste nel creare un endpoint HTTP di funzioni di Azure che viene mojify un'immagine del passato e restituisce quindi l'immagine mojified in modo da visualizzare sullo schermo.

Al termine si apprenderà:

- Come convertire un `JavaScript` progetto funzioni di Azure a `TypeScript`.
- Come restituire `raw` dati dell'immagine da un endpoint della funzione.

## <a name="convert-to-typescript"></a>Converti in TypeScript

Si sta scrittura di codice nel `TypeScript` ma l'app per le funzioni creata una `JavaScript` file, è necessario convertirlo in `TypeScript`.

1. Rinominare il `index.js` file `index.ts`

> **NOTA**
>
> Se si esegue `npm run build` e i relativi `tsc -w` comando il `index.ts` file viene compilato immediatamente al `index.js` e l'utente deve avere un `index.map` file creato.
>
> Se il `index.js` e `index.map` i file non vengono creati automaticamente quindi ricontrollare il `npm run build` esecuzione del comando e l'output non visualizzi eventuali errori.

2. Incollare questo codice in `index.ts`

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

3. Eseguire l'app per le funzioni

Assicurarsi che la funzione app viene eseguita usando uno dei metodi è stato illustrato in precedenza, usare il `func host start` comando oppure eseguire l'applicazione tramite il menu di debug.

Quindi si accede al http://localhost:7171/api/MojifyImage in un browser.

Se tutto funziona correttamente, allora si dovrebbe visualizzare `Hello!` nel browser.

Ora convertito il trigger di funzione per essere TypeScript invece che in JavaScript 👏.

## <a name="environment-variables-in-azure-functions"></a>Variabili di ambiente in funzioni di Azure

Per la sicurezza della connessione, impostare come hardcoded i dati privati non è nel nostro codice. invece abbiamo utilizzato le variabili di ambiente.

In particolare, abbiamo utilizzato le variabili di ambiente per archiviare l'URL dell'API viso e la chiave, come illustrato di seguito:

```bash
export FACE_API_URL=[face-api-url]
export FACE_API_KEY=[face-api-key]
```

Quando si crea un progetto funzioni di Azure, crea anche un file denominato `local.settings.json`. Questo file è disponibile anche nella `.gitignore` file pertanto _isnot archiviato nel controllo del codice sorgente_.

È una posizione in cui archiviare la configurazione sensibile o solo quelle locale, che non si desidera pubblicare.

Per impostazione predefinita simile alla seguente:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node"
  }
}
```

Qualsiasi elemento aggiunto per il `Values` oggetto è reso disponibile al codice NodeJS come variabile di ambiente.

Pertanto, se è stato aggiunto nelle variabili di API viso, ad esempio, in modo:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "FACE_API_URL": "[face-api-url]",
    "FACE_API_KEY": "[face-api-key]"
  }
}
```

L'oggetto `FACE_API_URL` e `FACE_API_KEY` saranno disponibile al nodo di variabili come variabili di ambiente come segue:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="copy-existing-code-from-the-mojifyts-script"></a>Copiare il codice esistente dal `mojify.ts` script

Il lavoro abbiamo impiegati nelle lezioni precedenti creazione nostro `bin\mojify.ts` codice non ci sia sprechi, la maggior parte di tale codice può ora essere copiata in questa funzione.

Copiare tutti gli elementi **eccetto** il `main` funzione tramite il `index.ts` file.

Non è importante where ma posso avere tutte le importazioni e le funzioni dipendenti nella parte superiore del file e il `index` funzione nella parte inferiore.

> **IMPORTANTE**
>
> Non sovrascrivere il `index` funzione, questa operazione deve esistere affinché la funzione di Azure da usare.

Non verrà discussa marginalmente il `getFaces` funzione; rimane completamente invariato.

Tuttavia, il `createMojifiedImage` funzione esegue necessità una modifica nella versione originale di questa funzione sono state salvataggio dell'immagine su disco con una riga simile alla seguente alla fine del codice:

```typescript
compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

Nella nuova versione del codice di funzione di Azure, non si desidera salvare il file sul disco. Vogliamo _restituire_ l'immagine per l'utente e visualizzarlo nella finestra del browser.

Per farlo è necessario un elemento chiamato un' `buffer`, possiamo porre `Jimp` commenti il buffer per l'immagine, ma è necessario inserire il codice in un `Promise` perché il `Jimp.getBuffer` funzione è asincrona.

Quindi, sostituire il codice precedente `compositeImage.write` con questa riga:

```typescript
return new Promise((resolve, reject) => {
  compositeImage.getBuffer(Jimp.MIME_JPEG, (error, buffer) => {
    // get a buffer of the composed image
    if (error) {
      let message = "There was an error adding the emoji to the image";
      context.log.error(error);
      reject(message);
    } else {
      // put the image into the context body
      resolve(buffer);
    }
  });
});
```

Restituisce il buffer dell'immagine, i dati binari non elaborati.

Si noterà ora che TypeScript sia si lamentano che non riconosce il `context` variabile.

Per risolvere questo problema, aggiungere `context` come primo argomento per il `createMojifiedImage` funzione, come illustrato di seguito:

```typescript
async function createMojifiedImage(context, imageUrl, faces) {
  ...
}
```

## <a name="connect-it-all-in-the-index-function"></a>Connetterlo tutte la `index` (funzione)

Quando un utente accede a questo endpoint si desidera:

1. Ottenere il `imageUrl` richiedono
2. Mojify il `imageUrl` e ottenere il buffer non elaborato.
3. Rispondere alla richiesta HTTP in modo che un'immagine viene visualizzata nel browser.

Quindi, iniziamo integrano a turno la funzione di indice.

Il `context.res` proprietà descrive la risposta di questa funzione, cosa abbiamo impostato qui si ottiene il valore restituito al browser.

È possibile configurare `context.res` come segue:

```typescript
context.res = {
  status: 200,
  headers: {
    "Content-Type": "image/jpeg"
  },
  isRaw: true
};
```

Aggiungere il codice precedente all'inizio del `index` funzione, si imposta la risposta da restituire un codice di stato 200, imposta il tipo di contenuto restituito in formato JPEG e la `isRaw` flag su true in modo che possono essere restituiti binari _immagine_ dei dati.

Successivamente, si desidera estrarre il `imageUrl` dai parametri di query in modo da sapere quale immagine dovremmo essere mojifying. Vogliamo anche restituito alcune messaggio di errore interessante se l'utente non fornirla.

```typescript
const { imageUrl } = req.query;
if (!imageUrl) {
  let message = `imageUrl is required`;
  context.log.error(message);
  return context.done(message, { status: 400, body: message });
} else {
  context.log(`Called with imageUrl: "${imageUrl}"`);
}
```

Infine, si decide di chiamare il `getFaces` e `createMojifiedImage` funzioni viene aggiunto all'inizio del file utilizzato.

```typescript
// Get the response from the faceAPI
try {
  let faces = await getFaces(imageUrl);
  let buffer = await createMojifiedImage(context, imageUrl, faces);
  context.res.body = buffer;
  context.log(`Posted reply with mojified image`);
  context.done();
} catch (err) {
  let message =
    "There was an error processing this image: " + JSON.stringify(err);
  context.log.error(message);
  context.done(null, {
    status: 400,
    body: message
  });
}
```

Le righe di due importanti precedente sono:

```typescript
context.res.body = buffer;
context.done();
```

Questo imposta i dati binari non elaborati come corpo in cui l'oggetto risposta e chiamiamo il metodo `context.done()` in modo che l'app per le funzioni sa è stata completata e che può essere eseguita l'attività di restituzione della risposta.

Il listato completo per la funzione index.ts consiste nel modo seguente:

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");

  context.res = {
    status: 200,
    headers: {
      "Content-Type": "image/jpeg"
    },
    isRaw: true
  };

  const { imageUrl } = req.query;
  if (!imageUrl) {
    let message = `imageUrl is required`;
    context.log.error(message);
    return context.done(message, { status: 400, body: message });
  } else {
    context.log(`Called with imageUrl: "${imageUrl}"`);
  }

  // Get the response from the faceAPI
  try {
    let faces = await getFaces(imageUrl);
    let buffer = await createMojifiedImage(context, imageUrl, faces);
    context.res.body = buffer;
    context.log(`Posted reply with mojified image`);
    context.done();
  } catch (err) {
    let message =
      "There was an error processing this image: " + JSON.stringify(err);
    context.log.error(message);
    context.done(null, {
      status: 400,
      body: message
    });
  }
}
```

## <a name="try-it-out"></a>Provare il servizio

Verificare che l'app per le funzioni sia in esecuzione effettuando avviandolo dal menu debug oppure eseguirlo dal terminale in questo modo:

```bash
func host start
```

Se l'app per le funzioni è stato avviato correttamente quindi la finestra di output dovrebbe avere un aspetto simile al seguente:

```
Http Functions:
        MojifyImage: http://localhost:7071/api/MojifyImage
```

Se l'app per le funzioni è stato avviato correttamente quindi visitare l'URL ricordando di passare l'URL di un'immagine tramite il `imageUrl` param, eseguire una query come illustrato di seguito:

http://localhost:7171/api/MojifyImage?imageUrl=[url-a-un-image]

Se tutto funziona correttamente, quindi deve restituire una versione mojified dell'immagine come illustrato di seguito:

![Immagine di Mojified](/media-drafts/7.mojified-image.png)
