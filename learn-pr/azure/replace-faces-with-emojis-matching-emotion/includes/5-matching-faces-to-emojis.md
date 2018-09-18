<span data-ttu-id="ff31b-101">Nel capitolo precedente si è visto che il file `shared/mojis.ts` include un elenco di emoji e dei rispettivi punti emotivi.</span><span class="sxs-lookup"><span data-stu-id="ff31b-101">In the last chapter we learnt that `shared/mojis.ts` file has a list of emojis and their emotional points.</span></span>

<span data-ttu-id="ff31b-102">In questo capitolo verrà presentato il resto del codice da usare per associare un viso a un'emoji.</span><span class="sxs-lookup"><span data-stu-id="ff31b-102">In this chapter we will learn about the rest of the code that we will use to map a face to an emoji.</span></span>

<span data-ttu-id="ff31b-103">Verrà descritto come:</span><span class="sxs-lookup"><span data-stu-id="ff31b-103">You will learn how to:</span></span>

1. <span data-ttu-id="ff31b-104">Creare uno script in cui passare un URL di un'immagine di un viso.</span><span class="sxs-lookup"><span data-stu-id="ff31b-104">Create a script where you pass in a URL of an image of a face.</span></span>
2. <span data-ttu-id="ff31b-105">Calcolare il punto emotivo di qualsiasi viso nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="ff31b-105">Calculate the emotional point of any faces in the image.</span></span>
3. <span data-ttu-id="ff31b-106">Associare i visi nell'immagine alle emoji più vicine</span><span class="sxs-lookup"><span data-stu-id="ff31b-106">Map the faces in the image to the closest emojis</span></span>

<span data-ttu-id="ff31b-107">Questa funzionalità verrà infine implementata come comando di Slack, ma per il momento si inizierà con un semplice script Node che è possibile eseguire nella riga di comando, come questo:</span><span class="sxs-lookup"><span data-stu-id="ff31b-107">Eventually we will be implementing this functionally as a Slack command, but for now we are going to start with a simple node script that you can run on the comand line, like so:</span></span>

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript"></a><span data-ttu-id="ff31b-108">Debug di codice TypeScript</span><span class="sxs-lookup"><span data-stu-id="ff31b-108">Debugging TypeScript</span></span>

<span data-ttu-id="ff31b-109">Il codice viene scritto come TypeScript, ma eseguito come JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff31b-109">We are writing in TypeScript but executing JavaScript.</span></span> <span data-ttu-id="ff31b-110">Questo approccio complica il debug, perché è necessario impostare punti di interruzione ed eseguire il debug nei file JavaScript transcompilati, che possono risultare difficili da leggere.</span><span class="sxs-lookup"><span data-stu-id="ff31b-110">This makes debugging hard as we would have to set breakpoints and debug in the transpiled JavaScript files which can be hard to read.</span></span>

<span data-ttu-id="ff31b-111">Idealmente, è preferibile scrivere il codice _ed_ eseguirne il debug in TypeScript.</span><span class="sxs-lookup"><span data-stu-id="ff31b-111">What we ideally want is to write _and_ debug in TypeScript.</span></span>

<span data-ttu-id="ff31b-112">La buona notizia è che questo è possibile con Visual Studio Code con una configurazione minima.</span><span class="sxs-lookup"><span data-stu-id="ff31b-112">The good news is that it's possible with vs code with just a little bit of configuration.</span></span>

<span data-ttu-id="ff31b-113">Aprire il file `launch.json` usando il riquadro comandi `Ctrl+P > Debug: Open launch.json`</span><span class="sxs-lookup"><span data-stu-id="ff31b-113">Open up the `launch.json` file by using the command paletee `Ctrl+P > Debug: Open launch.json`</span></span>

> <span data-ttu-id="ff31b-114">TODO: Immagine</span><span class="sxs-lookup"><span data-stu-id="ff31b-114">TODO: Image</span></span>

<span data-ttu-id="ff31b-115">Aggiungere un'opzione di configurazione come questa:</span><span class="sxs-lookup"><span data-stu-id="ff31b-115">Add in a configuration option like so:</span></span>

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

<span data-ttu-id="ff31b-116">Nel menu Debug verrà ora visualizzata un'opzione denominata `Mojify` che esegue lo script mojify passando un URL come argomento.</span><span class="sxs-lookup"><span data-stu-id="ff31b-116">Now in the debug menu you will see an option called `Mojify` this will run the mojify script passing in a URL as the argument.</span></span> <span data-ttu-id="ff31b-117">Sarà possibile impostare punti di interruzione nel file TypeScript e controllare le variabili direttamente da qui.</span><span class="sxs-lookup"><span data-stu-id="ff31b-117">You will be able to set breakpoints in the TypeScript file and inspect variables directly from there.</span></span>

