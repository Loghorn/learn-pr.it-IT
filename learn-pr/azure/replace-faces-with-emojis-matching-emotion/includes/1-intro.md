<span data-ttu-id="2f32f-101">Mojifier è un comando _slash_ di Slack che sostituisce i visi delle persone nelle immagini con emoji corrispondenti alle loro emozioni, in questo modo:</span><span class="sxs-lookup"><span data-stu-id="2f32f-101">TheMojifier is a Slack _slash_ command which replaces peoples faces in images with emojis matching their emotion, like so:</span></span>

![Immagine di esempio](/media-drafts/example-mojify-image.png)

<span data-ttu-id="2f32f-103">È progettato per essere usato da Slack come comando personalizzato ed è possibile assegnare al comando il nome desiderato. In questo documento è stato chiamato `mojify`.</span><span class="sxs-lookup"><span data-stu-id="2f32f-103">It's designed to work from Slack as a custom command, you can name the command how you want, for this document I've named it `mojify`.</span></span>

<span data-ttu-id="2f32f-104">Per eseguire il comando, digitare `/mojify <image to mojify>`, in questo modo:</span><span class="sxs-lookup"><span data-stu-id="2f32f-104">To execute the commmand type `/mojify <image to mojify>`, like so:</span></span>

![Immagine di esempio](/media-drafts/9.slack-type-mojify.png)

<span data-ttu-id="2f32f-106">Il comando mojifier:</span><span class="sxs-lookup"><span data-stu-id="2f32f-106">The mojifier then:</span></span>

1.  <span data-ttu-id="2f32f-107">Calcola l'emozione di qualsiasi persona presente nell'immagine.</span><span class="sxs-lookup"><span data-stu-id="2f32f-107">Calculates the emotion of any people in the image.</span></span>
2.  <span data-ttu-id="2f32f-108">Associa le emozioni a emoji.</span><span class="sxs-lookup"><span data-stu-id="2f32f-108">Matches emotions to emojis.</span></span>
3.  <span data-ttu-id="2f32f-109">Sostituisce i visi con emoji.</span><span class="sxs-lookup"><span data-stu-id="2f32f-109">Replaces the faces with emojis.</span></span>
4.  <span data-ttu-id="2f32f-110">Ripubblica l'immagine in Twitter come risposta.</span><span class="sxs-lookup"><span data-stu-id="2f32f-110">Posts the image back to Twitter as a reply.</span></span>

<span data-ttu-id="2f32f-111">Viene scritto tramite TypeScript e diverse tecnologie di Azure, tra cui [Funzioni di Azure](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) e [Servizi cognitivi di Azure](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)</span><span class="sxs-lookup"><span data-stu-id="2f32f-111">It’s written using TypeScript and several Azure technologies including [Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) and [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)</span></span>

<span data-ttu-id="2f32f-112">In questa esercitazione viene descritto come è stato realizzato Mojifier e come creare un comando di Slack personalizzato usando tecnologie di Azure.</span><span class="sxs-lookup"><span data-stu-id="2f32f-112">In this tutorial I’m going to explain how TheMojifier was made and show you how to create your own Slack command using Azure technologies.</span></span>

> <span data-ttu-id="2f32f-113">TODO, qual è la posizione?</span><span class="sxs-lookup"><span data-stu-id="2f32f-113">TODO, where will this be now?</span></span>
> <span data-ttu-id="2f32f-114">Tutto il codice per Mojifier è disponibile in [GitHub](https://github.com/jawache/mojifier)</span><span class="sxs-lookup"><span data-stu-id="2f32f-114">All the code for Mojifier is available on [GitHub](https://github.com/jawache/mojifier)</span></span>

# <a name="requirements"></a><span data-ttu-id="2f32f-115">Requisiti</span><span class="sxs-lookup"><span data-stu-id="2f32f-115">Requirements</span></span>

<span data-ttu-id="2f32f-116">Per creare il comando mojifier, è necessario usare diversi servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="2f32f-116">To build the mojifier, we need to use several Azure services.</span></span>

## <a name="azure-cognitive-services"></a><span data-ttu-id="2f32f-117">Servizi cognitivi di Azure</span><span class="sxs-lookup"><span data-stu-id="2f32f-117">Azure Cognitive Services</span></span>

<span data-ttu-id="2f32f-118">Servizi cognitivi di Azure è un set di API di alto livello che può essere usato per aggiungere rapidamente funzionalità di intelligenza artificiale avanzate nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f32f-118">Azure Cognitive Services are a set of high-level APIs you can use to add advanced AI functionality into your application quickly.</span></span> <span data-ttu-id="2f32f-119">Se è possibile effettuare una richiesta HTTP, è possibile usare Servizi cognitivi.</span><span class="sxs-lookup"><span data-stu-id="2f32f-119">If you can make an HTTP request, you can use Cognitive Services.</span></span>

[<span data-ttu-id="2f32f-120">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="2f32f-120">More info</span></span>](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## <a name="azure-functions"></a><span data-ttu-id="2f32f-121">Funzioni di Azure</span><span class="sxs-lookup"><span data-stu-id="2f32f-121">Azure Functions</span></span>

<span data-ttu-id="2f32f-122">Servizio potente come App per la logica. A volte è necessario scrivere logica di business sfruttando l'espressività completa di un linguaggio di programmazione.</span><span class="sxs-lookup"><span data-stu-id="2f32f-122">As powerful as Logic Apps are sometimes you need to write business logic using the full expressiveness of a programming language.</span></span> <span data-ttu-id="2f32f-123">Funzioni di Azure è una tecnologia che permette di ospitare frammenti di codice in grado di rispondere a eventi o a richieste HTTP, mentre Azure gestisce tutti i problemi di scalabilità e permette di pagare solo per le risorse effettivamente usate.</span><span class="sxs-lookup"><span data-stu-id="2f32f-123">Azure Functions is a technology that lets you host snippets of code that can respond to events or HTTP requests, Azure handles all of the scaling issues for you and you only pay for what you use.</span></span>

[<span data-ttu-id="2f32f-124">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="2f32f-124">More info</span></span>](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
