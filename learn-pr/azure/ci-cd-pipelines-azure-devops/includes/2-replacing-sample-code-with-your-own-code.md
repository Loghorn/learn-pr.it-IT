Se dopo avere creato un progetto di Azure DevOps si ricevono prima di tutto domande su come sostituire l'app di esempio con un'app personalizzata, si tratta di una procedura abbastanza semplice e in questa unità verranno illustrati due modi per eseguirla.

1. Sostituzione del codice nel repository Git per Visual Studio Team Services con codice effettivo

2. Specificare come riferimento per la pipeline di compilazione un repository Git esterno contenente il codice effettivo

## <a name="replacing-code-in-vsts-git-repository"></a>Sostituzione del codice nel repository Git di Visual Studio Team Services

Un approccio semplice consiste nel clonare il repository Git in Visual Studio Team Services sul disco rigido, sostituendo tutto con il codice personalizzato e infine ricaricando il repository in Visual Studio Team Services. Il codice verrà compilato e distribuito mediante la pipeline CI/CD.

Per questa unità verranno prima di tutto eseguiti il download e l'archiviazione del codice sorgente dell'app Node.js sul disco rigido.

1. Scaricare il codice sorgente da <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip>

2. Estrarre i contenuti di MicrosoftLearnDevOps.zip in una posizione sul disco rigido. Per questo esempio è stato usato `C:\users\abel\Downloads\MicrosoftLearnDevOps`  
![Directory non compressa](/media-draft/2-unzippedfolder.png)

È quindi necessario clonare il repository sul disco rigido e sostituire l'app di esempio con l'app Node.js effettiva. In questa unità si presuppone che Git sia già installato nel computer.

1. Dal portale di Azure passare al progetto di Azure DevOps e quindi fare clic sul collegamento del repository di codice.  
![](/media-draft/2-browsetorepolink.png)

2. Fare clic su Clona e copiare l'URL del repository Git disponibile in alto a destra.  
![Copiare l'URL di clonazione](/media-draft/2-copycloneurl.png)  
L'URL del repository verrà copiato negli Appunti

3. Clonare il repository sul disco rigido  
![Git Clone](/media-draft/2-gitclone.png)  
In questo esempio il repository è stato clonato in C:\Users\abel\Source\TripleCrown\DevOps

4. Eliminare tutti gli elementi dal repository locale, ad eccezione della directory `.git`  
![Eliminare tutti gli elementi dal repository](/media-draft/2-deleterepoofeverything.png)

5. Copiare il codice sorgente per l'app Node.js scaricata nella cartella del repository  
![Codice sostituito](/media-draft/2-replacedeverything.png)

6. Aggiungere tutte le modifiche al repository locale digitando `git add *` dalla riga di comando  
![Git Add All](/media-draft/2-gitaddall.png)

7. Eseguire il commit delle modifiche nel repository locale digitando `git commit -m "replace sample app with real code"`  
![Git Commit](/media-draft/2-gitcommit.png)

8. Eseguire il push delle modifiche nel repository Git in Visual Studio Team Services con `git push`  
![Git Push](/media-draft/2-gitpush.png)

9. Dopo il push delle modifiche in Visual Studio Team Services, il codice dell'applicazione effettiva dovrebbe essere sottoposto a compilazione  
![Compilazione attivata](/media-draft/2-buildkickedoff.png)  
![Compilazione in corso](/media-draft/2-buildinaction.png) e pipeline di versione fino ad Azure  
 ![Versione in esecuzione](/media-draft/2-releaserunning.png)

 Al termine della distribuzione è possibile verificare che l'app effettiva sia stata distribuita tornando nel portale di Azure

 1. Passare al portale di Azure, passare al progetto di Azure DevOps e fare clic sull'app distribuita, disponibile a destra  
 ![Collegamento per l'avvio dell'app di esempio](/media-draft/2-launchapp.png)

 2. L'app in esecuzione verrà avviata nel browser  
 ![App in esecuzione](/media-draft/2-apprunning.png)

## <a name="using-external-git-repo"></a>Uso di un repository Git esterno

Un altro modo per sostituire l'app di esempio con il codice effettivo dell'app consiste nello specificare come riferimento per la pipeline di compilazione un repository Git esterno che include il codice dell'app. Per questo esempio, caricare il codice effettivo dell'app in un repository di GitHub.

Dopo il caricamento del codice effettivo in GitHub, seguire questa procedura per specificare tale repository di GitHub come riferimento per la pipeline di compilazione

1. Dal portale di Azure passare al progetto di Azure DevOps e fare clic sul collegamento per la compilazione  
![Collegamento alla compilazione](/media-draft/2-buildlink.png)

2. Verranno visualizzate le pipeline di compilazione. Fare clic sulla pipeline di compilazione specifica e quindi su `Edit`  
![Fare clic sull'opzione di modifica della compilazione](/media-draft/2-clickeditbuildlink.png)

3. Verrà visualizzato l'editor compilazioni. Fare clic su `Get sources`  
![Fare clic sull'opzione per il recupero dell'origine](/media-draft/2-clickgetsource.png)

4. Verrà visualizzata la pagina Selezionare un'origine. Si noti che per la pipeline di compilazione è possibile usare non solo Visual Studio Team Services, ma anche GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud e qualsiasi altro repository esterno basato su Git. Per questo esercizio, selezionare GitHub  
![Selezionare GitHub](/media-draft/2-selectgithub.png)

5. Verrà visualizzata la pagina di connessione a GitHub. Usare OAuth o un token di accesso personale per connettersi con il proprio account GitHub

6. Selezionare il repository di GitHub che include il codice effettivo dell'app e quindi fare clic su `Save & queue`  
![Salvare e accodare](/media-draft/2-saveandqueue.png)

7. Fare clic sul pulsante `Save & queue`  
![](/media-draft/2-saveandqueuedialog.png)

8. Questa azione consente di salvare e attivare la compilazione, inviando il codice effettivo dell'app ospitato in GitHub tramite le pipeline di compilazione e di versione fino ad Azure  
![Compilazione in esecuzione](/media-draft/2-buildrunning.png)

## <a name="summary"></a>Riepilogo

In questa unità sono stati appresi due modi diversi per sostituire il codice di esempio del progetto DevOps con il codice effettivo dell'app. È possibile eseguire questa operazione sostituendo il codice nel repository Git di Visual Studio Team Services oppure collegando la pipeline di compilazione con un altro repository esterno che include il codice dell'app.

Verrà quindi illustrato come personalizzare le pipeline di compilazione e di versione.