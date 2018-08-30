Ora che abbiamo creato un'app per le funzioni, scopriamo come creare, configurare ed eseguire una funzione di Azure.

### <a name="triggers"></a>Trigger
Le funzioni di Azure sono basate su eventi e vengono eseguite in risposta a un evento, ad esempio la ricezione di una richiesta HTTP o di un messaggio da aggiungere a una coda. 

Il tipo di evento che avvia la funzione viene chiamato trigger. Le funzioni di Azure richiedono che venga configurato almeno un trigger; in caso contrario, non c'è modo di eseguire la funzione.

 Azure supporta un'ampia gamma di trigger, tra cui:
* HTTPTrigger
* TimerTrigger
* Webhook di GitHub
* CosmosDBTrigger
* BlobTrigger
* QueueTrigger
* EventHubTrigger
* ServiceBusQueueTrigger
* ServiceBusTopicTrigger

### <a name="bindings"></a>Associazioni
Le associazioni delle funzioni di Azure sono una modalità dichiarativa per connettere i dati alla funzione a livello di programmazione.

Le associazioni sono la connessione ai servizi dati. Una funzione di Azure con zero, una o più associazioni consente di accedere a più origini dati. Queste associazioni sono definite anche associazioni di input o output o anche associazioni di lettura o scrittura.

Si supponga di disporre di un oggetto QueueTrigger che avvia una funzione quando un BLOB viene aggiunto a una coda. Un'associazione BLOB di input viene usata per connettersi ad Archiviazione BLOB di Azure. 

Le associazioni di output vengono usate per archiviare i dati. Queste associazioni inviano ad esempio il valore restituito della funzione all'archiviazione tabelle di Azure.

Un elenco completo delle associazioni disponibili è incluso nella [documentazione di Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings).

### <a name="a-sample-trigger-and-binding-functionjson"></a>Un trigger e un'associazione di esempio (function.json)
Questa è una definizione di esempio di un trigger e di un'associazione per una funzione. Si noti che a seconda del tipo di associazione che si descrive, i valori di proprietà da impostare sono diversi. Ogni associazione presenta anche una direzione che definisce se si tratta di un'associazione di input o output. I trigger sono sempre associazioni di input. Questo esempio illustra una funzione attivata da un messaggio che viene aggiunto a una coda denominata **myqueue-items**. Il valore restituito della funzione viene quindi inviato alla tabella **outTable** nell'archiviazione tabelle di Azure.

