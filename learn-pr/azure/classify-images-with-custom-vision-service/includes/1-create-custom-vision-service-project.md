### <a name="exercise-1-create-a-custom-vision-service-project"></a><span data-ttu-id="034a1-101">Esercizio 1: Creare un progetto del Servizio visione artificiale personalizzato</span><span class="sxs-lookup"><span data-stu-id="034a1-101">Exercise 1: Create a Custom Vision Service project</span></span>

<span data-ttu-id="034a1-102">Il primo passaggio per la creazione del modello di classificazione delle immagini con il Servizio visione artificiale personalizzato consiste nel creare un progetto.</span><span class="sxs-lookup"><span data-stu-id="034a1-102">The first step in building an image-classification model with the Custom Vision Service is to create a project.</span></span> <span data-ttu-id="034a1-103">In questo esercizio si userà il portale del Servizio visione artificiale personalizzato per creare un progetto del Servizio visione artificiale personalizzato.</span><span class="sxs-lookup"><span data-stu-id="034a1-103">In this exercise, you will use the Custom Vision Service portal to create a Custom Vision Service project.</span></span>

1. <span data-ttu-id="034a1-104">Aprire il [portale del Servizio visione artificiale personalizzato](https://www.customvision.ai/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="034a1-104">Open the [Custom Vision Service portal](https://www.customvision.ai/) in your browser.</span></span> <span data-ttu-id="034a1-105">Fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="034a1-105">Then click **Sign In**.</span></span> 
 
    ![Accesso al portale del Servizio visione artificiale personalizzato](../images/portal-sign-in.png)

    <span data-ttu-id="034a1-107">_Accesso al portale del Servizio visione artificiale personalizzato_</span><span class="sxs-lookup"><span data-stu-id="034a1-107">_Signing in to the Custom Vision Service portal_</span></span>

1. <span data-ttu-id="034a1-108">Se viene chiesto di accedere, usare le credenziali per l'account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="034a1-108">If you are asked to sign in, do so using the credentials for your Microsoft account.</span></span> <span data-ttu-id="034a1-109">Se viene chiesto se consentire all'app di accedere alle proprie informazioni, fare clic su **Sì** e, se richiesto, accettare le condizioni del servizio.</span><span class="sxs-lookup"><span data-stu-id="034a1-109">If you are asked to let this app access your info, click **Yes**, and if prompted, agree to the terms of service.</span></span>

1. <span data-ttu-id="034a1-110">Fare clic su **Nuovo progetto** per creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="034a1-110">Click **New Project** to create a new project.</span></span>
  
    ![Creazione di un progetto del Servizio visione artificiale personalizzato](../images/portal-click-new-project.png)

    <span data-ttu-id="034a1-112">_Creazione di un progetto del Servizio visione artificiale personalizzato_</span><span class="sxs-lookup"><span data-stu-id="034a1-112">_Creating a Custom Vision Service project_</span></span>

1. <span data-ttu-id="034a1-113">Nella finestra "Nuovo progetto" assegnare al progetto il nome "Artworks", verificare che come dominio sia selezionato **Generale** e fare clic su **Crea progetto**.</span><span class="sxs-lookup"><span data-stu-id="034a1-113">In the "New project" dialog, name the project "Artworks," ensure that **General** is selected as the domain, and click **Create project**.</span></span>

    > <span data-ttu-id="034a1-114">Un dominio ottimizza un modello per tipi specifici di immagine.</span><span class="sxs-lookup"><span data-stu-id="034a1-114">A domain optimizes a model for specific types of images.</span></span> <span data-ttu-id="034a1-115">Ad esempio, se l'obiettivo è classificare immagini di cibi in base al tipo di alimento che contengono o all'esoticità dei piatti, può essere utile selezionare il dominio Food (Cibo).</span><span class="sxs-lookup"><span data-stu-id="034a1-115">For example, if your goal is to classify food images by the types of food they contain or the ethnicity of the dishes, then it might be helpful to select the Food domain.</span></span> <span data-ttu-id="034a1-116">Per scenari che non corrispondono ad alcuno dei domini disponibili o in caso di dubbi su quale dominio scegliere, selezionare il dominio Generale.</span><span class="sxs-lookup"><span data-stu-id="034a1-116">For scenarios that don't match any of the offered domains, or if you are unsure of which domain to choose, select the General domain.</span></span>

    ![Creazione di un progetto del Servizio visione artificiale personalizzato](../images/portal-create-project.png)

    <span data-ttu-id="034a1-118">_Creazione di un progetto del Servizio visione artificiale personalizzato_</span><span class="sxs-lookup"><span data-stu-id="034a1-118">_Creating a Custom Vision Service project_</span></span>

<span data-ttu-id="034a1-119">Il passaggio seguente consiste nel caricare immagini nel progetto e assegnare tag alle immagini per classificarle.</span><span class="sxs-lookup"><span data-stu-id="034a1-119">The next step is to upload images to the project and assign tags to those images to classify them.</span></span>