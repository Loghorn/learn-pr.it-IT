Per compilare la soluzione, è necessario ospitare il codice.  Un'app per le funzioni di Funzioni di Azure è ideale per ospitare la logica. 

## <a name="create-a-function-app-to-host-our-function"></a>Creare un'app per le funzioni per l'hosting della funzione

[!INCLUDE [resource-group-note](./rg-notice.md)]

1. Assicurarsi di accedere al portale di Azure all'indirizzo [https://portal.azure.com](https://portal.azure.com?azure-portal=true) con il proprio account Azure.

1. Selezionare il pulsante **Crea una risorsa** nell'angolo in alto a sinistra del portale di Azure e quindi selezionare **Calcolo** > **App per le funzioni**.

1. Specificare le impostazioni dell'app per le funzioni come indicato nella tabella seguente.


    | Impostazione      | Valore consigliato  | Descrizione                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome app** | Nome globalmente univoco | Nome che identifica la nuova app per le funzioni. I caratteri validi sono `a-z`, `0-9` e `-`.  | 
    | **Sottoscrizione** | Sottoscrizione in uso | Sottoscrizione in cui viene creata la nuova app per le funzioni. | 
    | **Gruppo di risorse**|  [!INCLUDE [resource-group-name](./rg-name.md)] | Nome del gruppo di risorse in cui creare l'app per le funzioni.<br/><br/>Assicurarsi di selezionare **Usa esistente** e usare il gruppo di risorse creato nell'esercizio precedente. In questo modo, tutte le risorse create in questo modulo vengono tenute insieme. | 
    | **Sistema operativo** | Windows | Il sistema operativo che ospita l'app per le funzioni.  |
    | **Hosting** |   Piano a consumo | Piano di hosting che definisce come vengono allocate le risorse all'app per le funzioni. Nel **piano a consumo** predefinito le risorse vengono aggiunte dinamicamente in base alle esigenze delle funzioni. In questo hosting [serverless](https://azure.microsoft.com/overview/serverless-computing/) si paga solo per il periodo in cui le funzioni sono in esecuzione.   |
    | **Località** | Stati Uniti occidentali | Scegliere un'[area](https://azure.microsoft.com/regions/) nelle vicinanze o vicino ad altri servizi a cui accedono le funzioni.<br/><br/>Selezionare la stessa area usata durante la creazione dell'account dell'API Analisi del testo nell'esercizio precedente. |
    | **Account di archiviazione** |  Nome globalmente univoco |  Nome del nuovo account di archiviazione usato dall'app per le funzioni. I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. In questa finestra di dialogo il campo è popolato con un nome univoco derivato dal nome assegnato all'app. Tuttavia, è possibile usare un nome diverso o anche un account esistente. |

3. Selezionare **Crea** per effettuare il provisioning dell'app per le funzioni e distribuirla.

4. Selezionare l'icona di notifica nell'angolo in alto a destra del portale e attendere la visualizzazione di un messaggio **Distribuzione in corso** analogo al seguente.

![Notifica che la distribuzione dell'app per le funzioni è in corso](../media-draft/func-app-deploy-progress-small.PNG)

5. La distribuzione potrebbe richiedere tempo. Rimanere pertanto nell'hub di notifica e attendere la visualizzazione di un messaggio **Distribuzione completata** analogo al seguente.

![Notifica che la distribuzione dell'app per le funzioni è stata completata](../media-draft/func-app-text-analytics-deploy-success.png)

6. La procedura è stata completata. L'app per le funzioni è stata creata e distribuita. Selezionare **Vai alla risorsa** per visualizzare la nuova app per le funzioni.

>[!TIP]
>In caso di problemi nell'individuare le app per le funzioni nel portale, provare ad [aggiungere le app per le funzioni ai preferiti nel portale di Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function-to-hold-our-logic"></a>Creare una funzione per contenere la logica

Ora che si dispone di un'app per le funzioni, è il momento di creare una funzione. Una funzione viene attivata tramite un trigger. In questo modulo si userà un trigger Coda. Il runtime eseguirà il polling di una coda e avvierà questa funzione per l'elaborazione di un nuovo messaggio.

1. Espandere la nuova app per le funzioni e quindi passare il mouse sulla raccolta **Funzioni**. Selezionare il pulsante Aggiungi (**+**) quando viene visualizzato per avviare il processo di creazione della funzione.

![Animazione del segno più che viene visualizzata quando l'utente passa il mouse sulla voce di menu delle funzioni.](../media-draft/func-app-plus-hover-small.gif)

2. Nella pagina **Iniziare rapidamente** che viene ora visualizzata selezionare **Funzione personalizzata** per caricare l'elenco dei modelli di funzione disponibili. 

1. Selezionare **JavaScript** nella voce dell'elenco del modello **Trigger Coda**.

![Screenshot dei modelli di Funzioni di Azure con JavaScript selezionato nella voce Trigger Coda.](../media-draft/quickstart-select-queue-trigger.png)

1. Nella finestra di dialogo **Nuova funzione** che viene visualizzata immettere i valori seguenti.


|Proprietà  |Valore  |
|---------|---------|
|Linguaggio     |   **JavaScript**      |
|Nome     |   **discover-sentiment-function**      |
|Nome della coda     |   **new-feedback-q**      |
|Connessione dell'account di archiviazione        |  **AzureWebJobsDashboard**       |

![Screenshot dei modelli di Funzioni di Azure con JavaScript selezionato nella voce Trigger Coda.](../media-draft/new-function-dialog.png)

5. Selezionare **Crea** per avviare il processo di creazione della funzione.

1. Viene creata una funzione nel linguaggio prescelto usando il modello di funzione Trigger Coda. Anche se si implementerà la funzione in JavaScript in questo modulo, è possibile creare una funzione in uno qualsiasi dei [linguaggi supportati](https://docs.microsoft.com/azure/azure-functions/supported-languages).

Al termine del processo di creazione, l'editor di codice viene aperto nel portale e carica la pagina *index.js*. Questo file è il file di codice in cui si scrive la logica della funzione.

## <a name="try-it-out"></a>Provare questa operazione

È possibile testare quello che è stato fatto finora. Non è ancora stato scritto alcun codice, pertanto questo test ha lo scopo di verificare che quanto configurato finora venga eseguito correttamente.

1. Fare clic su **Esegui** nella parte superiore dell'editor di codice.

2. Osservare la scheda **Log** visualizzata nella parte inferiore della schermata. Se tutto funziona come previsto, verrà visualizzato un messaggio simile al seguente.
![Screenshot del messaggio di risposta di una chiamata alla funzione eseguita correttamente.](../media-draft/func-default-run.PNG)

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

>[!TIP]
>È possibile visualizzare index.js e function.json espandendo il menu **Visualizza file** a destra del pannello della funzione nel portale di Azure. 

Questo esercizio ha trattato principalmente la realizzazione dell'infrastruttura di Funzioni di Azure. È stata creata una funzione operativa ospitata in un'app per le funzioni che viene eseguita quando arriva un nuovo messaggio nella coda denominata [!INCLUDE [input-q](./q-name-input.md)]. Nel prossimo esercizio si procederà all'aggiunta del codice per chiamare un servizio cognitivo Microsoft per l'analisi del sentiment.