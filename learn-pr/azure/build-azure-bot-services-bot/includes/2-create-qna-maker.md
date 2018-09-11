### <a name="exercise-2-create-a-knowledge-base-with-microsoft-qna-maker"></a>Esercizio 2: Creare una knowledge base con Microsoft QnA Maker

[Microsoft QnA Maker](https://www.qnamaker.ai/) fa parte di [Servizi cognitivi di Azure](https://www.microsoft.com/cognitive-services/), una suite di servizi e API per la creazione di app intelligenti supportate da intelligenza artificiale e Machine Learning. Anziché codificare un bot in modo che anticipi ogni possibile domanda di un utente e fornire una risposta, è possibile connetterlo a una knowledge base di domande e risposte creata con QnA Maker. Uno scenario di utilizzo comune consiste nel creare una knowledge base da una serie di domande frequenti, in modo che il bot possa rispondere a domande specifiche del dominio, ad esempio "Dove si trova il codice Product Key di Windows?" o "Dove si può scaricare Visual Studio Code?".

In questo esercizio si userà QnA Maker per creare una knowledge base contenente domande del tipo "What NFL teams have won the most Super Bowls?" e "What is the largest city in the world?". Quindi si distribuirà la knowledge base in un'app Web di Azure in modo che sia accessibile tramite un endpoint HTTPS.

1. Aprire il [portale di Microsoft QnA Maker](https://www.qnamaker.ai/) nel browser e accedere con l'account Microsoft se non è già stato fatto. Quindi fare clic su **Create a knowledge base** (Crea una knowledge base) nella barra dei menu nella parte superiore della pagina.
 
    ![Creazione di una knowledge base](../images/qna-new-kb.png)

    _Creazione di una knowledge base_

1. Fare clic su **Create a QnA service** (Crea un servizio QnA).

    ![Creazione di un servizio QnA](../images/create-kb-1.png)

    _Creazione di un servizio QnA_

1. Immettere un nome nella casella **Nome**, ad esempio "qa-factbot". Questo nome deve essere univoco all'interno di Azure, dunque assicurarsi che accanto ad esso *e* nella casella **Nome dell'app** più in basso nel pannello compaia un segno di spunta verde. Selezionare **Usa esistente** sotto **Gruppo di risorse** e selezionare il gruppo di risorse "factbot-rg" creato durante la distribuzione del bot app Web di Azure nell'[Esercizio 1](#Exercise1). Selezionare i piani tariffari **F0** e **F**. Entrambi i piani sono gratuiti e sono ideali per provare a usare i bot. Selezionare la località più vicina in entrambi gli elenchi a discesa e quindi scegliere il pulsante **Crea** nella parte inferiore del pannello.

    ![Creazione di un servizio QnA](../images/new-qna-maker-service.png)

    _Creazione di un servizio QnA_

1. Fare clic su **Gruppi di risorse** nella barra multifunzione sul lato sinistro del portale e aprire il gruppo di risorse "factbot-rg". Attendere finché "Distribuzione in corso" non cambia in "Operazione completata" nella parte superiore del pannello per indicare che il servizio QnA e le risorse associate sono stati distribuiti correttamente. Di nuovo, è possibile fare clic su **Aggiorna** nella parte superiore del pannello per aggiornare lo stato di distribuzione.

    ![Distribuzione completata](../images/resource-group-master-2.png)

    _Distribuzione completata_

1. Tornare alla pagina[Create a knowledge base](https://www.qnamaker.ai/Create) (Crea una knowledge base) nel portale di QnA Maker. Sotto **Azure QnA service** (Servizio Azure QnA) selezionare il servizio QnA il cui nome è stato specificato nel passaggio 3. Assegnare quindi un nome alla knowledge base, ad esempio "Factbot Knowledge Base".

    ![Assegnazione del nome alla knowledge base](../images/create-kb-2-3.png)

    _Assegnazione del nome alla knowledge base_

1. È possibile inserire domande e risposte in una knowledge base QnA Maker manualmente oppure importarle da domande frequenti online o file locali. I formati supportati sono i file di testo delimitati da tabulazioni, i documenti di Microsoft Word, i fogli di calcolo di Excel e i file PDF.

    Per una dimostrazione, [fare clic qui](https://topcs.blob.core.windows.net/public/bots-resources.zip) per scaricare un file ZIP contenente un file di testo denominato **Factbot.tsv** e copiare il file nel computer. Quindi scorrere verso il basso nel portale di QnA Maker, fare clic su **+ Add file** (+ Aggiungi file)e selezionare **Factbot.tsv**. Questo file contiene 20 domande e risposte in formato delimitato da tabulazioni.

    ![Popolamento della knowledge base](../images/create-kb-4.png)

    _Popolamento della knowledge base_

1. Fare clic su **Create your KB** (Crea knowledge base personalizzata) in fondo alla pagina e attendere la creazione della knowledge base. Per il completamento dovrebbe essere necessario meno di un minuto.

    ![Creazione della knowledge base](../images/create-kb-5.png)

    _Creazione della knowledge base_

1. Verificare che le domande e risposte importate da **Factbot.tsv** vengano visualizzate nella knowledge base. Quindi fare clic su **Save and train** (Salva ed esegui training) e attendere il completamento del training.

    ![Training della knowledge base](../images/save-and-train.png)

    _Training della knowledge base_

1. Fare clic sul pulsante **Test** a destra del pulsante **Save and train** (Salva ed esegui training). Digitare "Hi" nella finestra di messaggio e premere **INVIO**. Verificare che la risposta sia "Welcome to the QnA Factbot" come illustrato di seguito.

    ![Test della knowledge base](../images/test-kb.png)

    _Test della knowledge base_

1. Digitare "What book has sold the most copies?" nella finestra di messaggio e premere **INVIO**. Qual è la risposta?

1. Fare di nuovo clic sul pulsante **Test** per comprimere il pannello Test. Quindi scegliere **Pubblica** dal menu nella parte superiore della pagina e fare clic sul pulsante **Pubblica** nella parte inferiore della pagina per pubblicare la knowledge base. La *pubblicazione* rende disponibile la knowledge base in un endpoint HTTPS.

    ![Pubblicazione della knowledge base](../images/publish-kb.png)

    _Pubblicazione della knowledge base_ 

Attendere che il processo di pubblicazione sia completato e verificare che compaia il messaggio che informa che il servizio QnA è stato distribuito. Ora che la knowledge base è ospitata in una propria app Web di Azure, il passaggio successivo consiste nel distribuire un bot che possa usarla.