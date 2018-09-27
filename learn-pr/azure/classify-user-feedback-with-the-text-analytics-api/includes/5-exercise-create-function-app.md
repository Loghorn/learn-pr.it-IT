 Un'app di funzione offre un contesto per la gestione e l'esecuzione delle funzioni. Verrà creata un'app per le funzioni a cui poi verrà aggiunta una funzione.

## <a name="create-a-function-app-to-host-our-function"></a>Creare un'app per le funzioni per ospitare la funzione

1. Accedere al [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) usando lo stesso account con cui è stata attivata la sandbox.

1. Selezionare il pulsante **Crea una risorsa** visualizzato nell'angolo superiore sinistro del portale di Azure, quindi selezionare **Calcolo** > **App per le funzioni**.

1. Specificare le impostazioni dell'app per le funzioni come indicato nella tabella seguente.

    | Impostazione      | Valore  | Descrizione                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome app** | Nome globalmente univoco | Nome che identifica la nuova app per le funzioni. I caratteri validi sono `a-z`, `0-9` e `-`.  |
    | **Sottoscrizione** | **Sottoscrizione Concierge** | Sottoscrizione in cui viene creata questa nuova app per le funzioni. |
    | **Gruppo di risorse**|  **<rgn>[Nome gruppo di risorse sandbox]</rgn>** | Nome del gruppo di risorse in cui creare l'app per le funzioni.<br/><br/>Assicurarsi di selezionare **Usa esistente** e usare il gruppo di risorse dell'esercizio precedente. In questo modo, tutte le risorse create in questo modulo vengono tenute insieme. |
    | **Sistema operativo** | Windows | Il sistema operativo che ospita l'app per le funzioni.  |
    | **Hosting** |   Piano a consumo | Piano di hosting che definisce come vengono allocate le risorse all'app per le funzioni. Nel **piano a consumo** predefinito le risorse vengono aggiunte dinamicamente in base alle esigenze delle funzioni. In questo hosting [serverless](https://azure.microsoft.com/overview/serverless-computing/) si paga solo per il periodo in cui le funzioni sono in esecuzione.   |
    | **Località** | Selezionare la stessa località usata in precedenza. | Scegliere un'area nelle vicinanze o vicino ad altri servizi a cui accedono le funzioni.<br/><br/>Selezionare la stessa area usata durante la creazione dell'account dell'API Analisi del testo nell'esercizio precedente. |
    | **Account di archiviazione** |  Nome globalmente univoco |  Nome del nuovo account di archiviazione usato dall'app per le funzioni. I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. In questa finestra di dialogo il campo è popolato con un nome univoco derivato dal nome assegnato all'app. Tuttavia, è possibile usare un nome diverso o anche un account esistente. |

1. Per effettuare il provisioning dell'app per le funzioni e distribuirla, selezionare **Crea**.

1. Selezionare l'icona di notifica nell'angolo superiore destro del portale e attendere la visualizzazione del messaggio **Distribuzione in corso**.

1. La distribuzione potrebbe richiedere tempo. Rimanere pertanto nell'hub di notifica e attendere la visualizzazione del messaggio **Distribuzione completata**.

1. Al completamento della distribuzione dell'app per le funzioni, accedere al portale e selezionare **Tutte le risorse**. L'app per le funzioni sarà inclusa nell'elenco con il tipo **Servizio app** e il nome che le è stato assegnato. Per aprire l'app per le funzioni, selezionarla dall'elenco.

    La procedura è stata completata. L'app per le funzioni è stata creata e distribuita.

