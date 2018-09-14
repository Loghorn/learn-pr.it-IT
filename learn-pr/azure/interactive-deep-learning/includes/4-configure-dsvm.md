## <a name="introduction-to-jupyter-for-more-interactive-deep-learning"></a><span data-ttu-id="882ee-101">Introduzione a Jupyter per l'apprendimento profondo più interattivo</span><span class="sxs-lookup"><span data-stu-id="882ee-101">Introduction to Jupyter for more interactive deep learning</span></span> 

<span data-ttu-id="882ee-102">Jupyter Notebook è un'applicazione Web open source che consente di creare e condividere documenti contenenti codice in tempo reale, equazioni, visualizzazioni e testo narrativo.</span><span class="sxs-lookup"><span data-stu-id="882ee-102">Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations, and narrative text.</span></span> <span data-ttu-id="882ee-103">Gli usi possibili includono: pulizia e trasformazione dei dati, simulazione numerica, modellazione statistica, visualizzazione dei dati, apprendimento automatico e molto altro ancora.</span><span class="sxs-lookup"><span data-stu-id="882ee-103">Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.</span></span>

## <a name="serving-jupyter-notebooks-with-nvidia-docker-on-an-azure-dsvm"></a><span data-ttu-id="882ee-104">Uso di notebook Jupyter con Nvidia Docker in una DSVM di Azure</span><span class="sxs-lookup"><span data-stu-id="882ee-104">Serving Jupyter Notebooks with Nvidia Docker on an Azure DSVM</span></span>

### <a name="step-1-create-a-linux-dsvm"></a><span data-ttu-id="882ee-105">Passaggio 1: Creare una DSVM Linux</span><span class="sxs-lookup"><span data-stu-id="882ee-105">Step 1 Create a Linux DSVM</span></span>

<span data-ttu-id="882ee-106">Usare il codice di chiamata dall'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="882ee-106">Use call code from the Azure CLI</span></span>

```
code .
```

<span data-ttu-id="882ee-107">Compilare lo schema di distribuzione seguente e salvarlo come parameter_file.json</span><span class="sxs-lookup"><span data-stu-id="882ee-107">Fill in the following deployment schema and save it as parameter_file.json</span></span>

``` 
{ 
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "YOURUSERNAME FOR NEW DSVM"},
     "adminPassword": { "value" : "PASSWORD FOR NEW DSVM"},
     "vmName": { "value" : "HOSTNAME OF DSVM"},
     "vmSize": { "value" : "VM SIZE For eg: Standard_DS2_v2"},
  }
}
```

<span data-ttu-id="882ee-108">È possibile visualizzare un elenco delle dimensioni delle macchine virtuali disponibili in [Ubuntu DSVM ARM template](https://azure.microsoft.com/global-infrastructure/services/?WT.mc_id=blog-learning-abornst) (Modello ARM di DSVM Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="882ee-108">A list of available vm sizes can be found here [Ubuntu DSVM ARM template](https://azure.microsoft.com/global-infrastructure/services/?WT.mc_id=blog-learning-abornst).</span></span>


### <a name="create-a-resource-group-for-your-dsvm-in-a-region-of-your-choice"></a><span data-ttu-id="882ee-109">Creare un gruppo di risorse per la DSVM in un'area a propria scelta:</span><span class="sxs-lookup"><span data-stu-id="882ee-109">Create a resource group for your DSVM in a region of your choice:</span></span>
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

<span data-ttu-id="882ee-110">È possibile visualizzare un elenco delle aree disponibili in [Aree di Azure](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="882ee-110">A list of available regions can be found here [Azure Regions](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).</span></span>

### <a name="deploy-your-dsvm-to-your-new-resource-group"></a><span data-ttu-id="882ee-111">Distribuire la DSVM nel nuovo gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="882ee-111">Deploy your DSVM to your new resource group</span></span>

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## <a name="step-2-open-the-port-8888-22-on-the-dsvm"></a><span data-ttu-id="882ee-112">Passaggio 2: Aprire la porta 8888, 22 nella DSVM</span><span class="sxs-lookup"><span data-stu-id="882ee-112">Step 2 Open the Port 8888, 22 on the DSVM</span></span> 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

<span data-ttu-id="882ee-113">La porta 8888 è la porta predefinita per i notebook Jupyter. Per informazioni dettagliate su come aprire una porta, [fare clic qui](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)</span><span class="sxs-lookup"><span data-stu-id="882ee-113">Port 8888 is the default port for Jupyter Notebooks For detailed steps on opening a port [click here](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)</span></span>
 
## <a name="step-3-connect-to-the-dsvm-with-the-azure-shell"></a><span data-ttu-id="882ee-114">Passaggio 3: Connettersi alla DSVM con la shell di Azure</span><span class="sxs-lookup"><span data-stu-id="882ee-114">Step 3 Connect to the DSVM with the Azure Shell</span></span> 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## <a name="step-4-run-jupyter-in-docker-container--link-8888-port-to-the-vm-host"></a><span data-ttu-id="882ee-115">Passaggio 4: Eseguire Jupyter nel contenitore Docker e collegare la porta 8888 all'host della macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="882ee-115">Step 4 Run Jupyter in Docker Container & link 8888 port to the VM Host</span></span> 

<span data-ttu-id="882ee-116">Collegare la porta 8888 tra la macchina virtuale e il contenitore Docker, installare jupyter e seguire le esercitazioni su PyTorch.</span><span class="sxs-lookup"><span data-stu-id="882ee-116">Link port 8888 between the VM and the docker container, install jupyter and pull pytorch tutorials.</span></span>  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## <a name="step-5-navigate-to-the-jupyter-notebook-in-the-browser"></a><span data-ttu-id="882ee-117">Passaggio 5: Passare a Jupyter Notebook nel browser</span><span class="sxs-lookup"><span data-stu-id="882ee-117">Step 5 Navigate to the Jupyter Notebook in the Browser</span></span> 

<span data-ttu-id="882ee-118">Quando Jupyter Notebook è in esecuzione, verrà visualizzato un messaggio che indica quanto segue:</span><span class="sxs-lookup"><span data-stu-id="882ee-118">Once the Jupyter notebook is running you will see a message as follows :</span></span> 

> <span data-ttu-id="882ee-119">Copiare/incollare l'URL nel browser quando si esegue la connessione per la prima volta per accedere con un token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="882ee-119">Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}</span></span>

<span data-ttu-id="882ee-120">Sostituire la parte **http://(5b8783e7911d or 127.0.0.1)** dell'URL con **[[NOME HOST DSVM]].westus2.cloudapp.azure.com** e passare all'indirizzo in una nuova scheda del browser:</span><span class="sxs-lookup"><span data-stu-id="882ee-120">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the url with **[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com** and navigate to the address  in a new a tab in your browser:</span></span>
- <span data-ttu-id="882ee-121">[[NOME HOST DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="882ee-121">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span></span>
