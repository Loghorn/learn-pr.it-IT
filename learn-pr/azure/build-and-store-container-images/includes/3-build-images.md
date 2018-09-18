<span data-ttu-id="3ad09-101">La società usa immagini del contenitore per gestire i carichi di lavoro di calcolo nella società.</span><span class="sxs-lookup"><span data-stu-id="3ad09-101">Your company makes use of container images to manage compute workloads in the company.</span></span> <span data-ttu-id="3ad09-102">Per creare le immagini del contenitore vengono usati gli strumenti Docker locali.</span><span class="sxs-lookup"><span data-stu-id="3ad09-102">You use local Docker tooling to build your container images.</span></span> <span data-ttu-id="3ad09-103">La decisione presa relativa all'uso di un Registro contenitori di Azure consente ora di creare immagini del contenitore sul cloud.</span><span class="sxs-lookup"><span data-stu-id="3ad09-103">The decision to make use of an Azure Container Registry now allows you to build container images in the cloud.</span></span> 

<span data-ttu-id="3ad09-104">È ora possibile usare Azure Container Registry Build per creare tali contenitori.</span><span class="sxs-lookup"><span data-stu-id="3ad09-104">You can now use Azure Container Registry Build to build these containers.</span></span> <span data-ttu-id="3ad09-105">Container Registry Build consente anche l'integrazione del processo DevOps con compilazione automatica al commit del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="3ad09-105">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="3ad09-106">È ora possibile automatizzare la creazione di un'immagine del contenitore usando Azure Container Registry Build.</span><span class="sxs-lookup"><span data-stu-id="3ad09-106">Let's automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-azure-container-registry-build"></a><span data-ttu-id="3ad09-107">Creare un'immagine del contenitore con Azure Container Registry Build</span><span class="sxs-lookup"><span data-stu-id="3ad09-107">Create a container image with Azure Container Registry Build</span></span>

<span data-ttu-id="3ad09-108">Per fornire le istruzioni di compilazione viene usato un Dockerfile standard.</span><span class="sxs-lookup"><span data-stu-id="3ad09-108">You use a standard Dockerfile to provide build instructions.</span></span> <span data-ttu-id="3ad09-109">Azure Container Registry Build consente di riutilizzare qualsiasi Dockerfile attualmente disponibile nell'ambiente, incluse le compilazioni in più fasi.</span><span class="sxs-lookup"><span data-stu-id="3ad09-109">Azure Container Registry Build allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.</span></span>

<span data-ttu-id="3ad09-110">Per l'esempio verrà usato un nuovo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="3ad09-110">We'll use new Dockerfile for our example.</span></span> 

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="3ad09-111">Il primo passaggio consiste nel creare un nuovo file denominato `Dockerfile`.</span><span class="sxs-lookup"><span data-stu-id="3ad09-111">The first step is to create a new file named `Dockerfile`.</span></span> <span data-ttu-id="3ad09-112">È possibile usare qualsiasi editor di testo per modificare il file.</span><span class="sxs-lookup"><span data-stu-id="3ad09-112">You can use any text editor to edit the file.</span></span> <span data-ttu-id="3ad09-113">Per questo esempio si userà l'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="3ad09-113">We'll use Cloud Shell Editor for this example.</span></span> <span data-ttu-id="3ad09-114">Immettere il comando</span><span class="sxs-lookup"><span data-stu-id="3ad09-114">Enter the command</span></span>

```bash
code
```
<span data-ttu-id="3ad09-115">nella finestra di Cloud Shell per aprire l'editor.</span><span class="sxs-lookup"><span data-stu-id="3ad09-115">into the Cloud Shell window to open the editor.</span></span> 

<span data-ttu-id="3ad09-116">Copiare i contenuti seguenti nel nuovo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="3ad09-116">Copy the following contents to your new Dockerfile.</span></span> <span data-ttu-id="3ad09-117">Assicurarsi di salvare il file.</span><span class="sxs-lookup"><span data-stu-id="3ad09-117">Make sure to safe the file.</span></span> 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="3ad09-118">Per salvarlo, usare la combinazione di tasti `ctrl+s`.</span><span class="sxs-lookup"><span data-stu-id="3ad09-118">Use key combination `ctrl+s` to save.</span></span> <span data-ttu-id="3ad09-119">Quando viene richiesto, assegnare al file il nome `Dockerfile`.</span><span class="sxs-lookup"><span data-stu-id="3ad09-119">Name the file `Dockerfile` when prompted.</span></span>

<span data-ttu-id="3ad09-120">Questa configurazione aggiunge un'applicazione Node.js all'immagine `node:9-alpine`.</span><span class="sxs-lookup"><span data-stu-id="3ad09-120">This configuration adds a Node.js application to the `node:9-alpine` image.</span></span> <span data-ttu-id="3ad09-121">Configura quindi il contenitore in modo che gestisca l'applicazione sulla porta 80 tramite l'istruzione *EXPOSE*.</span><span class="sxs-lookup"><span data-stu-id="3ad09-121">Then configures the container to serve the application on port 80 via the *EXPOSE* instruction.</span></span>

<span data-ttu-id="3ad09-122">Eseguire ora il comando dell'interfaccia della riga di comando di Azure, `az acr build`, per compilare l'immagine del contenitore da Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="3ad09-122">Now run the Azure CLI command, `az acr build`, to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="3ad09-123">Verrà mostrata la creazione dell'immagine e ne verrà eseguito il push nel registro contenitori durante l'esecuzione del comando.</span><span class="sxs-lookup"><span data-stu-id="3ad09-123">You'll see the image being built and pushed to your Container Registry as you run the command.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="3ad09-124">Verificare l'immagine</span><span class="sxs-lookup"><span data-stu-id="3ad09-124">Verify the image</span></span>

<span data-ttu-id="3ad09-125">Eseguire il comando seguente per verificare che l'immagine sia stata creata e archiviata nel registro.</span><span class="sxs-lookup"><span data-stu-id="3ad09-125">Run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="3ad09-126">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3ad09-126">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="3ad09-127">L'immagine `helloacrbuild` è ora pronta per l'uso.</span><span class="sxs-lookup"><span data-stu-id="3ad09-127">The `helloacrbuild` image is now ready to be used.</span></span>
