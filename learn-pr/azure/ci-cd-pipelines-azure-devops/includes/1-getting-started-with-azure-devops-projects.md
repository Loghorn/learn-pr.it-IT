La configurazione rapida di una pipeline CI/CD è sempre stata complessa. Da oggi è facilissimo implementare da zero un progetto Azure DevOps end to end completo. In più, nel progetto Azure DevOps si ottiene quanto segue:

1. Provisioning dell'infrastruttura in Azure.

2. Un progetto team in un'istanza di VSTS.

3. Codice sorgente per un'app di esempio nel linguaggio scelto nel repository dell'istanza di VSTS.

4. Una pipeline di compilazione e versione adeguata alle tecnologie selezionate.

Al termine, il progetto Azure DevOps compila l'app di esempio per poi rilasciarla tramite le pipeline nell'infrastruttura di cui esegue il provisioning in Azure. E tutti questi vantaggi si ottengono con un paio di clic.

## <a name="create-an-azure-devops-project"></a>Creare un progetto Azure DevOps

È possibile creare il progetto Azure DevOps nel portale di Azure.

1. Nel [portale di Azure](https://portal.azure.com) fare clic su `Create a resource`  
![](/media-draft/1-azureportal.png)

2. Fare clic su `DevOps Project`  
![Scegliere Progetto DevOps](/media-draft/1-pickdevopsproject.png)

3. Nella schermata successiva è possibile scegliere quale linguaggio si vuole usare. Si noti che è possibile scegliere .NET (naturalmente), java, node, php, python, ruby e go. Si può usare un codice personalizzato da un repository Git. Per questa unità, procedere e creare un'app Node.js. Fare clic su Node.js, quindi su Avanti  
![Selezionare Node.js come linguaggio](/media-draft/1-picknodejsforlang.png)

4. Successivamente verrà chiesto quale framework di node.js si vuole usare. Per questa unità, selezionare un'app Node.js semplice e fare clic su Avanti  
![Scegliere Nodo semplice](/media-draft/1-picksimplenode.png)

5. Successivamente, verrà chiesto il tipo di infrastruttura di cui si vuole effettuare il provisioning e in cui eseguire l'app. Per questa unità, eseguiamo il provisioning e la distribuzione in un cluster Kubernetes con il Servizio Kubernetes di Azure. Selezionare Servizio Kubernetes e scegliere Avanti  
![Selezionare Kubernetes](/media-draft/1-pickkubernetes.png)

6. A questo punto, è possibile creare una nuova istanza di VSTS o sceglierne una esistente. È anche possibile configurare dove e come si vuole eseguire il cluster Kubernetes. Per questa unità, proseguire e creare una nuova istanza di VSTS scegliendo Selezionare Nuovo, quindi assegnarle un nome univoco. Inserire Learn come nome del Progetto, scegliere la sottoscrizione Azure, denominare il cluster LearnCluster, selezionare Stati Uniti orientali come località, quindi fare clic su Fine  
![Schermata di conferma finale](/media-draft/1-finalconfirmation.png)

E questo è tutto. Questa operazione richiede un po' di tempo, di cui gran parte per l'esecuzione del provisioning e la configurazione dell'infrastruttura Azure. Per questo modulo, si tratta del servizio Kubernetes di Azure e del Registro contenitori di Azure.

## <a name="a-lap-around-the-finished-azure-devops-project"></a>Panoramica del progetto Azure DevOps completato

Quando Azure avrà terminato, si riceverà una notifica in Avvisi

1. Fare clic sull'avviso e quindi su Vai alla risorsa  
![Vai alla risorsa da Avviso](/media-draft/1-gotoresourcefromalert.png)

2. Viene visualizzato un pannello del portale con tutto ciò che stato sottoposto a provisioning. A sinistra è visualizzata la pipeline CI/CD. Sono visualizzati il repository del codice, la definizione di compilazione e anche la definizione di versione. Tutti i collegamenti indirizzano direttamente alla risorsa in VSTS. A destra, invece, è visualizzata tutta l'infrastruttura distribuita automaticamente in Azure. È visualizzato il cluster Kubernetes con l'app di esempio già distribuita, oltre ad Application Insight. Anche in questo caso, tutti i collegamenti indirizzano direttamente alle risorse in Azure.  
![Progetto DevOps portale](/media-draft/1-pickdevopsproject.png)

3. Fare clic sul collegamento al codice sorgente  
![Collegamento al codice sorgente](/media-draft/1-linktosource.png)

4. Verrà visualizzato il repository Git nel progetto VSTS. Si noti che questo è solo un normale repository Git che contiene l'app node.js di esempio con i grafici Helm.  
![Repository VSTS](/media-draft/1-vstsrepo.png)

5. Passare alle compilazioni  
![Collegamento alle compilazioni](/media-draft/1-navtobuild.png)

6. Modificare la compilazione creata facendo clic su di essa, quindi su Modifica  
![](/media-draft/1-editbuildlink.png)

7. Verrà visualizzata una pipeline di compilazione adeguata alle tecnologie selezionate. Dal momento che è stata selezionata un'app node.js in un cluster Kubernetes, si ottiene una pipeline di compilazione che usa le attività di Docker per compilare l'app Node.js, creare l'immagine del contenitore delle immagini e quindi le attività Helm per creare un pacchetto Helm.  
![Pipeline di compilazione](/media-draft/1-buildpipeline.png)

8. Fare clic su `Releases` per Passare alla pipeline di rilascio  
![Passare a Versioni](/media-draft/1-gotoreleases.png)

9. Verrà visualizzata la pipeline di versione creata. Per modificarla, fare clic sulla versione e selezionare `Edit pipeline`  
![Modifica versione](/media-draft/1-editrelease.png)

10. Azure ha creato una pipeline di versione adeguata alle tecnologie selezionate. Un'app node in esecuzione in un contenitore ospitato in un cluster Kubernetes.
![Pipeline di versione](/media-draft/1-releasepipeline.png)

11. Tornare al portale di Azure e fare clic sull'endpoint esterno per il servizio Kubernetes  
![](/media-draft/1-clickonendpoint.png)

12. Verrà visualizzata la compilazione dell'app di esempio, distribuita nel cluster AKS  
![App in esecuzione](/media-draft/1-apprunning.png)

## <a name="summary"></a>Riepilogo

In questa unità è stato creato un progetto DevOps che consiste in:

1. Infrastruttura dell'app: Servizio Kubernetes di Azure e Application Insight

2. Un progetto team in un'istanza di VSTS.

3. Codice sorgente per un'app di esempio Node.js in un contenitore nel repository dell'istanza di VSTS.

4. Una pipeline di compilazione e versione per un'app contenitore Node.js in esecuzione nell'istanza del Servizio Kubernetes di Azure.

In seguito si apprenderà come sostituire l'app di esempio con una vera app.