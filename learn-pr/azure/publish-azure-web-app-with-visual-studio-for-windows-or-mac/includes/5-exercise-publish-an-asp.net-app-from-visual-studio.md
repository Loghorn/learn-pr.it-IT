Ora si dispone di un'applicazione Web di ASP.NET Core in esecuzione localmente. In questo esercizio si pubblicherà l'applicazione nel servizio app di Azure.

### <a name="visual-studio-for-windows"></a>Visual Studio per Windows

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Pubblica**.

1. Nella finestra di dialogo che viene visualizzata scegliere, a sinistra, **Servizio app** come destinazione della pubblicazione.  A destra selezionare **Crea nuovo** per creare un nuovo servizio app.

1. Scegliere il pulsante **Pubblica** per creare il nuovo servizio app.

> [!NOTE]
> Quando si distribuiscono le app, è possibile creare un nuovo piano di servizio app o continuare ad aggiungere le app a un piano esistente. Per questo esercizio, si creerà un nuovo **servizio app di Azure**.

### <a name="visual-studio-mac"></a>Visual Studio per Mac

1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni.

1. Selezionare **Pubblica**.

1. Fare clic su **Pubblica in Azure**. Questo permette di connettersi all'account di Azure. Potrebbero occorrere alcuni minuti per connettersi e visualizzare l'elenco dei servizi correnti.

1. Fare clic su **Nuovo** per creare un nuovo servizio app.

## <a name="configure-your-new-azure-app-service"></a>Configurare il nuovo servizio app di Azure

1. Nella finestra di dialogo **Crea servizio app** fare clic su **Aggiungi un account** e accedere alla sottoscrizione di Azure. Se è già stato eseguito l'accesso, selezionare l'account contenente la sottoscrizione desiderata dal menu a discesa.

1. Immettere le informazioni necessarie relative al piano di servizio app.

    ![Creare un servizio app](../media-draft/5-CreateAppService.png)

    Nella finestra di dialogo **Crea servizio app** specificare le informazioni seguenti:

    - **Nome app**: il nome dell'applicazione.  Ciò determinerà l'URL dell'applicazione pubblicata, che sarà https://_NomeApp_. azurewebsites.net.  Deve essere un valore univoco. Pertanto, sarà necessario provare alcuni nomi diversi per trovarne uno non riservato.

    - **Sottoscrizione**: sottoscrizione di Azure a cui si desidera distribuire il servizio app.

    - **Gruppo di risorse:** fare clic sul pulsante **Nuovo...** accanto al gruppo di risorse e immettere un nome univoco per il gruppo di risorse.

        ![Nuovo gruppo di risorse](../media-draft/5-NewResourceGroup.png)

    - **Piano di hosting:** specifica la località, le dimensioni e le funzionalità della server farm Web che ospita l'app. È possibile selezionare un piano di hosting esistente o crearne uno nuovo. Per questo esercizio, se ne creerà uno nuovo.

        Fare clic sul pulsante **Nuovo...** accanto al piano di hosting.

        ![Nuovo piano di hosting](../media-draft/5-NewHostingPlan.png)

        Assegnare un nome al piano di hosting e quindi specificare la località e le dimensioni.  
        
        > [!NOTE]
        > La specifica di dimensioni e località diverse influirà sul costo del servizio. Per questo esercizio, si consiglia di specificare la dimensione **gratuita**, affinché non venga addebitato alcun costo.

    - **Application Insights:** consente di specificare se usare Application Insights per l'applicazione. Per questo esercizio, è consigliabile scegliere **Nessuna**.

1. Scegliere il pulsante **Crea** per avviare il provisioning del servizio app. Viene visualizzato lo stato di avanzamento della distribuzione:

    ![Stato di avanzamento della distribuzione](../media-draft/5-DeployProgress.png)

1. Al termine del provisioning del servizio app, Visual Studio compila e distribuisce l'app Web.  Nell'output di compilazione in Visual Studio, è possibile vedere l'output della distribuzione.

    ![Risultato della pubblicazione](../media-draft/5-PublishResult.png)

1. Complimenti, l'applicazione Web di ASP.NET Core è ora pubblicata e live. Sarà possibile vedere l'URL finale del sito nell'output di compilazione e anche nella pagina di pubblicazione in Visual Studio.

    ![Risultato della pubblicazione](../media-draft/5-PublishPage.png)

1. Per testare il sito Web, passare all'URL indicato.

    ![Sito Live](../media-draft/5-WebPageLive.png)

## <a name="summary"></a>Riepilogo

In questo esercizio è stato illustrato il processo di provisioning di un servizio app di Azure e la distribuzione di un'applicazione Web di ASP.NET Core. Ora si dispone di un'app Web in tempo reale.
