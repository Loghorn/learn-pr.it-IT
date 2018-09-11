Dopo aver creato un'app per le funzioni, verrà spiegato come compilare, configurare ed eseguire una funzione.

### <a name="triggers"></a>Trigger

Le funzioni sono basate sugli eventi, ovvero vengono eseguite in risposta a un evento.

Il tipo di evento che avvia la funzione viene chiamato *trigger*. La funzione deve essere configurata con un solo trigger.

Azure supporta i trigger per i servizi seguenti.

| Servizio                 | Descrizione del trigger  |
|-------------------------|---------|
| Archiviazione BLOB            | Avvia una funzione quando viene rilevato un BLOB nuovo o aggiornato.       |
| Cosmos DB               | Avvia una funzione quando vengono rilevati inserimenti e aggiornamenti.      |
| Griglia di eventi              | Avvia una funzione quando viene ricevuto un evento da Griglia di eventi.       |
| HTTP                    | Avvia una funzione con una richiesta HTTP.      |
| Eventi di Microsoft Graph  | Avvia una funzione in risposta a un webhook in ingresso di Microsoft Graph. Ogni istanza del trigger può rispondere a un tipo di risorsa di Microsoft Graph.       |
| Archiviazione code           | Avvia una funzione quando viene ricevuto un nuovo elemento in una coda. Il messaggio in coda viene inviato come input alla funzione.      |
| Bus di servizio             | Avvia una funzione in risposta ai messaggi di una coda del bus di servizio.       |
| Timer                   | Avvia una funzione in una pianificazione.       |
| Webhook                | Avvia una funzione con una richiesta webhook.       |

### <a name="bindings"></a>Associazioni

Le associazioni sono una modalità dichiarativa per connettere dati e servizi alla funzione. Sono in grado di parlare a servizi diversi e questo significa che non è necessario scrivere codice nella funzione per connettersi alle origini dati e gestire le connessioni. La piattaforma risolve automaticamente questa complessità come parte del codice di associazione. Ogni associazione ha una direzione: il codice legge i dati dalle associazioni di *input* e scrive i dati nelle associazioni di *output*. Ogni funzione può avere zero o più associazioni per gestire i dati di input e output elaborati dalla funzione.

> [!NOTE]
> Tecnicamente, un trigger è un tipo speciale di associazione di input che in più consente di avviare l'esecuzione.

Azure offre [numerose associazioni](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings) per connettersi a diversi servizi di archiviazione e messaggistica.

### <a name="a-sample-binding-definition"></a>Un esempio di definizione di associazione

L'esempio che segue illustra la configurazione di una funzione con un'associazione di input (trigger) e un'associazione di output. Si supponga di voler leggere i dati dall'archiviazione BLOB, elaborarli nella funzione e quindi scrivere un messaggio a una coda. Sarà necessario configurare un'_associazione di input_ di tipo *blob* e un'_associazione di output_ di tipo *queue*.

Le associazioni possono essere definite nel portale di Azure e vengono archiviate come file JSON che possono anche essere modificati direttamente. Il file JSON seguente è una definizione di esempio di un trigger e di un'associazione per una funzione.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Questo esempio illustra una funzione attivata da un messaggio che viene aggiunto a una coda denominata **myqueue-items**. Il valore restituito della funzione viene quindi inviato alla tabella **outTable** nell'archiviazione tabelle di Azure. Questo è un esempio molto semplice, in cui è possibile modificare l'output in modo che sia un messaggio di posta elettronica con un'associazione SendGrid, inserire un evento in un bus di servizio per inviare notifiche a un altro componente dell'architettura o persino avere più associazioni di output per effettuare il push dei dati a vari servizi.

## <a name="creating-a-function-in-the-azure-portal"></a>Creazione di una funzione nel portale di Azure

Azure offre diversi modelli di funzione già pronti per gli scenari comuni.

### <a name="quickstart-templates"></a>Modelli di avvio rapido

