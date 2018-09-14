Dopo aver creato un progetto Azure DevOps, chiedere a tutti gli utenti prima di tutto è come si sostituisce l'app di esempio con la tua app? È piuttosto semplice e in questa unità, si apprenderà due modi per farlo.

1. Sostituendo il codice nel repository git di Visual Studio Team Services con il codice reale

2. Verso la pipeline di compilazione a un repository git esterno che contiene il codice reale

## <a name="replacing-code-in-vsts-git-repository"></a>Sostituzione di codice in repository git VSTS

Un modo semplice è clonando il repository git in Visual Studio Team Services nel disco rigido, sostituire tutti gli elementi con il proprio codice, il caricamento di tornare a Visual Studio Team Services e gioco. Il codice verrà ora compilato e distribuito tramite la pipeline di integrazione continua/recapito Continuo.

Per questa unità, iniziare scaricando e archiviare il codice sorgente per l'app Node. js sul disco rigido.

1. Scaricare il codice sorgente da <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip>

2. Estrarre il contenuto del MicrosoftLearnDevOps.zip in una posizione sul disco rigido. In questo esempio `C:\users\abel\Downloads\MicrosoftLearnDevOps` utilizzato  
![Directory non compressa](../media-drafts/2-unzippedfolder.png)

Successivamente, è necessario clonare il repository nel disco rigido e sostituire l'app di esempio con l'app Node. js reali. Questa unità presuppone che si dispone già di git installato nel computer.

1. Dal portale di Azure passare al tuo progetto DevOps di Azure e fare clic sul collegamento al repository di codice.  
![](../media-drafts/2-browsetorepolink.png)

2. Fare clic su Clone e copiare l'url per il repository git nella parte superiore destra.  
![Clonare repository](../media-drafts/2-clonerepo2.png) l'url del repository verranno copiati negli Appunti

3. Clonare il repository nel disco rigido  
![Clone GIT](../media-drafts/2-gitclone.png)  
In questo esempio è stato clonato il repository per C:\Users\abel\Source\TripleCrown\DevOps

4. Eliminare tutti gli elementi dal repository locale, ad eccezione del `.git` directory  
![Elimina repository di tutti gli elementi](..//media-drafts/2-deleterepoofeverything.png)

5. Copiare il codice sorgente per l'app Node. js scaricato nella cartella del repository  
![Codice sostituito](../media-drafts/2-replacedeverything.png)

6. Aggiungere tutte le modifiche nel repository locale digitando `git add *` dalla riga di comando  
![GIT Add tutti](../media-drafts/2-gitaddall.png)

7. Eseguire il commit delle modifiche nel repository locale digitando `git commit -m "replace sample app with real code"`  
![Commit GIT](../media-drafts/2-gitcommit.png)

8. Push delle modifiche al repository git in VSTS con `git push`  
![GIT Push](../media-drafts/2-gitpush.png)

9. Dopo il push delle modifiche nuovamente in Visual Studio Team Services, si deve inviare il codice dell'app reali tramite la compilazione  
![Compilazione avviata-Off](../media-drafts/2-buildkickedoff2.png)
![compilazione esecuzione](../media-drafts/2-buildrunning2.png) e pipeline di rilascio completamente in azure  
 ![Versione in esecuzione](../media-drafts/2-releaserunning2.png)

 Al termine della distribuzione, è possibile verificare che l'app reale è stata distribuita, tornare al portale di Azure

 1. Passare al portale di Azure, passare al tuo progetto DevOps di Azure e fare clic sull'app distribuita sul lato destro  
 ![Avvio collegamento di App di esempio](../media-drafts/2-launchapp.png)

 2. Verrà avviata l'app in esecuzione nel browser  
 ![App in esecuzione](../media-drafts/2-apprunning.png)

## <a name="using-external-git-repo"></a>Usare il repository git esterno

Un altro modo per lo swapping delle app di esempio con il codice dell'app reale è posizionando la pipeline di compilazione a un repository git esterno che contiene il codice dell'app. Per questo esempio, caricare il codice dell'app reali in un repository github.

Dopo aver caricato il codice reale in github, eseguire le operazioni seguenti per scegliere la pipeline di compilazione per questo repository github

1. Dal portale di Azure, passare il progetto DevOps di Azure e fare clic sul collegamento di compilazione  
![Collegamento di compilazione](../media-drafts/2-buildlink.png)

2. Ciò consente di visualizzare le pipeline di compilazione, fare clic su pipeline di compilazione e quindi `Edit`  
![Fare clic su Modifica la Pipeline di compilazione](../media-drafts/2-editbuildpipelinelink.png)

3. Ciò consente di visualizzare l'editor di compilazione, fare clic su `Get sources`  
![Fare clic sull'attività di origine di Get](../media-drafts/2-clickgetsourcetask.png)

4. Ciò consente di selezionare una pagina di origine. Si noti come è possibile non solo usare Visual Studio Team Services Git, ma anche GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud e qualsiasi repository Git basato su esterno per la pipeline di compilazione. Per questo esercizio, selezionare GitHub  
![Selezionare GitHub](../media-drafts/2-selectgithub2.png)

5. Consente di andare alla pagina di connessione di GitHub. Usare OAuth o un Token di accesso personale per la connessione con il proprio account GitHub

6. Selezionare il repository di github che contiene il codice dell'app reali e fare clic su `Save & queue`  
![Salva e accoda](../media-drafts/2-saveandqueue2.png)

7. Fare clic su di `Save & queue` pulsante  
![Salva e accoda pulsante](../media-drafts/2-saveandqueuebutton.png)

8. Questa azione consente di salvare e viene attivata disattivare la compilazione, l'invio dell'app reale ospitata in GitHub tramite la compilazione del codice e release pipeline completamente in Azure  
![Compilazione in esecuzione](../media-drafts/2-buildpipelinerunning.png)

## <a name="summary"></a>Riepilogo

In questa unità, si è appreso due diversi modi per sostituire il codice di esempio nel progetto DevOps con codice di app reali. Questa operazione può essere eseguita, sostituendo il codice nel repository git VSTS o collegando la pipeline di compilazione con un altro repository esterno che contiene il codice dell'app.

Successivamente, informazioni su come personalizzare la compilazione e le pipeline di rilascio.