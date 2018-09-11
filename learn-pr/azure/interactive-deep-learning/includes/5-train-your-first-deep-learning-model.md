## <a name="train-your-first-deep-learning-model-using-pytorch-and-jupyter"></a><span data-ttu-id="b9dcc-101">Eseguire il training del primo modello di apprendimento profondo con PyTorch e Jupyter</span><span class="sxs-lookup"><span data-stu-id="b9dcc-101">Train your first deep learning model using PyTorch and Jupyter</span></span>

![Logo di PyTorch](../media/5-image1.PNG) 

<span data-ttu-id="b9dcc-103">Gli ingegneri specializzati in apprendimento profondo non eseguono in genere a mano tutte le operazioni algebriche su matrici hardcoded,</span><span class="sxs-lookup"><span data-stu-id="b9dcc-103">Typically deep learning engineers do not hard code matrix algebra operations all by hand.</span></span> <span data-ttu-id="b9dcc-104">ma si servono di framework come PyTorch o TensorFlow.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-104">They instead use frameworks such as PyTorch or TensorFlow.</span></span>  

<span data-ttu-id="b9dcc-105">PyTorch è un framework basato su Python che offre flessibilità come piattaforma di sviluppo per l'apprendimento profondo.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-105">PyTorch is a python-based framework that provides flexibility as a deep learning development platform.</span></span> <span data-ttu-id="b9dcc-106">Il flusso di lavoro di PyTorch è basato sulla libreria di calcolo scientifico Python numpy.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-106">PyTorch's workflow is built on top of python scientific computing library numpy.</span></span> 

<span data-ttu-id="b9dcc-107">A questo punto ci si potrebbe chiedere perché usare PyTorch per creare modelli di apprendimento avanzato?</span><span class="sxs-lookup"><span data-stu-id="b9dcc-107">Now you might ask, why would we use PyTorch to build deep learning models?</span></span>  

- <span data-ttu-id="b9dcc-108">È un'API semplice da usare, analogamente a Python.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-108">Easy to use API – It's as simple as python can be.</span></span>
- <span data-ttu-id="b9dcc-109">Include il supporto per Python. PyTorch si integra infatti con lo stack di calcolo scientifico.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-109">Python support – PyTorch smoothly integrates with the scientific computing stack.</span></span>
- <span data-ttu-id="b9dcc-110">Include i grafici di calcolo dinamico. Invece di grafici predefiniti con funzionalità specifiche, PyTorch crea dinamicamente grafici di calcolo che è possibile modificare in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-110">Dynamic computation graphs – Instead of predefined graphs with specific functionalities, PyTorch build computational graphs dynamically that can be modified during runtime.</span></span> <span data-ttu-id="b9dcc-111">I grafici di calcolo dinamico sono utili per l'invio in batch annidato e quando non si conosce la memoria necessaria per la creazione di una determinata rete.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-111">Dynamic computation graphs are valuable for nested batching and when we do not know how much memory will be needed for creating a given network.</span></span>

## <a name="run-your-first-pytorch-model"></a><span data-ttu-id="b9dcc-112">Eseguire il primo modello PyTorch</span><span class="sxs-lookup"><span data-stu-id="b9dcc-112">Run your first PyTorch model</span></span>

<span data-ttu-id="b9dcc-113">Passare all'istanza di Jupyter Notebook configurata nell'ultimo capitolo.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-113">Navigate to the Jupyter Notebook that you set up in the last chapter.</span></span>

- <span data-ttu-id="b9dcc-114">[[NOME HOST DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="b9dcc-114">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span></span>

<span data-ttu-id="b9dcc-115">Selezionare il primo notebook first_pytorch_classifier.ipynb.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-115">Select the first_pytorch_classifier.ipynb notebook</span></span>

![Selezionare il primo notebook first_pytorch_classifier.ipynb](../media/5-image2.PNG)

<span data-ttu-id="b9dcc-117">Seguire le istruzioni contenute nel notebook per eseguire il training del primo classificatore PyTorch.</span><span class="sxs-lookup"><span data-stu-id="b9dcc-117">Follow the instructions in the notebook to train your first PyTorch classifer.</span></span>

![Screenshot del notebook](../media/5-image3.PNG)
