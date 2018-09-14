 Un'app di funzione fornisce un contesto per la gestione e l'esecuzione delle funzioni. È possibile creare un'app per le funzioni e quindi aggiungere una funzione a esso. 

## <a name="create-a-function-app-to-host-our-function"></a>Creare un'App per le funzioni per ospitare la funzione

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Selezionare il **crea una risorsa** pulsante trovato nell'angolo superiore sinistro del portale di Azure, quindi seleziona **Compute** > **App per le funzioni**.

1. Immettere le impostazioni dell'app funzioni come specificato nella tabella seguente.

    | Impostazione      | Valore consigliato  | Descrizione                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome app** | Nome globalmente univoco | Nome che identifica la nuova app per le funzioni. I caratteri validi sono `a-z`, `0-9` e `-`.  | 
    | **Sottoscrizione** | Sottoscrizione in uso | Sottoscrizione in cui viene creata questa nuova app per le funzioni. | 
    | **Gruppo di risorse**|  <rgn>[Nome gruppo di risorse di tipo sandbox]</rgn> | Nome del gruppo di risorse in cui creare app per le funzioni.<br/><br/>Assicurarsi di selezionare **Usa esistente** e usare il gruppo di risorse nell'ultimo esercizio. In questo modo, tutte le risorse sono apportati in questo modulo vengono mantenuti assieme. | 
    | **Sistema operativo** | Windows | Il sistema operativo che ospita l'app per le funzioni.  |
    | **Hosting** |   Piano a consumo | Piano di hosting che definisce come vengono allocate le risorse all'app per le funzioni. Nel **piano a consumo** predefinito le risorse vengono aggiunte dinamicamente in base alle esigenze delle funzioni. In questo hosting [senza server](https://azure.microsoft.com/overview/serverless-computing/) si paga solo per il periodo in cui le funzioni sono in esecuzione.   |
    | **Posizione** | Selezionare dall'elenco | Scegliere un'[area](https://azure.microsoft.com/regions/) nelle vicinanze o vicino ad altri servizi a cui accedono le funzioni.<br/><br/>Selezionare la stessa area usato durante la creazione dell'account di API traduzione testuale Analitica nell'esercizio precedente. |
    | **Account di archiviazione** |  Nome globalmente univoco |  Nome del nuovo account di archiviazione usato dall'app per le funzioni. I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. Questa finestra di dialogo consente di popolare il campo con un nome univoco derivato dal nome assegnato all'app. Tuttavia, è possibile usare un nome diverso o anche un account esistente. |

1. Selezionare **Crea** per effettuare il provisioning dell'app per le funzioni e distribuirla.

1. Selezionare l'icona di notifica nell'angolo superiore sinistro del portale e attendere un **distribuzione in corso** messaggio analogo al seguente.

1. Distribuzione può richiedere tempo. Quindi, rimanere nell'hub di notifica e attendere un **distribuzione ha avuto esito positivo** messaggio analogo al seguente.

1. La procedura è stata completata. Aver creato e distribuito l'app per le funzioni. Selezionare **Vai alla risorsa** per visualizzare la nuova app per le funzioni.

> [!TIP]
> In caso di problemi nell'individuare le app per le funzioni nel portale, provare ad [aggiungere le app per le funzioni ai preferiti nel portale di Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function-to-execute-our-logic"></a>Creare una funzione per eseguire la logica

Ora che abbiamo app per le funzioni, è possibile creare una funzione. Una funzione viene attivata tramite un trigger. In questo modulo, si userà un trigger della coda. Il runtime esegue il polling di una coda e questa funzione per l'elaborazione di un nuovo messaggio di avvio.

1. Espandere la nuova app di funzione e quindi passare il mouse sul **funzioni** raccolta. Selezionare Aggiungi (__+__) dopo che viene visualizzato per avviare il processo di creazione della funzione.

1. Nel **iniziare a usare rapidamente** che ora viene visualizzata la pagina Seleziona **funzione personalizzata**, che carica l'elenco dei modelli di funzione di disponibile.

1. Selezionare **JavaScript** nel **trigger della coda** voce dell'elenco di modelli.

![Modelli di schermata di funzioni di Azure con JavaScript selezionata all'immissione di trigger della coda.](../media/quickstart-select-queue-trigger.png)

4. Nel **nuova funzione** finestra di dialogo visualizzata, immettere i valori seguenti.

|Proprietà  |Valore  |
|---------|---------|
|Lingua     |   **JavaScript**      |
|Nome     |   **discover-sentiment-function**      |
|Nome della coda     |   **new-feedback-q**      |
|Connessione dell'account di archiviazione        |  **AzureWebJobsDashboard**       |

![Modelli di schermata di funzioni di Azure con JavaScript selezionata all'immissione di trigger della coda.](../media/new-function-dialog.png)

5. Selezionare **Create** per avviare il processo di creazione della funzione.

1. Viene creata una funzione nel linguaggio prescelto usando il modello di funzione Trigger della coda. Mentre la funzione JavaScript implementeremo in questo modulo, è possibile creare una funzione in una qualsiasi [lingua supportata](https://docs.microsoft.com/azure/azure-functions/supported-languages).

Una volta completato il processo di creazione, apertura dell'editor di codice nel portale e carichi i *index. js* pagina. Questo file è il file di codice in cui è scrivere la logica della funzione.

## <a name="try-it-out"></a>Provare il servizio

È possibile testare quello che abbiamo finora. È ancora stato scritto alcun codice, in modo che questo test consiste nell'assicurarsi che ciò che abbiamo configurato fino ad ora, viene eseguito.

1. Fare clic su **eseguire** nella parte superiore dell'editor del codice.

1. Osservare il **registri** scheda visualizzata nella parte inferiore della schermata. Se tutto funziona come previsto, si verrà visualizzato un messaggio analogo al seguente.
    ![Screenshot del messaggio di risposta di una chiamata alla funzione.](../media/func-default-run.PNG)

Il **eseguiti** pulsante avviato la funzione e passati *campionare i dati della coda*, il testo predefinito dal **Test** finestra di richiesta per la funzione.

State eseguite le operazioni. È stata aver aggiunto una funzione attivata dalla coda app per le funzioni e testate per verificare che funzioni come previsto. Verranno aggiunte altre funzionalità alla funzione nel prossimo esercizio.

È possibile esaminare brevemente la funzione di altro file, il *Function. JSON* file config. I dati di configurazione da questo file sono illustrati nel seguente elenco di JSON.

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

Come può notare, questa funzione ha un'associazione di trigger denominata **myQueueItem** di tipo `queueTrigger`. Quando arriva un nuovo messaggio nella coda è stato denominato **nuovi commenti e suggerimenti-q**, la funzione viene chiamata. Abbiamo fare riferimento al messaggio nuovo tramite il parametro di associazione myQueueItem. Le associazioni davvero ci occupiamo della parte del carico di lavoro per noi!

Nel passaggio successivo, si aggiungerà codice per chiamare il servizio API traduzione testuale Analitica.

> [!TIP]
> È possibile visualizzare index. js e Function. JSON, espandendo il **visualizzare file** menu a destra del Pannello di funzione nel portale di Azure.

Questo esercizio è ottenere la nostra infrastruttura di funzioni di Azure in posizione. È disponibile una funzione di lavoro ospitata in un'app di funzione che viene eseguito quando arriva un nuovo messaggio nel nostro coda che è stato denominato [!INCLUDE [input-q](./q-name-input.md)]. La parte più divertente inizia nel prossimo esercizio, quando si aggiunge codice per chiamare un servizio cognitivi Microsoft a scopo di analisi del sentiment.