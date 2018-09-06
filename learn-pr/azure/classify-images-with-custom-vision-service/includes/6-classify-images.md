### <a name="exercise-6-use-the-app-to-classify-images"></a><span data-ttu-id="afff3-101">Esercizio 6: Usare l'app per classificare immagini</span><span class="sxs-lookup"><span data-stu-id="afff3-101">Exercise 6: Use the app to classify images</span></span>

<span data-ttu-id="afff3-102">In questo esercizio si userà l'app Artworks per inviare immagini al Servizio visione artificiale personalizzato per la classificazione.</span><span class="sxs-lookup"><span data-stu-id="afff3-102">In this exercise, you will use the Artworks app to submit images to the Custom Vision Service for classification.</span></span> <span data-ttu-id="afff3-103">L'app usa le informazioni JSON restituite da chiamate al metodo [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) dell'API per le stime del Servizio visione artificiale personalizzato per indicare quando un'immagine rappresenta un dipinto di Picasso, Rembrandt, Pollock o di nessuno di questi artisti.</span><span class="sxs-lookup"><span data-stu-id="afff3-103">The app uses the JSON information returned from calls to the Custom Vision Prediction API's [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) method to tell you whether an image represents a painting by Picasso, Rembrandt, Pollock, or none of the above.</span></span> <span data-ttu-id="afff3-104">L'app indica anche la probabilità che la classificazione assegnata all'immagine sia corretta.</span><span class="sxs-lookup"><span data-stu-id="afff3-104">It also shows the probability that the classification assigned to the image is correct.</span></span>

