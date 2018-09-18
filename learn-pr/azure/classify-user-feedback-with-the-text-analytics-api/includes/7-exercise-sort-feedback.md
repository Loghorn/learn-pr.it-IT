Ogni funzione deve avere solo un'associazione di trigger, che definisce in che modo il codice viene attivato per l'esecuzione. Oltre a un trigger, è possibile definire associazioni per la connessione alle origini dati. Come illustrato nel diagramma della soluzione, si vogliono inviare messaggi a tre code. Pertanto, si definiranno tali connessioni come associazioni di output nella funzione. È possibile creare queste associazioni tramite l'interfaccia utente di **Associazione di output**. Tuttavia, per risparmiare tempo, il file config verrà modificato direttamente.

1. Selezionare la funzione [!INCLUDE [func-name-discover](./func-name-discover.md)] nel portale delle app per le funzioni.

1. Espandere il menu **Visualizza file** a destra della schermata.

1. Nella scheda **Visualizza file** selezionare **function.json** per aprire il file config nell'editor.

1. Sostituire l'intero contenuto del file config con il codice JSON seguente. 

[!code-json[](../code/function.json)]

Sono state aggiunte tre nuove associazioni al file config.

- Ogni nuova associazione è di tipo `queue`. Queste associazioni sono per le tre code che saranno popolate con i messaggi di feedback quando se ne conoscerà il sentiment.
- Ogni associazione ha una direzione definita come `out`, dal momento che i messaggi verranno pubblicati in queste code.
- Ogni associazione usa la stessa connessione all'account di archiviazione.
- Ogni associazione ha un valore univoco `queueName` e `name`.

Inviare un messaggio a una coda è estremamente semplice, ad esempio `context.bindings.negativeFeedbackQueueItem = "<message>"`.

## <a name="update-implementation-to-sort-feedback-into-queues-based-on-sentiment-score"></a>Aggiornare l'implementazione per ordinare il feedback nelle code in base al punteggio del sentiment

L'obiettivo della soluzione per l'ordinamento del feedback è quello di ordinare i commenti in tre bucket, positivi, neutrali e negativi. Finora si dispone di una coda di input e del codice per chiamare l'API Analisi del testo e sono state definite le code di output. In questa sezione verrà aggiunta la logica per spostare i messaggi in tali code in base al sentiment.

1. Passare alla funzione [!INCLUDE [func-name-discover](./func-name-discover.md)] e aprire nuovamente `index.js` nell'editor di codice.

1. Sostituire l'implementazione con l'aggiornamento seguente.
[!code-javascript[](../code/discover-sentiment+sort.js?highlight=25-48)]

Il codice evidenziato è stato aggiunto all'implementazione. Il codice analizza la risposta dal servizio cognitivo dell'API Analisi del testo. In base al punteggio del sentiment, il messaggio viene inoltrato a una delle tre code di output. Il codice per inviare il messaggio viene visualizzato semplicemente impostando il parametro di associazione corretto.

## <a name="try-it-out"></a>Provare questa operazione

Per testare l'implementazione aggiornata, è necessario tornare in Storage Explorer. 

1. Passare al gruppo di risorse nella sezione **Gruppi di risorse** del portale.

1. Selezionare il gruppo di risorse usato in questa lezione.

