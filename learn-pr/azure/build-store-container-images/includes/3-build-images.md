<span data-ttu-id="b3ecf-101">Con Azure Container Registry Build (ACR Build) è possibile creare immagini del contenitore nel cloud, senza bisogno di strumenti Docker locali.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-101">With Azure Container Registry Build, container images can be built in the cloud, without the need for local Docker tooling.</span></span> <span data-ttu-id="b3ecf-102">ACR Build consente inoltre l'integrazione dei processi DevOps con compilazione automatizzata al commit del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-102">Container Registry Build also allows for DevOps process integration with automated build on source code commit.</span></span>

<span data-ttu-id="b3ecf-103">In questa unità si automatizza la creazione di un'immagine del contenitore usando ACR Build.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-103">In this unit, you automate the creation of a container image using Azure Container Registry Build.</span></span>

## <a name="create-a-container-image-with-build"></a><span data-ttu-id="b3ecf-104">Creare un'immagine del contenitore con ACR Build</span><span class="sxs-lookup"><span data-stu-id="b3ecf-104">Create a container image with Build</span></span>

<span data-ttu-id="b3ecf-105">Quando si usa ACR Build, per fornire le istruzioni di compilazione viene utilizzato un Dockerfile standard.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-105">When using Azure Container Registry Build, a standard Dockerfile is used to provide build instructions.</span></span> <span data-ttu-id="b3ecf-106">Con ACR Build si può usare qualsiasi Dockerfile attualmente in uso nell'ambiente, incluse le compilazioni in più fasi.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-106">Any Dockerfile currently used in your environment, including multi-staged builds, works with Azure Container Registry Build.</span></span>

<span data-ttu-id="b3ecf-107">Creare un file denominato `Dockerfile` e aprirlo con qualsiasi editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-107">Create a file named `Dockerfile` and open it with any text editor.</span></span>

```bash
code Dockerfile
```

<span data-ttu-id="b3ecf-108">Copiare il contenuto seguente, salvare il file e uscire dall'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-108">Copy in the following contents, save the file, and exit the text editor.</span></span> <span data-ttu-id="b3ecf-109">Questo Dockerfile aggiunge un'applicazione Node.js all'immagine `node:9-alpine` e configura il contenitore per fungere da server per l'applicazione sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-109">This Dockerfile adds a Node.js application to the `node:9-alpine` image and configures the container to server the application on port 80.</span></span>

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

<span data-ttu-id="b3ecf-110">Eseguire il comando seguente per compilare l'immagine del contenitore dal Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-110">Run the following command to build the container image from the Dockerfile.</span></span>

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

<span data-ttu-id="b3ecf-111">Durante l'esecuzione di questa operazione, si vedrà che l'immagine viene compilata e ne viene eseguito il push nel registro contenitori.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-111">As this operation runs, you will see the image being built and pushed to your Container Registry.</span></span>

## <a name="verify-the-image"></a><span data-ttu-id="b3ecf-112">Verificare l'immagine</span><span class="sxs-lookup"><span data-stu-id="b3ecf-112">Verify the image</span></span>

<span data-ttu-id="b3ecf-113">Al termine dell'operazione di compilazione, eseguire il comando seguente per verificare che l'immagine sia stata creata e archiviata nel registro.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-113">When the build operation has completed, run the following command to verify that the image has been created and stored in the registry.</span></span>

```azurecli
az acr repository list --name <acrName> --output table
```

<span data-ttu-id="b3ecf-114">L'output dovrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-114">The output should look similar to the following.</span></span>

```console
Result
-------------
helloacrbuild
```

<span data-ttu-id="b3ecf-115">L'immagine `helloacrbuild` è ora pronta per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-115">The `helloacrbuild` image is now ready to be used.</span></span>

## <a name="summary"></a><span data-ttu-id="b3ecf-116">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b3ecf-116">Summary</span></span>

<span data-ttu-id="b3ecf-117">In questa unità è stato usato ACR Build per automatizzare la creazione di un'immagine del contenitore.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-117">In this unit, Azure Container Registry Build was used to automate the creation of a container image.</span></span> <span data-ttu-id="b3ecf-118">L'immagine è stata quindi archiviata nel Registro contenitori di Azure.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-118">This image was then stored in the Azure Container Registry.</span></span> <span data-ttu-id="b3ecf-119">Nella prossima unità si distribuirà un'istanza della nuova immagine del contenitore in Istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="b3ecf-119">In the next unit, you will deploy an instance of the new container image to Azure Container Instances.</span></span>