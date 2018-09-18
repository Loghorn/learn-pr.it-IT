### <a name="create-an-ubuntu-data-science-vm"></a>Creare una Data Science VM Ubuntu

La Data Science Virtual Machine per Linux è un'immagine di macchina virtuale che semplifica l'uso della tecnologia Data Science. Molti strumenti sono già stati compilati, installati e configurati per consentire all'utente di essere subito operativo. Sono inoltre inclusi il driver NVIDIA GPU, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) e la libreria [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn) (cuDNN), così come [Jupyter](http://jupyter.org/), vari notebook Jupyter di esempio e [TensorFlow](https://www.tensorflow.org/). Tutti i framework preinstallati sono abilitati per la GPU, ma funzionano anche su CPU. In questa unità verrà creata un'istanza di Data Science Virtual Machine (DSVM) per Linux in Azure.

1. Aprire il [portale di Azure](https://portal.azure.com/?azure-portal=true) nel browser.

1. Fare clic su **Crea una risorsa** nel menu sul lato sinistro del portale e quindi digitare "data science" (senza virgolette) nella casella di ricerca. Selezionare **Data Science Virtual Machine for Linux (Ubuntu)** nell'elenco dei risultati.

    ![Ricerca della Data Science VM Ubuntu](../media-draft/1-new-data-science-vm.png)

1. Dedicare qualche istante all'esame dell'elenco di strumenti inclusi nella macchina virtuale. Fare quindi clic su **Crea** nella parte inferiore del pannello.

1. Compilare il pannello "Informazioni di base" come illustrato di seguito. Specificare una password di almeno 12 caratteri contenente una combinazione di lettere maiuscole, lettere minuscole, numeri e caratteri speciali. *Assicurarsi di ricordare il nome utente e la password immessi, perché saranno necessari in un secondo momento nel modulo.*

    ![Immissione delle informazioni di base sulla macchina virtuale](../media-draft/1-create-data-science-vm-1.png)

1. Nel pannello "Scegli una dimensione" selezionare **DS1_V2 Standard**, che rappresenta un modo economico per sperimentare le Data Science VM. Fare quindi clic sul pulsante **Seleziona** nella parte inferiore del pannello.

    ![Scelta della dimensione della macchina virtuale](../media-draft/1-create-data-science-vm-2.png)

1. Nel pannello **Impostazioni** confermare le impostazioni predefinite e fare clic sul pulsante **OK**.

1. Nel pannello **Crea** dedicare qualche istante alla verifica delle opzioni selezionate per la macchina virtuale e fare clic su **Crea** per avviare il processo di creazione della macchina virtuale.

    ![Creazione della macchina virtuale](../media-draft/1-create-data-science-vm-4.png)

1. Fare clic su **Gruppi di risorse** nel menu sul lato sinistro del portale. Fare quindi clic sul gruppo di risorse **data-science-rg**.

    ![Apertura del gruppo di risorse](../media-draft/1-open-resource-group.png)

  
1. Attendere fino a quando la distribuzione delle modifiche non viene segnalata come completata, che indica che la DSVM e le risorse di Azure di supporto sono state create. Per la distribuzione sono richiesti in genere al massimo cinque minuti. Fare clic periodicamente su **Aggiorna** nella parte superiore del pannello per aggiornare lo stato di distribuzione.

    ![Monitoraggio dello stato di distribuzione](../media-draft/1-deployment-succeeded.png)
