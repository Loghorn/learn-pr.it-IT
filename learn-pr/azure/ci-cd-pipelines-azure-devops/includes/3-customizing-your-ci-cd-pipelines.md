L'esperienza predefinita del progetto Azure DevOps crea pipeline di compilazione e versione adeguate per le tecnologie selezionate. Per questo modulo, è stata creata una pipeline di compilazione e versione adeguata per un'app node.js in esecuzione in un contenitore ospitato in un cluster Kubernetes. 

Spesso è necessario personalizzare le pipeline di compilazione e versione per eseguire operazioni specifiche per il progetto. Le pipeline di compilazione e di versione in VSTS sono personalizzabili al 100%. È possibile controllare le operazioni eseguite dalle pipeline.

In questa unità si apprenderà come personalizzare le pipeline di compilazione e di versione.

## <a name="customize-the-build-pipeline"></a>Personalizzare la pipeline di compilazione

Il motore di compilazione in VSTS è solo uno strumento di esecuzione attività, che esegue un'attività dopo l'altra. Per personalizzare la compilazione, è sufficiente aggiungere o rimuovere le attività e inserire i parametri corretti per l'attività.

Per impostazione predefinita, VSTS include circa 100 attività che è possibile usare. Se è necessario eseguire un'operazione non predefinita, controllare nel marketplace in cui sono presenti oltre 700 attività di compilazione e versione, pronte per essere scaricate e usate. È anche possibile scrivere attività personalizzate.

Per questa unità, si personalizzerà la pipeline di compilazione installando le attività di marketplace WhiteSource Bolt per eseguire l'analisi di sicurezza del codice.

1. Passare al progetto di Azure DevOps nel portale di Azure e fare clic sul collegamento alla definizione di compilazione  
![Collegamento alla compilazione](/media-draft/3-buildlink.png)

2. Verrà visualizzata la pagina delle pipeline di compilazione. Fare clic sulla compilazione e selezionare `Edit`  
![Modifica compilazione](/media-draft/3-editbuild.png)

3. Verrà visualizzata la definizione di compilazione. Fare clic su `+` per aggiungere un'attività al Processo agente 1  
![Aggiungi attività](/media-draft/3-addtask.png)

4. Nel campo di testo digitare `bolt` e fare clic su `Get it free`  
![Scarica gratuitamente](/media-draft/3-getitfree.png)

5. Consente di andare alla pagina del Marketplace di WhiteSource Bolt. Fare clic su `Get it free`  
![Scarica WhiteSource Bolt gratuitamente](/media-draft/3-getwhitesourceboltfree.png)

6. Scegliere l'organizzazione di VSTS e quindi fare clic su `Install`  
![Installa](/media-draft/3-install.png)

7. Attivare WhiteSource Bolt seguendo le istruzioni riportate qui <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate>

8. Tornare al portale di Azure con il progetto DevOps caricato e fare clic sul collegamento alla pipeline di compilazione  
![Collegamento alla pipeline di compilazione](/media-draft/3-buildpipelinelink.png)

9. Selezionare la compilazione e fare clic su `Edit`  
![Modifica compilazione](/media-draft/3-editbuild.png)

10. Fare clic su `+` Aggiungi un'attività a Processo agente 1, digitare `bolt` nel campo di ricerca e fare clic su `Add`  
![Aggiungi Bolt](/media-draft/3-addbolt.png)

11. Verrà aggiunta l'attività di WhiteSource Bolt nella parte inferiore dell'elenco attività, trascinarla nella parte superiore  
![Bolt in alto](/media-draft/3-boltattop.png)

12. Fare clic su `Save & queue` e selezionare `Save & queue`  
![Salvare e accodare](/media-draft/3-saveandqueue.png)

13. Fare clic su `Save & queue`  
![Salva e accoda](/media-draft/3-saveandqueuedialog.png)

La pipeline di compilazione modificata viene salvata e la compilazione viene messa in coda. Al termine della compilazione, esaminando la compilazione `WhiteSource Bolt Build Report` è possibile osservare che il codice sorgente è stato analizzato da WhiteSource Bolt alla ricerca di vulnerabilità della sicurezza.

![Report di compilazione](/media-draft/3-buildreport.png)

## <a name="customize-the-release-pipeline"></a>Personalizzare la pipeline di versione

Proprio come per la compilazione, anche la pipeline di versione è uno strumento di esecuzione attività, che può essere personalizzato in modo analogo. Per questa unità, alla fine della versione si aggiungerà un test delle prestazioni Web, che consente di verificare che l'app sia distribuita e correttamente eseguita nel cluster Kubernetes.

1. Passare al progetto DevOps nel portale di Azure e fare clic sul collegamento alla pipeline di versione  
![Collegamento alla versione](/media-draft/3-releaselink.png)

2. Verrà visualizzata la pagina della pipeline di versione. Scegliere la pipeline di versione e fare clic su `Edit`  
![Modifica la pipeline di versione](/media-draft/3-editreleasepipeline.png)

3. Fare clic sulle attività nella fase `Dev` della versione  
![Fase di versione](/media-draft/3-releasestage.png)

4. Fare clic su `+` Aggiungi un'attività alla Fase1, digitare `web test` nel campo di ricerca e fare clic su `Add` per l'attività di test delle prestazioni Web basato sul cloud  
![Aggiungi test Web](/media-draft/3-addwebtest.png)

5. Modificare l'attività Test prestazioni Web rapido facendo clic su di essa e aggiungendo l'URL alla propria app nell'URL del sito Web (per trovare l'URL, passare alla pagina del progetto DevOps del portale di Azure e, a destra, fare clic con il pulsante destro del mouse sull'endpoint esterno dell'app di esempio e copiare il collegamento), quindi per TestName immettere `Ping Test`  
![Copia URL](/media-draft/3-copyurl.png)  
![Modificare l'attività di test Web](/media-draft/3-editwebtesttask.png)

6. Fare clic su `Save`  
![Salvare la versione](/media-draft/3-saverelease.png)

A questo punto, quando si esegue una versione, dopo aver distribuito il nuovo pacchetto Helm, viene eseguito un test Web per raggiungere correttamente l'URL dell'app.

![Test Web](/media-draft/3-webtest.png)


## <a name="summary"></a>Riepilogo

In questa unità si è appreso come personalizzare le pipeline di compilazione e di versione. Si è anche appreso come installare e usare le attività dal Marketplace.