### <a name="train-a-tensorflow-model"></a>Eseguire il training di un modello TensorFlow

In questo esercizio si eseguirà il training di un modello di classificazione delle immagini compilato con [TensorFlow](https://www.tensorflow.org/) per riconoscere le immagini che contengono hot dog. Invece di creare il modello da zero, operazione che richiederebbe una quantità elevata di potenza di calcolo e decine o centinaia di migliaia di immagini, si personalizzerà un modello preesistente, pratica nota come [transfer learning](https://en.wikipedia.org/wiki/Transfer_learning). La tecnica del transfer learning consente di raggiungere livelli elevati di precisione con un training di pochi minuti su un laptop o PC standard con poche decine di immagini.

Nel contesto del deep learning, il transfer learning richiede di iniziare con una rete neurale profonda già sottoposta a training per eseguire la classificazione di immagini, per poi aggiungere un livello che personalizza la rete per il dominio specifico del problema, ad esempio, per classificare le immagini in due gruppi : quelle contenenti hot dog e quelle che non li contengono. Sono disponibili in più di 20 modelli di classificazione delle immagini di TensorFlow già sottoposti a training all'indirizzo <https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models.>. I modelli [Inception](https://arxiv.org/abs/1512.00567) e [ResNet](https://towardsdatascience.com/an-overview-of-resnet-and-its-variants-5281e2f56035) sono caratterizzati da una maggiore precisione e requisiti a livello di risorse proporzionalmente più alti, mentre i modelli MobileNet sono stati sviluppati per i dispositivi motivi per offrire compattezza ed efficienza a discapito della precisione. Tutti questi modelli sono ben noti alla community del deep learning e sono stati usati in numerose competizioni, così come in applicazioni reali. Si userà uno dei modelli MobileNet come base per la rete neurale, in modo da ottenere un equilibrio ragionevole tra precisione e tempi di training.

Il training del modello prevede poco di più che eseguire uno script Python che scarica il modello di base e aggiunge un livello sottoposto a training con le immagini e le etichette specifiche del dominio. Lo script necessario è disponibile in GitHub e le immagini che verranno usate sono state raccolte da migliaia di immagini di cibo di dominio pubblico disponibili da [Kaggle](https://www.kaggle.com).

1. Nella Data Science VM fare clic sull'icona del Terminale nella parte inferiore della schermata per aprire una finestra del terminale.

    ![Avvio di una finestra del terminale](../media-draft/3-launch-terminal.png)

    _Avvio di una finestra del terminale_

1. Eseguire il comando seguente nella finestra del terminale per passare alla cartella "notebooks":

    ```bash
    cd notebooks
    ```
    Questa cartella viene prepopolata con i blocchi appunti di Jupyter di esempio preparati per la DSVM.

1. A questo punto è possibile usare il comando seguente per clonare il repository "TensorFlow for Poets" da GitHub:

    ```bash
    git clone https://github.com/googlecodelabs/tensorflow-for-poets-2
    ```
    > **Suggerimento**: è possibile copiare questa riga negli Appunti e quindi usare **MAIUSC+INS** per incollarla nella finestra del terminale.

    Questo repository contiene script per la creazione di modelli di transfer learning, richiamando un modello sottoposto a training per classificare un'immagine, e altro ancora. Fa parte di [Google Codelabs](https://codelabs.developers.google.com/), che contiene un'ampia gamma di risorse e lab pratici per gli sviluppatori di software interessati ad approfondire TensorFlow e altri strumenti e API di Google.

1. Una volta completata la clonazione, passare alla cartella contenente il modello clonato:

    ```bash
    cd tensorflow-for-poets-2
    ```

1. Usare il comando seguente per scaricare le immagini che verranno usate per il training del modello:

    ```bash
    wget https://topcs.blob.core.windows.net/public/tensorflow-resources.zip -O temp.zip; unzip temp.zip -d tf_files; rm temp.zip
    ```

    Questo comando scarica un file ZIP contenente centinaia di immagini di cibo (metà che includono hot dog e metà senza) e le copia nella sottodirectory denominata "tf_files."

1. Fare clic sull'icona di File Manager nella parte inferiore della schermata per aprire una finestra di File Manager.

    ![Avvio di File Manager](../media-draft/3-launch-file-manager.png)

    _Avvio di File Manager_

1. In File Manager passare alla cartella "notebooks/tensorflow-for-poets-2/tf_files". Verificare che la cartella contenga una coppia di sottodirectory denominate "hot_dog" e "not_hot_dog." La prima contiene numerose centinaia di immagini che includono hot dog, mentre la seconda contiene un numero uguale di immagini che **non** includono hot dog. Esplorare le immagini nella cartella "hot_dog" per farsi un'idea dell'aspetto di queste immagini. Dare un'occhiata anche alle immagini nella cartella "not_hot_dog".

    > Per eseguire il training di una rete neurale per determinare se un'immagine include un hot dog, si eseguirà il training sia con immagini che includono hot dog che con immagini che non li includono.

    ![Immagini nella cartella "hot_dog"](../media-draft/3-hot-dog-images.png)

    *Immagini nella cartella "hot_dog"*

    Verificare anche che la cartella contenga un file di testo denominato **retrained_labels_hotdog.txt**. Questo file identifica le sottodirectory contenenti le immagini di training e viene usato dallo script di Python che esegue il training del modello. Lo script enumera i file in ogni sottodirectory identificato nel file di testo (nome del file di testo è un parametro passato allo script) e utilizza questi file per il training della rete.

1. Aprire una seconda finestra del terminale e passare alla cartella "notebooks/tensorflow-for-poets-2", ovvero la stessa aperta nella prima finestra del terminale. Usare quindi il comando seguente per avviare [TensorBoard](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard), ovvero un set di strumenti usati per visualizzare i modelli di TensorFlow e ottenere informazioni dettagliate per il processo di transfer learning:

     ```bash
     tensorboard --logdir tf_files/training_summaries
     ```

     > Questo comando avrà esito negativo se è già in esecuzione un'istanza di TensorBoard. Se si riceve una notifica che la porta 6006 è già in uso, usare un comando ```pkill -f "tensorboard"``` per terminare il processo esistente. Eseguire quindi di nuovo il comando ```tensorboard```.

1. Tornare alla finestra del terminale originale ed eseguire i comandi seguenti:

    ```bash
    IMAGE_SIZE=224;
    ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}";
    ```

    Questi comandi inizializzano le variabili di ambiente specificando la risoluzione delle immagini di training e il modello di base su cui verrà creata la rete neurale. I valori validi per IMAGE_SIZE sono 128, 160, 192 e 224. Valori più alti aumentano il tempo di training, ma anche l'accuratezza del classificatore.

1. A questo punto eseguire il comando seguente per avviare il processo di transfer learning, vale a dire, per eseguire il training del modello con le immagini scaricate:

    ```bash
    python scripts/retrain.py \
    --bottleneck_dir=tf_files/bottlenecks \
    --how_many_training_steps=500 \
    --model_dir=tf_files/models/ \
    --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
    --output_graph=tf_files/retrained_graph_hotdog.pb \
    --output_labels=tf_files/retrained_labels_hotdog.txt \
    --architecture="${ARCHITECTURE}" \
    --image_dir=tf_files \
    --testing_percentage=15 \
    --validation_percentage=15
    ```

    **retrain.py** è uno degli script nel repository scaricato. È complesso e composto da più di 1.000 righe di codice e commenti. Il processo dello script prevede il download del modello specificato con l'opzione ```--architecture``` e l'aggiunta a tale modello di un nuovo livello sottoposto a training con le immagini disponibili nelle sottodirectory della directory specificata con l'opzione ```--image_dir```. Ogni immagine viene etichettata con il nome della sottodirectory in cui si trova, in questo caso, "hot_dog" o "not_hot_dog" e in questo modo la rete neurale modificata può classificare le immagini di input come immagini con hot-dog ("hot_dog") o immagini senza hot dog ("not_hot_dog"). L'output della sessione di training è un file di modello TensorFlow denominato **retrained_graph_hotdog.pb**. Il nome e il percorso vengono specificati nell'opzione ```--output_graph```.

1. Attendere il completamento del training, che dovrebbe richiedere meno di cinque minuti. Controllare quindi l'output per determinare la precisione del modello. I risultati possono variare leggermente rispetto a quello riportato di seguito, perché il processo di training include un minimo di stima casuale.

      ![Valutazione della precisione del modello](../media-draft/3-running-transfer-learning.png)

1. Fare clic sull'icona del browser nella parte inferiore del desktop per aprire il browser installato nella Data Science VM. Passare quindi a <http://0.0.0.0:6006> per connettersi a TensorBoard.

    ![Avvio di Firefox](../media-draft/3-launch-firefox.png)

1. Esaminare il grafico con l'etichetta "accuracy_1". La linea blu rappresenta la precisione ottenuta nel tempo durante l'esecuzione dei 500 passaggi di training specificati con l'opzione ```how_many_training_steps```. Questa metrica è importante, perché mostra come la precisione del modello si evolva man mano che procede il training. Altrettanto importante è la distanza tra le linee blu e arancione che quantifica la quantità di overfitting che si è verificato e che dovrebbe essere sempre ridotto al minimo. [Overfitting](https://en.wikipedia.org/wiki/Overfitting) significa che il modello è ideale per la classificazione delle immagini con il quale è stato sottoposto a training, ma non per la classificazione di altre immagini presentate. In questo caso i risultati sono accettabili, perché esiste una differenza minore del 10% tra la linea arancione (la precisione di "training" ottenuta con le immagini di training) e la linea blu (la precisione di "convalida" ottenuta con il test con immagini all'esterno del set di training).

    ![Visualizzazione dei valori scalari di TensorBoard](../media-draft/3-tensorboard-scalars.png)

1. Fare clic su **GRAPHS** (GRAFICI) nel menu di TensorBoard ed esaminare il grafico visualizzato. Lo scopo principale di questo grafico è rappresentare la rete neurale e i livelli che la compongono. In questo esempio, "input_1" è il livello che è stato sottoposto a training con le immagini di cibo e aggiunto alla rete. "MobilenetV1" è la rete neurale di base di partenza. Contiene molti livelli che non sono visualizzati. Se si fosse creata una rete neurale dettagliata da zero, tutti i livelli sarebbero rappresentati in questo grafico. (Se si desidera vedere i livelli che costituiscono MobileNet, fare doppio clic sul blocco "MobilenetV1" nel diagramma.) Per altre informazioni sulla visualizzazione Graphs (Grafici) e le informazioni presentate, vedere [TensorBoard: Graph Visualization](https://www.tensorflow.org/programmers_guide/graph_viz) (TensorBoard: visualizzazione dei grafici).

    ![Visualizzazione Graphs (Grafici) di TensorBoard](../media-draft/3-tensorboard-graphs.png)

1. Tornare a File Manager e passare alla cartella "notebooks/tensorflow-for-poets-2/tf_files". Verificare che contenga un file denominato **retrained_graph_hotdog.pb**. *Questo file è stato creato durante il processo di training e contiene il modello TensorFlow sottoposto a training*. Verrà usato nel prossimo esercizio per richiamare il modello dall'app NotHotDog.

Lo script eseguito nel passaggio 10 specifica 500 passaggi di training, che rappresentano un buon compromesso tra precisione e tempo necessario per il training. Se si preferisce, provare a eseguire di nuovo il training del modello con un valore ```how_many_training_steps``` più elevato, ad esempio 1000 o 2000. Un numero di passaggi più alto corrisponde in genere a una maggiore precisione, ma a discapito di tempi di training maggiori. Prestare attenzione all'overfitting, che, come promemoria, è rappresentato dalla differenza tra le linee blu e arancione nella visualizzazione dei valori scalari di TensorBoard.