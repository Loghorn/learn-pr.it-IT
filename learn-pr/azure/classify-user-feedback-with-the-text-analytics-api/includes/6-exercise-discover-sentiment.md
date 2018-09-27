Ora si aggiornerà l'implementazione della funzione per chiamare il servizio API Analisi del testo e ottenere un punteggio del sentiment.

1. Selezionare la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)] nell'app per le funzioni all'interno del portale.

1. Espandere il menu **Visualizza file** sul lato destro della schermata.

1. Nella scheda **Visualizza file** selezionare **index.js** per aprire il file di codice nell'editor.

1. Sostituire l'intero contenuto del file **index.js** con il codice JavaScript seguente e fare clic su **Salva**.

    [!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

1. Aggiornare il valore di `accessKey` nel codice incollato con la chiave di accesso per l'API Analisi del testo salvata in precedenza in questo modulo. 

1. Aggiornare il valore `uri` con l'area da cui è stata ottenuta la chiave di accesso.

Osservare cosa avviene nel codice:

- Ogni chiamata al servizio Analisi del testo richiede la chiave di accesso, che viene aggiunta come intestazione `Ocp-Apim-Subscription-Key`. 
- Ogni chiamata viene effettuata a un endpoint specifico dell'area geografica, definito da `uri` nel nostro codice.
- Alla fine del file di codice è stata definita una matrice `documents`. Questa matrice è il payload che viene inviato al servizio Analisi del testo.
- La matrice `documents` dispone di una singola entità in questo caso, ovvero il messaggio della coda che ha attivato la funzione. Anche se si ha solo un documento nella matrice, questo non significa che la soluzione sia in grado di gestire solo uno messaggio alla volta. Il runtime di Funzioni di Azure recupera ed elabora i messaggi in batch, chiamando diverse istanze della funzione *in parallelo*. Attualmente, le dimensioni predefinite del batch sono pari a 16, mentre quelle massime sono di 32.
- `id` deve essere univoco nella matrice. La proprietà `language` la lingua del testo del documento.
- Viene quindi chiamato il metodo `get_sentiments`, che usa il modulo HTTPS per eseguire la chiamata all'API Analisi del testo. Si noti che nell'intestazione di ogni richiesta viene passata la chiave di sottoscrizione, o chiave di accesso.
- Al termine del servizio viene chiamato `response_handler` e la risposta viene registrata nella console tramite `context.log`


## <a name="try-it-out"></a>Provare questa operazione

Prima di esaminare l'ordinamento nelle code, si procederà a un'esecuzione dei test.

1. Con la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)] selezionata nell'area delle app per le funzioni del portale, fare clic sulla voce di menu **Test** all'estrema destra.

1. Selezionare la voce di menu **Test** e verificare che il pannello Test sia aperto.

1. Aggiungere una stringa di testo nel corpo della richiesta, come illustrato nello screenshot.

    ![Screenshot che illustra il pannello Test della funzione espanso.](../media/test-panel-open-small.png)

1.  Fare clic su **Esegui** nella parte inferiore del pannello Test.

1. Assicurarsi che la scheda **Log** sia espansa nell'angolo in basso a sinistra della schermata principale, in corrispondenza dell'editor di codice.

1. Verificare che la scheda **Log** visualizzi le informazioni dei log che la funzione ha completato. Nella finestra sarà inoltre visualizzata la risposta dalla chiamata all'API Analisi del testo.

    ![Screenshot che mostra il pannello Test e il risultato di un test con esito positivo.](../media/sentiment-response-log1.png)

La procedura è stata completata. [!INCLUDE [func-name-discover](./func-name-discover.md)] funziona come previsto. In questo esempio è stato passato un messaggio molto positivo e si è ottenuto un punteggio di oltre 0,98. Provare a modificare il messaggio rendendolo meno ottimistico, eseguire nuovamente il test e osservare la risposta.

## <a name="add-a-message-to-the-queue"></a>Aggiungere un messaggio alla coda

Ora ripetere il test. Questa volta, invece di usare la finestra Test del portale, si inserirà effettivamente un messaggio nella coda di input e si osserverà cosa accade.

1. Passare al gruppo di risorse nella sezione **Gruppi di risorse** del portale.

1. Selezionare <rgn>[Nome del gruppo di risorse sandbox]</rgn>, il gruppo di risorse usato in questa lezione.

1. Nel pannello **Gruppo di risorse** visualizzato individuare la voce Account di archiviazione e selezionarla.

    ![Screenshot dell'account di archiviazione selezionato nella finestra Gruppo di risorse.](../media/select-storage-account.png)

1. Selezionare **Storage Explorer (anteprima)** dal menu a sinistra della finestra principale di Account di archiviazione. Questa azione apre Azure Storage Explorer all'interno del portale. 

    ![Screenshot di Storage Explorer con l'account di archiviazione, attualmente senza code.](../media/sa-no-queue.png)

    Come si può notare, in questo account di archiviazione non ci sono ancora code, quindi aggiungerne una.

5. Più indietro in questa lezione la coda associata al trigger è stata denominata **new-feedback-q**. Fare clic con il pulsante destro del mouse sull'elemento **Code** in Storage Explorer e scegliere *Crea coda*.

1. Nella finestra di dialogo visualizzata immettere **new-feedback-q** e fare clic su **OK**. È ora disponibile una coda di input.

1. Selezionare la nuova coda nel menu a sinistra per visualizzare Esplora dati per questa coda. Come previsto, la coda è vuota. Aggiungere un messaggio alla coda usando il comando **Aggiungi messaggio** nella parte superiore della finestra.

1. Nella finestra di dialogo **Aggiungi messaggio** immettere il messaggio "This message came from our input queue, new-feedback-q" nel campo **Testo del messaggio** e fare clic su **OK** nella parte inferiore della finestra di dialogo.

1. Osservare il messaggio in Esplora dati, simile a quello dello screenshot seguente.
    ![Screenshot di Storage Explorer con l'account di archiviazione e con il messaggio creato nella coda.](../media/message-in-input-queue.png)

1. Dopo alcuni secondi, fare clic su **Aggiorna** per aggiornare la visualizzazione della coda. Si osservi che la coda è ancora una volta **vuota**. Il messaggio è stato letto nella coda.

1. Tornare alla funzione nel portale e aprire la scheda **Monitoraggio**. Selezionare il messaggio più recente nell'elenco. Si noti che la funzione ha elaborato il messaggio della coda che era stato inserito in new-feedback-q. I risultati possono apparire in ritardo in questo log, quindi può essere necessario attendere qualche minuto e premere *Aggiorna*.

    ![Screenshot del dashboard di monitoraggio con una voce che indica che la funzione ha elaborato il messaggio della coda inserito in new-feedback-q.](../media/message-in-monitor.png)

    In questo test è stato eseguito un round trip completo di inserimento di un elemento nella coda e successiva elaborazione da parte della funzione.

La soluzione creata è in continua evoluzione e ora la funzione sta eseguendo nuove funzionalità utili. Riceve testo dalla coda di input e quindi chiama il servizio API Analisi del testo per ottenere un punteggio del sentiment. Si è inoltre appreso come testare la funzione tramite il portale di Azure e Storage Explorer. Nel prossimo esercizio si vedrà come è facile scrivere nelle code tramite le associazioni di output.