### <a name="exercise-6-use-the-app-to-classify-images"></a>Esercizio 6: Usare l'app per classificare immagini

In questo esercizio si userà l'app Artworks per inviare immagini al Servizio visione artificiale personalizzato per la classificazione. L'app usa le informazioni JSON restituite da chiamate al metodo [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) dell'API per le stime del Servizio visione artificiale personalizzato per indicare quando un'immagine rappresenta un dipinto di Picasso, Rembrandt, Pollock o di nessuno di questi artisti. L'app indica anche la probabilità che la classificazione assegnata all'immagine sia corretta.

1. Fare clic sul pulsante **Browse** (Sfoglia) nell'app Artworks. 

    ![Esplorazione di immagini locali nell'app Artworks](../images/app-click-browse.png)

    _Esplorazione di immagini locali nell'app Artworks_ 

1. Passare alla cartella "Quick Tests" nelle risorse del lab. Selezionare il file denominato **PicassoTest_02.jpg** e quindi fare clic su **Apri**.

1. Fare clic sul pulsante **Predict** (Stima) per inviare l'immagine al Servizio visione artificiale personalizzato.

    ![Invio dell'immagine al Servizio visione artificiale personalizzato](../images/app-click-predict.png)

    _Invio dell'immagine al Servizio visione artificiale personalizzato_ 

1. Verificare che l'app identifichi il dipinto come opera di Picasso.

    ![Classificazione di un'immagine come opera di Picasso](../images/app-prediction-01.png)

    _Classificazione di un'immagine come opera di Picasso_ 

1. Ripetere i passaggi da 1 a 3 per **RembrandtTest_01.jpg** e **PollockTest_01.jpg** e verificare che l'app sia in grado di identificare i dipinti come opere di Rembrandt e Pollock.

    ![Classificazione di un'immagine come opera di Rembrandt](../images/app-prediction-02.png)

    _Classificazione di un'immagine come opera di Rembrandt_ 

1. Ripetere i passaggi da 1 a 3 per **VanGoghTest_01.png** e **VanGoghTest_02.png** e verificare che l'app non identifichi questi capolavori di Van Gogh come opere di Picasso, Rembrandt o Pollock.

    ![Opera identificata come non di Picasso, Rembrandt o Pollock](../images/app-prediction-03.png)

    _Opera identificata come non di Picasso, Rembrandt o Pollock_ 

1. Come si può osservare, l'uso dell'API per le stime da un'app è altrettanto affidabile quanto tramite il portale del Servizio visione artificiale personalizzato e anche molto più divertente. Inoltre, se si passa alla pagina Predictions (Stime) nel portale, anche qui verrà visualizzata ognuna delle immagini caricate tramite l'app.
 
    ![Immagine inviata al Servizio visione artificiale personalizzato](../images/portal-all-predictions.png)

    _Immagine inviata al Servizio visione artificiale personalizzato_ 

È possibile testare liberamente immagini personali e valutare la capacità del modello di identificare gli artisti o di determinare un'immagine come non di Picasso, Rembrandt o Pollock. Se si vuole eseguire il training del modello perché riconosca anche le opere di Van Gogh, caricare alcuni dipinti di Van Gogh, corredarli del tag "Van Gogh" ed eseguire di nuovo il training del modello. Non vi sono limiti all'intelligenza che è possibile aggiungere se si è disposti a eseguire nuovi training. Ricordare inoltre che in generale più sono le immagini di cui si esegue il training, più intelligente sarà il modello.