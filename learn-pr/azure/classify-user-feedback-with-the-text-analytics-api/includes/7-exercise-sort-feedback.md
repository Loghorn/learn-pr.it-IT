Ogni funzione deve avere uno e solo uno, trigger. Definisce come il nostro codice viene attivato per l'esecuzione. Oltre a un trigger, è possibile definire associazioni che ci si connettono a origini dati. Se si ricorda il diagramma della soluzione, si vuole inviare messaggi alle code di tre. Pertanto, si definirà tali connessioni come le associazioni di output nella funzione. È possibile creare tali associazioni tramite il **associazione di Output** dell'interfaccia utente. Tuttavia, per risparmiare tempo, verrà verrà modificato il file di configurazione direttamente.

1. Selezionare la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)], nel portale di App per le funzioni.

1. Espandere la **visualizzare i file** menu a destra della schermata.

1. Sotto il **visualizzare i file** scheda, seleziona **Function. JSON** per aprire il file di configurazione nell'editor.

1. Sostituire l'intero contenuto del file di configurazione con il codice JSON seguente.

[!code-json[](../code/function.json)]

Sono state aggiunte tre nuove associazioni di file di configurazione.

- Ogni nuova associazione è di tipo `queue`. Queste associazioni sono per le tre code che si saranno popolare con i messaggi di feedback quando si conosce il sentiment dei commenti e suggerimenti.
- Ogni associazione presenta una direzione definita come `out`, dal momento che vengono pubblicati i messaggi a queste code.
- Ogni associazione utilizza la stessa connessione all'account di archiviazione.
- Ogni associazione presenta un valore univoco `queueName` e `name`.

Invio di un messaggio a una coda è facile come, ovviamente, ad esempio, `context.bindings.negativeFeedbackQueueItem = "<message>"`.

## <a name="update-implementation-to-sort-feedback-into-queues-based-on-sentiment-score"></a>Implementazione dell'aggiornamento per ordinare i commenti e suggerimenti nelle code di base punteggio del sentiment

L'obiettivo del nostro sequenza di commenti e suggerimenti è per ordinare i commenti e suggerimenti in tre bucket, positivo, negativo e neutro. Finora, abbiamo la coda di input, il codice per chiamare API di Analitica di testo, e abbiamo definito i nostri code di output. In questa sezione si aggiungerà la logica per spostare i messaggi in tali code di base del sentiment.

1. Passare alla nostra funzione [!INCLUDE [func-name-discover](./func-name-discover.md)], quindi aprire `index.js` nell'editor del codice anche in questo caso.

1. Sostituire l'implementazione con l'aggiornamento seguente.

[!code-javascript[](../code/discover-sentiment+sort.js?highlight=25-48)]

È stato aggiunto il codice evidenziato per la nostra implementazione. Il codice analizza la risposta dal servizio cognitivo API traduzione testuale Analitica. In base al punteggio del sentiment, il messaggio viene inoltrato a uno dei o tre code di output. Il codice per inviare il messaggio viene semplicemente impostando il parametro di associazione corretto.

## <a name="try-it-out"></a>Provare il servizio

Per testare l'implementazione aggiornata, si sarà head nuovamente a Storage Explorer.

1. Passare al gruppo di risorse nel **gruppi di risorse** sezione del portale.

1. Selezionare il gruppo di risorse usato in questa lezione.

1. Nel **gruppo di risorse** pannello che si apre, individuare la voce di Account di archiviazione e selezionarlo.
    ![Schermata account di archiviazione selezionato nella finestra del gruppo di risorse.](../media/select-storage-account.png)