## <a name="open-up-binmojists"></a><span data-ttu-id="ff31b-118">Aprire `bin/mojis.ts`</span><span class="sxs-lookup"><span data-stu-id="ff31b-118">Open up `bin/mojis.ts`</span></span>

<span data-ttu-id="ff31b-119">È già stato eseguito lo scaffolding del file con tutte le importazioni necessarie, come qui:</span><span class="sxs-lookup"><span data-stu-id="ff31b-119">The file is already scaffolded with all the required imports, like so:</span></span>

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

<span data-ttu-id="ff31b-120">`dotenv` è un pacchetto helper che carica il contenuto di un file ENV nella radice del progetto come variabili di ambiente, utili per lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="ff31b-120">`dotenv` is a helper package which loads up the contents of a .env file in the root of your project as environment variables, useful for development.</span></span>

<span data-ttu-id="ff31b-121">`node-fetch` viene usato per effettuare richieste HTTP all'API Viso di Azure.</span><span class="sxs-lookup"><span data-stu-id="ff31b-121">`node-fetch` we use to make http requests to the Azure Face API.</span></span>

<span data-ttu-id="ff31b-122">`EmotivePoint` è una classe helper che descrive un punto nello _spazio emotivo_. Questo argomento verrà approfondito di seguito.</span><span class="sxs-lookup"><span data-stu-id="ff31b-122">`EmotivePoint` is a helper class that describes a point in _emotional space_ - we will be discussing this in more details below.</span></span>

<span data-ttu-id="ff31b-123">`Rect` è una classe helper per rappresentare una forma rettangolare usata per descrivere la posizione di un viso in un'immagine.</span><span class="sxs-lookup"><span data-stu-id="ff31b-123">`Rect` is a helper class to describe a rectangle shape, we use this to describe the position of a face in an image.</span></span>

<span data-ttu-id="ff31b-124">`Face` è una classe di utilità helper che combina le informazioni su rettangolo e punto emotivo di un viso in un'immagine.</span><span class="sxs-lookup"><span data-stu-id="ff31b-124">`Face` is a helper utility class which combines the rectangle and emotive point informatoin about a face in an image.</span></span>

<span data-ttu-id="ff31b-125">A metà del file è possibile osservare alcune funzioni stub, come in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="ff31b-125">In the middle of the file you should see some stub functions, like so:</span></span>

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

<span data-ttu-id="ff31b-126">Si tratta delle funzioni che verranno usate in questa lezione e nella successiva</span><span class="sxs-lookup"><span data-stu-id="ff31b-126">These are the functions you will be fleshing out in this lecture and the next</span></span>

<span data-ttu-id="ff31b-127">Alla fine del file è possibile osservare questo codice</span><span class="sxs-lookup"><span data-stu-id="ff31b-127">At the end of the file you should see this code</span></span>

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a><span data-ttu-id="ff31b-128">Aggiungere le variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="ff31b-128">Add the environment variables</span></span>

<span data-ttu-id="ff31b-129">Si chiamerà ora l'API Viso e di conseguenza è necessario usare le chiavi segrete e gli URL generati prima, aggiungendoli all'inizio del file sotto le importazioni:</span><span class="sxs-lookup"><span data-stu-id="ff31b-129">We are going to call the Face API so we need to use those secret keys and urls we generated before, add this to the top of the file under the imports:</span></span>

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a><span data-ttu-id="ff31b-130">Chiamare l'API Viso con l'immagine fornita e ottenere una risposta</span><span class="sxs-lookup"><span data-stu-id="ff31b-130">Call the Face API with the provided image and get a response</span></span>

<span data-ttu-id="ff31b-131">Per effettuare una richiesta all'API Viso, è necessario aggiungere questo codice all'inizio della funzione `getFaces`</span><span class="sxs-lookup"><span data-stu-id="ff31b-131">To make a reques to the Face API we add this code to the top of the `getFaces` function</span></span>

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

<span data-ttu-id="ff31b-132">Il codice precedente usa il comando `fetch` per inviare una richiesta `POST` all'API Viso.</span><span class="sxs-lookup"><span data-stu-id="ff31b-132">The code above uses the `fetch` command to send a `POST` request to the Face API.</span></span>

