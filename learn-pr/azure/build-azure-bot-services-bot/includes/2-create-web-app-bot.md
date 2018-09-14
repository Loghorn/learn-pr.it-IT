Il primo passaggio nella creazione di un bot è fornire un percorso per l'hosting del bot in Azure. La funzionalità [app Web](https://azure.microsoft.com/services/app-service/web/) del servizio app di Azure è ottimale per ospitare applicazioni bot e il servizio Azure Bot è progettato per effettuarne automaticamente il provisioning. In questa unità si userà il portale di Azure per effettuare il provisioning di un bot app Web di Azure.

1. Aprire il [portale di Azure](https://portal.azure.com/?azure-portal=true) nel browser. Se viene richiesto di accedere, usare l'account Microsoft.

1. Fare clic su **+ Crea una risorsa**, su **Intelligenza artificiale e Machine Learning** e quindi su **Web App Bot** (Bot app Web).
 
    ![Creazione di un nuovo bot app Web di Azure](../media-draft/2-new-bot-service.png)

1. Immettere un nome nella casella **Nome dell'app**, ad esempio "qa-factbot". *Questo nome deve essere univoco all'interno di Azure, dunque assicurarsi che accanto a esso compaia un segno di spunta verde.* In **Gruppo di risorse** selezionare **Crea nuovo** e immettere il nome gruppo di risorse "factbot-rg". Selezionare la località più vicina e il piano tariffario gratuito **F0**. Quindi fare clic su **Bot template** (Modello bot).

    ![Configurazione del bot app Web di Azure](../media-draft/2-portal-start-bot-creation.png)

1. Selezionare **Node.js** come linguaggio dell'SDK e **Domanda e risposta** come tipo di modello. Fare quindi clic su **Seleziona** nella parte inferiore del pannello.   
  
    ![Selezione di linguaggio e modello](../media-draft/2-portal-select-template.png)

1. A questo punto fare clic su **Piano di servizio app/Località**, seguito da **Crea nuovo** e quindi creare un piano di servizio app denominato "qa-factbot-service-plan" o un nome simile nella stessa area selezionata nel passaggio 3. Al termine dell'operazione, fare clic su **Crea** nella parte inferiore del pannello "Web App Bot" (Bot app Web) per avviare la distribuzione. 

1. Fare clic su **Gruppi di risorse** nella barra multifunzione sul lato sinistro del portale. Quindi fare clic su **factbot-rg** per aprire il gruppo di risorse creato per il bot app Web di Azure. Attendere finché "Distribuzione in corso" non cambia in "Operazione completata" nella parte superiore del pannello, indicando che il bot app Web di Azure è stato distribuito correttamente. La distribuzione richiede in genere due minuti o meno. Fare clic periodicamente su **Aggiorna** nella parte superiore del pannello per aggiornare lo stato di distribuzione.

    ![Distribuzione completata](../media-draft/2-deployment-succeeded.png)
  
Quando è stato distribuito il bot app Web di Azure, sono state eseguite numerose operazioni in background. È stato creato e registrato un bot, è stata creata un'[app Web di Azure](https://azure.microsoft.com/services/app-service/web/) per ospitarlo e il bot è stato configurato per l'uso di [Microsoft QnA Maker](https://www.qnamaker.ai/). Il passaggio successivo consiste nell'usare QnA Maker per creare una knowledge base di domande e risposte per integrare intelligenza nel bot.