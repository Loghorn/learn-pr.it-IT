<span data-ttu-id="0ed12-101">A questo punto del flusso dell'applicazione, ecco che cosa è possibile determinare:</span><span class="sxs-lookup"><span data-stu-id="0ed12-101">At this point in the flow of the application we know:</span></span>

1.  <span data-ttu-id="0ed12-102">L'elenco dei visi nell'immagine (se presenti).</span><span class="sxs-lookup"><span data-stu-id="0ed12-102">The list of faces in the image (if any).</span></span>
2.  <span data-ttu-id="0ed12-103">Le emoji da usare per ogni viso.</span><span class="sxs-lookup"><span data-stu-id="0ed12-103">The emoji to use for each face.</span></span>
3.  <span data-ttu-id="0ed12-104">Il rettangolo di delimitazione di ogni viso nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="0ed12-104">The bounding rectangle of each face in the image.</span></span>

<span data-ttu-id="0ed12-105">Per ogni viso individuato nell'immagine, è necessario sovrapporre un'emoji sul viso, ridimensionandola per adattarla alle dimensioni del viso.</span><span class="sxs-lookup"><span data-stu-id="0ed12-105">So for each face discovered in the image, we need to layer an emoji over the face, resizing the emoji to fit the face.</span></span>

<span data-ttu-id="0ed12-106">Per implementare questa funzionalità, si userà la libreria di modifica di immagini open source [Jimp](https://www.npmjs.com/package/jimp).</span><span class="sxs-lookup"><span data-stu-id="0ed12-106">To implement this functionality, we will use the open source image manipulation library [Jimp](https://www.npmjs.com/package/jimp).</span></span>

<span data-ttu-id="0ed12-107">L'obiettivo di questa lezione è descrivere come usare la libreria `Jimp` per modificare le immagini e in particolare come sovrapporre un'emoji su un viso e salvare l'immagine di nuovo nel disco.</span><span class="sxs-lookup"><span data-stu-id="0ed12-107">The goal of this lecture is to learn how to use the `Jimp` library to manipulate images and specifically to learn how to layer an emoji over a face and save that image back out to disk.</span></span>

<span data-ttu-id="0ed12-108">Si elaborerà lo script `bin/mojify.ts` iniziato nella lezione precedente elaborando la funzione `createMojifiedImage`.</span><span class="sxs-lookup"><span data-stu-id="0ed12-108">We are going to expand on the `bin/mojify.ts` script we started in the previous lecture by fleshing out the `createMojifiedImage` function.</span></span>

> <span data-ttu-id="0ed12-109">NOTA Tutto il codice di questo script verrà usato di nuovo per creare il comando di Slack e la funzione di Azure nelle prossime lezioni.</span><span class="sxs-lookup"><span data-stu-id="0ed12-109">NOTE We will be re-using all the code from this script when we create the Slack command and Azure Function in later lectures.</span></span> <span data-ttu-id="0ed12-110">Non sarà stato uno sforzo vano!</span><span class="sxs-lookup"><span data-stu-id="0ed12-110">It's not wasted effort!</span></span>

## <a name="steps"></a><span data-ttu-id="0ed12-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0ed12-111">Steps</span></span>

### <a name="required-imports"></a><span data-ttu-id="0ed12-112">Importazioni necessarie</span><span class="sxs-lookup"><span data-stu-id="0ed12-112">Required Imports</span></span>

<span data-ttu-id="0ed12-113">Per esercitarsi con Jimp e modificare i file nel file system, è necessario importare alcuni pacchetti</span><span class="sxs-lookup"><span data-stu-id="0ed12-113">To play around with Jimp and manipulate files on our filesystem we need to import a few packages at the top</span></span>

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> <span data-ttu-id="0ed12-114">NOTA È necessario usare `path` perché i file devono essere caricati dal disco</span><span class="sxs-lookup"><span data-stu-id="0ed12-114">NOTE `path` is needed because we want to load files from disk</span></span>

### <a name="basic-use-case"></a><span data-ttu-id="0ed12-115">Caso d'uso di base</span><span class="sxs-lookup"><span data-stu-id="0ed12-115">Basic Use Case</span></span>

<span data-ttu-id="0ed12-116">Per eliminare molti fattori di complessità, determinare che cosa è necessario per:</span><span class="sxs-lookup"><span data-stu-id="0ed12-116">Let's strip away a lot of the complexity and ask what it would take to:</span></span>

1. <span data-ttu-id="0ed12-117">Caricare un'immagine</span><span class="sxs-lookup"><span data-stu-id="0ed12-117">Load up an image</span></span>
2. <span data-ttu-id="0ed12-118">Posizionare l'emoji 😕 nell'angolo in alto a destra (ridimensionata a 50 x 50 pixel)</span><span class="sxs-lookup"><span data-stu-id="0ed12-118">Place the 😕 emoji in the top right corner (resized to 50x50px)</span></span>
3. <span data-ttu-id="0ed12-119">Salvare l'immagine</span><span class="sxs-lookup"><span data-stu-id="0ed12-119">Save the image</span></span>

<span data-ttu-id="0ed12-120">È possibile implementare tutte le funzionalità indicate sopra in 6 righe di codice, come in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="0ed12-120">We can implement all the functionality above in 6 lines of code, like so:</span></span>

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

<span data-ttu-id="0ed12-121">La procedura verrà suddivisa in singoli passaggi.</span><span class="sxs-lookup"><span data-stu-id="0ed12-121">We'll break it down step by step.</span></span>

<span data-ttu-id="0ed12-122">Per caricare un'immagine usando `Jimp`, è necessario usare la funzione `Jimp.read` in questo modo:</span><span class="sxs-lookup"><span data-stu-id="0ed12-122">To load an image using `Jimp` we use the `Jimp.read` function, like so:</span></span>

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

<span data-ttu-id="0ed12-123">È disponibile una directory di file PNG per ogni emoji in `shared/emojis`.</span><span class="sxs-lookup"><span data-stu-id="0ed12-123">We have a directory of png files for each emoji in `shared/emojis`.</span></span> <span data-ttu-id="0ed12-124">Poiché ogni file PNG di un'emoji ha come nome <emoji>.png, `😕.png` è un file che contiene un'immagine PNG dell'emoji 😕.</span><span class="sxs-lookup"><span data-stu-id="0ed12-124">Each emoji png is named as <emoji>.png, so `😕.png` is a file that contains a png of the 😕 emoji.</span></span>

<span data-ttu-id="0ed12-125">Caricare `😕.png`, come in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="0ed12-125">We load up `😕.png` like so:</span></span>

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

<span data-ttu-id="0ed12-126">È quindi necessario ridimensionare l'immagine dell'emoji a 50 pixel di larghezza x 50 pixel di altezza, usando a questo scopo la funzione di ridimensionamento in questo modo:</span><span class="sxs-lookup"><span data-stu-id="0ed12-126">Next up we need to resize the emojiImage to 50 pixels width x 50 pixels height, we can do that by using the resize function like so:</span></span>

```typescript
emojiImage.resize(50, 50);
```

<span data-ttu-id="0ed12-127">L'elemento `emojiImage` è stato ora ridimensionato in modo da occupare uno spazio di 50 x 50 pixel.</span><span class="sxs-lookup"><span data-stu-id="0ed12-127">The `emojiImage` has now been resized to fit in a 50x50 px space.</span></span>

<span data-ttu-id="0ed12-128">È necessario _posizionare_ emojiImage su sourceImage nell'angolo in alto a sinistra, in questo modo:</span><span class="sxs-lookup"><span data-stu-id="0ed12-128">We now need to _place_ the emojiImage over the sourceImage in the top left corner, like so:</span></span>

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

<span data-ttu-id="0ed12-129">Usare la funzione `composite`, che posiziona `emojiImage` su `sourceImage` a 0 pixel dall'alto e 0 pixel dal basso.</span><span class="sxs-lookup"><span data-stu-id="0ed12-129">We use the `composite` function, which places `emojiImage` ontop of `sourceImage` 0 pixels from the top and 0 pixels down.</span></span> <span data-ttu-id="0ed12-130">La funzione `composite` non modifica `sourceImage`, ma restituisce una copia di `sourceImage` con `emojiImage` posizionato sopra.</span><span class="sxs-lookup"><span data-stu-id="0ed12-130">The `composite` fucntion doesn't alter `sourceImage` in place, instead it returns a copy of `sourceImage` with the `emojiImage` placed on top.</span></span>

<span data-ttu-id="0ed12-131">È infine necessario salvare l'immagine di output sul disco in questo modo:</span><span class="sxs-lookup"><span data-stu-id="0ed12-131">Finally we save the output image to disk like so:</span></span>

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### <a name="full-use-case"></a><span data-ttu-id="0ed12-132">Caso d'uso completo</span><span class="sxs-lookup"><span data-stu-id="0ed12-132">Full Use Case</span></span>

<span data-ttu-id="0ed12-133">A questo punto, si dovrebbe avere una buona comprensione del funzionamento di `Jimp` e di come usarlo per comporre immagini.</span><span class="sxs-lookup"><span data-stu-id="0ed12-133">Hopefully by now you have a good understanding of how `Jimp` works and how we can use it to composite images.</span></span> <span data-ttu-id="0ed12-134">Quando verrà esaminato il codice completo della funzione `createMojifiedImage`, sarà tutto più chiaro.</span><span class="sxs-lookup"><span data-stu-id="0ed12-134">So now when we go through the full code for the `createMojifiedImage` function it should make a lot more sense.</span></span>

<span data-ttu-id="0ed12-135">Copiare e incollare il codice seguente nella funzione `createMojifiedImage` in `bin/mojify.ts`.</span><span class="sxs-lookup"><span data-stu-id="0ed12-135">Copy and paste the bellow code into your `createMojifiedImage` function in `bin/mojify.ts`.</span></span>

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

<span data-ttu-id="0ed12-136">Il codice precedente è molto simile al caso d'uso di base appena descritto: invece di impostare come hardcoded un'emoji e posizionarla, si sceglie quale emoji comporre e dove posizionarla in base alla matrice di visi passati.</span><span class="sxs-lookup"><span data-stu-id="0ed12-136">The above code is very similar to the base case we just went through, rather than hardcoding an emoji and poisition however we are deciding which emoji to composite and where to place it based on the array of faces passed in.</span></span>

<span data-ttu-id="0ed12-137">La matrice dei visi viene restituita dalla funzione `getFaces` elaborata nell'ultima lezione ed è tutto collegato insieme nella funzione principale, in questo modo:</span><span class="sxs-lookup"><span data-stu-id="0ed12-137">The array of faces comes from the `getFaces` function we fleshed out in the last lecture, it's all connected up together in the main function, like so:</span></span>

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

<span data-ttu-id="0ed12-138">Chiamare `getFaces` con l'elemento `imageUrl` passato per ottenere la matrice di istanze di `Face`.</span><span class="sxs-lookup"><span data-stu-id="0ed12-138">We call `getFaces` with the passed in `imageUrl` to get the array of `Face` instances.</span></span>
<span data-ttu-id="0ed12-139">Passare la matrice alla funzione `createMojifiedImage` insieme all'immagine originale. Questa funzione compone le emoji sui visi delle persone e salva il file risultante nella cartella radice del progetto come `mojified.jpg`</span><span class="sxs-lookup"><span data-stu-id="0ed12-139">We pass this array to the `createMojifiedImage` function along with the original image, this function composites emojis on peoples faces and saves the resulting file to the project root folder as `mojified.jpg`</span></span>

### <a name="try-it-out"></a><span data-ttu-id="0ed12-140">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="0ed12-140">Try it out</span></span>

<span data-ttu-id="0ed12-141">Provare personalmente il codice, come in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="0ed12-141">Try this code out yourself, like so:</span></span>

```bash
node bin/mojify.js <url>
```

<span data-ttu-id="0ed12-142">Se funziona, una versione trasformata in emoji del file di origine verrà archiviata nella radice del progetto, denominata `mojified.jpg`.</span><span class="sxs-lookup"><span data-stu-id="0ed12-142">If this worked then a mojified version of the source fil should be stored in the project root called `mojified.jpg`.</span></span>

<span data-ttu-id="0ed12-143">Provare il codice con immagini diverse.</span><span class="sxs-lookup"><span data-stu-id="0ed12-143">Try it out with different images!</span></span>
