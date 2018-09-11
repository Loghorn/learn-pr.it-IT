## <a name="train-your-first-deep-learning-model-using-pytorch-and-jupyter"></a>Eseguire il training del primo modello di apprendimento profondo con PyTorch e Jupyter

![Logo di PyTorch](../media/5-image1.PNG) 

Gli ingegneri specializzati in apprendimento profondo non eseguono in genere a mano tutte le operazioni algebriche su matrici hardcoded, ma si servono di framework come PyTorch o TensorFlow.  

PyTorch è un framework basato su Python che offre flessibilità come piattaforma di sviluppo per l'apprendimento profondo. Il flusso di lavoro di PyTorch è basato sulla libreria di calcolo scientifico Python numpy. 

A questo punto ci si potrebbe chiedere perché usare PyTorch per creare modelli di apprendimento avanzato?  

- È un'API semplice da usare, analogamente a Python.
- Include il supporto per Python. PyTorch si integra infatti con lo stack di calcolo scientifico.
- Include i grafici di calcolo dinamico. Invece di grafici predefiniti con funzionalità specifiche, PyTorch crea dinamicamente grafici di calcolo che è possibile modificare in fase di esecuzione. I grafici di calcolo dinamico sono utili per l'invio in batch annidato e quando non si conosce la memoria necessaria per la creazione di una determinata rete.

## <a name="run-your-first-pytorch-model"></a>Eseguire il primo modello PyTorch

Passare all'istanza di Jupyter Notebook configurata nell'ultimo capitolo.

- [[NOME HOST DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}

Selezionare il primo notebook first_pytorch_classifier.ipynb.

![Selezionare il primo notebook first_pytorch_classifier.ipynb](../media/5-image2.PNG)

Seguire le istruzioni contenute nel notebook per eseguire il training del primo classificatore PyTorch.

![Screenshot del notebook](../media/5-image3.PNG)
