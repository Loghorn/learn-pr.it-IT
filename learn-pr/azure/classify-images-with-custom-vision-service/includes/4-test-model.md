È stato eseguito il training del modello ed è giunto il momento di testarlo. Verranno fornite nuove immagini al modello e si esaminerà la capacità del modello di classificarle.

1. Fare clic su **Quick Test** (Test rapido) nella parte superiore della pagina.

    ![Test del modello](../media/4-portal-click-quick-test.png)

1. Fare clic su **Esplora file locali** e quindi passare alla cartella "Quick Tests" nella cartella delle risorse del modulo scaricata in precedenza. Selezionare **PicassoTest_01.jpg** e quindi fare clic su **Apri**.

    ![Selezione di un'immagine di test di Picasso](../media/4-portal-select-test-01.png)

1. Esaminare i risultati del test nella finestra di dialogo "Quick Test" (Test rapido). Qual è la probabilità che il dipinto sia un Picasso? Qual è la probabilità che sia un Rembrandt o un Pollock?

    ![Selezione di un'immagine di test di Picasso](../media/4-quick-test-result.png)

1. Chiudere la finestra di dialogo "Quick Test" (Test rapido). Nella parte superiore della pagina fare quindi clic su **Predictions** (Stime).

    ![Visualizzazione dei test eseguiti](../media/4-portal-select-predictions.png)

1. Fare clic sull'immagine di test caricata per mostrarne un dettaglio. Aggiungere all'immagine un tag "Picasso" selezionando **Picasso** nell'elenco a discesa e quindi fare clic su **Salva e chiudi**.

    > Aggiungendo tag alle immagini di test in questo modo, è possibile perfezionare il modello senza caricare altre immagini di training.

    ![Aggiunta di tag all'immagine di test](../media/4-tag-test-image.png)

1. Eseguire un altro test rapido usando questa volta il file denominato **FlowersTest.jpg** nella cartella "Quick Test". Verificare che a questa immagine sia assegnata una bassa probabilità che si tratti di un Picasso, un Rembrandt o un Pollock.

Il modello è stato sottoposto a training ed è pronto per l'uso e sembra in grado di identificare i dipinti di determinati artisti. È possibile chiamare l'endpoint di stima tramite HTTP e vedere cosa succede.