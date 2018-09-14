È stata creata correttamente una pipeline CI/CD per l'applicazione personalizzata usando progetti DevOps di Azure. 

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo aver completato utilizzano questa pipeline CI/CD, è possibile eliminare tutte le risorse create durante l'esercitazione.

1. Dal portale di Azure, passare a `Resource Groups` e fare clic su `VstsResourceGroup-LearnCluster-xxxx` dove xxx rappresenta gruppi casuale di lettere e numeri  
![LearnClusterRG](../media-drafts/4-learnclusterrg.png)

2. Fare clic sul progetto DevOps denominato `Learn`  
![Informazioni su collegamento](../media-drafts/4-learnlink.png)

3. Fare clic su `Delete`.  
![Eliminare](../media-drafts/4-deleteproj.png)

4. Fare clic su `Yes`.  
![Sì](../media-drafts/4-yes.png)

Questa operazione eliminerà tutte le risorse create in Azure.

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come:
> [!div class="checklist"]
> * Creare un progetto DevOps di Azure
> * Sostituire l'app di esempio nel progetto Azure DevOps con i propri
> * Personalizzare le pipeline nella compilazione e rilascio
