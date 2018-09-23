In questa unità si userà il portale di Azure per verificare che l'hub eventi funzioni con prestazioni corrispondenti alle aspettative. Verrà inoltre testato il funzionamento della messaggistica dell'hub eventi quando è temporaneamente non disponibile e si useranno le metriche dell'hub eventi per controllarne le prestazioni.

## <a name="view-event-hub-activity"></a>Visualizzare l'attività dell'hub eventi

1. Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui è stato attivato l'ambiente sandbox.

1. Trovare l'hub eventi usando la barra di ricerca e aprirlo.

1. Nella pagina Panoramica visualizzare il numero di messaggi.

    ![Screenshot del portale di Azure che mostra lo spazio dei nomi dell'hub eventi con il numero di messaggi](../media/6-view-messages.png)

1. Le applicazioni SimpleSend ed EventProcessorSample sono configurate per l'invio e la ricezione di 100 messaggi. Si noterà che l'hub eventi ha elaborato 100 messaggi dall'applicazione SimpleSend e ha trasmesso 100 messaggi all'applicazione EventProcessorSample.

## <a name="test-event-hub-resilience"></a>Testare la resilienza dell'hub eventi

Usare la procedura seguente per vedere cosa accade quando un'applicazione invia messaggi a un hub eventi temporaneamente non disponibile.

1. Inviare di nuovo i messaggi all'hub eventi usando l'applicazione SimpleSend. Usare il comando di seguito:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. Quando viene visualizzato il messaggio **Invio completato...**, premere <kbd>INVIO</kbd>.

1. Selezionare l'hub eventi nella schermata **Panoramica**; verranno visualizzati i dettagli specifici dell'hub eventi. È possibile accedere a questa schermata anche tramite la voce **Hub eventi** dalla pagina dello spazio dei nomi.

1. Selezionare **Impostazioni** > **Proprietà**.

1. In Stato hub eventi fare clic su **Disabilitato**. Salvare le modifiche.

    ![Disabilitare l'hub eventi](../media/7-disable-event-hub.png)

    **Attendere almeno cinque minuti.**

1. Fare clic su **Attivo** in Stato hub eventi per abilitare nuovamente l'hub eventi e salvare le modifiche.

1. Eseguire di nuovo l'applicazione EventProcessorSample per ricevere i messaggi. Usare il comando di seguito.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. Quando non vengono più visualizzati messaggi nella console, premere <kbd>INVIO</kbd>.

1. Tornando al portale di Azure, passare nuovamente allo spazio dei nomi dell'hub eventi. Se ancora ci si trova nella pagina dell'hub eventi, è possibile utilizzare la barra di navigazione nella parte superiore della schermata per tornare indietro oppure cercare lo spazio dei nomi e selezionarlo.

1. Fare clic su **MONITORAGGIO** > **Metriche (anteprima)**.

    ![Screenshot che mostra le metriche dell'hub eventi con la visualizzazione del numero di messaggi in arrivo e in uscita.](../media/7-event-hub-metrics.png)

1. Nell'elenco **Metrica** selezionare **Messaggi in arrivo** e fare clic su **Aggiungi metrica**.

1. Nell'elenco **Metrica** selezionare **Messaggi in uscita** e fare clic su **Aggiungi metrica**.

1. Nella parte superiore del grafico fare clic su **Ultime 24 ore (automatico)** per modificare il periodo di tempo in **Ultimi 30 minuti** ed espandere il grafico dei dati.

Si noterà che, anche se i messaggi sono stati inviati prima che l'hub eventi fosse portato offline per un periodo di tempo, tutti i 100 messaggi sono stati trasmessi correttamente.

## <a name="summary"></a>Riepilogo

In questa unità sono state usate le metriche dell'hub eventi per verificare che l'hub eventi abbia completato correttamente l'elaborazione dell'invio e della ricezione di messaggi.
