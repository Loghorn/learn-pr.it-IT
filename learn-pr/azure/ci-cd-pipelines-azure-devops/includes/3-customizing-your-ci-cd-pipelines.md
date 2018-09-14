Il timeout dell'esperienza di finestra con un progetto Azure DevOps crea build e le pipeline di rilascio che ha senso per le tecnologie selezionato. Per questo modulo, è creata una pipeline di compilazione e versione che risulta utile per un'app Node. js in esecuzione in un contenitore e ospitato in un cluster Kubernetes. 

Spesso che è necessario Personalizza le pipeline di compilazione e versione per eseguire operazioni specifiche per il progetto. Le pipeline di compilazione e rilascio in Visual Studio Team Services sono personalizzabili al 100%. È possibile rendere la pipeline è necessario eseguire.

In questa unità, informazioni su come personalizzare la compilazione e le pipeline di rilascio.

## <a name="customize-the-build-pipeline"></a>Personalizzare la pipeline di compilazione

Il motore di compilazione in Visual Studio Team Services è semplicemente un strumento di esecuzione attività, eseguire una sola attività, dopo l'altra. Per personalizzare la compilazione, sufficiente aggiungere o rimuovere le attività e inserire i parametri corretti per l'attività.

Per impostazione predefinita, Visual Studio Team Services include circa 100 attività che è possibile usare. Se è necessario eseguire un'operazione che non esiste predefiniti, controllare nel marketplace in cui sono presenti oltre 700 compilare e rilasciare attività pronta per essere scaricata e usata. È in grado di scrivere attività personalizzate.

Per questa unità, si personalizzerà la pipeline di compilazione installando le attività di marketplace WhiteSource Bolt per eseguire l'analisi di sicurezza del codice.

1. Individuare il progetto DevOps di Azure nel portale di Azure e fare clic sul collegamento di definizione di compilazione  
![Collegamento di compilazione](../media-drafts/3-buildlink.png)

2. Consente di andare alla pagina di pipeline di compilazione. Fare clic sulla compilazione e selezionare `Edit`  
![Modifica compilazione](../media-drafts/3-editbuild2.png)

3. Ciò consente la definizione di compilazione. Fare clic su di `+` per aggiungere un'attività per agente processo 1  
![Aggiungi attività](../media-drafts/3-addtask2.png)

4. Nel campo testo digitare `bolt` e fare clic su `Get it free`  
![Ottieni origine bianco Bolt](../media-drafts/3-getwhitesourcebolt.png)

5. Consente di andare alla pagina WhiteSource Bolt Marketplace. Fare clic su`Get it free`.  
![Ottenere WhiteSource Bolt gratuito](../media-drafts/3-getwhitesourceboltfree2.png)

6. Scegliere l'organizzazione di Azure DevOps e quindi fare clic su `Install`  
![Installare](../media-drafts/3-installwsb.png)

7. Attivare WhiteSource Bolt seguendo le istruzioni riportate di seguito <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate>

8. Tornare al portale di Azure con il progetto caricato e fare clic sul collegamento di pipeline di compilazione di DevOps  
![Collegamento alla Pipeline di compilazione](../media-drafts/3-buildpipelinelink.png)

9. Selezionare la compilazione e fare clic su `Edit`  
![Modifica compilazione](../media-drafts/3-editbuild2.png)

10. Scegliere il `+` aggiungere un'attività al processo dell'agente 1, digitare `bolt` nel campo di ricerca e fare clic su `Add`  
![Aggiungere Bolt](../media-drafts/3-addwsbolt.png)

11. Verrà aggiunta l'attività di WhiteSource Bolt nella parte inferiore dell'elenco attività, trascinarlo nella parte superiore  
![Bolt di spostamento verso l'alto](../media-drafts/3-moveboltototopoftasklist.png)

12. Fare clic su `Save & queue` e selezionare `Save & queue`  
![Salva e accoda](../media-drafts/3-saveandqueue2.png)

13. Fare clic su`Save & queue`.  
![Salva e accoda pulsante](../media-drafts/3-saveandqueuebutton.png)

Questo Salva la pipeline di compilazione modificata e mette in coda la compilazione. Al termine della compilazione, esaminando la compilazione `WhiteSource Bolt Build Report`, è possibile visualizzare il codice sorgente è stato analizzato da WhiteSource Bolt alla ricerca di vulnerabilità della sicurezza.

![Report di origine bianco](../media-drafts/3-whitesourcereport.png)

## <a name="customize-the-release-pipeline"></a>Personalizzare la pipeline di rilascio

Ad esempio la compilazione, la pipeline di rilascio è task runner e può essere personalizzata allo stesso modo. Per questa unità, si aggiungerà un test prestazioni web alla fine della versione. Ciò consente di verificare che l'app viene distribuita e correttamente in esecuzione nel cluster Kubernetes.

1. Passare al progetto DevOps nel portale di Azure e fare clic sul collegamento per la pipeline di rilascio  
![Collegamento di versione](../media-drafts/3-releaselink.png)

2. Consente di andare alla pagina di pipeline di rilascio. Scegliere la pipeline di rilascio e fare clic su `Edit`  
![Modifica la versione](../media-drafts/3-editreleasebutton.png)

3. Fare clic sull'attività nel rilascio `Dev` fase  
![Fase di rilascio](../media-drafts/3-releasestage2.png)

4. Fare clic sui `+` aggiungere un'attività alla fase 1, tipo `web test` nel campo di ricerca e fare clic su `Add` per l'attività di Test delle prestazioni Web basato su Cloud  
![Aggiungere Test Web](../media-drafts/3-addwebtest2.png)

5. Modificare l'attività di Test delle prestazioni Web rapido facendo clic su di esso e aggiunta dell'url per l'app nell'URL del sito Web (per trovare l'url, passare alla pagina del progetto DevOps portale Azure e sul lato destro, fare doppio clic il campione esterno dell'endpoint e Copia collegamento dell'app) e quindi per Giornamenti tName, immettere nel `Ping Test` e quindi su `Save`  
![Copia URL](../media-drafts/3-copyurl.png)  
![Salvare Test Web](../media-drafts/3-savewebtest.png)

A questo punto, quando si esegue una versione, dopo aver distribuito il nuovo pacchetto di helm, viene eseguito un test web raggiungere l'url dell'app completata.

![Esecuzione dei Test Web](../media-drafts/3-webtestrun.png)

## <a name="summary"></a>Riepilogo

In questa unità, è stato descritto come personalizzare la compilazione e le pipeline di rilascio. Inoltre appreso come installare e usare le attività dal Marketplace.