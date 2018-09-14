<span data-ttu-id="43453-101">La società usa immagini del contenitore per gestire i carichi di lavoro di calcolo nella società.</span><span class="sxs-lookup"><span data-stu-id="43453-101">Your company makes use of container images to manage compute workloads in the company.</span></span> <span data-ttu-id="43453-102">Per creare le immagini del contenitore vengono usati gli strumenti Docker locali.</span><span class="sxs-lookup"><span data-stu-id="43453-102">You use local Docker tooling to build your container images.</span></span> <span data-ttu-id="43453-103">La decisione presa relativa all'uso di un Registro contenitori di Azure consente ora di creare immagini del contenitore sul cloud.</span><span class="sxs-lookup"><span data-stu-id="43453-103">The decision to make use of an Azure Container Registry now allows you to build container images in the cloud.</span></span> 

<span data-ttu-id="43453-104">È ora possibile usare Azure Container Registry Build per creare tali contenitori.</span><span class="sxs-lookup"><span data-stu-id="43453-104">You can now use Azure Container Registry Build to build these containers.</span></span> <span data-ttu-id="43453-105">Container Registry Build consente anche l'integrazione del processo DevOps con compilazione automatica al commit del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="43453-105">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="43453-106">È ora possibile automatizzare la creazione di un'immagine del contenitore usando Azure Container Registry Build.</span><span class="sxs-lookup"><span data-stu-id="43453-106">Let's automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-azure-container-registry-build"></a><span data-ttu-id="43453-107">Creare un'immagine del contenitore con Azure Container Registry Build</span><span class="sxs-lookup"><span data-stu-id="43453-107">Create a container image with Azure Container Registry Build</span></span>

<span data-ttu-id="43453-108">Per fornire le istruzioni di compilazione viene usato un Dockerfile standard.</span><span class="sxs-lookup"><span data-stu-id="43453-108">You use a standard Dockerfile to provide build instructions.</span></span> <span data-ttu-id="43453-109">Azure Container Registry Build consente di riutilizzare qualsiasi Dockerfile attualmente disponibile nell'ambiente, incluse le compilazioni in più fasi.</span><span class="sxs-lookup"><span data-stu-id="43453-109">Azure Container Registry Build allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.</span></span>

<span data-ttu-id="43453-110">Per l'esempio verrà usato un nuovo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="43453-110">We'll use new Dockerfile for our example.</span></span> 

<span data-ttu-id="43453-111">Il primo passaggio consiste nel creare un nuovo file denominato `Dockerfile`.</span><span class="sxs-lookup"><span data-stu-id="43453-111">The first step is to create a new file named `Dockerfile`.</span></span> <span data-ttu-id="43453-112">È possibile usare qualsiasi editor di testo per modificare il file.</span><span class="sxs-lookup"><span data-stu-id="43453-112">You can use any text editor to edit the file.</span></span> <span data-ttu-id="43453-113">Per questo esempio verrà usato Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="43453-113">We'll use Visual Studio Code for this example.</span></span>

```bash
code Dockerfile
```

<span data-ttu-id="43453-114">Copiare i contenuti seguenti nel nuovo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="43453-114">Copy the following contents to your new Dockerfile.</span></span> <span data-ttu-id="43453-115">Assicurarsi di salvare il file.</span><span class="sxs-lookup"><span data-stu-id="43453-115">Make sure to safe the file.</span></span> 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="43453-116">Questa configurazione aggiunge un'applicazione Node.js all'immagine `node:9-alpine`.</span><span class="sxs-lookup"><span data-stu-id="43453-116">This configuration adds a Node.js application to the `node:9-alpine` image.</span></span> <span data-ttu-id="43453-117">Configura quindi il contenitore in modo che gestisca l'applicazione sulla porta 80 tramite l'istruzione *EXPOSE*.</span><span class="sxs-lookup"><span data-stu-id="43453-117">Then configures the container to serve the application on port 80 via the *EXPOSE* instruction.</span></span>

<span data-ttu-id="43453-118">Eseguire ora il comando dell'interfaccia della riga di comando di Azure, `az acr build`, per compilare l'immagine del contenitore da Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="43453-118">Now run the Azure CLI command, `az acr build`, to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="43453-119">Verrà mostrata la creazione dell'immagine e ne verrà eseguito il push nel registro contenitori durante l'esecuzione del comando.</span><span class="sxs-lookup"><span data-stu-id="43453-119">You'll see the image being built and pushed to your Container Registry as you run the command.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="43453-120">Verificare l'immagine</span><span class="sxs-lookup"><span data-stu-id="43453-120">Verify the image</span></span>

<span data-ttu-id="43453-121">Eseguire il comando seguente per verificare che l'immagine sia stata creata e archiviata nel registro.</span><span class="sxs-lookup"><span data-stu-id="43453-121">Run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="43453-122">L'output dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="43453-122">The output should look similar to the following:</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="43453-123">L'immagine `helloacrbuild` è ora pronta per l'uso.</span><span class="sxs-lookup"><span data-stu-id="43453-123">The `helloacrbuild` image is now ready to be used.</span></span>
