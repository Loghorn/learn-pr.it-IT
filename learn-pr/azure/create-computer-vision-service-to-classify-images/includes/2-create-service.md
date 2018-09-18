<span data-ttu-id="60d2e-101">In questa unità si creerà un servizio API Visione artificiale usando l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="60d2e-101">In this unit, you will create a Computer Vision API service using the Azure CLI.</span></span>

# <a name="exercise-create-a-computer-vision-service"></a><span data-ttu-id="60d2e-102">Esercizio: Creare un servizio Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="60d2e-102">Exercise: Create a Computer Vision service</span></span>

<span data-ttu-id="60d2e-103">[Visione artificiale](/azure/cognitive-services/computer-vision/home) fa parte di [Servizi cognitivi di Azure](/azure/cognitive-services/welcome), un set completo di algoritmi di apprendimento automatico disponibile come servizio che chiunque può usare.</span><span class="sxs-lookup"><span data-stu-id="60d2e-103">[Computer Vision](/azure/cognitive-services/computer-vision/home) is part of [Azure Cognitive Services](/azure/cognitive-services/welcome), which is a complete set of machine learning algorithms available as a service for anyone to use.</span></span> <span data-ttu-id="60d2e-104">Invece di generare dati sufficienti per il training di un modello da zero, vengono resi disponibili modelli di cui è già stato eseguito il training, che chiunque può usare.</span><span class="sxs-lookup"><span data-stu-id="60d2e-104">Instead of generating enough data to train a model from scratch, we make available pre-trained models for anyone to use.</span></span>

<span data-ttu-id="60d2e-105">Tra i molti disponibili, vengono offerti servizi in grado di gestire voce, immagini, video, ricerca, linguaggio e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="60d2e-105">Among the many services available, we provide services that can handle voice, image, video, search, language, and more.</span></span>

<span data-ttu-id="60d2e-106">Questo esercizio è incentrato sulla creazione del servizio necessario per gestire immagini.</span><span class="sxs-lookup"><span data-stu-id="60d2e-106">In this exercise, we're going to focus on creating the necessary service that will allow us to handle images.</span></span> <span data-ttu-id="60d2e-107">Al termine dell'esercizio, si sarà in grado di creare un endpoint API capace di identificare immagini.</span><span class="sxs-lookup"><span data-stu-id="60d2e-107">After this exercise, you will be able to create an API endpoint that will be able to identify images.</span></span>

# <a name="create-a-computer-vision-api-service"></a><span data-ttu-id="60d2e-108">Creare un servizio API Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="60d2e-108">Create a Computer Vision API service</span></span>

<span data-ttu-id="60d2e-109">Prima di creare il servizio API Visione artificiale, è necessario un *gruppo di risorse* in cui distribuire il servizio.</span><span class="sxs-lookup"><span data-stu-id="60d2e-109">Before you create your Computer Vision API service, you need a *resource group* to deploy it to.</span></span> <span data-ttu-id="60d2e-110">Un gruppo di risorse è una raccolta logica in cui vengono distribuite e gestite tutte le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="60d2e-110">A resource group is a logical collection into which all Azure resources are deployed and managed.</span></span>

```azurecli
az group create --name ComputerVisionRG --location westus2
```

<span data-ttu-id="60d2e-111">Dopo aver creato il gruppo di risorse, creare il servizio con il comando `az cognitiveservices account create` e il nome del servizio.</span><span class="sxs-lookup"><span data-stu-id="60d2e-111">Once you've created the resource group, create the service with the `az cognitiveservices account create` command and the name of your service.</span></span> 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
