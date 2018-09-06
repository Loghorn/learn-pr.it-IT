### <a name="exercise-1-create-an-ubuntu-data-science-vm"></a>Esercizio 1: Creare una Data Science VM Ubuntu

La Data Science Virtual Machine per Linux è un'immagine di macchina virtuale che semplifica gli approcci iniziali alle tecnologie di Data Science. Più strumenti sono già compilati, installati e configurati per essere operativi rapidamente. Sono inoltre inclusi il driver NVIDIA GPU, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads), e la libreria [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn) (cuDNN), così come [Jupyter](http://jupyter.org/), vari blocchi appunti di Jupyter di esempio e [TensorFlow](https://www.tensorflow.org/). Tutti i framework preinstallati sono abilitati per la GPU, ma funzionano anche su CPU. In questo esercizio verrà creata un'istanza di Data Science Virtual Machine per Linux in Azure.

1. Aprire il [portale di Azure](https://portal.azure.com) nel browser. Se viene richiesto di accedere, usare l'account Microsoft.

1. Fare clic su **+ Crea una risorsa** nel menu sul lato sinistro del portale e quindi digitare "data science" (senza virgolette) nella casella di ricerca. Selezionare **Data Science Virtual Machine for Linux (Ubuntu)** nell'elenco dei risultati.

    ![Ricerca della Data Science VM Ubuntu](../images/new-data-science-vm.png)

    _Ricerca della Data Science VM Ubuntu_

1. Prendersi del tempo per esaminare l'elenco di strumenti inclusi nella macchina virtuale. Fare quindi clic su **Crea** nella parte inferiore del pannello.

1. Compilare il pannello "Informazioni di base" come illustrato di seguito. Specificare una password di almeno 12 caratteri contenente una combinazione di lettere maiuscole, lettere minuscole, numeri e caratteri speciali. *Assicurarsi di ricordare il nome utente e la password immessi, perché saranno necessari in un secondo momento nel lab.*

    ![Immissione delle informazioni di base sulla macchina virtuale](../images/create-data-science-vm-1.png)

    _Immissione delle impostazioni di base_

1. Nel pannello "Scegli una dimensione" selezionare **DS1_V2 Standard**, che rappresenta un modo economico per sperimentare le Data Science VM. Fare quindi clic sul pulsante **Seleziona** nella parte inferiore del pannello.

    ![Scelta della dimensione della macchina virtuale](../images/create-data-science-vm-2.png)

    _Scelta della dimensione della macchina virtuale_

1. Nel pannello "Impostazioni" selezionare **SSH (22)** nell'elenco delle porte in ingresso, in modo che i client possano connettersi alla macchina virtuale usando il protocollo [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) sulla porta 22. Fare quindi clic su **OK**.

    ![Creazione della macchina virtuale](../images/create-data-science-vm-3.png)

    _Creazione della macchina virtuale_

1. Nel pannello "Crea" prendersi un po' di tempo per verificare le opzioni selezionate per la macchina virtuale e fare clic su **Crea** per avviare il processo di creazione della macchina virtuale.

    ![Creazione della macchina virtuale](../images/create-data-science-vm-4.png)

    _Creazione della macchina virtuale_

1. Fare clic su **Gruppi di risorse** nel menu sul lato sinistro del portale. Fare quindi clic sul gruppo di risorse "data-science-rg".

    ![Apertura del gruppo di risorse](../images/open-resource-group.png)

    _Apertura del gruppo di risorse_

1. Attendere fino a quando la distribuzione delle modifiche non viene segnalata come completata, che indica che la DSVM e le risorse di Azure di supporto sono state create. Per la distribuzione sono richiesti in genere al massimo 5 minuti. Fare clic periodicamente su **Aggiorna** nella parte superiore del pannello per aggiornare lo stato di distribuzione.

    ![Monitoraggio dello stato di distribuzione](../images/deployment-succeeded.png)

    _Monitoraggio della distribuzione_

Dopo aver completato la distribuzione, procedere con il prossimo esercizio.