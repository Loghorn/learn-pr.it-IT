<span data-ttu-id="1e097-101">Non è possibile passare un'immagine dell'emoji all'API Viso per ottenerne l'emozione, in quanto non è un'immagine umana.</span><span class="sxs-lookup"><span data-stu-id="1e097-101">We can’t pass an image of the emoji to the Face API to get it’s emotion because, well because it’s not human.</span></span> <span data-ttu-id="1e097-102">Di conseguenza, per ogni emoji è stato necessario un proxy umano, ovvero l'autore stesso di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="1e097-102">So for each emoji, I needed a human proxy, me.</span></span>

<span data-ttu-id="1e097-103">L'autore ha scattato immagini di se stesso in modo _accurato_, simulando ogni emoji, e ha usato il _punto emotivo_ per l'immagine come proxy per l'emoji.</span><span class="sxs-lookup"><span data-stu-id="1e097-103">I took pictures of myself _accurately_ mimicking each emoji, and used the _emotional point_ for that image as the proxy for the emoji.</span></span> <span data-ttu-id="1e097-104">Per rendere ancora più interessante l'esperimento, l'autore ha scelto anche persone del suo team, associando anche loro a emoji, come mostrato qui:</span><span class="sxs-lookup"><span data-stu-id="1e097-104">To keep things interesting I also chose people from my team and associated them with emojis as well, like so:</span></span>

![Emoji del team](/media-drafts/team.jpg)

<span data-ttu-id="1e097-106">Per l'emoji degli occhi innamorati (😍), l'autore ha scelto un'immagine della moglie ❤️.</span><span class="sxs-lookup"><span data-stu-id="1e097-106">For the emoji of love eyes (😍) I chose a picture of my wife ❤️.</span></span> <span data-ttu-id="1e097-107">In memoria di [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking), è stata selezionata un'immagine dello scienziato per rappresentare 🤔.</span><span class="sxs-lookup"><span data-stu-id="1e097-107">In memory of [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking) I picked a picture of him to represent 🤔.</span></span>

<span data-ttu-id="1e097-108">L'elenco delle immagini proxy per ogni emoji è disponibile nella cartella `bin/proxy-images` nel codice di esempio associato a questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1e097-108">You can see the list of proxy images for each emoji in the `bin/proxy-images` folder in the sample code associated with this tutorial.</span></span>

<span data-ttu-id="1e097-109">In questo capitolo si genererà una chiave in modo da poter usare l'API Viso di Azure per calibrare tutte le emoji usando immagini proxy dell'autore.</span><span class="sxs-lookup"><span data-stu-id="1e097-109">In this chapter you will generate a key so you can use the Azure Face API and then use the Face API to calibrate all the emojies using proxied images of me.</span></span>

## <a name="generate-an-azure-face-api-key"></a><span data-ttu-id="1e097-110">Generare una chiave API Viso di Azure</span><span class="sxs-lookup"><span data-stu-id="1e097-110">Generate an Azure Face API Key</span></span>

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

<span data-ttu-id="1e097-111">Per usare l'API Viso di Azure, è necessaria una chiave di autenticazione speciale. Vedere https://azure.microsoft.com/try/cognitive-services/ e registrarsi per ottenere la versione di valutazione dell'API Viso.</span><span class="sxs-lookup"><span data-stu-id="1e097-111">To use the Azure Face API we need a special authentication key, head over to https://azure.microsoft.com/try/cognitive-services/ and signup to trial the Face API.</span></span>

![Emoji del team](/media-drafts/4.calibrating-emojis.get-face-api.png)

> <span data-ttu-id="1e097-113">TODO: Trovare i comandi az per creare l'API Viso e recuperare le chiavi</span><span class="sxs-lookup"><span data-stu-id="1e097-113">TODO: Find az commands to create faceAPI and grab keys</span></span>

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## <a name="setup-the-environment-variables"></a><span data-ttu-id="1e097-114">Configurare le variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1e097-114">Setup the environment variables</span></span>

<span data-ttu-id="1e097-115">Lo script di calibrazione deve avere l'URL e la chiave dell'API Viso per poter effettuare le chiamate corrette. Invece di impostarli come hardcoded nello script, verranno usate variabili di ambiente. Eseguire questi comandi nel terminale in cui si prevede di eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="1e097-115">The calibration script needs to know your Face API URL and Key in order to make the correct calls, rather than hardcoding these in the script we are going to use environment varialbes, run these commands in the terminal you expect to run the application in:</span></span>

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## <a name="create-some-proxy-images-for-emojis"></a><span data-ttu-id="1e097-116">Creare alcune immagini proxy per le emoji</span><span class="sxs-lookup"><span data-stu-id="1e097-116">Create some proxy images for emojis</span></span>

<span data-ttu-id="1e097-117">Benché le immagini proxy vengano già fornite qui, è possibile generarne di personalizzate.</span><span class="sxs-lookup"><span data-stu-id="1e097-117">I've provided all the proxy images myself, but feel free to generate your own!</span></span>

<span data-ttu-id="1e097-118">Per ogni emoji nella cartella `bin/proxy-images`, acquisire un'immagine di se stessi simulando l'emoji e sostituire l'immagine con la propria.</span><span class="sxs-lookup"><span data-stu-id="1e097-118">For each emoji in the `bin/proxy-images` folder, take a picture of yourself mimicking that emoji and replace the image with your image.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="1e097-119">Provare questa operazione</span><span class="sxs-lookup"><span data-stu-id="1e097-119">Try it out</span></span>

<span data-ttu-id="1e097-120">Questa è la parte più divertente!</span><span class="sxs-lookup"><span data-stu-id="1e097-120">Now comes the fun part!</span></span> <span data-ttu-id="1e097-121">Ognuna delle immagini verrà eseguita in `bin/proxy-images` tramite l'API Viso per calcolare un punto emotivo per l'emoji nello _spazio emotivo_. A questo scopo, eseguire:</span><span class="sxs-lookup"><span data-stu-id="1e097-121">We are going to run each of the images in the `bin/proxy-images` through the Face API to calculate an emotional point for that emoji in _emotional space_, run:</span></span>

```bash
node bin/calibrate.js
```

<span data-ttu-id="1e097-122">L'output di questo comando sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1e097-122">The output of this command should look something like so:</span></span>

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

<span data-ttu-id="1e097-123">Verrà innanzitutto stampata l'emoji elaborata e infine verrà stampata nella console una matrice che definisce i valori della classe `EmotivePoint` di tutte l'emoji.</span><span class="sxs-lookup"><span data-stu-id="1e097-123">It will first print out the emoji's it is processing and then finally print out to the console an array which defines the `EmotivePoint` of all the emoji's.</span></span> <span data-ttu-id="1e097-124">Questa matrice ha lo stesso formato di quella in `shared/mojis.ts`.</span><span class="sxs-lookup"><span data-stu-id="1e097-124">This is the same format as the array in `shared/mojis.ts`.</span></span>

<span data-ttu-id="1e097-125">Se alcune delle immagini proxy sono state modificate, copiare l'output di questo script nella sezione pertinente di `mojis.ts`</span><span class="sxs-lookup"><span data-stu-id="1e097-125">If you changed some of the proxied images then copy the output of this script to the relevant section of `mojis.ts`</span></span>
