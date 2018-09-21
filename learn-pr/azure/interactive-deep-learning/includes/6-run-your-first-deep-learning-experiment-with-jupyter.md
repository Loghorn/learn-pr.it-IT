![Logo di PyTorch](../media/5-image1.png) 

Gli ingegneri specializzati in apprendimento profondo non implementano in genere a mano tutte le operazioni algebriche su matrici hardcoded, ma si servono di framework come PyTorch o TensorFlow.  

PyTorch è un framework basato su Python che offre flessibilità come piattaforma di sviluppo per l'apprendimento profondo. È basato su NumPy, la libreria di calcolo scientifico Python. 

A questo punto ci si potrebbe chiedere perché usare PyTorch per creare modelli di apprendimento avanzato?  

- L'API è intuitiva: l'apprendimento è rapido se l'utente già conosce Python.
- Include il supporto per Python. PyTorch si integra infatti con lo stack di calcolo scientifico.
- Include i grafici di calcolo dinamico. Invece di grafici predefiniti con funzionalità specifiche, PyTorch crea dinamicamente grafici di calcolo che è possibile modificare in fase di esecuzione. I grafici di calcolo dinamico sono utili per l'invio in batch annidato e quando non si conosce la memoria necessaria per la creazione di una determinata rete.

Per altre informazioni su PyTorch, vedere la [documentazione ufficiale su PyTorch.org](https://pytorch.org/about/).

## <a name="run-your-first-pytorch-model"></a>Eseguire il primo modello PyTorch

Dopo aver creato un contenitore Docker sottoposto a provisioning da un'immagine PyTorch, è il momento di iniziare a sperimentare. Come si ricorderà, è stato scaricato un notebook da [python.org](https://python.org). Questo notebook di esempio illustra la procedura per eseguire il training di una rete finalizzato alla classificazione di immagini in categorie diverse. Definisce una rete neurale convoluzionale (CNN) profonda.

1. Passare nel browser locale al server Jupyter Notebook configurato nell'esercizio precedente. L'URL sarà nel formato:

    `<HOSTNAME>.<REGION>.cloudapp.azure.com:8888/?token={sometoken}`

1. Selezionare il notebook `first_pytorch_classifier.ipynb` nel dashboard.

    ![Selezionare il primo notebook first_pytorch_classifier.ipynb](../media/5-image2.PNG)

    Seguire le istruzioni contenute nel notebook per eseguire il training del primo classificatore PyTorch.

    ![screenshot del notebook di training di un classificatore](../media/5-image3.PNG)

2. Iniziare dalla parte superiore del notebook ed eseguire ogni cella in ordine. Tenere presente quanto segue:

    - L'esecuzione di alcune celle richiede molto tempo. Osservare il puntino in alto a destra del notebook accanto alle parole "Python 3". Quando il kernel è occupato con un'operazione, il punto diventa un cerchio pieno più scuro. Rimarrà visualizzato in questo modo fino al termine dell'operazione. 
    - Si esegue il training di una rete CNN per classificare le immagini. Una volta completato il training, il notebook testerà le immagini etichettate a fronte del modello. Registra la stima eseguita per ogni immagine e calcola l'accuratezza del modello. I risultati verranno visualizzati nel formato seguente.

    ![Risultati del training che indicano l'accuratezza del modello](../media/accuracy.png)
    
    - È possibile acquisire familiarità con il notebook nella [documentazione delle esercitazioni di PyTorch](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html) online.
    
    - Verso la fine del notebook, le note indicano un training su una GPU. Se si sono eseguiti gli esercizi in questo modulo, è stata configurata una VM basata su CPU. Questa situazione è ideale per un modello di queste dimensioni e potrebbero non essere riscontrati miglioramenti significativi nel tempo di training con una GPU. Per provare il modulo usando una macchina virtuale con GPU, è necessario apportare due modifiche:
    - Effettuare il provisioning di DSVM in una dimensione di VM serie N abilitata per la GPU.
    - Creare un contenitore usando `nvidia-docker` invece di `docker` nell'esercizio precedente.