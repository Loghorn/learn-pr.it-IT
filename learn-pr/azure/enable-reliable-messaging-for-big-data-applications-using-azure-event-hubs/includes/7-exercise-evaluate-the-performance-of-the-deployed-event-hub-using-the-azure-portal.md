In questo esercizio si userà il portale di Azure per verificare che l'hub eventi funzioni con prestazioni corrispondenti alle aspettative. Verrà inoltre testato il funzionamento della messaggistica dell'hub eventi quando è temporaneamente non disponibile e si useranno le metriche dell'hub eventi per controllare le prestazioni dell'hub eventi.

## <a name="view-event-hub-activity"></a>Visualizzare l'attività dell'hub eventi

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

1. Trovare l'hub eventi usando la barra di ricerca e aprirlo.

1. Nella pagina Panoramica visualizzare i conteggi dei messaggi.

    ![Visualizzare i messaggi dell'hub eventi](../media-draft/6-view-messages.png)

1. Le applicazioni SimpleSend ed EventProcessorSample sono configurate per l'invio e la ricezione di 100 messaggi. Si noterà che l'hub eventi ha elaborato 100 messaggi dall'applicazione SimpleSend e ha trasmesso 100 messaggi all'applicazione EventProcessorSample.

## <a name="test-event-hub-resilience"></a>Testare la resilienza dell'hub eventi

Usare la procedura seguente per vedere cosa accade quando un'applicazione invia messaggi a un hub eventi, mentre è temporaneamente non disponibile.

1. Inviare di nuovo i messaggi all'hub eventi usando l'applicazione SimpleSend. Usare il comando seguente:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. Quando viene visualizzato il messaggio **Invio completato...** premere INVIO.

1. Nel portale di Azure fare clic su **Istanza di hub eventi** > **Impostazioni** > **Proprietà**.

1. In Stato hub eventi fare clic su **Disabilitato**.

    ![Disabilitare l'hub eventi](../media-draft/7-disable-event-hub.png)

Attendere almeno cinque minuti.

1. Fare clic su **Attivo** in Stato hub eventi per abilitare di nuovo l'hub eventi e salvare le modifiche.

1. Eseguire di nuovo l'applicazione EventProcessorSample per ricevere messaggi. Usare il comando seguente.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ENTER
    ```

1. Quando non vengono più visualizzati messaggi nella console, premere INVIO.

1. Nel portale di Azure individuare lo **_spazio dei nomi_** dell'hub eventi e aprirlo. 

1. Fare clic su **Spazio dei nomi hub eventi** > **Monitoraggio** > **Metriche (anteprima)**.

    ![Usare le metriche dell'hub eventi](../media-draft/7-event-hub-metrics.png)

1. Nell'elenco **Metrica** selezionare **Messaggi in arrivo** e fare clic su **Aggiungi metrica**.

1. Nell'elenco **Metrica** selezionare **Messaggi in uscita** e fare clic su **Aggiungi metrica**.

1. Nella parte superiore del grafico fare clic su **Ultime 24 ore (automatico)** per modificare il periodo di tempo in **Ultimi 30 minuti**.

1. Fare clic su **Applica**.

Si noterà che, anche se i messaggi sono stati inviati prima che l'hub eventi fosse portato offline per un periodo di tempo, tutti i 100 messaggi sono stati trasmessi correttamente.

## <a name="summary"></a>Riepilogo

In questa unità sono state usate le metriche di hub eventi per verificare che l'hub eventi abbia completato correttamente l'elaborazione dell'invio e della ricezione di messaggi.
