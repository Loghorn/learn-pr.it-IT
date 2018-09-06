### <a name="exercise-4-test-the-model"></a><span data-ttu-id="e4b3e-101">Esercizio 4: Testare il modello</span><span class="sxs-lookup"><span data-stu-id="e4b3e-101">Exercise 4: Test the model</span></span>

<span data-ttu-id="e4b3e-102">In [Esercizio 5](../5-build-app.yml) si creerà un'app Node.js che usa il modello per identificare l'artista dei dipinti presentati.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-102">In [Exercise 5](../5-build-app.yml), you will create a Node.js app that uses the model to identify the artist of paintings presented to it.</span></span> <span data-ttu-id="e4b3e-103">Tuttavia, non è necessario scrivere un'app per testare il modello, ma è possibile eseguire i test nel portale e perfezionare ulteriormente il modello tramite le immagini usate per i test.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-103">But you don't have to write an app to test the model; you can do your testing in the portal, and you can further refine the model using the images that you test with.</span></span> <span data-ttu-id="e4b3e-104">In questo esercizio si testerà la capacità del modello di identificare l'artista di un dipinto usando immagini di test fornite.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-104">In this exercise, you will test the model's ability to identify the artist of a painting using test images provided for you.</span></span>

1. <span data-ttu-id="e4b3e-105">Fare clic su **Quick Test** (Test rapido) nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-105">Click **Quick Test** at the top of the page.</span></span>
 
    ![Test del modello](../images/portal-click-quick-test.png)

    <span data-ttu-id="e4b3e-107">_Test del modello_</span><span class="sxs-lookup"><span data-stu-id="e4b3e-107">_Testing the model_</span></span> 

1. <span data-ttu-id="e4b3e-108">Fare clic su **Esplora file locali** e quindi passare alla cartella "Quick Tests" nelle risorse del lab.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-108">Click **Browse local files**, and then browse to the "Quick Tests" folder in the lab resources.</span></span> <span data-ttu-id="e4b3e-109">Selezionare **PicassoTest_01.jpg** e quindi fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-109">Select **PicassoTest_01.jpg**, and click **Open**.</span></span>

    ![Selezione di un'immagine di test di Picasso](../images/portal-select-test-01.png)

    <span data-ttu-id="e4b3e-111">_Selezione di un'immagine di test di Picasso_</span><span class="sxs-lookup"><span data-stu-id="e4b3e-111">_Selecting a Picasso test image_</span></span> 

1. <span data-ttu-id="e4b3e-112">Esaminare i risultati del test nella finestra di dialogo "Quick Test" (Test rapido).</span><span class="sxs-lookup"><span data-stu-id="e4b3e-112">Examine the results of the test in the "Quick Test" dialog.</span></span> <span data-ttu-id="e4b3e-113">Qual è la probabilità che il dipinto sia un Picasso?</span><span class="sxs-lookup"><span data-stu-id="e4b3e-113">What is the probability that the painting is a Picasso?</span></span> <span data-ttu-id="e4b3e-114">Qual è la probabilità che sia un Rembrandt o un Pollock?</span><span class="sxs-lookup"><span data-stu-id="e4b3e-114">What is the probability that it is a Rembrandt or Pollock?</span></span>

1. <span data-ttu-id="e4b3e-115">Chiudere la finestra di dialogo "Quick Test" (Test rapido).</span><span class="sxs-lookup"><span data-stu-id="e4b3e-115">Close the "Quick Test" dialog.</span></span> <span data-ttu-id="e4b3e-116">Nella parte superiore della pagina fare quindi clic su **Predictions** (Stime).</span><span class="sxs-lookup"><span data-stu-id="e4b3e-116">Then click **Predictions** at the top of the page.</span></span>
 
    ![Visualizzazione dei test eseguiti](../images/portal-select-predictions.png)

    <span data-ttu-id="e4b3e-118">_Visualizzazione dei test eseguiti_</span><span class="sxs-lookup"><span data-stu-id="e4b3e-118">_Viewing the tests that have been performed_</span></span> 

1. <span data-ttu-id="e4b3e-119">Fare clic sull'immagine di test caricata per mostrarne un dettaglio.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-119">Click the test image that you uploaded to show a detail of it.</span></span> <span data-ttu-id="e4b3e-120">Aggiungere all'immagine un tag "Picasso" selezionando **Picasso** nell'elenco a discesa e quindi fare clic su **Salva e chiudi**.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-120">Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.</span></span>

    > <span data-ttu-id="e4b3e-121">Aggiungendo tag alle immagini di test in questo modo, è possibile perfezionare il modello senza caricare altre immagini di training.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-121">By tagging test images this way, you can refine the model without uploading additional training images.</span></span>
 
    ![Aggiunta di tag all'immagine di test](../images/tag-test-image.png)

    <span data-ttu-id="e4b3e-123">_Aggiunta di tag all'immagine di test_</span><span class="sxs-lookup"><span data-stu-id="e4b3e-123">_Tagging the test image_</span></span> 

1. <span data-ttu-id="e4b3e-124">Eseguire un altro test rapido usando il file denominato **FlowersTest.jpg** nella cartella "Quick Tests".</span><span class="sxs-lookup"><span data-stu-id="e4b3e-124">Perform another quick test using the file named **FlowersTest.jpg** in the "Quick Test" folder.</span></span> <span data-ttu-id="e4b3e-125">Verificare che a questa immagine venga assegnata una bassa probabilità che si tratti di un Picasso, un Rembrandt o un Pollock.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-125">Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.</span></span>

<span data-ttu-id="e4b3e-126">Il modello è stato sottoposto a training ed è pronto per l'uso e sembra in grado di identificare i dipinti di determinati artisti.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-126">The model is trained and ready to go and appears to be adept at identifying paintings by certain artists.</span></span> <span data-ttu-id="e4b3e-127">A questo punto, si compirà un ulteriore passo avanti, integrando l'intelligenza del modello in un'app.</span><span class="sxs-lookup"><span data-stu-id="e4b3e-127">Now let's go a step further and incorporate the model's intelligence into an app.</span></span>