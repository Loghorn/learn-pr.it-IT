<span data-ttu-id="62a2a-101">Per impostazione predefinita, Istanze di contenitore di Azure è senza stato.</span><span class="sxs-lookup"><span data-stu-id="62a2a-101">By default, Azure Container Instances is stateless.</span></span> <span data-ttu-id="62a2a-102">Se il contenitore si blocca o si arresta, lo stato viene perso.</span><span class="sxs-lookup"><span data-stu-id="62a2a-102">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="62a2a-103">Per rendere persistente lo stato oltre la durata del contenitore, è necessario montare un volume da un archivio esterno.</span><span class="sxs-lookup"><span data-stu-id="62a2a-103">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span>

<span data-ttu-id="62a2a-104">In questo caso, si monterà una condivisione file di Azure in un'istanza di contenitore di Azure per l'archiviazione e il recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="62a2a-104">Here, you will mount an Azure file share to an Azure container instance for data storage and retrieval.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="62a2a-105">Creare una condivisione file di Azure</span><span class="sxs-lookup"><span data-stu-id="62a2a-105">Create an Azure file share</span></span>

<span data-ttu-id="62a2a-106">Eseguire lo script seguente per creare un account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="62a2a-106">Run the following script to create a storage account.</span></span> <span data-ttu-id="62a2a-107">Il nome dell'account di archiviazione deve essere globalmente univoco, quindi lo script aggiunge un valore casuale alla stringa di base:</span><span class="sxs-lookup"><span data-stu-id="62a2a-107">The storage account name must be globally unique, so the script adds a random value to the base string:</span></span>

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --sku Standard_LRS \
    --location eastus
```

<span data-ttu-id="62a2a-108">Eseguire il comando seguente per inserire la stringa di connessione dell'account di archiviazione nella variabile di ambiente *AZURE_STORAGE_CONNECTION_STRING*.</span><span class="sxs-lookup"><span data-stu-id="62a2a-108">Run the following command to place the storage account connection string into an environment variable named *AZURE_STORAGE_CONNECTION_STRING*.</span></span> <span data-ttu-id="62a2a-109">Questa variabile di ambiente viene riconosciuta dall'interfaccia della riga di comando di Azure e può essere usata in operazioni correlate all'archiviazione:</span><span class="sxs-lookup"><span data-stu-id="62a2a-109">This environment variable is understood by the Azure CLI and can be used in storage-related operations:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv)
```

<span data-ttu-id="62a2a-110">Creare una condivisione file nell'account di archiviazione eseguendo il comando `az storage share create`.</span><span class="sxs-lookup"><span data-stu-id="62a2a-110">Create a file share in the storage account by running the `az storage share create` command.</span></span> <span data-ttu-id="62a2a-111">L'esempio seguente crea una condivisione denominata *aci-share-demo*:</span><span class="sxs-lookup"><span data-stu-id="62a2a-111">The following example creates a share with the name *aci-share-demo*:</span></span>

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a><span data-ttu-id="62a2a-112">Ottenere le credenziali di archiviazione</span><span class="sxs-lookup"><span data-stu-id="62a2a-112">Get storage credentials</span></span>

<span data-ttu-id="62a2a-113">Per montare una condivisione file di Azure come volume in Istanze di contenitore di Azure, sono necessari tre valori: il nome dell'account di archiviazione, il nome della condivisione e la chiave di accesso all'account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="62a2a-113">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage account access key.</span></span>

<span data-ttu-id="62a2a-114">Se si usa lo script precedente, il nome dell'account di archiviazione viene creato con un valore casuale alla fine.</span><span class="sxs-lookup"><span data-stu-id="62a2a-114">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="62a2a-115">Per eseguire una query sulla stringa finale (inclusa la parte casuale), usare i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="62a2a-115">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group <rgn>[sandbox resource group name]</rgn> --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="62a2a-116">Il nome della condivisione è già noto (aci-share-demo), quindi non resta che la chiave dell'account di archiviazione, che può essere trovata usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="62a2a-116">The share name is already known (aci-share-demo), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group <rgn>[sandbox resource group name]</rgn> --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a><span data-ttu-id="62a2a-117">Distribuire contenitori e montare volumi</span><span class="sxs-lookup"><span data-stu-id="62a2a-117">Deploy container and mount volume</span></span>

<span data-ttu-id="62a2a-118">Per montare una condivisione file di Azure come volume in un contenitore, specificare il punto di montaggio della condivisione e del volume quando si crea il contenitore:</span><span class="sxs-lookup"><span data-stu-id="62a2a-118">To mount an Azure file share as a volume in a container, specify the share and volume mount point when you create the container:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --location eastus \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

<span data-ttu-id="62a2a-119">Dopo aver creato il contenitore, ottenere l'indirizzo IP pubblico:</span><span class="sxs-lookup"><span data-stu-id="62a2a-119">Once the container has been created, get the public IP address:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --query ipAddress.ip \
    --output tsv
```

<span data-ttu-id="62a2a-120">Aprire un browser e passare all'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="62a2a-120">Open a browser and navigate to the container's IP address.</span></span> <span data-ttu-id="62a2a-121">Verrà visualizzato un modulo semplice.</span><span class="sxs-lookup"><span data-stu-id="62a2a-121">You will be presented with a simple form.</span></span> <span data-ttu-id="62a2a-122">Immettere il testo e fare clic su **Invia**.</span><span class="sxs-lookup"><span data-stu-id="62a2a-122">Enter some text and click **Submit**.</span></span> <span data-ttu-id="62a2a-123">Questa azione consente di creare un file nella condivisione di File di Azure che contiene il testo immesso.</span><span class="sxs-lookup"><span data-stu-id="62a2a-123">This action will create a file in the Azure Files share containing the entered text.</span></span>

![Demo della condivisione file di Istanze di contenitore di Azure](../media/5-files-ui.png)

<span data-ttu-id="62a2a-125">Per convalidare, è possibile passare alla condivisione file nel portale di Azure e scaricare il file.</span><span class="sxs-lookup"><span data-stu-id="62a2a-125">To validate, you can navigate to the file share in the Azure portal and download the file.</span></span>

1. <span data-ttu-id="62a2a-126">Ottenere il nome del file</span><span class="sxs-lookup"><span data-stu-id="62a2a-126">Get the filename</span></span>

    ```azurecli
    az storage file list -s aci-share-demo -o table
    ```

1. <span data-ttu-id="62a2a-127">Scaricarlo e assicurarsi di sostituire l'elemento `<filename>` qui sotto.</span><span class="sxs-lookup"><span data-stu-id="62a2a-127">Download it, make sure to replace the `<filename>` below.</span></span>

    ```azurecli
    az storage file download -s aci-share-demo -n <filename>
    ```
    
![Esempio di file di testo con l'applicazione demo Contents](../media/5-sample-text.png)

<span data-ttu-id="62a2a-129">Se i file e i dati archiviati nella condivisione di File di Azure avevano un valore qualsiasi, tale condivisione potrebbe essere rimontata in una nuova istanza di contenitore per fornire i dati con stato.</span><span class="sxs-lookup"><span data-stu-id="62a2a-129">If the files and data stored in the Azure Files share were of any value, this share could be remounted on a new container instance to provide stateful data.</span></span>