> [!TIP]
> Problemi nel trovare le app per le funzioni nel portale? Provare ad [aggiungere App per le funzioni ai Preferiti nel portale di Azure](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function-to-execute-our-logic"></a>Creare una funzione per eseguire la logica

Ora che è disponibile un'app per le funzioni, è il momento di creare una funzione. Una funzione viene attivata tramite un trigger. In questo modulo si userà un trigger Coda. Il runtime eseguirà il polling di una coda e avvierà questa funzione per l'elaborazione di un nuovo messaggio.

1. Espandere la nuova app per le funzioni e quindi passare il mouse sulla raccolta **Funzioni**. Selezionare il pulsante Aggiungi (__+__) quando viene visualizzato per avviare il processo di creazione della funzione.

1. Nella pagina **Iniziare rapidamente** che viene ora visualizzata selezionare **Funzione personalizzata** nella parte inferiore della pagina per caricare l'elenco dei modelli di funzione disponibili.

1. Selezionare **JavaScript** nella voce dell'elenco del modello **Trigger Coda**.

    ![Screenshot dei modelli di Funzioni di Azure con JavaScript selezionato nella voce Trigger Coda.](../media/quickstart-select-queue-trigger.png)

4. Nella finestra di dialogo **Nuova funzione** che viene visualizzata immettere i valori seguenti.

    |Proprietà  |Valore  |
    |---------|---------|
    |Linguaggio     |   **JavaScript**      |
    |Nome     |   **discover-sentiment-function**      |
    |Nome della coda     |   **new-feedback-q**      |
    |Connessione dell'account di archiviazione        |  **AzureWebJobsDashboard**       |

5. Selezionare **Crea** per avviare il processo di creazione della funzione.

1. Viene creata una funzione nel linguaggio prescelto usando il modello di funzione Trigger Coda. Anche se si implementerà la funzione in JavaScript in questo modulo, è possibile creare una funzione in uno qualsiasi dei [linguaggi supportati](https://docs.microsoft.com/azure/azure-functions/supported-languages).

Al termine del processo di creazione, l'editor di codice viene aperto nel portale e carica la pagina *index.js*. Questo file è il file di codice in cui si scrive la logica della funzione.

## <a name="try-it-out"></a>Provare questa operazione

È possibile testare quello che è stato fatto finora. Non è ancora stato scritto alcun codice, pertanto questo test ha lo scopo di verificare che quanto configurato finora venga eseguito correttamente.

1. Fare clic su **Esegui** nella parte superiore dell'editor di codice.

1. Osservare la scheda **Log** visualizzata nella parte inferiore della schermata. Se tutto funziona come previsto, verrà visualizzato un messaggio simile al seguente.
    ![Screenshot del messaggio di risposta di una chiamata alla funzione eseguita correttamente.](../media/func-default-run.PNG)

    Il pulsante **Esegui** ha avviato la funzione e ha passato i *dati della coda di esempio*, il testo predefinito dalla finestra della richiesta **Test**, alla funzione.

La procedura è conclusa. È stata aggiunta una funzione attivata dalla coda all'app per le funzioni ed è stato eseguito un test specifico per verificarne il corretto funzionamento. Nell'esercizio successivo verranno aggiunte altre funzionalità alla funzione.

Si esaminerà ora brevemente l'altro file della funzione, il file config *function.json*. I dati di configurazione di questo file sono visualizzati nell'elenco JSON seguente.

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "new-feedback-q",
      "connection": "AzureWebJobsDashboard"
    }
  ],
  "disabled": false
}
```

Come si può notare, questa funzione ha un'associazione di trigger chiamata **myQueueItem** di tipo `queueTrigger`. Quando arriva un nuovo messaggio nella coda denominata **new-feedback-q**, viene chiamata la funzione. Si fa riferimento al nuovo messaggio tramite il parametro di associazione myQueueItem. Le associazioni risultano davvero utili in questi casi.

Nel passaggio successivo si aggiungerà del codice per chiamare il servizio API Analisi del testo.

> [!TIP]
> È possibile visualizzare index.js e function.json espandendo il menu **Visualizza file** a destra del pannello della funzione nel portale di Azure.

Questo esercizio ha trattato principalmente la realizzazione dell'infrastruttura di Funzioni di Azure. È stata creata una funzione operativa ospitata in un'app per le funzioni che viene eseguita quando arriva un nuovo messaggio nella coda denominata [!INCLUDE [input-q](./q-name-input.md)]. Nel prossimo esercizio si procederà all'aggiunta del codice per chiamare un servizio cognitivo di Azure per l'analisi del sentiment.