Quando si aggiunge la prima funzione, viene visualizzata la schermata Avvio rapido. Questa schermata consente di scegliere un tipo di trigger (HTTP, Timer o Dati) e un linguaggio di programmazione (C#, JavaScript, F# o Java). Quindi, in base alle selezioni effettuate, Azure genera il codice della funzione e la configurazione con un codice di esempio usato per visualizzare i dati di input ricevuti nel registro. 
 
### <a name="custom-function-templates"></a>Modelli di funzione personalizzati

La selezione dei modelli di Avvio rapido consente di accedere facilmente agli scenari più comuni. Tuttavia, Azure offre altri 30 modelli con cui è possibile iniziare e che possono essere selezionati dalla schermata di elenco dei modelli durante la creazione delle funzioni successive o usando l'opzione **Funzione personalizzata** nella schermata Avvio rapido.

- Trigger HTTP con C#, F# o JavaScript
- Trigger timer con C#, F# o JavaScript
- Trigger coda con C#, F# o JavaScript
- Trigger coda bus di servizio con C#, F# o JavaScript
- Trigger Cosmos DB con C#, F# o JavaScript
- Hub IoT (hub eventi) con C#, F# o JavaScript
- ... e molti altri

## <a name="navigating-to-your-function-and-files"></a>Passaggio alla funzione e ai file

Quando si crea una funzione da un modello, vengono creati numerosi file. Ad esempio, se si è scelto di usare l'Avvio rapido per Webhook + API con JavaScript, i file generati saranno un file di configurazione, **function.json**, e un file del codice sorgente, **index.js**. Le funzioni create in un'app per le funzioni vengono visualizzate sotto la voce di menu **Funzioni** nel portale delle app per le funzioni.

Quando si seleziona una funzione nell'app per le funzioni, un editor di codice si apre e visualizza il codice relativo alla funzione, come illustrato nello screenshot seguente.

![Interfaccia di selezione del modello di funzione, in cui sono elencati i modelli che si possono usare per velocizzare lo sviluppo delle funzioni.](../media-draft/4-file-navigation.png)

Come si può vedere nello screenshot precedente, a destra è visualizzato un menu a comparsa che include una scheda per la **visualizzazione dei file**. Selezionando la scheda, viene visualizzata la struttura di file che compone la funzione.  

## <a name="testing-your-azure-function"></a>Test della funzione di Azure

Dopo aver creato una funzione, è opportuno testarla. Esistono due approcci: l'esecuzione manuale e il test nel portale di Azure.

### <a name="manual-execution"></a>Esecuzione manuale

È possibile avviare una funzione attivando manualmente il trigger configurato. Se ad esempio si usa un trigger HTTP, è possibile usare uno strumento come Postman o cURL per avviare una richiesta HTTP all'URL dell'endpoint della funzione, disponibile nella definizione del trigger HTTP (**Recupera URL della funzione**).  

### <a name="testing-in-the-azure-portal"></a>Test nel portale di Azure

Il portale offre un altro modo pratico per testare le funzioni. Sul lato destro della finestra del codice è presente un menu di spostamento con schede a comparsa. In questo menu è presente la scheda **Test**. Se si espande il menu e si seleziona questa scheda, è possibile eseguire la funzione e visualizzarne il risultato. Quando si fa clic su **Esegui** in questa finestra di test, i risultati vengono visualizzati nella finestra di output insieme a un codice di stato. 

## <a name="monitoring-dashboard"></a>Dashboard di monitoraggio

La possibilità di monitorare le funzioni è fondamentale durante lo sviluppo e nell'ambiente di produzione. Il portale di Azure offre un dashboard di monitoraggio disponibile se si attiva l'integrazione di Application Insights. Nel menu di spostamento dell'app per le funzioni, quando si espande il nodo della funzione, viene visualizzata la voce di menu **Monitor**. Il dashboard di monitoraggio offre un modo rapido per visualizzare la cronologia delle esecuzioni della funzione e visualizza il timestamp, il codice di risultato, la durata e l'ID operazione inseriti da Application Insights.

![Screenshot del dashboard di monitoraggio avviato usando la voce di menu **Monitor** della funzione che visualizza un elenco di chiamate riuscite e non riuscite alla funzione.](../media-draft/4-monitor-function.png)

## <a name="streaming-log-window"></a>Flusso della finestra registro

È anche possibile aggiungere alla funzione istruzioni di registrazione per il debug nel portale di Azure. I metodi chiamati per ogni lingua vengono passati a un oggetto di "registrazione" che può essere usato per registrare le informazioni nella finestra registro, che si trova in un menu a schede a comparsa nella parte inferiore della finestra del codice. 

Il seguente frammento di codice JavaScript illustra come registrare un messaggio usando il metodo `context.log` (l'oggetto `context` viene passato al gestore).

```javascript
  context.log('Enter your logging statement here');
```  

Si può eseguire la stessa operazione in C# usando il metodo `log.Info`. In questo caso l'oggetto `log` viene passato al metodo C# che elabora la funzione.

```csharp
  log.Info("Enter your logging statement here");
```

### <a name="errors-and-warnings-window"></a>Finestra degli errori e degli avvisi

La scheda della finestra degli errori e degli avvisi si trova nello stesso menu a comparsa della finestra registro. In questa finestra vengono visualizzati gli errori e gli avvisi di compilazione all'interno del codice.
