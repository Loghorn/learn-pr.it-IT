Abbiamo creato il `MojifyImage` funzione di Azure che restituisce un'immagine mojified; è necessario un secondo endpoint quali slack viene chiamato ogni volta che un utente esegue il `\mojify` comando.

Questo secondo endpoint deve restituire l'URL per il `MojifyImage` funzioni di Azure.

Quando si esegue un comando simile a slack `\mojify [some-image-url]` effettua una richiesta POST all'endpoint è stato configurato e passare `[some-image-url]` come un `text` param query incorporato nel corpo del messaggio.

L'obiettivo di questa lezione è per creare questa funzione che coordina le risposte ai comandi slack; Chiamiamo la funzione di Azure secondo `RespondToSlackCommand`.

Al termine di questa lezione si apprenderà:

- Come creare un endpoint HTTP che risponde a Slack comandi, è possibile comprendere il formato Slack prevede che per la richiesta e come rispondere con un'immagine.

## <a name="create-the-function-trigger-and-convert-to-typescript"></a>Creare il Trigger di funzione e convertire in TypeScript

È necessario creare un'altra funzione di Azure attivata da HTTP. Queste istruzioni sono gli stessi la lezione precedente; l'unica differenza è che si sta chiamando questa funzione `RespondToSlackCommand` invece di `MojifyImage`.

1. Tipo di <kbd>Ctrl</kbd>+<kbd>P</kbd> per aprire il riquadro comandi, quindi cercare e selezionare `Azure Functions: Create Function...`

   ![Creare una nuova funzione](/media-drafts/7.create-function.png)

2. Selezionare la cartella in cui la creazione del progetto di funzione in precedenza (la prima opzione è la cartella corrente)

   ![Seleziona cartella](/media-drafts/7.select-current-project.png)

3. Stiamo creando un _HTTP Trigger_, se si desidera selezionare tale opzione.

   ![Seleziona cartella](/media-drafts/7.select-trigger.png)

4. Digitare `RespondToSlackCommand` come il nome della funzione da creare

   ![Scegli nome](/media-drafts/7.choose-function-name.png)

5. Scegliere l'autenticazione a livello anonimo come il livello di autenticazione

   ![Scegli nome](/media-drafts/7.choose-auth-level.png)

Se quindi completata correttamente una cartella denominata `RespondToSlackCommand` deve essere presente nella directory radice.

Uguale all'ultimo lezione si convert ora l'impostazione corrente `JavaScript` a `TypeScript`.

6. Creare un file denominato `index.ts` nella `RespondToSlackCommand` cartella.

   Verificare che il `TypeScript` processo di compilazione è ancora in esecuzione e che compilato automaticamente nella `index.js` e `index.map` file.

7. Incollare questo codice in `index.ts`

```typescript
export async function index(context, req) {
  context.log("RespondToSlackCommand HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

## <a name="try-it-out"></a>Provare il servizio

Assicurarsi che tutto funzioni visitando http://localhost:7171/api/RespondToSlackCommand in un browser, vengono stampate nostro `Hello!`.

## <a name="flesh-out-the-index-function"></a>Approfondire il `index` (funzione)

Ciò `index` funzione è molto più veloce scrivere rispetto alla funzione precedente.

È possibile impostare il `context.res` oggetto

```typescript
context.res = {
  headers: {
    "Content-Type": "application/json"
  },
  body: null
};
```

Semplice rispetto a quello in precedenza, non viene impostata la `isRaw` proprietà perché il valore predefinito è `false`, ma viene impostato il tipo di contenuto sia `application/json`.

Come accennato in precedenza in Slack invia l'URL dell'immagine si vuole elaborare come una stringa di query con la chiave di `text`, ma per motivi di sicurezza incorpora questo nel corpo della richiesta, pertanto è necessario usare un po' più difficile per ottenere le informazioni necessarie , non è troppo difficile se!

Per rendere più semplice è possibile importare il nodo nostre vite `querystring` pacchetto, questa viene fornito come parte di NodeJS pertanto non occorre installare alcun componente aggiuntivo.

```typescript
import * as querystring from "querystring";
```

Quindi nel nostro `index` funzione per consentire la conversione il `req.body` in un oggetto dal quale verrà estratto il `text` proprietà, come illustrato di seguito:

```typescript
const { text } = querystring.parse(req.body);
```

Se l'utente ha digitato correttamente il comando, il testo deve contenere l'URL di un'immagine, aggiungiamo una convalida di base come segue:

```typescript
let message = "Your mojified image my liege...";
if (!text) {
  message = "You must provide an image to mojify";
}
```

Tenere presente consiste nel chiamare il comando slack https://somedomain.com/api/RespondToSlackCommand, ed è necessario rispondere con l'URL MojifyImage impattando più probabilmente nello stesso dominio, https://somedomain.com/api/MojifyImage

Anziché impostare come hardcoded il dominio nel file, è possibile invece usare lo stesso dominio della richiesta come dominio di te `MojifyImage` url, come illustrato di seguito:

```typescript
const mojifyUrl = req.url.substr(0, req.url.lastIndexOf("/")) + "/MojifyImage";
```

Infine è possibile impostare il corpo della risposta, il comando slack prevede un formato specifico per la risposta, come illustrato di seguito:

```typescript
context.res.body = {
  response_type: "in_channel",
  text: message,
  attachments: [
    {
      image_url: `${mojifyUrl}?imageUrl=${text}`
    }
  ]
};

context.done();
```

L'aspetto fondamentale da notare sopra è il `image_url` nella `attachments` proprietà; viene impostato per la restituzione il `mojifyUrl` passando in URL l'utente specificato nel comando come il `imageUrl` param di query.

## <a name="try-it-out"></a>Provare il servizio

Assicurarsi che tutto funzioni, vedere http://localhost:7171/api/RespondToSlackCommand in un browser, dovrebbe ora visualizzare alcuni `json` come di seguito:

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "http://localhost:7071/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```
