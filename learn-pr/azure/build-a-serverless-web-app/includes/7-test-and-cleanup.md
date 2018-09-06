<span data-ttu-id="91498-101">È stata completata la creazione di un'applicazione completa e senza server con i servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="91498-101">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="91498-102">Pulire le risorse</span><span class="sxs-lookup"><span data-stu-id="91498-102">Clean up resources</span></span>

<span data-ttu-id="91498-103">Quando si è terminato di usare questa applicazione, è possibile eseguire il comando seguente per eliminare tutte le risorse create durante l'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="91498-103">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="91498-104">Quando richiesto, digitare `y`.</span><span class="sxs-lookup"><span data-stu-id="91498-104">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="91498-105">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="91498-105">Next steps</span></span>

<span data-ttu-id="91498-106">In questo modulo si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="91498-106">In this module, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="91498-107">Configurare l'archiviazione BLOB di Azure per ospitare un sito Web statico e le immagini caricate.</span><span class="sxs-lookup"><span data-stu-id="91498-107">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="91498-108">Caricare le immagini nell'archiviazione BLOB di Azure con Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="91498-108">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="91498-109">Ridimensionare le immagini con Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="91498-109">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="91498-110">Archiviare i metadati delle immagini in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="91498-110">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="91498-111">Usare l'API Visione artificiale di Servizi cognitivi per generare automaticamente le didascalie delle immagini.</span><span class="sxs-lookup"><span data-stu-id="91498-111">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="91498-112">Usare Azure Active Directory per proteggere l'app Web tramite l'autenticazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="91498-112">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="91498-113">Per informazioni su come connettere ancora più servizi a Funzioni, continuare con l'esercitazione sulle app per la logica.</span><span class="sxs-lookup"><span data-stu-id="91498-113">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="91498-114">Funzioni con App per la logica</span><span class="sxs-lookup"><span data-stu-id="91498-114">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)