In questa unità si eseguirà il training del modello usando le immagini caricate e contrassegnate con tag nell'esercizio precedente. Al termine del training, è possibile perfezionare il modello caricando altre immagini con tag e ripetendone il training.

1. Fare clic sul pulsante **Train** (Esegui training) nella parte superiore della pagina per eseguire il training del modello. Ogni volta che si esegue il training del modello, viene creata una nuova iterazione. Il Servizio visione artificiale personalizzato mantiene diverse iterazioni, permettendo di confrontare i progressi nel tempo.

    ![Training del modello](../media/2-portal-click-train.png)

1. Attendere il completamento del processo di training. Dovrebbero essere necessari solo pochi secondi. Esaminare quindi le statistiche di training presentate per l'iterazione 1. 

    I risultati mostrano due misure di accuratezza del modello, **precisione** e **richiamo**. Si supponga di avere presentato al modello tre immagini di Picasso e tre di Van Gogh. Si supponga che il modello abbia identificato correttamente due degli esempi di Picasso come immagini "Picasso", ma che abbia erroneamente identificato i due esempi di Van Gogh come due Picasso. La **Precisione** è in questo caso del 50%, poiché il modello ha identificato correttamente due immagini su quattro. Il punteggio del **Richiamo** è del 67% perché il modello ha identificato correttamente due immagini di Picasso su tre.

    ![Risultati del training del modello](../media/2-portal-train-complete.png)

Nel prossimo esercizio si testerà il modello usando la funzionalità Quick Test (Test rapido) del portale, che permette di inviare immagini al modello e determinare in che modo vengono classificate usando le informazioni ottenute dalle immagini di training.

> [!TIP]
> Oltre a eseguire il training del modello usando l'interfaccia utente del portale del Servizio visione artificiale personalizzato, è possibile esercitarsi chiamando il metodo [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d9a10a4a5f8549599f1ecafc435119fa/operations/58d5835bc8cb231380095bed) nell'[API di training del Servizio visione artificiale personalizzato](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d9a10a4a5f8549599f1ecafc435119fa/operations/58d5835bc8cb231380095be3).