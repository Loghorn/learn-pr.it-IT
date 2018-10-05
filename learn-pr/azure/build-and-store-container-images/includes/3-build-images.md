<span data-ttu-id="98c4b-101">Si supponga che l'azienda faccia uso di immagini del contenitore per gestire i carichi di lavoro di calcolo.</span><span class="sxs-lookup"><span data-stu-id="98c4b-101">Suppose your company makes use of container images to manage compute workloads.</span></span> <span data-ttu-id="98c4b-102">Per compilare le immagini del contenitore vengono usati gli strumenti Docker locali.</span><span class="sxs-lookup"><span data-stu-id="98c4b-102">You use the local Docker tooling to build your container images.</span></span>

<span data-ttu-id="98c4b-103">È ora possibile usare Azure Container Registry Tasks per creare tali contenitori.</span><span class="sxs-lookup"><span data-stu-id="98c4b-103">You can now use Azure Container Registry Tasks to build these containers.</span></span> <span data-ttu-id="98c4b-104">Container Registry Tasks consente anche l'integrazione del processo DevOps con compilazione automatica al commit del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="98c4b-104">Container Registry Tasks also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="98c4b-105">È ora possibile automatizzare la creazione di un'immagine del contenitore usando Azure Container Registry Tasks.</span><span class="sxs-lookup"><span data-stu-id="98c4b-105">Let's automate the creation of a container image using Azure Container Registry Tasks.</span></span>

## <a name="create-a-container-image-with-azure-container-registry-tasks"></a><span data-ttu-id="98c4b-106">Creare un'immagine del contenitore con Azure Container Registry Tasks</span><span class="sxs-lookup"><span data-stu-id="98c4b-106">Create a container image with Azure Container Registry Tasks</span></span>

<span data-ttu-id="98c4b-107">Per fornire le istruzioni di compilazione viene usato un Dockerfile standard.</span><span class="sxs-lookup"><span data-stu-id="98c4b-107">A standard Dockerfile to provides build instructions.</span></span> <span data-ttu-id="98c4b-108">Azure Container Registry Tasks consente di riutilizzare qualsiasi Dockerfile attualmente disponibile nell'ambiente, incluse le compilazioni in più fasi.</span><span class="sxs-lookup"><span data-stu-id="98c4b-108">Azure Container Registry Tasks allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.</span></span>

<span data-ttu-id="98c4b-109">Per l'esempio verrà usato un nuovo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="98c4b-109">We'll use a new Dockerfile for our example.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="98c4b-110">Il primo passaggio consiste nel creare un nuovo file denominato `Dockerfile`.</span><span class="sxs-lookup"><span data-stu-id="98c4b-110">The first step is to create a new file named `Dockerfile`.</span></span> <span data-ttu-id="98c4b-111">È possibile usare qualsiasi editor di testo per modificare il file.</span><span class="sxs-lookup"><span data-stu-id="98c4b-111">You can use any text editor to edit the file.</span></span> <span data-ttu-id="98c4b-112">Per questo esempio si userà l'editor di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="98c4b-112">We'll use Cloud Shell Editor for this example.</span></span> <span data-ttu-id="98c4b-113">Per aprire l'editor, immettere il comando seguente nella finestra di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="98c4b-113">Enter the following command into the Cloud Shell window to open the editor.</span></span>

```bash
code
```

<span data-ttu-id="98c4b-114">Copiare i contenuti seguenti nel nuovo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="98c4b-114">Copy the following contents to your new Dockerfile.</span></span> <span data-ttu-id="98c4b-115">Assicurarsi di salvare il file.</span><span class="sxs-lookup"><span data-stu-id="98c4b-115">Make sure to save the file.</span></span>

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="98c4b-116">Per il salvataggio usare la combinazione di tasti <kbd>Ctrl+S</kbd>.</span><span class="sxs-lookup"><span data-stu-id="98c4b-116">Use the key combination <kbd>Ctrl+S</kbd> to save.</span></span> <span data-ttu-id="98c4b-117">Quando viene richiesto, assegnare al file il nome `Dockerfile`.</span><span class="sxs-lookup"><span data-stu-id="98c4b-117">Name the file `Dockerfile` when prompted.</span></span>

<span data-ttu-id="98c4b-118">Questa configurazione aggiunge un'applicazione Node.js all'immagine `node:9-alpine`.</span><span class="sxs-lookup"><span data-stu-id="98c4b-118">This configuration adds a Node.js application to the `node:9-alpine` image.</span></span> <span data-ttu-id="98c4b-119">Configura quindi il contenitore in modo che gestisca l'applicazione sulla porta 80 tramite l'istruzione *EXPOSE*.</span><span class="sxs-lookup"><span data-stu-id="98c4b-119">After that, it configures the container to serve the application on port 80 via the *EXPOSE* instruction.</span></span>

<span data-ttu-id="98c4b-120">Eseguire ora il comando dell'interfaccia della riga di comando di Azure `az acr build` per compilare l'immagine del contenitore da Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="98c4b-120">Now run the Azure CLI command `az acr build` to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrtasks:v1 .
```

<span data-ttu-id="98c4b-121">Verrà mostrata la creazione dell'immagine e ne verrà eseguito il push nel registro contenitori durante l'esecuzione del comando.</span><span class="sxs-lookup"><span data-stu-id="98c4b-121">You'll see the image being built and pushed to your Container Registry as you run the command.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="98c4b-122">Verificare l'immagine</span><span class="sxs-lookup"><span data-stu-id="98c4b-122">Verify the image</span></span>

<span data-ttu-id="98c4b-123">Eseguire il comando seguente per verificare che l'immagine sia stata creata e archiviata nel registro.</span><span class="sxs-lookup"><span data-stu-id="98c4b-123">Run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="98c4b-124">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="98c4b-124">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrtasks
```

<span data-ttu-id="98c4b-125">L'immagine `helloacrtasks` è ora pronta per l'uso.</span><span class="sxs-lookup"><span data-stu-id="98c4b-125">The `helloacrtasks` image is now ready to be used.</span></span>
