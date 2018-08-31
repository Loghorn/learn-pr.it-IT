In questo esercizio verrà creata un'app per le funzioni di Azure.

1. Accedere al [portale di Azure](https://portal.azure.com) con il proprio account Azure.
1. Selezionare il pulsante **Crea una risorsa** visualizzato nell'angolo superiore sinistro del portale di Azure, quindi selezionare **Calcolo > App per le funzioni**.
  ![Creare una risorsa app per le funzioni](../images/4-create-function-app-blade.png)
1. Scegliere un nome app globalmente univoco. Questo nome sarà l'URL di base del servizio. È possibile denominarla **escalator-functions-xxxxxxx**, dove la x può essere sostituita con le proprie iniziali e l'anno di nascita. Se il nome non risulta globalmente univoco, è possibile provare un'altra combinazione. I caratteri validi sono a-z, 0-9 e -.
1. Selezionare la sottoscrizione di Azure in cui ospitare l'app per le funzioni.
1. Creare un nuovo gruppo di risorse denominato **escalator-functions-group**. Questo approccio ne semplificherà la successiva eliminazione.
1. Selezionare **Windows** come sistema operativo.
1. Per il **Piano di hosting**, selezionare **Piano a consumo** in modo da poter usufruire dei vantaggi delle funzionalità di Azure senza server.
1. Selezionare la località geografica più vicina all'utente (o ai clienti).
1. Creare un nuovo account di archiviazione e denominarlo **escalatorfunctions**.
1. Verificare che Azure Application Insights sia impostato su **On**e selezionare l'area più vicina all'utente.
1. Selezionare **Crea**; il processo di distribuzione richiederà alcuni minuti. L'utente riceverà una notifica al completamento dell'operazione.
  ![Impostazioni dell'app per le funzioni](../images/4-create-function-app-settings.png)

## <a name="verify-your-azure-function-app"></a>Verificare l'app per le funzioni di Azure

1. Nel menu a sinistra del portale di Azure selezionare **Gruppi di risorse**. Viene visualizzato **escalator-functions-group** nell'elenco dei gruppi disponibili.
  ![Gruppo di risorse](../images/4-resource-group.png)
1. Selezionare **escalator-functions-group**. Viene visualizzato un elenco di risorse simile al seguente: ![Elenco di risorse](../images/4-resource-list.png)
