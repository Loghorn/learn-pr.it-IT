<span data-ttu-id="8e626-101">In questa unità si aggiungeranno immagini di celebri dipinti di Picasso, Pollock e Rembrandt al progetto Artworks, assegnando tag alle immagini in modo che il Servizio visione artificiale personalizzato apprenda a differenziare un artista dall'altro.</span><span class="sxs-lookup"><span data-stu-id="8e626-101">In this unit, you will add images of famous paintings by Picasso, Pollock, and Rembrandt to the Artworks project, and tag the images so the Custom Vision Service can learn to differentiate one artist from another.</span></span>
  
1. <span data-ttu-id="8e626-102">Fare clic su **Add images** (Aggiungi immagini) per aggiungere immagini al progetto.</span><span class="sxs-lookup"><span data-stu-id="8e626-102">Click **Add images** to add images to the project.</span></span>

    ![Aggiunta di immagini al progetto Artworks](../media-draft/2-portal-click-add-images.png)

1. <span data-ttu-id="8e626-104">Fare clic su **Esplora file locali**.</span><span class="sxs-lookup"><span data-stu-id="8e626-104">Click **Browse local files**.</span></span>

    ![Esplorazione di immagini locali](../media-draft/2-portal-click-browse-local-files.png)

    <span data-ttu-id="8e626-106">_Esplorazione di immagini locali_</span><span class="sxs-lookup"><span data-stu-id="8e626-106">_Browsing for local images_</span></span> 
 
