<span data-ttu-id="dda7d-101">È stato eseguito il training del modello ed è giunto il momento di testarlo.</span><span class="sxs-lookup"><span data-stu-id="dda7d-101">Now that we've trained our model, it's time to test it.</span></span> <span data-ttu-id="dda7d-102">Verranno fornite nuove immagini al modello e si esaminerà la capacità del modello di classificarle.</span><span class="sxs-lookup"><span data-stu-id="dda7d-102">We'll give the model new images and see how well it classifies it.</span></span>

1. <span data-ttu-id="dda7d-103">Fare clic su **Quick Test** (Test rapido) nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="dda7d-103">Click **Quick Test** at the top of the page.</span></span>

    ![Test del modello](../media/4-portal-click-quick-test.png)

1. <span data-ttu-id="dda7d-105">Fare clic su **Esplora file locali** e quindi passare alla cartella "Quick Tests" nella cartella delle risorse del modulo scaricata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="dda7d-105">Click **Browse local files**, and then browse to the "Quick Tests" folder in the module resources folder you download previously.</span></span> <span data-ttu-id="dda7d-106">Selezionare **PicassoTest_01.jpg** e quindi fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="dda7d-106">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![Selezione di un'immagine di test di Picasso](../media/4-portal-select-test-01.png)

1. <span data-ttu-id="dda7d-108">Esaminare i risultati del test nella finestra di dialogo "Quick Test" (Test rapido).</span><span class="sxs-lookup"><span data-stu-id="dda7d-108">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="dda7d-109">Qual è la probabilità che il dipinto sia un Picasso?</span><span class="sxs-lookup"><span data-stu-id="dda7d-109">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="dda7d-110">Qual è la probabilità che sia un Rembrandt o un Pollock?</span><span class="sxs-lookup"><span data-stu-id="dda7d-110">What is the probability that it's a Rembrandt or Pollock?</span></span>

    ![Selezione di un'immagine di test di Picasso](../media/4-quick-test-result.png)

1. <span data-ttu-id="dda7d-112">Chiudere la finestra di dialogo "Quick Test" (Test rapido).</span><span class="sxs-lookup"><span data-stu-id="dda7d-112">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="dda7d-113">Nella parte superiore della pagina fare quindi clic su **Predictions** (Stime).</span><span class="sxs-lookup"><span data-stu-id="dda7d-113">Then click **Predictions** at the top of the page.</span></span>

    ![Visualizzazione dei test eseguiti](../media/4-portal-select-predictions.png)

1. <span data-ttu-id="dda7d-115">Fare clic sull'immagine di test caricata per mostrarne un dettaglio.</span><span class="sxs-lookup"><span data-stu-id="dda7d-115">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="dda7d-116">Aggiungere all'immagine un tag "Picasso" selezionando **Picasso** nell'elenco a discesa e quindi fare clic su **Salva e chiudi**.</span><span class="sxs-lookup"><span data-stu-id="dda7d-116">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="dda7d-117">Aggiungendo tag alle immagini di test in questo modo, è possibile perfezionare il modello senza caricare altre immagini di training.</span><span class="sxs-lookup"><span data-stu-id="dda7d-117">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>

    ![Aggiunta di tag all'immagine di test](../media/4-tag-test-image.png)

1. <span data-ttu-id="dda7d-119">Eseguire un altro test rapido usando questa volta il file denominato **FlowersTest.jpg** nella cartella "Quick Test".</span><span class="sxs-lookup"><span data-stu-id="dda7d-119">Run another quick test, this time using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="dda7d-120">Verificare che a questa immagine sia assegnata una bassa probabilità che si tratti di un Picasso, un Rembrandt o un Pollock.</span><span class="sxs-lookup"><span data-stu-id="dda7d-120">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="dda7d-121">Il modello è stato sottoposto a training ed è pronto per l'uso e sembra in grado di identificare i dipinti di determinati artisti.</span><span class="sxs-lookup"><span data-stu-id="dda7d-121">The model is trained and ready to go and appears to be skilled at identifying paintings by certain artists.</span></span> <span data-ttu-id="dda7d-122">È possibile chiamare l'endpoint di stima tramite HTTP e vedere cosa succede.</span><span class="sxs-lookup"><span data-stu-id="dda7d-122">Let's call the prediction endpoint over HTTP and see what happens.</span></span>