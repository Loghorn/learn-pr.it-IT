Il primo passaggio per approntare il nuovo sito consiste nel preparare l'ambiente di sviluppo. Per la creazione e la distribuzione di applicazioni Web ASP.NET è necessario avere gli strumenti necessari installati nel computer locale. In questo modulo verranno presentati gli strumenti necessari e le procedure per installarli.

## <a name="prepare-your-development-environment"></a>Preparare l'ambiente di sviluppo

Visual Studio 2017 include due carichi di lavoro necessari per creare, pubblicare e distribuire il sito Web in Azure. Questi carichi di lavoro includono tutti i modelli per il sito ASP.NET e offrono la possibilità di connettere ad Azure e distribuire il sito in Azure.

È necessario assicurarsi di avere i carichi di lavoro seguenti installati:

- Sviluppo ASP.NET e Web

Il carico di lavoro Sviluppo ASP.NET e Web in Visual Studio 2017 è progettato per massimizzare la produttività sviluppando applicazioni Web con ASP.NET e tecnologie basate su standard come HTML e JavaScript.

- Sviluppo di Azure

Il carico di lavoro Sviluppo di Azure in Visual Studio 2017 installa la versione più recente di Azure SDK per .NET e gli strumenti per Visual Studio. Dopo aver installato questi elementi, è possibile visualizzare le risorse in Cloud Explorer, creare risorse con gli strumenti di Azure Resource Manager, creare applicazioni per servizi cloud e Web di Azure ed eseguire operazioni sui Big Data usando gli strumenti Azure Data Lake.

## <a name="how-to-install-the-required-workloads"></a>Come installare i carichi di lavoro richiesti

Si userà il programma di installazione di Visual Studio per modificare i componenti installati come parte di Visual Studio.

- Per avviare il programma di installazione, nel menu Start di Windows scorrere verso il basso fino alla lettera **P** e quindi fare clic su **Programma di installazione di Visual Studio**. In alternativa, dopo aver aperto il menu Start, è sufficiente digitare ```Visual Studio Installer``` per trovare il collegamento al programma di installazione. Premere quindi **INVIO**.

- Verrà visualizzata la finestra del programma di installazione di Visual Studio. Fare clic sul pulsante **Modifica**. Se non è visibile, è possibile selezionare **Modifica** nel menu a discesa **Altro**.

    ![Modificare Visual Studio](../media-draft/3-visual-studio-installer-modify.PNG)

- Assicurarsi che i carichi di lavoro **Sviluppo ASP.NET e Web** e **Sviluppo di Azure** siano selezionati nella sezione **Web e cloud** della scheda **Carichi di lavoro**.   ![Installare i carichi di lavoro](../media-draft/2-select-workloads.png)

Fare quindi clic sul pulsante **Modifica** in basso a destra nel programma di installazione. Il programma di installazione di Visual Studio scaricherà e installerà i componenti necessari. A questo punto si è pronti per creare un'app Web ASP.NET e caricarla in Microsoft Azure.

> [!IMPORTANT]
> Visual Studio per Mac _dovrebbe_ avere i carichi di lavoro necessari preinstallati. Se è necessario reinstallarli, si dovrà scaricare [Visual Studio per Mac](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio-mac/?sku=communitymac&rel=15_), che eseguirà il programma di installazione. Da qui, sarà possibile scegliere i carichi di lavoro da aggiungere.

## <a name="summary"></a>Riepilogo

È possibile creare, gestire e pubblicare un sito Web ASP.NET da Visual Studio 2017 con i carichi di lavoro **Sviluppo ASP.NET e Web** e **Sviluppo di Azure**.