<span data-ttu-id="ff31b-133">Viene passato `API_KEY` nell'intestazione in modo che l'API Viso sia in grado di determinare che la richiesta proviene dal mittente corretto, altrimenti viene rifiutata.</span><span class="sxs-lookup"><span data-stu-id="ff31b-133">We pass the `API_KEY` in the header so the Face API knows the request comes from us, otherwise the request is rejected.</span></span>

<span data-ttu-id="ff31b-134">Passare `imageUrl` che l'API Viso deve analizzare nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ff31b-134">We pass the `imageUrl` we want the Face API to analyse in the body.</span></span>

<span data-ttu-id="ff31b-135">Viene quindi ottenuta la risposta dall'API Viso, che viene stampata.</span><span class="sxs-lookup"><span data-stu-id="ff31b-135">We then get the responce from the API request and print it out.</span></span>

<span data-ttu-id="ff31b-136">Se ora si esegue lo script con</span><span class="sxs-lookup"><span data-stu-id="ff31b-136">If you now run the script with</span></span>

```bash
node bin/mojify.js <path-to-image>
```

<span data-ttu-id="ff31b-137">Verrà stampata la risposta JSON restituita dal passaggio dell'immagine all'API Viso.</span><span class="sxs-lookup"><span data-stu-id="ff31b-137">It should print out the json responce returned from passing that image to the face API.</span></span>

## <a name="parse-the-responce"></a><span data-ttu-id="ff31b-138">Analizzare la risposta</span><span class="sxs-lookup"><span data-stu-id="ff31b-138">Parse the responce</span></span>

<span data-ttu-id="ff31b-139">Per calcolare le emoji, è necessario convertire ogni viso restituito nella risposta dell'API in un'istanza di una classe `Face`.</span><span class="sxs-lookup"><span data-stu-id="ff31b-139">To calculate the emojis ee need to convert each face returned in the responce from the API to an instance of a `Face` class.</span></span>

<span data-ttu-id="ff31b-140">Aggiungere questo codice subito dopo il codice necessario per chiamare l'API nella funzione `getFaces`:</span><span class="sxs-lookup"><span data-stu-id="ff31b-140">We add this code just after the code to call the API in the `getFaces` fucntion:</span></span>

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

- <span data-ttu-id="ff31b-141">Eseguire il ciclo di ogni viso restituito nella risposta.</span><span class="sxs-lookup"><span data-stu-id="ff31b-141">We loop through each face returned in the responce.</span></span>
- <span data-ttu-id="ff31b-142">Generare gli oggetti `EmotivePoint`, `Rect` e `Face` dal codice JSON restituito.</span><span class="sxs-lookup"><span data-stu-id="ff31b-142">We generate an `EmotivePoint`, a `Rect` and a `Face` from the returned json.</span></span>
- <span data-ttu-id="ff31b-143">La creazione di un'istanza di `Face` associa il viso a un'emoji</span><span class="sxs-lookup"><span data-stu-id="ff31b-143">Creating the `Face` instance matches the face to an emoji</span></span>
- <span data-ttu-id="ff31b-144">Per esaminare le emoji associate, stampare l'output di `mojiicon`.</span><span class="sxs-lookup"><span data-stu-id="ff31b-144">To see which emoji was matched we print out the `mojiicon`.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="ff31b-145">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="ff31b-145">Try it out</span></span>

<span data-ttu-id="ff31b-146">Se ora si esegue lo script, ecco cosa succederà:</span><span class="sxs-lookup"><span data-stu-id="ff31b-146">Now if you run the script it should:</span></span>

- <span data-ttu-id="ff31b-147">L'immagine fornita verrà passata tramite l'API Viso calcolando l'emozione.</span><span class="sxs-lookup"><span data-stu-id="ff31b-147">Pass the provided image through the Face API and calculate the emotion.</span></span>
- <span data-ttu-id="ff31b-148">Verranno associate emozioni a emoji.</span><span class="sxs-lookup"><span data-stu-id="ff31b-148">Match emotions to emojis.</span></span>
- <span data-ttu-id="ff31b-149">Verranno stampate le emoji nel terminale.</span><span class="sxs-lookup"><span data-stu-id="ff31b-149">Print the emojis to the terminal.</span></span>

<span data-ttu-id="ff31b-150">Ecco un esempio:</span><span class="sxs-lookup"><span data-stu-id="ff31b-150">Like so:</span></span>

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
