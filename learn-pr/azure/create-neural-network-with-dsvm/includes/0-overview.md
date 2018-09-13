La **Data Science Virtual Machine** per Linux è un'immagine di macchina virtuale che semplifica gli approcci iniziali alle tecnologie di Data Science. Molti strumenti sono già stati compilati, installati e configurati per consentire all'utente di essere subito operativo. Sono anche inclusi il driver GPU NVIDIA, NVIDIA CUDA, e la libreria NVIDIA CUDA Deep Neural Network (cuDNN), così come Jupyter e TensorFlow. Tutti i framework preinstallati sono abilitati per la GPU, ma funzionano anche su CPU.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Creare una Data Science Virtual Machine di Linux in Azure.
- Connettersi alla DSVM tramite desktop remoto.
- Eseguire il training di un modello TensorFlow per classificare le immagini contenenti hot dog e quelle che non contengono hot dog.
- Usare il modello in un'app Python.

### <a name="prerequisites"></a>Prerequisiti
<!---TODO: This is really long, need to make more concise and also add to index.yml--->

Per completare questo modulo, sono necessari una sottoscrizione di Azure e un client desktop remoto Xfce. Per altri dettagli, vedere la sezione dei prerequisiti. Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

 1. **Account di Microsoft Azure**: per questo modulo è necessario un account di Azure valido e attivo. Se non è disponibile, è possibile registrarsi per accedere a una [versione di valutazione gratuita](https://azure.microsoft.com/free/).

    * I sottoscrittori attivi di Visual Studio hanno diritto a un credito di € 45-130 al mese. Per altri dettagli, fare riferimento a questo [collegamento](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), che consente di accedere anche alle istruzioni per attivare e iniziare a usare il credito Azure mensile.

    * Se non si è un sottoscrittore di Visual Studio, è possibile iscriversi al programma gratuito [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) per creare un **account Azure gratuito** (include un anno di servizi gratuiti e € 170 di crediti per il primo mese).

    * Gli studenti possono iscriversi per richiedere un account [Azure for Students](https://aka.ms/azure4students) e ottenere € 86 di crediti Azure e un anno di servizi gratuiti, senza carta di credito. 

1. Un client desktop remoto [Xfce](https://xfce.org/), come [X2Go](https://wiki.x2go.org/doku.php/download:start)