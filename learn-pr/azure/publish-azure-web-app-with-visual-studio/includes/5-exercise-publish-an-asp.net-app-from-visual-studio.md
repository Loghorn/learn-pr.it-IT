È ora disponibile un'applicazione Web ASP.NET Core locale in esecuzione. In questa unità si pubblicherà l'applicazione nel servizio app di Azure.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="visual-studio-for-windows"></a>Visual Studio per Windows

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Pubblica**.

1. Nella finestra di dialogo che viene visualizzata scegliere, a sinistra, **Servizio app** come destinazione della pubblicazione.  A destra, selezionare **Crea nuovo** per creare un'app di servizio App.

1. Scegliere il **pubblica** pulsante per creare l'app.

> [!NOTE]
> Quando si distribuiscono le app, è possibile creare un piano di servizio app o continuare ad aggiungere le app a un piano esistente. Per questo esercizio, si creerà un'app in **servizio App di Azure**.

### <a name="visual-studio-mac"></a>Visual Studio per Mac

1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni.

1. Selezionare **Pubblica**.

1. Fare clic su **Pubblica in Azure**. Questo permette di connettersi all'account di Azure. Potrebbero occorrere alcuni minuti per connettersi e visualizzare l'elenco dei servizi correnti.

1. Fare clic su **New** per creare un'app.

## <a name="configure-your-new-azure-app-service"></a>Configurare il nuovo servizio app di Azure

1. Nella finestra di dialogo **Crea servizio app** fare clic su **Aggiungi un account** e accedere alla sottoscrizione di Azure. Se è già stato eseguito l'accesso, selezionare l'account contenente la sottoscrizione specifica dal menu a discesa.

1. Immettere le informazioni necessarie relative al piano di servizio app.

    ![Creare un servizio app](../media-draft/5-CreateAppService.png)

    Nella finestra di dialogo **Crea servizio app** specificare le informazioni seguenti:

    - **Nome app**: il nome dell'applicazione.  Ciò determinerà l'URL dell'applicazione pubblicata, che sarà https://_NomeApp_. azurewebsites.net.  Deve essere un valore univoco. Sarà quindi necessario provare alcuni nomi diversi per trovarne uno non riservato.

    - **Sottoscrizione**: sottoscrizione di Azure che si desidera distribuire l'app.

    - **Gruppo di risorse:** seleziona esistente <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn> gruppo di risorse.

    - **Piano di hosting:** specifica la località, le dimensioni e le funzionalità della server farm Web che ospita l'app. È possibile selezionare un piano di hosting esistente o crearne uno nuovo. Per questo esercizio, si creerà uno.

        Fare clic sul pulsante **Nuovo** accanto al piano di hosting.

        ![Nuovo piano di hosting](../media-draft/5-NewHostingPlan.png)

        Assegnare un nome al piano di hosting e quindi specificare la località e le dimensioni.  
        
        > [!NOTE]
        > Specificare dimensioni e località diverse influirà sul costo del servizio. Per questo esercizio, si consiglia di specificare la dimensione **gratuita**, affinché non venga addebitato alcun costo.

    - **Application Insights:** consente di specificare se usare Application Insights per l'applicazione. Per questo esercizio, è consigliabile scegliere **Nessuna**.

1. Scegliere il **crea** per avviare l'app di provisioning. Viene visualizzato lo stato di avanzamento della distribuzione:

    ![Stato di avanzamento della distribuzione](../media-draft/5-DeployProgress.png)

1. Al termine del provisioning dell'app, Visual Studio verranno creare e distribuire il codice dell'applicazione.  Nell'output di compilazione in Visual Studio, è possibile vedere l'output della distribuzione.

    ![Risultato della pubblicazione](../media-draft/5-PublishResult.png)

1. L'applicazione Web di ASP.NET Core è ora pubblicata e live. Sarà possibile visualizzare l'URL finale per il sito nell'output di compilazione e anche nella pagina di pubblicazione in Visual Studio.

    ![Risultato della pubblicazione](../media-draft/5-PublishPage.png)

1. Per testare il sito Web, passare all'URL indicato.

    ![Sito Live](../media-draft/5-WebPageLive.png)

## <a name="summary"></a>Riepilogo

In questo esercizio viene illustrato il processo di provisioning di un'app nel servizio App di Azure e la distribuzione di un'applicazione web ASP.NET Core. Ora è disponibile un'app Web live.