1. Nel pannello **Gruppo di risorse** che si apre individuare la voce Account di archiviazione e selezionarla.
![Screenshot dell'account di archiviazione selezionato nella finestra Gruppo di risorse.](../media-draft/select-storage-account.png)

1. Selezionare **Storage Explorer (anteprima)** dal menu a sinistra della finestra principale di Account di archiviazione.  Questa azione apre Azure Storage Explorer all'interno del portale. La schermata dovrebbe essere simile allo screenshot seguente in questa fase.
![Screenshot di Storage Explorer con l'account di archiviazione, attualmente con una coda.](../media-draft/storage-explorer-menu-inputq.png)

Nella raccolta **Code** è elencata una coda. Si tratta di [!INCLUDE [input-q](./q-name-input.md)], la coda di input definita nella sezione di test precedente del modulo.

1. Selezionare [!INCLUDE [input-q](./q-name-input.md)] nel menu a sinistra per visualizzare Esplora dati per questa coda. Come previsto, la coda non contiene dati. Aggiungere un messaggio alla coda usando il comando **Aggiungi messaggio** nella parte superiore della finestra. 

1. Nella finestra di dialogo **Aggiungi messaggio** immettere il messaggio "Mi sono divertito con questo esercizio!" nel campo **Testo del messaggio** e fare clic su **OK** nella parte inferiore della finestra di dialogo. 

1. Il messaggio viene visualizzato nella finestra Dati per [!INCLUDE [input-q](./q-name-input.md)]. Dopo alcuni secondi, fare clic su **Aggiorna** nella parte superiore della visualizzazione Dati per aggiornare la visualizzazione della coda. Si noti che il messaggio scomparirà dopo alcuni secondi. Dove è andato a finire?

1. Fare clic con il pulsante destro del mouse sulla raccolta **CODE** nel menu a sinistra. Si osservi che è apparsa una *nuova* coda.
![Screenshot di Storage Explorer che mostra che nella raccolta è stata creata una nuova coda. La coda contiene un messaggio.](../media-draft/sa-new-output-q.png)

La coda [!INCLUDE [positive-q](./q-name-positive.md)] è stata creata automaticamente quando è stato inserito un messaggio per la prima volta. Con le associazioni di output della coda di Funzioni di Azure, non è necessario creare manualmente la coda di output prima di inserirvi dei messaggi. Ora che il messaggio in arrivo è stato ordinato dalla funzione in [!INCLUDE [positive-q](./q-name-positive.md)], è il momento di vedere dove vanno a finire i messaggi seguenti.

5. Usando la stessa procedura precedentemente illustrata, aggiungere i messaggi seguenti a [!INCLUDE [input-q](./q-name-input.md)].

- "Mi piacciono i broccoli!"
- "Microsoft è un'azienda"

6. Fare clic su **Aggiorna** finché [!INCLUDE [input-q](./q-name-input.md)] non è nuovamente vuoto. Questo processo potrebbe richiedere alcuni istanti e diversi aggiornamenti.

1. Fare clic con il pulsante destro del mouse sulla raccolta **CODE** e osservare le due nuove code visualizzate. Le code sono denominate [!INCLUDE [neutral-q](./q-name-neutral.md)] e [!INCLUDE [negative-q](./q-name-negative.md)]. Questa operazione potrebbe richiedere alcuni secondi, pertanto continuare ad aggiornare la raccolta **CODE** fino a visualizzare le nuove code. Al termine, l'elenco delle code dovrebbe essere analogo al seguente.

![Screenshot del menu di Storage Explorer che mostra quattro code nella raccolta CODE.](../media-draft/sa-final-q-list.png)

Fare clic su ogni coda nell'elenco per verificare se sono presenti messaggi. Se sono stati aggiunti i messaggi suggeriti, si noterà un messaggio in [!INCLUDE [positive-q](./q-name-positive.md)], [!INCLUDE [neutral-q](./q-name-neutral.md)] e [!INCLUDE [negative-q](./q-name-negative.md)].

La procedura è stata completata. Ora si dispone di una soluzione funzionante per l'ordinamento del feedback. Quando i messaggi arrivano nella coda di input, la funzione usa il servizio API Analisi del testo per ottenere un punteggio del sentiment. In base a tale punteggio, la funzione inoltra i messaggi alla coda appropriata. Anche se sembra che la funzione elabori un solo elemento della coda alla volta, il runtime di Funzioni di Azure leggerà effettivamente i batch degli elementi della coda e avvierà altre istanze della funzione per l'elaborazione in parallelo. 