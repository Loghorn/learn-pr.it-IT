In questa unità si userà un modello di Azure Resource Manager per decrittografare la macchina virtuale Windows creata in precedenza. Nella macchina virtuale Windows è stata crittografata l'unità del sistema operativo. Nell'unità del sistema operativo, tuttavia, non sarà presente alcuna informazione e di conseguenza è possibile lasciarla decrittografata. È possibile usare un modello per decrittografare l'unità del sistema operativo.

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a>Configurare e distribuire una nuova macchina virtuale usando un modello di Azure Resource Manager

Viene usato un modello di che Microsoft ha pubblicato su Github per creare una nuova macchina virtuale con la crittografia attivata per impostazione predefinita.

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) con lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Fare clic su **Crea risorsa** nella barra laterale sinistra.

1. Digitare **modello** nella casella di ricerca.

1. Selezionare **Modello distribuzione** nell'elenco dei risultati e fare clic su **Crea**.

    ![Schermata che illustra l'elemento di distribuzione del modello selezionato con il pulsante Crea evidenziato](../media/6-create-template.png)

1. Nella casella di ricerca **Selezionare modello** iniziare a digitare "201-decrypt" e selezionare il modello "201-decrypt-running-windows-vm-without-aad".

    ![Schermata che illustra la casella di ricerca di selezione di un modello con il completamento automatico](../media/6-custom-deployment.png)

1. Fare clic su **Seleziona modello** per avviare lo strumento di esecuzione del modello.

1. Nella vista impostazioni immettere le informazioni seguenti:
    - Selezionare _Concierge Subscription_ (Sottoscrizione Concierge) per il campo **Sottoscrizione**.
    - Selezionare il gruppo di risorse creato nell'ambiente sandbox.
    - Selezionare la località in cui è stata creata la macchina virtuale.
    - Immettere "fmdata vm01" per il **Nome macchina virtuale**.
    - Lasciare **Tipo di volume** impostato su _Tutti_.

1. Selezionare la casella di controllo **Accetto i termini e le condizioni**.
1. Fare clic su **Acquisto** per eseguire il modello. Si noti che non sono previsti costi perché questo è un pulsante standard.

Per il completamento della distribuzione possono essere necessari alcuni minuti.

## <a name="verify-the-encryption-status-of-the-vm"></a>Verificare lo stato di crittografia della macchina virtuale

1. Nella barra laterale del portale di Azure fare clic su **Macchine virtuali** e quindi selezionare la macchina virtuale **fmdata-vm01**. In alternativa, è possibile cercare la macchina virtuale in base al nome in **Tutte le risorse**.

1. Nel pannello **Macchina virtuale**, in **IMPOSTAZIONI**, fare clic su **Dischi**.

1. Nel pannello **Dischi** notare che lo stato di crittografia dei dischi del sistema operativo è ora **Disabilitato**.
