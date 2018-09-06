La **Data Science Virtual Machine** per Linux è un'immagine di macchina virtuale che semplifica gli approcci iniziali alle tecnologie di Data Science. Più strumenti sono già compilati, installati e configurati per essere operativi rapidamente. Sono anche inclusi il driver GPU NVIDIA, NVIDIA CUDA, e la libreria NVIDIA CUDA Deep Neural Network (cuDNN), così come Jupyter e TensorFlow. Tutti i framework preinstallati sono abilitati per la GPU, ma funzionano anche su CPU.

## <a name="what-is-covered-in-this-lab"></a>Contenuto del lab

 In questo lab si eseguiranno le attività seguenti:
* Creare una Data Science Virtual Machine di Linux in Azure
* Connettersi alla DSVM tramite desktop remoto
* Eseguire il training di un modello TensorFlow per classificare le immagini contenenti hot dog e quelle che non contengono hot dog
* Usare il modello in un'app Python

Per completare questo lab, sono necessari una sottoscrizione di Azure e un client di desktop remoto Xfce. Vedere la sezione dei prerequisiti per altri dettagli. Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) prima di iniziare.

### <a name="prerequisites-for-the-lab"></a>Prerequisiti per il lab

 1. **Account di Microsoft Azure**: sarà necessario un account di Azure valido e attivo per questo lab. Se non è disponibile, è possibile iscriversi per una [versione di valutazione gratuita](https://azure.microsoft.com/en-us/free/).

    * I sottoscrittori attivi di Visual Studio hanno diritto a un credito di € 45-130 al mese. È possibile fare riferimento a questo [collegamento](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/) per altri dettagli, incluse le istruzioni per attivare e iniziare a usare il credito Azure mensile.

    * Se non si è un sottoscrittore di Visual Studio, è possibile effettuare l'iscrizione per il programma gratuito [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) per creare un **account Azure gratuito** (include 1 anno di servizi gratuiti e € 170 per il primo mese).

    * Gli studenti possono iscriversi per richiedere un account [Azure for Students](https://aka.ms/azure4students) e ottenere $ 100 di crediti di Azure e un anno di servizi gratuiti, senza carta di credito. 

1. Un client desktop remoto [Xfce](https://xfce.org/), come [X2Go](https://wiki.x2go.org/doku.php/download:start).