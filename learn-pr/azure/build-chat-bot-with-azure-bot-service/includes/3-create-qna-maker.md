
[QnA Maker](https://www.qnamaker.ai/) è incluso in [Servizi cognitivi di Azure](https://www.microsoft.com/cognitive-services/), una suite di servizi e API per la creazione di app intelligenti, supportate da intelligenza artificiale e Machine Learning. Anziché codificare un bot in modo che anticipi ogni possibile domanda di un utente e fornisca una risposta, è possibile connetterlo a una knowledge base di domande e risposte creata con QnA Maker. Uno scenario di utilizzo comune consiste nel creare una knowledge base da una serie di domande frequenti, in modo che il bot possa rispondere a domande specifiche del dominio, ad esempio "How do I find my Windows product key" o "Where can I download Visual Studio Code?".

In questa unità si userà QnA Maker per creare una knowledge base contenente domande di tipo "What NFL teams have won the most Super Bowls" e "What is the largest city in the world?". Si distribuirà quindi la knowledge base in un'app Web di Azure in modo che sia accessibile tramite un endpoint HTTPS.

1. Aprire il [portale di QnA Maker](https://www.qnamaker.ai/) nel browser e accedere con l'account Microsoft, se non è già stato fatto. Fare quindi clic su **Create a knowledge base** (Crea una knowledge base) sulla barra dei menu nella parte superiore della pagina.

1. Fare clic su **Create a QnA service** (Crea un servizio QnA).

1. Immettere un nome nella casella **Nome**, ad esempio "qa-factbot-kb". Questo nome deve essere univoco all'interno di Azure. Assicurarsi quindi che accanto al nome *e* nella casella **Nome dell'app** più in basso nel pannello compaia un segno di spunta verde. Selezionare **Usa esistente** in **Gruppo di risorse** e quindi selezionare il gruppo di risorse "factbot-rg" creato durante la distribuzione del bot app Web di Azure nell'esercizio precedente. Selezionare i piani tariffari **F0** ed **F**. Entrambi i piani sono gratuiti e sono ideali per provare a usare i bot. Selezionare la località più vicina in entrambi gli elenchi a discesa e quindi scegliere il pulsante **Crea** nella parte inferiore del pannello.

    ![Screenshot del portale di Azure che mostra il pannello di creazione di QnA Maker con i valori di configurazione descritti.](../media/3-new-qna-maker-service.png)

1. Fare clic su **Gruppi di risorse** sulla barra multifunzione sul lato sinistro del portale e aprire il gruppo di risorse "factbot-rg". Attendere finché "Distribuzione in corso" non cambia in "Operazione completata" nella parte superiore del pannello per indicare che il servizio QnA e le risorse associate sono stati distribuiti correttamente. Di nuovo, è possibile fare clic su **Aggiorna** nella parte superiore del pannello per aggiornare lo stato di distribuzione.

1. Tornare alla pagina [Create a knowledge base](https://www.qnamaker.ai/Create) (Crea una knowledge base) nel portale di QnA Maker. Sotto **Azure QnA service** (Servizio Azure QnA) selezionare il servizio QnA il cui nome è stato specificato nel Passaggio 3. Assegnare quindi un nome alla knowledge base, ad esempio "Factbot Knowledge Base".

1. È possibile immettere domande e risposte in una knowledge base QnA Maker manualmente oppure importarle da domande frequenti online o file locali. I formati supportati sono i file di testo delimitati da tabulazioni, i documenti di Microsoft Word, i fogli di calcolo di Excel e i file PDF.

    Per una dimostrazione, [fare clic qui](https://topcs.blob.core.windows.net/public/bots-resources.zip) per scaricare un file ZIP contenente un file di testo denominato **Factbot.tsv** e quindi copiare il file nel computer. Scorrere quindi verso il basso nel portale di QnA Maker, fare clic su **+ Add file** (+ Aggiungi file) e selezionare **Factbot.tsv**. Questo file contiene 20 domande e risposte in formato delimitato da tabulazioni.

1. Fare clic su **Create your KB** (Crea knowledge base personalizzata) in fondo alla pagina e attendere la creazione della knowledge base. Per il completamento dovrebbe essere necessario meno di un minuto.

1. Verificare che le domande e risposte importate da **Factbot.tsv** vengano visualizzate nella knowledge base. Quindi fare clic su **Save and train** (Salva ed esegui training) e attendere il completamento del training.

    ![Screenshot del sito Factbot Knowledge Base che mostra i dati di caricamento della knowledge base.](../media/3-save-and-train.png)

1. Fare clic sul pulsante **Test** a destra del pulsante **Save and train** (Salva ed esegui training). Digitare "Hi" nella finestra di messaggio e premere **INVIO**. Verificare che la risposta sia "Welcome to the QnA Factbot" come illustrato di seguito.

    ![Screenshot di un'interazione di test con il bot chat creato.](../media/3-test-kb.png)

1. Digitare "What book has sold the most copies?" nella finestra di messaggio e premere **INVIO**. Qual è la risposta?

1. Fare di nuovo clic sul pulsante **Test** per comprimere il pannello Test. Scegliere quindi **Pubblica** dal menu nella parte superiore della pagina e fare clic sul pulsante **Pubblica** nella parte inferiore della pagina per pubblicare la knowledge base. La *pubblicazione* rende disponibile la knowledge base in un endpoint HTTPS.

Attendere il completamento del processo di pubblicazione e verificare che venga visualizzato il messaggio che indica che il servizio QnA è stato distribuito. Ora che la knowledge base è ospitata in una propria app Web di Azure, il passaggio successivo consiste nel distribuire un bot che possa usarla.
