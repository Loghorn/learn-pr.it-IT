<span data-ttu-id="bb9c3-101">Per definire l'immagine in base alla quale creare la macchina virtuale si è usato `Debian`.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-101">We used `Debian` for the image to create the virtual machine.</span></span> <span data-ttu-id="bb9c3-102">Azure offre diverse immagini di macchina virtuale standard che è possibile usare per creare una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-102">Azure has several standard VM images you can use to create a virtual machine.</span></span> 

## <a name="listing-images"></a><span data-ttu-id="bb9c3-103">Elenco di immagini</span><span class="sxs-lookup"><span data-stu-id="bb9c3-103">Listing images</span></span>

<span data-ttu-id="bb9c3-104">È possibile ottenere un elenco delle immagini di macchina virtuale disponibili usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-104">You can get a list of the available VM images using the following command.</span></span> 

```azurecli
az vm image list --output table
```

<span data-ttu-id="bb9c3-105">L'output del comando include le immagini più diffuse che fanno parte di un elenco offline integrato nell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-105">This will output the most popular images that are part of an offline list built into the Azure CLI.</span></span> <span data-ttu-id="bb9c3-106">In Azure Marketplace sono tuttavia disponibili _centinaia_ di opzioni di immagine.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-106">However, there are _hundreds_ of image options available in the Azure Marketplace.</span></span> 

## <a name="getting-all-images"></a><span data-ttu-id="bb9c3-107">Elenco di tutte le immagini</span><span class="sxs-lookup"><span data-stu-id="bb9c3-107">Getting all images</span></span>

<span data-ttu-id="bb9c3-108">È possibile ottenere un elenco completo aggiungendo il flag `--all` al comando.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-108">You can get a full list by adding the `--all` flag to the command.</span></span> <span data-ttu-id="bb9c3-109">Poiché l'elenco delle immagini nel Marketplace è molto ampio, è utile filtrarlo con le opzioni `--publisher`, `--sku` o `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-109">Since the list of images in the Marketplace is very large, it is helpful to filter the list with the `--publisher`, `--sku` or `–-offer` options.</span></span>

<span data-ttu-id="bb9c3-110">Si provi ad esempio a usare il comando seguente per visualizzare _tutte_ le immagini di Wordpress disponibili:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-110">For example, try the following command to see _all_ Wordpress images available:</span></span>

```azurecli
az vm image list --sku Wordpress --output table --all
```

<span data-ttu-id="bb9c3-111">Oppure si può provare questo comando per visualizzare tutte le immagini Microsoft:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-111">Or this command to see all images provided by Microsoft:</span></span>

```azurecli
az vm image list --publisher Microsoft --output table --all
```

## <a name="location-specific-images"></a><span data-ttu-id="bb9c3-112">Immagini specifiche dell'area</span><span class="sxs-lookup"><span data-stu-id="bb9c3-112">Location-specific images</span></span>

<span data-ttu-id="bb9c3-113">Alcune immagini sono disponibili solo in determinate aree.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-113">Some images are only available in certain locations.</span></span> <span data-ttu-id="bb9c3-114">Provare ad aggiungere il flag `--location [location]` al comando per limitare l'ambito dei risultati alle immagini disponibili nell'area in cui si vuole creare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-114">Try adding the `--location [location]` flag to the command to scope the results to ones available in the region where you want to create the virtual machine.</span></span> <span data-ttu-id="bb9c3-115">Digitare ad esempio quanto segue in Azure Cloud Shell per ottenere l'elenco delle immagini disponibili nell'area `eastus`.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-115">For example, type the following into Azure Cloud Shell to get a list of images available in the `eastus` region.</span></span>

```azurecli
az vm image list --location eastus --output table
```

<span data-ttu-id="bb9c3-116">Provare a controllare alcune delle immagini presenti in altre posizioni disponibili nell'ambiente sandbox di Azure:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-116">Try checking some of the images in the other Azure Sandbox available locations:</span></span>

[!include[](../../../includes/azure-sandbox-regions-note.md)]

> [!TIP]
> <span data-ttu-id="bb9c3-117">Queste sono le immagini standard offerte da Azure.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-117">These are the standard images that are provided by Azure.</span></span> <span data-ttu-id="bb9c3-118">Tenere presente che è anche possibile [creare e caricare immagini personalizzate](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) per creare macchine virtuali in base a configurazioni univoche oppure versioni o distribuzioni meno comuni di un sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-118">Keep in mind that you can also [create and upload your own custom images](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) to create VMs based on unique configurations or less common versions or distributions of an operating system.</span></span>

<span data-ttu-id="bb9c3-119">La finestra di comando a questo punto è probabilmente completa. È possibile digitare `clear` per cancellare la schermata se lo si vuole.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-119">Your command window is probably full now, you can type `clear` to clear the screen if you like.</span></span>