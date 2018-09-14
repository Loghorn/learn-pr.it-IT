A questo punto nel flusso dell'applicazione è vero:

1.  Elenco di visi nelle immagini (se presente).
2.  Emoji da usare per ogni viso.
3.  Il rettangolo di delimitazione di ogni viso nell'immagine.

Per ogni viso nell'immagine di scoperto, è necessario un emoji dei livelli tramite il viso, ridimensionando l'emoji per adattare il viso.

Per implementare questa funzionalità, si usa la libreria di manipolazione di immagini open source [Jimp](https://www.npmjs.com/package/jimp).

L'obiettivo di questa lezione è per imparare a usare il `Jimp` nuovamente libreria come gestire le immagini e in particolare per imparare a di livello un emoji tramite un viso e salvare tale immagine al disco.

Si intende espandere la `bin/mojify.ts` dello script è stato avviato nella lezione precedente da integrano a turno il `createMojifiedImage` (funzione).

> **Nota**
>
> Utilizziamo nuovamente tutto il codice da questo script quando si crea il comando Slack e la funzione di Azure in lezioni successive. Uno sforzo è non inutile!

## <a name="add-the-required-imports"></a>Aggiungere le importazioni necessarie

Per esercitarsi con Jimp e modificare i file sul nostro file System, è necessario importare alcuni pacchetti nella parte superiore del file, come illustrato di seguito:

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> **Nota**
>
> `path` è necessaria perché si desidera caricare i file dal disco

## <a name="simplified-use-case"></a>Caso d'uso semplificato

È possibile eliminare molte delle complessità e chiedere cosa ti servirebbe semplicemente:

1. Caricare un'immagine
2. Posizione di 😕 emoji nell'angolo superiore destro (ridimensionato a 50x50px)
3. Salvare l'immagine

È possibile implementare tutte le funzionalità in precedenza in 6 righe di codice, come illustrato di seguito:

```typescript
async function createMojifiedImage(imageUrl) {
  let sourceImage = await Jimp.read(imageUrl);

  let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
  let emojiImage = await Jimp.read(mojiPath);

  emojiImage.resize(50, 50);

  sourceImage = sourceImage.composite(emojiImage, 0, 0);
  sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
}
```

È possibile suddividerla dettagliata.

Caricare un'immagine utilizzando `Jimp` usiamo il `Jimp.read` funzione, come illustrato di seguito:

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

È disponibile una directory di file png per ogni emoji in `shared/emojis`. Ogni png emoji denominata come illustrato <emoji>PNG, pertanto `😕.png` è un file che contiene un file png del 😕 emoji.

Abbiamo carichiamo `😕.png` come segue:

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

Successivamente fino è necessario ridimensionare il `emojiImage` per 50 pixel in larghezza x 50 pixel in altezza, è possibile farlo usando la funzione di ridimensionamento in questo modo:

```typescript
emojiImage.resize(50, 50);
```

Il `emojiImage` a questo punto è stata ridimensionata per adattarla a uno spazio di 50 x 50 px.

È ora necessario _posizionare_ le `emojiImage` tramite il `sourceImage` in alto a sinistra, come illustrato di seguito:

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

Usiamo il `composite` funzione, che inserisce `emojiImage` nella parte superiore della `sourceImage`, gli ultimi due argomenti definiscono dove il `emojiImage` viene inserito, si sta posizionandolo 0 pixel del numero di pixel superiore e 0 verso il basso.

Il `composite` non modifica funzione `sourceImage` posto; al contrario restituisce una copia del `sourceImage` con il `emojiImage` posizionate in primo piano, ecco perché abbiamo assegnare il risultato al sourceImage `sourceImage = ...`

Infine si salva l'immagine di output su disco nel modo seguente:

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

## <a name="try-it-out"></a>Provare il servizio

Provare questo codice direttamente, come illustrato di seguito:

```bash
node bin/mojify.js [url-to-image]
```

Se l'azione è riuscita, quindi un file viene salvato nella radice del progetto chiamato `mojified.jpg` ricerca simile al seguente:

![Immagine semplice](/media-drafts/6.simple-mojified-image.jpg)

## <a name="full-use-case"></a>Caso d'uso completo

Si spera, a questo punto hai una buona conoscenza di come `Jimp` funziona e come è possibile usarlo per le immagini composite. A questo punto quando si passa attraverso il codice completo per il `createMojifiedImage` (funzione), dovrebbe avere un senso di molte altre.

Copiare e incollare il seguente codice nel `createMojifiedImage` funzionare in `bin/mojify.ts`.

```typescript
async function createMojifiedImage(imageUrl) {
  let sourceImage = await Jimp.read(imageUrl);
  // Create a composite image, we will "append" to this composite an emoji image for each face found
  let compositeImage = sourceImage;

  for (let face of faces) {
    const mojiIcon = face.mojiIcon;
    const faceHeight = face.faceRectangle.height;
    const faceWidth = face.faceRectangle.width;
    const faceTop = face.faceRectangle.top;
    const faceLeft = face.faceRectangle.left;

    // Load the emoji from disk
    let mojiPath = path.resolve(
      __dirname,
      "../shared/emojis/" + mojiIcon + ".png"
    );
    let emojiImage = await Jimp.read(mojiPath);

    // Resize the emoji to fit the face
    emojiImage.resize(faceWidth, faceHeight);

    // Compose the emoji on the image
    compositeImage = compositeImage.composite(emojiImage, faceLeft, faceTop);
  }
  compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
}
```

Il codice sopra riportato è molto simile al caso più semplice che questa sede, anziché impostare come hardcoded un emoji e posizione, tuttavia, si sta per decidere quali emoji per comporre e passato in cui inserire la matrice di volti in base.

La matrice delle facce proviene dal `getFaces` funzione è fondamentale nell'ultima lezione; relativo tutti collegati backup tramite la funzione main, come illustrato di seguito:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

Chiamiamo `getFaces` con il valore passato in `imageUrl` per ottenere la matrice di `Face` istanze.
Si passa questa matrice per la `createMojifiedImage` funzione insieme all'immagine originale, questa emoji compone funzione su peoples deve affrontare e Salva il file risulta nella cartella radice del progetto come `mojified.jpg`

## <a name="try-it-out"></a>Provare il servizio

Provare questo codice direttamente, come illustrato di seguito:

```bash
node bin/mojify.js <url>
```

Se l'azione è riuscita, quindi una versione mojified del file di origine viene salvata nella radice del progetto chiamato `mojified.jpg`, come illustrato di seguito:

![Immagine complessa](/media-drafts/6.complex-mojified-image.jpg)

Provarlo con immagini diverse.