1. <span data-ttu-id="8e626-107">Passare alla cartella "Artists\Picasso" nelle [risorse fornite con questo modulo](https://a4r.blob.core.windows.net/public/cvs-resources.zip), selezionare tutti i file nella cartella e quindi fare clic su **Apri**.</span><span class="sxs-lookup"><span data-stu-id="8e626-107">Browse to the "Artists\Picasso" folder in the [resources that accompany this module](https://a4r.blob.core.windows.net/public/cvs-resources.zip), select all of the files in the folder, and click **Open**.</span></span>

    ![Selezione di un'immagine](../media-draft/2-fe-browse-picasso-01.png)

1. <span data-ttu-id="8e626-109">Digitare "painting" (senza virgolette) nella casella **Add some tags** (Aggiungi alcuni tag).</span><span class="sxs-lookup"><span data-stu-id="8e626-109">Type "painting" (without quotation marks) into the **Add some tags...** box.</span></span> <span data-ttu-id="8e626-110">Fare clic su **+** per assegnare il tag alle immagini.</span><span class="sxs-lookup"><span data-stu-id="8e626-110">Then click **+** to assign the tag to the images.</span></span>

    ![Aggiunta di un tag "painting" alle immagini](../media-draft/2-portal-add-tags-01.png)

1. <span data-ttu-id="8e626-112">Ripetere il passaggio 4 per aggiungere un tag "Picasso" alle immagini.</span><span class="sxs-lookup"><span data-stu-id="8e626-112">Repeat Step 4 to add a "Picasso" tag to the images.</span></span>

1. <span data-ttu-id="8e626-113">Fare clic su **Upload 7 files** per caricare le immagini.</span><span class="sxs-lookup"><span data-stu-id="8e626-113">Click **Upload 7 files** to upload the images.</span></span> <span data-ttu-id="8e626-114">Al termine del caricamento, fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="8e626-114">Once the upload has finished, click **Done**.</span></span>

    ![Caricamento di immagini con tag](../media-draft/2-upload-picasso-images.png)

1. <span data-ttu-id="8e626-116">Verificare che le immagini caricate siano visualizzate nel portale, insieme ai tag assegnati.</span><span class="sxs-lookup"><span data-stu-id="8e626-116">Confirm that the images you uploaded appear in the portal, along with the tags assigned to them.</span></span>

    ![Immagini caricate](../media-draft/2-portal-tagged-01.png)

1. <span data-ttu-id="8e626-118">Con sette immagini di Picasso, il Servizio visione artificiale personalizzato è in grado di identificare piuttosto correttamente i dipinti di Picasso.</span><span class="sxs-lookup"><span data-stu-id="8e626-118">With seven Picasso images, the Custom Vision Service can do a decent job of identifying paintings by Picasso.</span></span> <span data-ttu-id="8e626-119">Tuttavia, se si esegue il training del modello a questo punto, il modello sarà in grado di riconoscere solo a cosa assomiglia un Picasso, ma non di identificare i dipinti di altri artisti.</span><span class="sxs-lookup"><span data-stu-id="8e626-119">But if you trained the model right now, it would only understand what a Picasso looks like, and it would not be able to identify paintings by other artists.</span></span>

    <span data-ttu-id="8e626-120">Il passaggio successivo consiste nel caricare alcuni dipinti di altri artisti.</span><span class="sxs-lookup"><span data-stu-id="8e626-120">The next step is to upload some paintings by another artist.</span></span> <span data-ttu-id="8e626-121">Fare clic su **Add images** (Aggiungi immagini) e selezionare tutte le immagini contenute nella cartella "Artists\Rembrandt" nelle risorse del modulo.</span><span class="sxs-lookup"><span data-stu-id="8e626-121">Click **Add images** and select all of the images in the "Artists\Rembrandt" folder in the module resources.</span></span> <span data-ttu-id="8e626-122">Assegnare alle immagini i tag "painting" e "Rembrandt" (non "Picasso") e caricarle nel progetto.</span><span class="sxs-lookup"><span data-stu-id="8e626-122">Tag them with the labels "painting" and "Rembrandt" (not "Picasso"), and upload them to the project.</span></span>

    > <span data-ttu-id="8e626-123">Per aggiungere il tag "painting," non è necessario digitarlo di nuovo.</span><span class="sxs-lookup"><span data-stu-id="8e626-123">When you add the tag "painting," you don't have to type it in again.</span></span> <span data-ttu-id="8e626-124">È possibile selezionarlo nell'elenco a discesa in corrispondenza della casella **Add some tags** (Aggiungi alcuni tag), come mostrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8e626-124">You can select it from the drop-down list attached to the **Add some tags...** box, as shown below.</span></span> <span data-ttu-id="8e626-125">Sarà **invece** necessario digitare "Rembrandt" e fare clic su **+** per aggiungere un tag "Rembrandt".</span><span class="sxs-lookup"><span data-stu-id="8e626-125">You **will** have to type "Rembrandt" and click **+** to add a "Rembrandt" tag.</span></span>

    ![Selezione di un tag esistente](../media-draft/2-select-painting-tag.png)

1. <span data-ttu-id="8e626-127">Verificare che le immagini di Rembrandt siano visualizzate accanto a quelle di Picasso nel progetto e che "Rembrandt" sia incluso nell'elenco di tag.</span><span class="sxs-lookup"><span data-stu-id="8e626-127">Confirm that the Rembrandt images appear alongside the Picasso images in the project, and that "Rembrandt" appears in the list of tags.</span></span>

    ![Immagini di Picasso e Rembrandt](../media-draft/2-portal-tagged-02.png)

1. <span data-ttu-id="8e626-129">Si aggiungeranno ora i dipinti dell'enigmatico artista Jackson Pollock per permettere al Servizio visione artificiale personalizzato di riconoscere anche i suoi dipinti.</span><span class="sxs-lookup"><span data-stu-id="8e626-129">Now add paintings by the enigmatic artist Jackson Pollock to enable the Custom Vision Service to recognize Pollock paintings, too.</span></span> <span data-ttu-id="8e626-130">Selezionare tutte le immagini contenute nella cartella "Artists\Pollock" nelle risorse del modulo, aggiungere alle immagini i tag con i termini "painting" e "Pollock" e quindi caricare le immagini nel progetto.</span><span class="sxs-lookup"><span data-stu-id="8e626-130">Select all of the images in the "Artists\Pollock" folder in the module resources, tag them with the terms "painting" and "Pollock", and upload them to the project.</span></span>

<span data-ttu-id="8e626-131">Dopo aver caricato le immagini con tag, il passaggio successivo consiste nell'eseguire il training del modello con queste immagini in modo che sia in grado di distinguere tra i dipinti di Picasso, Rembrandt e Pollock e di determinare se un dipinto è opera di uno di questi celebri artisti.</span><span class="sxs-lookup"><span data-stu-id="8e626-131">With the tagged images uploaded, the next step is to train the model with these images so it can distinguish between paintings by Picasso, Rembrandt, and Pollock, as well as determine whether a painting is a work by one of these famous artists.</span></span>