### <a name="exercise-6-delete-the-data-science-vm"></a><span data-ttu-id="0943e-101">Esercizio 6: Eliminare la Data Science VM</span><span class="sxs-lookup"><span data-stu-id="0943e-101">Exercise 6: Delete the Data Science VM</span></span>

<span data-ttu-id="0943e-102">In questo esercizio si eliminerà il gruppo di risorse creato nell'esercizio 1 durante la creazione della Data Science VM.</span><span class="sxs-lookup"><span data-stu-id="0943e-102">In this exercise, you will delete the resource group created in Exercise 1 when you created the Data Science VM.</span></span> <span data-ttu-id="0943e-103">L'eliminazione del gruppo di risorse comporta l'eliminazione di tutti gli elementi al suo interno e impedisce l'addebito di ulteriori costi per il gruppo.</span><span class="sxs-lookup"><span data-stu-id="0943e-103">Deleting the resource group deletes everything in it and prevents any further charges from being incurred for it.</span></span> <span data-ttu-id="0943e-104">I gruppi di risorse che vengono eliminati non possono essere ripristinati, dunque prima di eliminarli assicurarsi di non averne più bisogno.</span><span class="sxs-lookup"><span data-stu-id="0943e-104">Resource groups that are deleted can't be recovered, so be certain you're finished using it before deleting it.</span></span> <span data-ttu-id="0943e-105">È comunque **importante non lasciare questo gruppo di risorse distribuito più del necessario** perché una Data Science VM è moderatamente costosa.</span><span class="sxs-lookup"><span data-stu-id="0943e-105">However, it is **important not to leave this resource group deployed any longer than necessary** because a Data Science VM is moderately expensive.</span></span>

1. <span data-ttu-id="0943e-106">Tornare al pannello per il gruppo di risorse creato nell'esercizio 1.</span><span class="sxs-lookup"><span data-stu-id="0943e-106">Return to the blade for the resource group you created in Exercise 1.</span></span> <span data-ttu-id="0943e-107">Fare quindi clic sul pulsante **Elimina gruppo di risorse** nella parte superiore del pannello.</span><span class="sxs-lookup"><span data-stu-id="0943e-107">Then click the **Delete resource group** button at the top of the blade.</span></span>

    ![Eliminazione del gruppo di risorse](../images/delete-resource-group.png)

    <span data-ttu-id="0943e-109">_Eliminazione del gruppo di risorse_</span><span class="sxs-lookup"><span data-stu-id="0943e-109">_Deleting the resource group_</span></span>

1. <span data-ttu-id="0943e-110">Per motivi di sicurezza, è necessario digitare il nome del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="0943e-110">For safety, you are required to type in the resource group's name.</span></span> <span data-ttu-id="0943e-111">Una volta eliminato, infatti, un gruppo di risorse non può essere recuperato. Digitare il nome del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="0943e-111">(Once deleted, a resource group cannot be recovered.) Type the name of the resource group.</span></span> <span data-ttu-id="0943e-112">Fare clic sul pulsante **Elimina** per rimuovere tutte le tracce di questo lab dalla sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="0943e-112">Then click the **Delete** button to remove all traces of this lab from your Azure subscription.</span></span>

<span data-ttu-id="0943e-113">Dopo qualche minuto, il gruppo di risorse e tutte le risorse che contiene verranno eliminati.</span><span class="sxs-lookup"><span data-stu-id="0943e-113">After a few minutes, the resource group and all of its resources will be deleted.</span></span> <span data-ttu-id="0943e-114">Quando si fa clic su **Elimina** la fatturazione si interrompe, quindi il tempo necessario per eliminare le risorse non viene addebitato.</span><span class="sxs-lookup"><span data-stu-id="0943e-114">Billing stops when you click **Delete**, so you're not charged for the time required to delete the resources.</span></span> <span data-ttu-id="0943e-115">Analogamente, la fatturazione non inizia finché le risorse non sono state distribuite correttamente e completamente.</span><span class="sxs-lookup"><span data-stu-id="0943e-115">Similarly, billing doesn't start until the resources are fully and successfully deployed.</span></span>

### <a name="summary"></a><span data-ttu-id="0943e-116">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="0943e-116">Summary</span></span>

<span data-ttu-id="0943e-117">I passaggi descritti in questo lab potrebbero essere generalizzati per eseguire altri tipi di attività di classificazione delle immagini.</span><span class="sxs-lookup"><span data-stu-id="0943e-117">The steps in this lab may be generalized to perform other types of image-classification tasks.</span></span> <span data-ttu-id="0943e-118">Ad esempio, si potrebbe eseguire il training dello stesso modello di TensorFlow per riconoscere le immagini di gatti o per identificare parti difettose prodotte da una catena di montaggio.</span><span class="sxs-lookup"><span data-stu-id="0943e-118">For example, you could train the same TensorFlow model to recognize cat images or identify defective parts parts produced on an assembly line.</span></span> <span data-ttu-id="0943e-119">La classificazione delle immagini è attualmente uno degli usi più diffusi del Machine Learning, la cui utilità è destinata ad aumentare nel corso del tempo.</span><span class="sxs-lookup"><span data-stu-id="0943e-119">Image classification is one of the most prevalent uses of machine learning today, and its usefulness will only increase over time.</span></span> <span data-ttu-id="0943e-120">Ora che è disponibile una base di partenza, provare a creare alcuni modelli di classificazione delle immagini personalizzati</span><span class="sxs-lookup"><span data-stu-id="0943e-120">Now that you have a basis to work from, try creating some image-classification models of your own.</span></span> <span data-ttu-id="0943e-121">per sperimentare e trovare idee utili.</span><span class="sxs-lookup"><span data-stu-id="0943e-121">You never know what might come of it!</span></span>