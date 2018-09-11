## <a name="introduction-to-jupyter-for-more-interactive-deep-learning"></a>Introduzione a Jupyter per l'apprendimento profondo più interattivo 

Jupyter Notebook è un'applicazione Web open source che consente di creare e condividere documenti contenenti codice in tempo reale, equazioni, visualizzazioni e testo narrativo. Gli usi possibili includono: pulizia e trasformazione dei dati, simulazione numerica, modellazione statistica, visualizzazione dei dati, apprendimento automatico e molto altro ancora.

## <a name="serving-jupyter-notebooks-with-nvidia-docker-on-an-azure-dsvm"></a>Uso di notebook Jupyter con Nvidia Docker in una DSVM di Azure

### <a name="step-1-create-a-linux-dsvm"></a>Passaggio 1: Creare una DSVM Linux

Usare il codice di chiamata dall'interfaccia della riga di comando di Azure

```
code .
```

Compilare lo schema di distribuzione seguente e salvarlo come parameter_file.json

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

È possibile visualizzare un elenco delle dimensioni delle macchine virtuali disponibili in [Ubuntu DSVM ARM template](https://azure.microsoft.com/en-us/global-infrastructure/services/?WT.mc_id=blog-learning-abornst) (Modello ARM di DSVM Ubuntu).


### <a name="create-a-resource-group-for-your-dsvm-in-a-region-of-your-choice"></a>Creare un gruppo di risorse per la DSVM in un'area a propria scelta:
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

È possibile visualizzare un elenco delle aree disponibili in [Aree di Azure](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).

### <a name="deploy-your-dsvm-to-your-new-resource-group"></a>Distribuire la DSVM nel nuovo gruppo di risorse

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## <a name="step-2-open-the-port-8888-22-on-the-dsvm"></a>Passaggio 2: Aprire la porta 8888, 22 nella DSVM 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

La porta 8888 è la porta predefinita per i notebook Jupyter. Per informazioni dettagliate su come aprire una porta, [fare clic qui](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)
 
## <a name="step-3-connect-to-the-dsvm-with-the-azure-shell"></a>Passaggio 3: Connettersi alla DSVM con la shell di Azure 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## <a name="step-4-run-jupyter-in-docker-container--link-8888-port-to-the-vm-host"></a>Passaggio 4: Eseguire Jupyter nel contenitore Docker e collegare la porta 8888 all'host della macchina virtuale 

Collegare la porta 8888 tra la macchina virtuale e il contenitore Docker, installare jupyter e seguire le esercitazioni su PyTorch.  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## <a name="step-5-navigate-to-the-jupyter-notebook-in-the-browser"></a>Passaggio 5: Passare a Jupyter Notebook nel browser 

Quando Jupyter Notebook è in esecuzione, verrà visualizzato un messaggio che indica quanto segue: 

> Copiare/incollare l'URL nel browser quando si esegue la connessione per la prima volta per accedere con un token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}

Sostituire la parte **http://(5b8783e7911d or 127.0.0.1)** dell'URL con **[[NOME HOST DSVM]].westus2.cloudapp.azure.com** e passare all'indirizzo in una nuova scheda del browser:
- [[NOME HOST DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}