1. <span data-ttu-id="afff3-105">Fare clic sul pulsante **Browse** (Sfoglia) nell'app Artworks.</span><span class="sxs-lookup"><span data-stu-id="afff3-105">Click the **Browse (...)** button in the Artworks app.</span></span> 

    ![Esplorazione di immagini locali nell'app Artworks](../images/app-click-browse.png)

    <span data-ttu-id="afff3-107">_Esplorazione di immagini locali nell'app Artworks_</span><span class="sxs-lookup"><span data-stu-id="afff3-107">_Browsing for local images in the Artworks app_</span></span> 

1. <span data-ttu-id="afff3-108">Passare alla cartella "Quick Tests" nelle risorse del lab.</span><span class="sxs-lookup"><span data-stu-id="afff3-108">Browse to the "Quick Tests" folder in the lab resources.</span></span> <span data-ttu-id="afff3-109">Selezionare il file denominato **PicassoTest_02.jpg** e quindi fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="afff3-109">Select the file named **PicassoTest_02.jpg**, and then click **Open**.</span></span>

1. <span data-ttu-id="afff3-110">Fare clic sul pulsante **Predict** (Stima) per inviare l'immagine al Servizio visione artificiale personalizzato.</span><span class="sxs-lookup"><span data-stu-id="afff3-110">Click the **Predict** button to submit the image to the Custom Vision Service.</span></span>

    ![Invio dell'immagine al Servizio visione artificiale personalizzato](../images/app-click-predict.png)

    <span data-ttu-id="afff3-112">_Invio dell'immagine al Servizio visione artificiale personalizzato_</span><span class="sxs-lookup"><span data-stu-id="afff3-112">_Submitting the image to the Custom Vision Service_</span></span> 

1. <span data-ttu-id="afff3-113">Verificare che l'app identifichi il dipinto come opera di Picasso.</span><span class="sxs-lookup"><span data-stu-id="afff3-113">Confirm that the app identifies the painting as a Picasso.</span></span>

    ![Classificazione di un'immagine come opera di Picasso](../images/app-prediction-01.png)

    <span data-ttu-id="afff3-115">_Classificazione di un'immagine come opera di Picasso_</span><span class="sxs-lookup"><span data-stu-id="afff3-115">_Classifying an image as a Picasso_</span></span> 

1. <span data-ttu-id="afff3-116">Ripetere i passaggi da 1 a 3 per **RembrandtTest_01.jpg** e **PollockTest_01.jpg** e verificare che l'app sia in grado di identificare i dipinti come opere di Rembrandt e Pollock.</span><span class="sxs-lookup"><span data-stu-id="afff3-116">Repeat steps 1 through 3 for **RembrandtTest_01.jpg** and **PollockTest_01.jpg** and confirm that the app can identify paintings by Rembrandt and Pollock.</span></span>

    ![Classificazione di un'immagine come opera di Rembrandt](../images/app-prediction-02.png)

    <span data-ttu-id="afff3-118">_Classificazione di un'immagine come opera di Rembrandt_</span><span class="sxs-lookup"><span data-stu-id="afff3-118">_Classifying an image as a Rembrandt_</span></span> 

1. <span data-ttu-id="afff3-119">Ripetere i passaggi da 1 a 3 per **VanGoghTest_01.png** e **VanGoghTest_02.png** e verificare che l'app non identifichi questi capolavori di Van Gogh come opere di Picasso, Rembrandt o Pollock.</span><span class="sxs-lookup"><span data-stu-id="afff3-119">Repeat steps 1 through 3 for **VanGoghTest_01.png** and **VanGoghTest_02.png** and confirm that the app does not identify these Van Gogh masterworks as paintings by Picasso, Rembrandt, or Pollock.</span></span>

    ![Opera identificata come non di Picasso, Rembrandt o Pollock](../images/app-prediction-03.png)

    <span data-ttu-id="afff3-121">_Opera identificata come non di Picasso, Rembrandt o Pollock_</span><span class="sxs-lookup"><span data-stu-id="afff3-121">_Not a Picasso, Rembrandt, or Pollock_</span></span> 

1. <span data-ttu-id="afff3-122">Come si può osservare, l'uso dell'API per le stime da un'app è altrettanto affidabile quanto tramite il portale del Servizio visione artificiale personalizzato e anche molto più divertente.</span><span class="sxs-lookup"><span data-stu-id="afff3-122">As you can see, using the Prediction API from an app is as reliable as through the Custom Vision Service portal — and way more fun!</span></span> <span data-ttu-id="afff3-123">Inoltre, se si passa alla pagina Predictions (Stime) nel portale, anche qui verrà visualizzata ognuna delle immagini caricate tramite l'app.</span><span class="sxs-lookup"><span data-stu-id="afff3-123">What's more, if you go to the Predictions page in the portal, you'll find each of the images uploaded via the app shown there as well.</span></span>
 
    ![Immagine inviata al Servizio visione artificiale personalizzato](../images/portal-all-predictions.png)

    <span data-ttu-id="afff3-125">_Immagine inviata al Servizio visione artificiale personalizzato_</span><span class="sxs-lookup"><span data-stu-id="afff3-125">_image submitted to the Custom Vision Service_</span></span> 

<span data-ttu-id="afff3-126">È possibile testare liberamente immagini personali e valutare la capacità del modello di identificare gli artisti o di determinare un'immagine come non di Picasso, Rembrandt o Pollock.</span><span class="sxs-lookup"><span data-stu-id="afff3-126">Feel free to test with images of your own and gauge the model's adeptness at identifying artists or determining that an image is not a Picasso, Rembrandt, or Pollock.</span></span> <span data-ttu-id="afff3-127">Se si vuole eseguire il training del modello perché riconosca anche le opere di Van Gogh, caricare alcuni dipinti di Van Gogh, corredarli del tag "Van Gogh" ed eseguire di nuovo il training del modello.</span><span class="sxs-lookup"><span data-stu-id="afff3-127">If you'd like to train it to recognize Van Goghs, too, upload some Van Gogh paintings, tag them with "Van Gogh," and retrain the model.</span></span> <span data-ttu-id="afff3-128">Non vi sono limiti all'intelligenza che è possibile aggiungere se si è disposti a eseguire nuovi training.</span><span class="sxs-lookup"><span data-stu-id="afff3-128">There is no limit to the intelligence you can add if you're willing to do the training.</span></span> <span data-ttu-id="afff3-129">Ricordare inoltre che in generale più sono le immagini di cui si esegue il training, più intelligente sarà il modello.</span><span class="sxs-lookup"><span data-stu-id="afff3-129">And remember that in general, the more images you train with, the smarter the model will be.</span></span>