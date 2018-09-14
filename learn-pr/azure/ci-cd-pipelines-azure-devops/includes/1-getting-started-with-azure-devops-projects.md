Configurazione di una pipeline CI/CD sempre stato problematico per eseguire rapidamente. A questo punto, è estremamente facile da implementare da zero a un progetto Azure DevOps end to end completo. E nel progetto DevOps di Azure, si ottiene quanto segue:

1. Infrastruttura di provisioning per l'utente in Azure.

2. Un progetto Team in un'istanza di Visual Studio Team Services.

3. Codice sorgente per un'app di esempio nella lingua scelto nel repository dell'istanza di Visual Studio Team Services.

4. Una pipeline di compilazione e versione che risulta utile per le tecnologie selezionato.

E quando relativo dell'operazione eseguita, Azure DevOps progetto accetta l'app di esempio, compila e lo rilascia tramite la pipeline nell'infrastruttura viene effettuato il provisioning per l'iscrizione in Azure. E ottenere tutti questi vantaggi con un paio di clic.

## <a name="create-an-azure-devops-project"></a>Creare un progetto DevOps di Azure

Si crea un progetto Azure DevOps dal portale di Azure.

1. Head per il [portale di Azure](https://portal.azure.com) e fare clic su `Create a resource`  
![AzurePortal](../media-drafts/1-azureportal.png)

2. Fare clic su`DevOps Project`.  
![Scegliere il progetto DevOps](../media-drafts/1-pickdevopsproject.png)

3. Nella schermata successiva è in cui si ottiene scegliere quale lingua si desidera utilizzare. Si noti come è possibile scegliere .NET (naturalmente), java, nodo, php, python, ruby e go. Puoi anche inserire codice personalizzato da un repository git. Per questa unità, è possibile procedere e creare un'app Node. js. Fare clic su Node. js e fare clic su Avanti  
![Selezionare Node. js per lingua](../media-drafts/1-picknodejsforlang.png)

4. Successivamente verrà chiesto quali framework di Node. js che si desidera utilizzare. Per questa unità, selezionare l'app Node. js semplice e fare clic su Avanti  
![Selezionare il nodo semplice](../media-drafts/1-picksimplenode.png)

5. Successivamente, verrà chiesto il tipo di infrastruttura si vuole effettuare il provisioning ed eseguire l'app in? Per questa unità, è possibile eseguire il provisioning e distribuire in un cluster Kubernetes con Azure Kubernetes Service. Selezionare Kubernetes Service e fare clic su Avanti  
![Selezionare Kubernetes](../media-drafts/1-pickkubernetes.png)

6. E a questo punto, è possibile creare una nuova istanza di Visual Studio Team Services o sceglierne uno esistente. Puoi anche configurare dove e come si desidera che il cluster kubernetes per eseguire. Per questa unità, proseguire e creare una nuova istanza di Visual Studio Team Services scegliendo selezionare Nuovo e assegnare un nome univoco l'istanza di Visual Studio Team Services. Immettere informazioni per il nome del progetto, selezionare la sottoscrizione di azure, assegnare un nome LearnCluster il nome del Cluster, selezionare l'area Stati Uniti orientali per il percorso e fare clic su Fine  
![](../media-drafts/1-finalconfirmation2.png)

E questo è letteralmente tutto! Questa operazione richiede un po' di tempo quindi avviare nuovamente, ridurre e consentire solo ad Azure 2.(e relativa operazione. La maggior parte dei casi verrà eseguita il provisioning e la configurazione dell'infrastruttura di Azure. Per questo modulo, questo sarà il servizio Kubernetes di Azure e registro contenitori di Azure.

## <a name="a-lap-around-the-finished-azure-devops-project"></a>Panoramica di Azure DevOps progetto finito

Quando Azure ha terminato, riceverà una notifica degli avvisi

1. Fare clic sull'avviso e quindi Vai alla risorsa  
![Vai alla risorsa di avviso](../media-drafts/1-gotoresourcefromalert.png)

2. Verrà aperto un pannello del portale che visualizza tutto ciò che è stato effettuato il provisioning. Sul lato sinistro è la pipeline di integrazione continua/recapito Continuo. È necessario nel repository del codice, la definizione di compilazione e anche la definizione di versione. Tutti i collegamenti sono collegamenti diretti da consentono è direttamente la risorsa in Visual Studio Team Services. E a destra, visualizzata tutta l'infrastruttura distribuita automaticamente in Azure. Si dispone di un cluster Kubernetes con l'app di esempio già distribuite e anche application Insights. Ancora una volta, tutti i collegamenti seguenti sono collegamenti diretti alle risorse in Azure.  
![Portal DevOps Proj](../media-drafts/1-portaldevopsproj.png)

3. Fare clic sul collegamento per il codice sorgente  
![Collegamento all'origine dati](../media-drafts/1-linktosource.png)

4. Verrà visualizzata per il repository git nel codice di Azure. Si noti che questo è solo un repository git di normale che contiene l'app Node. js di esempio con i grafici di helm.  
![Repository di Azure](../media-drafts/1-azurerepo.png)

5. Analizzare le compilazioni  
![Collegamento alle compilazioni](../media-drafts/1-linktobuild.png)

6. Modifica di compilazione creata facendo clic sulla compilazione e quindi fare clic su Modifica  
![Modifica collegamento di compilazione](../media-drafts/1-editbuildlink2.png)

7. Vedrai una pipeline di compilazione che risulta utile per le tecnologie selezionato. Poiché abbiamo scelto un'app Node. js in un cluster Kubernetes, otteniamo una pipeline di compilazione che usa le attività di Docker per compilare l'app Node. js, creare l'immagine del contenitore immagine e quindi Helm le attività per creare un pacchetto di Helm.  
![La Pipeline di compilazione](../media-drafts/1-buildpipeline2.png)

8. Passare alla pipeline di rilascio, fare clic su `Releases`  
![Collegamento di versione](../media-drafts/1-gotoreleaselink.png)

9. Si noterà la pipeline di rilascio creata. Modificarlo facendo clic sul rilascio e selezionando `Edit pipeline`  
![Modifica la versione](../media-drafts/1-editrelease2.png)

10. Azure ha creato una pipeline di rilascio che risulta utile per le tecnologie selezionato. Un'app node in esecuzione in un contenitore e ospitato in un cluster Kubernetes.
![Pipeline di rilascio](../media-drafts/1-releasepipeline2.png)  
![Passaggi della Pipeline del rilascio](../media-drafts/1-pipelinesteps.png)

11. Tornare al portale di Azure e fare clic sull'endpoint esterni per il servizio di kubernetes  
![](../media-drafts/1-clickonendpoint.png)

12. Si dovrebbe essere visualizzato l'app di esempio di compilazione e distribuito nel cluster AKS  
![App in esecuzione](../media-drafts/1-apprunning.png)

## <a name="summary"></a>Riepilogo

In questa unità, è stato creato un progetto Azure DevOps costituito da:

1. Infrastruttura per l'app - Application Insights e Azure Kubernetes Service

2. Un progetto Team in un'istanza di Visual Studio Team Services.

3. Codice sorgente per un'app di esempio Node. js in esecuzione in un contenitore nel repository dell'istanza di Visual Studio Team Services.

4. Una pipeline di compilazione e versione per un'app contenitore di Node. js in esecuzione nell'istanza di Azure Kubernetes Service.

Successivamente, informazioni su come sostituire l'app di esempio con l'app reale.