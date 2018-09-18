È stata creata correttamente una pipeline CI/CD per l'applicazione personalizzata usando progetti Azure DevOps. 

## <a name="clean-up-resources"></a>Pulire le risorse

Dopo avere usato questa pipeline CI/CD, è possibile eliminare tutte le risorse create durante l'esercitazione.

1. Dal portale di Azure, passare a `Resource Groups` e fare clic su `VstsResourceGroup-LearnCluster-xxxx` dove xxx rappresenta gruppi casuali di lettere e numeri  
![LearnClusterRG](/media-draft/4-learnclusterrg.png)

2. Fare clic sul progetto DevOps denominato `Learn`  
![Collegamento informazioni](/media-draft/4-learnlink.png)

3. Fare clic su `Delete`  
![Eliminare](/media-draft/4-deleteproj.png)

4. Fare clic su `Yes`  
![Sì](/media-draft/4-yes.png)

Verranno eliminate tutte le risorse create in Azure.

## <a name="summary"></a>Riepilogo

Questa esercitazione ha descritto come:
> [!div class="checklist"]
> * Creare un progetto Azure DevOps
> * Sostituire l'app di esempio nel progetto Azure DevOps con la propria app
> * Personalizzare le pipeline di compilazione e versione