1. Selezionare **Storage Explorer (anteprima)** nel menu a sinistra della finestra principale di Account di archiviazione.  Questa azione apre Esplora archivi di Azure all'interno del portale. In questa fase, la schermata dovrebbe essere simile allo screenshot seguente.
    ![Screenshot di Esplora archiviazione con l'account di archiviazione, con una sola coda attualmente.](../media/storage-explorer-menu-inputq.png)

È disponibile una sola coda elencata sotto la **code** raccolta. La coda è [!INCLUDE [input-q](./q-name-input.md)], la coda di input è definiti nella sezione test precedente del modulo.

1. Selezionare [!INCLUDE [input-q](./q-name-input.md)] nel menu a sinistra per visualizzare Esplora dati per la coda. Come previsto, la coda in cui i dati. È possibile aggiungere un messaggio alla coda usando il **Aggiungi messaggio** comando nella parte superiore della finestra.

1. Nel **Aggiungi messaggio** finestra di dialogo immettere "Si sono verificato Divertiamoci con questo esercizio!" nel **testo del messaggio** campo e fare clic su **OK** nella parte inferiore della finestra di dialogo.

1. Viene visualizzato il messaggio nella finestra dei dati per [!INCLUDE [input-q](./q-name-input.md)]. Dopo alcuni secondi, fare clic su **Aggiorna** nella parte superiore della visualizzazione dei dati per aggiornare la visualizzazione della coda. Osservare che il messaggio scomparirà dopo un periodo di tempo. Pertanto, in cui è stato copiato il file?

1. Fare clic sui **code** insieme nel menu a sinistra. Si noti che un *nuovo* è apparsa coda.
    ![Screenshot di Storage Explorer che mostra una nuova coda è stato creato nella raccolta. La coda contiene un solo messaggio.](../media/sa-new-output-q.png)

La coda [!INCLUDE [positive-q](./q-name-positive.md)] creata automaticamente quando è stato pubblicato un messaggio ad esso per la prima volta. Con le associazioni di output della coda di funzioni di Azure, non è necessario creare manualmente la coda di output prima della registrazione a esso. Ora che abbiamo vedere un messaggio in arrivo siano stato ordinato, la funzione in [!INCLUDE [positive-q](./q-name-positive.md)], vediamo in cui i seguenti messaggi ' s land.

1. Usando la stessa procedura precedente, aggiungere i messaggi seguenti a [!INCLUDE [input-q](./q-name-input.md)].

- "Mi piace broccoli!"
- "Microsoft è un'azienda"

1. Fare clic su **Refresh** fino a quando non [!INCLUDE [input-q](./q-name-input.md)] ancora una volta è vuoto. Questo processo potrebbe richiedere alcuni istanti e richiedere alcuni aggiornamenti.

1. Fare clic sui **code** insieme e osservare due code più visualizzati. Le code sono denominate [!INCLUDE [neutral-q](./q-name-neutral.md)] e [!INCLUDE [negative-q](./q-name-negative.md)]. Ciò potrebbe richiedere alcuni secondi, quindi continuare ad aggiornare il **code** raccolta fino a nuove code. Al termine dell'esercitazione, l'elenco delle code dovrebbe essere simile al seguente.

![Menu di screenshot di Storage Explorer che mostra quattro code nella raccolta di code.](../media/sa-final-q-list.png)

Fare clic su ogni coda nell'elenco per verificare se dispone di messaggi. Se è stato aggiunto i messaggi suggeriti, si noterà un messaggio in [!INCLUDE [positive-q](./q-name-positive.md)], [!INCLUDE [neutral-q](./q-name-neutral.md)], e [!INCLUDE [negative-q](./q-name-negative.md)].

La procedura è stata completata. Ora si dispone di una sequenza di commenti e suggerimenti di lavoro. Quando i messaggi arrivano nella coda di input, la funzione Usa il servizio API traduzione testuale Analitica per ottenere un punteggio del sentiment. La funzione basato su tale punteggio, inoltra i messaggi alla coda appropriata. Anche se sembra che la funzione consente di elaborare un solo elemento della coda alla volta, il runtime di funzioni di Azure verrà effettivamente batch di elementi della coda di lettura e creare altre istanze della funzione per l'elaborazione in parallelo.