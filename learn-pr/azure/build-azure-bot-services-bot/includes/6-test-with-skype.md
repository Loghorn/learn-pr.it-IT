### <a name="exercise-6-test-the-bot-with-skype"></a>Esercizio 6: Testare il bot con Skype

Dopo la distribuzione, i bot possono essere connessi a canali come Skype, Slack, Microsoft Teams e Facebook Messenger, in cui è possibile interagire con essi come si farebbe con una persona. In questo esercizio si aggiungerà il bot ai contatti Skype e si avvierà una conversazione con il bot in Skype.

1. Se Skype non è già installato nel computer, installarlo ora. È possibile scaricare Skype per Windows, macOS e Linux alla pagina https://www.skype.com/en/download-skype/skype-for-computer/.

1. Tornare al bot app Web nel portale di Azure e fare clic su **Canali** nel menu a sinistra. Fare clic sull'icona **Skype**. Quindi, fare clic su **Annulla** nella parte inferiore del pannello.

    ![Modifica del canale Skype](../images/portal-edit-skype.png)

    _Modifica del canale Skype_
 
1. Fare clic su **Skype**. Fare clic su **Aggiungi ai contatti** per aggiungere il bot come contatto Skype avviare Skype.

    ![Connessione a Skype](../images/portal-click-skype.png)
    
    _Connessione a Skype_
 
1. Avviare una conversazione digitando "hi" nella finestra di Skype. Quindi comunicare con il bot ponendo domande e osservando come risponde. Fare riferimento al file **Factbot.tsv** usato per popolare la knowledge base nell'[Esercizio 2](#Exercise2) per esempi di domande da porre.
 
    ![Chat con il bot in Skype](../images/skype-responses.png)

    _Chat con il bot in Skype_

Ora si dispone di un bot perfettamente funzionante creato con il servizio Azure Bot, con intelligence fornita da Microsoft QnA Maker e disponibile per l'interazione con utenti in tutto il mondo. È possibile collegare il bot altri canali e testarlo in scenari diversi. Se si vuole rendere il bot più intelligente, provare a espandere la knowledge base con altre domande e risposte. Ad esempio, è possibile usare le [domande frequenti online](https://docs.microsoft.com/azure/bot-service/bot-service-resources-bot-framework-faq?view=azure-bot-service-3.0) su Bot Framework per eseguire il training del bot in modo che risponda alle domande sul framework stesso.