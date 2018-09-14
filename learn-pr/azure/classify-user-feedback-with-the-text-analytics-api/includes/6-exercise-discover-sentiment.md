Aggiorniamo la nostra implementazione di funzione per chiamare il servizio API traduzione testuale Analitica e riceve un punteggio del sentiment.

1. Selezionare la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)], nel nostro app per le funzioni nel portale.

1. Espandere la **visualizzare i file** menu a destra della schermata.

1. Sotto il **visualizzare i file** scheda, seleziona **index. js** per aprire il file di codice nell'editor.

1. Sostituire l'intero contenuto del **index. js** con il codice JavaScript seguente e **salvare**.

[!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

Si osservi cosa avviene nel codice seguente:

- Per chiamare il servizio di analitica di testo, impostare `accessKey`evidenziato nel frammento di codice, per la chiave salvata in precedenza.
- Update `uri` nell'area da cui ottenere la chiave di accesso, se quell'area è diversa da quello *westus* illustrato in questo esempio.
- Nella parte inferiore del file di codice, abbiamo definito un `documents` matrice. Questa matrice è il payload che viene inviato al servizio Analitica di testo.
- Il `documents` matrice dispone di una singola voce in questo caso, il messaggio della coda che ha attivato la funzione. Anche se si ha solo un documento nella matrice, questo non significa che la nostra soluzione può gestire solo uno messaggio alla volta. Il runtime di funzioni di Azure recupera ed elabora i messaggi in batch, la chiamata a diverse istanze della funzione *parallelamente*. Attualmente, le dimensioni del batch predefinito sono 16 e le dimensioni massime dei batch sono 32.
- Il `id` deve essere univoco all'interno della matrice. Il `language` proprietà specifica la lingua del testo del documento.
- Chiamiamo quindi il metodo `get_sentiments`, che usa il modulo HTTPS per la chiamata all'API di Analitica di testo. Si noti che la chiave di sottoscrizione o l'accesso, è necessario passare nell'intestazione di ogni richiesta.
- Quando termina, il servizio nostro `response_handler` viene chiamato e abbiamo log la risposta alla console usando `context.log`


## <a name="try-it-out"></a>Provare il servizio

Prima di esaminare l'ordinamento nelle code, passiamo a ciò che è stata per un test eseguita.

1. Con la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)], selezionata nel portale di App per le funzioni, fare clic sulla voce di menu Test all'estrema sinistra per espanderla.

1. Selezionare il **testare** menu item e verificare che è aperto il pannello test. Lo screenshot seguente mostra cosa sarà simile.

    ![Screenshot che mostra la funzione Test pannello espanso.](../media/test-panel-open-small.png)

1. Aggiungere una stringa di testo nel corpo della richiesta, come illustrato nello screenshot.

1.  Fare clic su **eseguire** nella parte inferiore del pannello del test.

1. Assicurarsi che il **registri** scheda è espansa nella parte inferiore della schermata principale, nell'editor di codice.

1. Verificare che il **registri** scheda vengono visualizzate informazioni di log che la funzione è stata completata. Nella finestra vengono visualizzate anche la risposta alla chiamata API traduzione testuale Analitica.

![Screenshot che mostra il pannello di Test e risultati di un test ha esito positivo.](../media/sentiment-response-log1.png)

La procedura è stata completata. Il [!INCLUDE [func-name-discover](./func-name-discover.md)] funziona come previsto. In questo esempio viene passato in un messaggio molto upbeat e ricevuti un punteggio pari a tramite 0.98. Provare a modificare il messaggio su un valore minore ottimistico, rieseguire il test e prendere nota della risposta.

## <a name="add-a-message-to-the-queue"></a>Aggiungere un messaggio alla coda

È possibile ripetere il test. Questa volta, invece di usare la finestra di Test del portale, si verrà effettivamente inserire un messaggio nella coda di input e osservare cosa accade.

1. Passare al gruppo di risorse nel **gruppi di risorse** sezione del portale.

1. Selezionare il gruppo di risorse usato in questa lezione.

1. Nel **gruppo di risorse** pannello che viene visualizzato, individuare la voce di Account di archiviazione e selezionarlo.

    ![Schermata account di archiviazione selezionato nella finestra del gruppo di risorse.](../media/select-storage-account.png)

1. Selezionare **Storage Explorer (anteprima)** nel menu a sinistra della finestra principale di Account di archiviazione.  Questa azione apre Esplora archivi di Azure all'interno del portale. In questa fase, la schermata dovrebbe essere simile allo screenshot seguente.

![Screenshot di Esplora archiviazione con l'account di archiviazione, con nessuna coda attualmente.](../media/sa-no-queue.png)

Come può notare, Microsoft non sono ancora presenti code nell'account di archiviazione, quindi, correggiamo.

5. Se si ricorda di più indietro in questa lezione, è denominata la coda associata al nostro trigger **nuovi commenti e suggerimenti-q**. Fare clic sui **code** in storage explorer e selezionare *Create Queue*.

1. Nella finestra di dialogo visualizzata, immettere **nuovi commenti e suggerimenti-q** e fare clic su **OK**. È ora disponibile la coda di input.

1. Selezionare la nuova coda nel menu a sinistra per visualizzare Esplora dati per la coda. Come previsto, la coda è vuota. È possibile aggiungere un messaggio alla coda usando il **Aggiungi messaggio** comando nella parte superiore della finestra.

1. Nel **Aggiungi messaggio** finestra di dialogo immettere "questo messaggio proviene dal nostro coda di input, nuovi commenti e suggerimenti-q" nel **testo del messaggio** campo e fare clic su **OK** nella parte inferiore della finestra di dialogo.

1. Osservare il messaggio, simile al messaggio nello screenshot seguente, in Esplora dati.
    ![Screenshot di Esplora archiviazione con l'account di archiviazione con il messaggio che è stato creato nella coda.](../media/message-in-input-queue.png)

1. Dopo alcuni secondi, fare clic su **Aggiorna** per aggiornare la visualizzazione della coda. Osservare che la coda sia vuota ancora una volta. Un elemento deve avere legge il messaggio dalla coda.

1. Tornare alla funzione nel portale e aprire il **Monitor** scheda. Selezionare il messaggio più recente e nell'elenco. Osservare che la funzione ha elaborato il messaggio di coda che è avessimo registrato per il nuovo-commenti e suggerimenti-q.

![Schermata di monitorare il dashboard che mostra una voce che indica che la funzione ha elaborato il messaggio di coda che abbiamo inviati al nuovo-commenti e suggerimenti-q.](../media/message-in-monitor.png)

In questo test è stato fatto un round trip completo di un elemento pubblicato nella coda e quindi osservando la funzione elaborarlo.

Stiamo facendo progressi con la nostra soluzione. La funzione ora sta eseguendo un'operazione utile. È la ricezione di testo dal nostro coda di input e quindi chiama il servizio API traduzione testuale Analitica per ottenere un punteggio del sentiment.  Abbiamo inoltre imparato come testare la funzione tramite il portale di Azure e di Storage Explorer. Nel prossimo esercizio, si vedrà come è facile scrivere nelle code tramite le associazioni di output.