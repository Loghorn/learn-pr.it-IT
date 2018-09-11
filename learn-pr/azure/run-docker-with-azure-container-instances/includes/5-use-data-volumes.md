<span data-ttu-id="779b2-101">Per impostazione predefinita, Istanze di contenitore di Azure è senza stato.</span><span class="sxs-lookup"><span data-stu-id="779b2-101">By default, Azure Container Instances is stateless.</span></span> <span data-ttu-id="779b2-102">Se il contenitore si blocca o si arresta, lo stato viene perso.</span><span class="sxs-lookup"><span data-stu-id="779b2-102">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="779b2-103">Per rendere persistente lo stato oltre la durata del contenitore, è necessario montare un volume da un archivio esterno.</span><span class="sxs-lookup"><span data-stu-id="779b2-103">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span>

<span data-ttu-id="779b2-104">In questa unità si monterà una condivisione di file Azure in un'istanza di contenitore Azure per l'archiviazione e il recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="779b2-104">In this unit, you will mount an Azure file share to an Azure container instance for data storage and retrieval.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="779b2-105">Creare una condivisione file di Azure</span><span class="sxs-lookup"><span data-stu-id="779b2-105">Create an Azure file share</span></span>

<span data-ttu-id="779b2-106">Prima di usare una condivisione file di Azure con Istanze di contenitore di Azure è necessario creare la condivisione.</span><span class="sxs-lookup"><span data-stu-id="779b2-106">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="779b2-107">Eseguire questo script per creare un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="779b2-107">Run the following script to create a storage account.</span></span> <span data-ttu-id="779b2-108">Il nome dell'account di archiviazione deve essere globalmente univoco, quindi lo script aggiunge un valore casuale alla stringa di base:</span><span class="sxs-lookup"><span data-stu-id="779b2-108">The storage account name must be globally unique, so the script adds a random value to the base string:</span></span>

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --location eastus --sku Standard_LRS
```

<span data-ttu-id="779b2-109">Eseguire questo comando per inserire la stringa di connessione dell'account di archiviazione in una variabile di ambiente denominata *AZURE_STORAGE_CONNECTION_STRING*.</span><span class="sxs-lookup"><span data-stu-id="779b2-109">Run the following command to place the storage account connection string into an environment variable named *AZURE_STORAGE_CONNECTION_STRING*.</span></span> <span data-ttu-id="779b2-110">Questa variabile di ambiente viene riconosciuta dall'interfaccia della riga di comando di Azure e può essere usata in operazioni correlate all'archiviazione:</span><span class="sxs-lookup"><span data-stu-id="779b2-110">This environment variable is understood by the Azure CLI and can be used in storage-related operations:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

<span data-ttu-id="779b2-111">Creare la condivisione di file eseguendo il comando `az storage share create`.</span><span class="sxs-lookup"><span data-stu-id="779b2-111">Create the file share by running the `az storage share create` command.</span></span> <span data-ttu-id="779b2-112">L'esempio seguente crea una condivisione denominata *aci-share-demo*:</span><span class="sxs-lookup"><span data-stu-id="779b2-112">The following example creates a share with the name *aci-share-demo*:</span></span>

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a><span data-ttu-id="779b2-113">Ottenere le credenziali di archiviazione</span><span class="sxs-lookup"><span data-stu-id="779b2-113">Get storage credentials</span></span>

<span data-ttu-id="779b2-114">Per montare una condivisione file di Azure come volume in Istanze di contenitore di Azure sono necessari tre valori: il nome dell'account di archiviazione, il nome della condivisione e la chiave di accesso alle risorse di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="779b2-114">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage access key.</span></span>

<span data-ttu-id="779b2-115">Se si usa lo script precedente, il nome dell'account di archiviazione viene creato con un valore casuale alla fine.</span><span class="sxs-lookup"><span data-stu-id="779b2-115">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="779b2-116">Per eseguire una query sulla stringa finale (inclusa la parte casuale), usare i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="779b2-116">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="779b2-117">Il nome della condivisione è già noto (aci-share-demo), quindi non resta che la chiave dell'account di archiviazione, che può essere trovata usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="779b2-117">The share name is already known (aci-share-demo), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a><span data-ttu-id="779b2-118">Distribuire il contenitore e montare il volume</span><span class="sxs-lookup"><span data-stu-id="779b2-118">Deploy container and mount volume</span></span>

<span data-ttu-id="779b2-119">Per montare una condivisione file di Azure come volume in un contenitore, specificare il punto di montaggio della condivisione e del volume quando si crea il contenitore:</span><span class="sxs-lookup"><span data-stu-id="779b2-119">To mount an Azure file share as a volume in a container, specify the share and volume mount point when you create the container:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

<span data-ttu-id="779b2-120">Dopo aver creato il contenitore, ottenere l'indirizzo IP pubblico:</span><span class="sxs-lookup"><span data-stu-id="779b2-120">Once the container has been created, get the public IP address:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-files --query ipAddress.ip -o tsv
```

<span data-ttu-id="779b2-121">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="779b2-121">Open a browser and navigate to the container's IP address.</span></span> <span data-ttu-id="779b2-122">Verrà visualizzato un modulo semplice.</span><span class="sxs-lookup"><span data-stu-id="779b2-122">You will be presented with a simple form.</span></span> <span data-ttu-id="779b2-123">Immettere il testo e fare clic su **Invia**.</span><span class="sxs-lookup"><span data-stu-id="779b2-123">Enter some text and click **Submit**.</span></span> <span data-ttu-id="779b2-124">Questa azione consente di creare un file nella condivisione file di Azure. Il corpo del file sarà il testo immesso.</span><span class="sxs-lookup"><span data-stu-id="779b2-124">This action will create a file in the Azure Files share with the text entered here as the file body.</span></span>

![Demo della condivisione file di Istanze di contenitore di Azure](../media-draft/files-ui.png)

<span data-ttu-id="779b2-126">Per la convalida è possibile passare alla condivisione file nel portale di Azure e scaricare il file.</span><span class="sxs-lookup"><span data-stu-id="779b2-126">To validate, you can navigate to the file share in the Azure portal and download the file.</span></span>

![Esempio di file di testo con l'applicazione demo Contents](../media-draft/sample-text.png)

<span data-ttu-id="779b2-128">Se i file e i dati archiviati nella condivisione file di Azure avevano un valore qualsiasi, tale condivisione potrebbe rimontata in una nuova istanza di contenitore per fornire i dati con stati.</span><span class="sxs-lookup"><span data-stu-id="779b2-128">If the files and data stored in the Azure Files share were of any value, this share could be remounted on a new container instance to provide stateful data.</span></span>


## <a name="summary"></a><span data-ttu-id="779b2-129">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="779b2-129">Summary</span></span>

<span data-ttu-id="779b2-130">In questa unità sono stati creati una condivisione file di Azure e un contenitore e la condivisione file è stata montata in tale contenitore.</span><span class="sxs-lookup"><span data-stu-id="779b2-130">In this unit, you have created an Azure Files share and a container, and have mounted the file share to that container.</span></span> <span data-ttu-id="779b2-131">La condivisione è stata quindi usata per archiviare i dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="779b2-131">This share was then used to store application data.</span></span>

<span data-ttu-id="779b2-132">Nell'unità successiva verranno trattate alcune operazioni comuni di risoluzione dei problemi delle istanze di contenitore.</span><span class="sxs-lookup"><span data-stu-id="779b2-132">In the next unit, you will work through some common Container Instances troubleshooting operations.</span></span>
