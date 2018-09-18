A questo punto del flusso dell'applicazione, ecco che cosa è possibile determinare:

1.  L'elenco dei visi nell'immagine (se presenti).
2.  Le emoji da usare per ogni viso.
3.  Il rettangolo di delimitazione di ogni viso nell'immagine.

Per ogni viso individuato nell'immagine, è necessario sovrapporre un'emoji sul viso, ridimensionandola per adattarla alle dimensioni del viso.

Per implementare questa funzionalità, si userà la libreria di modifica di immagini open source [Jimp](https://www.npmjs.com/package/jimp).

L'obiettivo di questa lezione è descrivere come usare la libreria `Jimp` per modificare le immagini e in particolare come sovrapporre un'emoji su un viso e salvare l'immagine di nuovo nel disco.

Si elaborerà lo script `bin/mojify.ts` iniziato nella lezione precedente elaborando la funzione `createMojifiedImage`.

> NOTA Tutto il codice di questo script verrà usato di nuovo per creare il comando di Slack e la funzione di Azure nelle prossime lezioni. Non sarà stato uno sforzo vano!

## <a name="steps"></a>Passaggi

### <a name="required-imports"></a>Importazioni necessarie

Per esercitarsi con Jimp e modificare i file nel file system, è necessario importare alcuni pacchetti

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> NOTA È necessario usare `path` perché i file devono essere caricati dal disco

### <a name="basic-use-case"></a>Caso d'uso di base

Per eliminare molti fattori di complessità, determinare che cosa è necessario per:

1. Caricare un'immagine
2. Posizionare l'emoji 😕 nell'angolo in alto a destra (ridimensionata a 50 x 50 pixel)
3. Salvare l'immagine

È possibile implementare tutte le funzionalità indicate sopra in 6 righe di codice, come in questo esempio:

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

La procedura verrà suddivisa in singoli passaggi.

Per caricare un'immagine usando `Jimp`, è necessario usare la funzione `Jimp.read` in questo modo:

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

È disponibile una directory di file PNG per ogni emoji in `shared/emojis`. Poiché ogni file PNG di un'emoji ha come nome <emoji>.png, `😕.png` è un file che contiene un'immagine PNG dell'emoji 😕.

Caricare `😕.png`, come in questo esempio:

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

È quindi necessario ridimensionare l'immagine dell'emoji a 50 pixel di larghezza x 50 pixel di altezza, usando a questo scopo la funzione di ridimensionamento in questo modo:

```typescript
emojiImage.resize(50, 50);
```

L'elemento `emojiImage` è stato ora ridimensionato in modo da occupare uno spazio di 50 x 50 pixel.

È necessario _posizionare_ emojiImage su sourceImage nell'angolo in alto a sinistra, in questo modo:

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

Usare la funzione `composite`, che posiziona `emojiImage` su `sourceImage` a 0 pixel dall'alto e 0 pixel dal basso. La funzione `composite` non modifica `sourceImage`, ma restituisce una copia di `sourceImage` con `emojiImage` posizionato sopra.

È infine necessario salvare l'immagine di output sul disco in questo modo:

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### <a name="full-use-case"></a>Caso d'uso completo

A questo punto, si dovrebbe avere una buona comprensione del funzionamento di `Jimp` e di come usarlo per comporre immagini. Quando verrà esaminato il codice completo della funzione `createMojifiedImage`, sarà tutto più chiaro.

Copiare e incollare il codice seguente nella funzione `createMojifiedImage` in `bin/mojify.ts`.

```typescript
async function createMojifiedImage(imageUrl) {
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

Il codice precedente è molto simile al caso d'uso di base appena descritto: invece di impostare come hardcoded un'emoji e posizionarla, si sceglie quale emoji comporre e dove posizionarla in base alla matrice di visi passati.

La matrice dei visi viene restituita dalla funzione `getFaces` elaborata nell'ultima lezione ed è tutto collegato insieme nella funzione principale, in questo modo:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

Chiamare `getFaces` con l'elemento `imageUrl` passato per ottenere la matrice di istanze di `Face`.
Passare la matrice alla funzione `createMojifiedImage` insieme all'immagine originale. Questa funzione compone le emoji sui visi delle persone e salva il file risultante nella cartella radice del progetto come `mojified.jpg`

### <a name="try-it-out"></a>Provare questa operazione

Provare personalmente il codice, come in questo esempio:

```bash
node bin/mojify.js <url>
```

Se funziona, una versione trasformata in emoji del file di origine verrà archiviata nella radice del progetto, denominata `mojified.jpg`.

Provare il codice con immagini diverse.