```javascript
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
## <a name="premade-functions-and-templates"></a>Funzioni e modelli predefiniti
Azure offre modelli preconfigurati per gli scenari di funzioni di Azure più comuni.

### <a name="quickstart-templates"></a>Modelli di Guida introduttiva
Per aggiungere una funzione di Azure, è necessario selezionare un'app per le funzioni. L'app per le funzioni può essere identificata dalla saetta presente nell'icona con parentesi a punta.  
![Icona di funzione](../images/5-function-icon.png)

Quando si aggiunge la prima funzione di Azure, viene visualizzata la schermata di avvio rapido. Questa schermata consente selezionare il tipo di trigger e il linguaggio di programmazione desiderati. In base alle selezioni effettuate, Azure genera quindi automaticamente il codice della funzione e la configurazione.  
![Avvio rapido della funzione](../images/5-quickstart-form.png)

### <a name="function-templates"></a>Modelli di funzione
La selezione di modelli non è limitata a quelli disponibili nell'avvio rapido. È possibile scegliere tra oltre 30 modelli diversi. È possibile accedere alla schermata di elenco dei modelli durante la creazione delle funzioni successive o selezionando l'opzione **Funzione personalizzata** nella schermata di avvio rapido.  
![Modelli di funzione](../images/5-template-list.png)

## <a name="navigating-to-your-function-and-files"></a>Passaggio alla funzione e ai file
Dopo aver creato una funzione da un modello, vengono creati numerosi file. Si supponga che avere scelto di usare l'Avvio rapito per Webhook + API usando JavaScript. I file generati sono un file di configurazione denominato **function.json** e un file del codice sorgente denominato **index.js**. Quando si accede all'app per le funzioni, viene visualizzata una struttura di menu che visualizza le funzioni che sono state create nell'app. È anche in questa voce di menu **Funzioni** che si possono aggiungere altre funzioni all'app per le funzioni (un'icona con un segno + appare quando si passa il puntatore sulla voce di menu).  
Pulsante ![Aggiungi funzione](../images/5-function-add-button.png) 

Nel caso dell'Avvio rapido summenzionato, quando si espande **Funzioni** nella visualizzazione albero, si vedrà una funzione con il nome predefinito **HttpTriggerJS1** (indicato anche dall'icona di funzione). La selezione di questa funzione aprirà una finestra del codice e visualizzerà il file di origine **index.js**. Sul lato destro della finestra del codice è presente un menu a comparsa che include una scheda per **visualizzare i file**. Se si seleziona questa scheda, viene visualizzata la struttura del file (riprodotta nell'archivio) che costituisce la funzione.  
![Modelli di funzione](../images/5-file-navigation.png)

## <a name="execution-options-for-testing-your-azure-function"></a>Opzioni di esecuzione per testare la funzione di Azure
Dopo avere creato una funzione di Azure, è necessario decidere come eseguirne il test. Esistono due approcci: l'esecuzione manuale e il test nel portale di Azure.

### <a name="manual-execution-of-a-function"></a>Esecuzione manuale di una funzione
È possibile avviare l'esecuzione di una funzione attivando manualmente il trigger configurato. Se ad esempio si usa un oggetto HTTPTrigger, è possibile usare uno strumento come Postman o cURL per avviare una richiesta HTTP all'URL dell'endpoint della funzione.  
![Esecuzione in Postman di una funzione](../images/5-postman-execution.png) È possibile ottenere l'URL dell'endpoint selezionando l'oggetto HTTPTrigger dal riquadro di navigazione di sinistra e quindi selezionando il pulsante **Get function URL** (Ottieni URL di funzione).  
![Get function URL](../images/5-get-function-url.png) (Ottieni URL di funzione)

### <a name="testing-a-function-in-the-azure-portal"></a>Esecuzione del test di una funzione nel portale di Azure
Il portale di Azure offre un altro modo pratico per testare le funzioni. Sul lato destro della finestra del codice è presente un menu di spostamento a schede a comparsa. In questo menu è presente la scheda **Test**. Se si espande il menu e si seleziona questa scheda, è possibile eseguire la funzione e visualizzarne il risultato. In linea con lo scenario di HTTPTrigger, è possibile impostare il metodo HTTP e aggiungere i parametri di stringa di query e le intestazioni HTTP alla richiesta. È anche possibile modificare il corpo della richiesta per testare altri scenari. La finestra di output mostra il risultato della funzione.  
![Esecuzione di una funzione nel portale](../images/5-portal-execution.png)

## <a name="monitoring-an-azure-function"></a>Monitoraggio di una funzione di Azure
Durante lo sviluppo di una funzione di Azure, la possibilità di registrare i messaggi e di determinare gli scenari di eccezione è essenziale per garantire che le funzioni create siano pronte per la produzione. Questa possibilità è importante in uno scenario di produzione, quando si esegue il monitoraggio per risalire all'origine di un difetto. Il portale di Azure offre un'interfaccia utente che include un dashboard di monitoraggio, nonché un modo per esaminare i log e le eccezioni di esecuzione derivanti dalle funzioni di Azure create.

### <a name="monitoring-dashboard"></a>Dashboard di monitoraggio
Nel menu di spostamento dell'app per le funzioni, quando si espande il nodo della funzione, viene visualizzata la voce di menu **Monitor** (Monitora). Il dashboard di monitoraggio offre un modo rapido per visualizzare la cronologia delle esecuzioni della funzione. Questa visualizzazione mostra il timestamp, il codice di risultato, la durata e l'ID operazione, nonché un messaggio che indica se la funzione è stata completata o meno correttamente. I dati vengono popolati tramite Azure Application Insights.  
![Monitoraggio della funzione](../images/5-monitor-function.png)

### <a name="log-window"></a>Finestra registro
È possibile aggiungere anche istruzioni di registrazione alla funzione. Queste istruzioni vengono visualizzate nella finestra registro della funzione. La finestra registro si trova nel menu a schede a comparsa nella parte inferiore della finestra del codice. Quando si usa una funzione basata su JavaScript, è necessario aggiungere un'istruzione di registrazione usando la sintassi seguente:
```javascript
  context.log('Enter your logging statement here');
```  
![Finestra registro](../images/5-log-window.png)

### <a name="errors-and-warnings-window"></a>Finestra degli errori e degli avvisi
La scheda della finestra degli errori e degli avvisi si trova nello stesso menu a comparsa della finestra registro. In questa finestra vengono visualizzati gli errori e gli avvisi di compilazione all'interno del codice. Poiché JavaScript è un linguaggio dinamico e interpretato, l'immagine seguente mostra un errore di compilazione in una funzione C#.  
![Finestra degli errori e degli avvisi](../images/5-errors-window.png)
