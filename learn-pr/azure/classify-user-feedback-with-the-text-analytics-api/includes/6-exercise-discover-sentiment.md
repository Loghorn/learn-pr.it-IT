Si procederà ora ad aggiornare l'implementazione della funzione per chiamare il servizio API Analisi del testo e ottenere un punteggio del sentiment.

1. Selezionare la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)] nel portale delle app per le funzioni.

1. Espandere il menu **Visualizza file** a destra della schermata.

1. Nella scheda **Visualizza file** selezionare **index.js** per aprire il file di codice nell'editor.

1. Sostituire l'intero contenuto del file di codice con il codice JavaScript seguente.

[!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

Osservare cosa avviene nel codice:

- Per chiamare il servizio di analisi del testo, impostare `accessKey`, evidenziato nel frammento di codice, sulla chiave salvata in precedenza.
- Aggiornare `uri` impostandolo sull'area da cui è stata ottenuta la chiave di accesso, se diversa da *westus* (Stati Uniti occidentali) come riportato in questo esempio.
- Alla fine del file di codice è stata definita una matrice `documents`. Questa matrice è il payload che viene inviato al servizio Analisi del testo. 
- La matrice `documents` dispone di una singola entità in questo caso, ovvero il messaggio della coda che ha attivato la funzione. Anche se si ha solo un documento nella matrice, questo non significa che la soluzione sia in grado di gestire solo uno messaggio alla volta. Il runtime di Funzioni di Azure recupera ed elabora i messaggi in batch, chiamando diverse istanze della funzione *in parallelo*. Attualmente, le dimensioni predefinite del batch sono pari a 16, mentre quelle massime sono di 32.
- `id` deve essere univoco nella matrice. La proprietà `language` la lingua del testo del documento.  
- Viene quindi chiamato il metodo `get_sentiments`, che usa il modulo HTTPS per eseguire la chiamata all'API Analisi del testo. Si noti che nell'intestazione di ogni richiesta viene passata la chiave di sottoscrizione, o di accesso.
- Al termine del servizio, viene chiamato `response_handler` e la risposta viene registrata nella console tramite `context.log` 

## <a name="try-it-out"></a>Provare questa operazione

Prima di esaminare l'ordinamento nelle code, si procederà a un'esecuzione dei test. 

1.  Con la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)] selezionata nel portale delle app per le funzioni, fare clic sulla voce di menu Test all'estrema sinistra per espanderla.

2. Selezionare la voce di menu **Test** e verificare che il pannello Test sia aperto. Lo screenshot seguente riporta quello che si dovrebbe vedere. 

![Screenshot che mostra il pannello Test della funzione espanso.](../media-draft/test-panel-open-small.png)

3. Aggiungere una stringa di testo nel corpo della richiesta, come illustrato nello screenshot. 

1.  Fare clic su **Esegui** nella parte inferiore del pannello Test.

1. Assicurarsi che la scheda **Log** sia espansa nell'angolo in basso a sinistra della schermata principale, in corrispondenza dell'editor di codice. 

1. Verificare che la scheda **Log** visualizzi le informazioni dei log che la funzione ha completato. Nella finestra sarà inoltre visualizzata la risposta dalla chiamata all'API Analisi del testo. 

![Screenshot che mostra il pannello Test e il risultato di un test con esito positivo.](../media-draft/sentiment-response-log1.png)

La procedura è stata completata. [!INCLUDE [func-name-discover](./func-name-discover.md)] funziona come previsto. In questo esempio è stato passato un messaggio molto positivo e si è ottenuto un punteggio di oltre 0,98. Provare a modificare il messaggio rendendolo meno ottimistico, eseguire nuovamente il test e osservare la risposta.

## <a name="try-it-out-again-optional"></a>Provare nuovamente questa operazione (facoltativo)

È possibile ripetere il test. Questa volta, invece di usare la finestra Test del portale, si inserirà effettivamente un messaggio nella coda di input e si osserverà cosa accade. 

1. Passare al gruppo di risorse nella sezione **Gruppi di risorse** del portale.

1. Selezionare il gruppo di risorse usato in questa lezione.

1. Nel pannello **Gruppo di risorse** che viene visualizzato individuare la voce Account di archiviazione e selezionarla.

![Screenshot dell'account di archiviazione selezionato nella finestra Gruppo di risorse.](../media-draft/select-storage-account.png)

1. Selezionare **Storage Explorer (anteprima)** dal menu a sinistra della finestra principale di Account di archiviazione.  Questa azione apre Azure Storage Explorer all'interno del portale. La schermata dovrebbe essere simile allo screenshot seguente in questa fase. 

![Screenshot di Storage Explorer con l'account di archiviazione, attualmente senza code.](../media-draft/sa-no-queue.png)

Come si può notare, in questo account di archiviazione non ci sono ancora code.

1. Più indietro in questa lezione la coda associata al trigger è stata denominata **new-feedback-q**. Fare clic con il pulsante destro del mouse sull'elemento **Code** in Storage Explorer e scegliere *Crea coda*.

1. Nella finestra di dialogo visualizzata immettere **new-feedback-q** e fare clic su **OK**. È ora disponibile una coda di input. 

1. Selezionare la nuova coda nel menu a sinistra per visualizzare Esplora dati per questa coda. Come previsto, la coda è vuota. Aggiungere un messaggio alla coda usando il comando **Aggiungi messaggio** nella parte superiore della finestra.

1. Nella finestra di dialogo **Aggiungi messaggio** immettere un messaggio che indica che "Il messaggio proviene dalla coda di input new-feedback-q" nel campo **Testo del messaggio** e fare clic su **OK** nella parte inferiore della finestra di dialogo. 

1. Osservare il messaggio, simile a quello dello screenshot seguente, in Esplora dati.
![Screenshot di Storage Explorer con l'account di archiviazione, con il messaggio creato nella coda.](../media-draft/message-in-input-queue.png)

1. Dopo alcuni secondi, fare clic su **Aggiorna** per aggiornare la visualizzazione della coda. Si osservi che la coda è ancora una volta vuota. Il messaggio deve essere stato letto dalla coda. 

1. Tornare alla funzione nel portale e aprire la scheda **Monitoraggio**. Selezionare il messaggio più recente nell'elenco. Si noti che la funzione ha elaborato il messaggio della coda inserito in new-feedback-q.

![Screenshot del dashboard di monitoraggio che mostra una voce che indica che la funzione ha elaborato il messaggio della coda inserito in new-feedback-q.](../media-draft/message-in-monitor.png)

In questo test è stato eseguito un round trip completo di inserimento di un elemento nella coda e successiva elaborazione da parte della funzione.

La soluzione creata è in continua evoluzione e ora la funzione sta eseguendo nuove funzionalità utili. Riceve testo dalla coda di input e quindi chiama il servizio API Analisi del testo per ottenere un punteggio del sentiment.  Si è inoltre appreso come testare la funzione tramite il portale di Azure e Storage Explorer. Nel prossimo esercizio si vedrà come è facile scrivere nelle code tramite le associazioni di output.