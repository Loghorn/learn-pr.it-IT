Dopo aver creato un'app per le funzioni, si apprenderà come compilare, configurare ed eseguire una funzione di Azure.

### <a name="triggers"></a>Trigger

Le funzioni sono basate sugli eventi, ovvero vengono eseguite in risposta a un evento.

Il tipo di evento che avvia la funzione viene chiamato *trigger*. Si configura una funzione con un solo trigger.

 Azure supporta i trigger per i servizi seguenti.


|Servizio  |Descrizione del trigger  |
|---------|---------|
|Archiviazione BLOB     |  Avvia una funzione quando viene rilevato un BLOB nuovo o aggiornato.       |
|Cosmos DB     |  Avvia una funzione quando vengono rilevati inserimenti e aggiornamenti.      |
|Griglia di eventi     |   Avvia una funzione quando viene ricevuto un evento da Griglia di eventi.       |
|HTTP     |   Avvia una funzione con una richiesta HTTP.      |
|Eventi di Microsoft Graph     |  Avvia una funzione in risposta a un webhook in ingresso di Microsoft Graph. Ogni istanza del trigger può rispondere a un tipo di risorsa di Microsoft Graph.       |
|Archiviazione code     |    Avvia una funzione quando viene ricevuto un nuovo elemento in una coda. Il messaggio in coda viene inviato come input alla funzione.      |
|Bus di servizio     |  Avvia una funzione in risposta ai messaggi di una coda del bus di servizio.       |
|Timer     |  Avvia una funzione in una pianificazione.       |
|Webhook     |  Avvia una funzione con una richiesta webhook.       |

### <a name="bindings"></a>Associazioni

Le associazioni delle funzioni di Azure sono una modalità dichiarativa per connettere dati e servizi alla funzione. Non è necessario scrivere codice nella funzione per connettersi alle origini dati e gestire le connessioni. La piattaforma risolve automaticamente questa complessità. Un'associazione ha una direzione. Il codice legge i dati dalle associazioni di *input* e scrive i dati nelle associazioni di *output*. Ecco un esempio.

Per leggere dati dall'archiviazione BLOB, si configura un'associazione di input di tipo *blob*. Per scrivere un messaggio all'archiviazione code, si configura un'associazione di output di tipo *queue*.

Per altre informazioni sulle associazioni e i trigger supportati, consultare la [documentazione di Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings).

### <a name="a-sample-trigger-and-binding-functionjson"></a>Un trigger e un'associazione di esempio (function.json)

Il file JSON seguente è una definizione di esempio di un trigger e di un'associazione per una funzione.

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

Come indicato in precedenza, ogni associazione ha una direzione che definisce se si tratta di un'associazione di input o di output. I trigger sono sempre associazioni di input. L'esempio precedente illustra una funzione attivata da un messaggio che viene aggiunto a una coda denominata **myqueue-items**. Il valore restituito della funzione viene quindi inviato alla tabella **outTable** nell'archiviazione tabelle di Azure.

## <a name="creating-a-function-in-the-azure-portal"></a>Creazione di una funzione nel portale di Azure

Azure offre modelli preconfigurati per gli scenari di funzioni di Azure più comuni.

### <a name="quickstart-templates"></a>Modelli di avvio rapido

Per aggiungere una funzione di Azure, è necessario selezionare un'app per le funzioni. L'app per le funzioni può essere identificata dalla saetta presente nell'icona con parentesi a punta.  
![Screenshot che illustra un'app per le funzioni selezionata in un gruppo di risorse.](../images/5-function-icon.png)

Quando si aggiunge la prima funzione di Azure, viene visualizzata la schermata Avvio rapido. Questa schermata consente di scegliere un tipo di trigger e un linguaggio di programmazione. In base alle selezioni effettuate, Azure genera quindi automaticamente il codice e la configurazione della funzione. 
 
![Avvio rapido della funzione](../images/5-quickstart-form.png)

### <a name="function-templates"></a>Modelli di funzione

La selezione dei modelli non è limitata a quelli visualizzati nell'Avvio rapido. È possibile scegliere tra più di 30 modelli diversi. È possibile accedere alla schermata di elenco dei modelli durante la creazione delle funzioni successive o selezionando l'opzione **Funzione personalizzata** nella schermata Avvio rapido.  
![Screenshot dell'interfaccia utente di avvio rapido di Funzioni di Azure che consente di essere subito operativi usando una funzione preconfezionata.](../images/5-template-list.png)

## <a name="navigating-to-your-function-and-files"></a>Passaggio alla funzione e ai file

Quando si crea una funzione da un modello, vengono creati numerosi file. Ad esempio, se si è scelto di usare l'Avvio rapido per Webhook + API con JavaScript, i file generati saranno un file di configurazione, **function.json**, e un file del codice sorgente, **index.js**. Le funzioni create in un'app per le funzioni vengono visualizzate sotto la voce di menu Funzioni nel portale delle app per le funzioni.

Quando si seleziona una funzione nell'app per le funzioni, un editor di codice si apre e visualizza il codice relativo alla funzione, come illustrato nello screenshot seguente.

![Screenshot che illustra l'interfaccia di selezione del modello di funzione, in cui sono elencati i modelli che si possono usare per velocizzare lo sviluppo delle funzioni.](../images/5-file-navigation.png)

Come si può vedere nello screenshot precedente, a destra è visualizzato un menu a comparsa che include una scheda per la **visualizzazione dei file**. Selezionando la scheda, viene visualizzata la struttura di file che compone la funzione.  

## <a name="execution-options-for-testing-your-azure-function"></a>Opzioni di esecuzione per testare la funzione di Azure

Dopo aver creato una funzione, è opportuno testarla. Esistono due approcci: l'esecuzione manuale e il test nel portale di Azure.

### <a name="manual-execution-of-a-function"></a>Esecuzione manuale di una funzione

È possibile avviare una funzione attivando manualmente il trigger configurato. Se ad esempio si usa un oggetto HTTPTrigger, è possibile usare uno strumento come Postman o cURL per avviare una richiesta HTTP all'URL dell'endpoint della funzione.  

![Screenshot parziale dell'interfaccia dell'applicazione Postman con un URL di funzione digitato nel campo url e il corpo della richiesta compilato con un esempio di corpo. ](../images/5-postman-execution.png)

Per ottenere l'URL dell'endpoint, selezionare Trigger HTTP dal riquadro di spostamento a sinistra e quindi selezionare il pulsante **Recupera URL della funzione**.  

![Screenshot del portale delle app per le funzioni, con una funzione selezionata e l'azione *Recupera URL della funzione* selezionata nella parte superiore della pagina.](../images/5-get-function-url.png)

### <a name="testing-a-function-in-the-azure-portal"></a>Esecuzione del test di una funzione nel portale di Azure

Il portale offre un altro modo pratico per testare le funzioni. Sul lato destro della finestra del codice è presente un menu di spostamento con schede a comparsa. In questo menu è presente la scheda **Test**. Se si espande il menu e si seleziona questa scheda, è possibile eseguire la funzione e visualizzarne il risultato. In linea con lo scenario di HTTPTrigger, è possibile impostare il metodo HTTP e aggiungere i parametri di stringa di query e le intestazioni HTTP alla richiesta. È anche possibile modificare il corpo della richiesta per testare altri scenari. Nell'esempio seguente il test è configurato come richiesta HTTP POST con un corpo della richiesta che ha un solo parametro denominato *name*.
 
![Screenshot della finestra Test del portale delle app per le funzioni che illustra una richiesta HTTP POST e un esempio di corpo della richiesta. Il riquadro di output contiene il risultato di una chiamata di funzione con le parole "Hello Azure".](../images/5-portal-execution.png)

Quando si fa clic su **Esegui** in questa finestra di test, i risultati vengono visualizzati nella finestra di output insieme a un codice di stato. 

## <a name="monitoring-an-azure-function"></a>Monitoraggio di una funzione di Azure

La possibilità di registrare i messaggi e monitorare le funzioni è fondamentale durante lo sviluppo e nell'ambiente di produzione. Il portale di Azure offre un dashboard di monitoraggio, nonché un modo per esaminare i log e le eccezioni di esecuzione derivanti dalle funzioni di Azure.

### <a name="log-window"></a>Finestra registro

È possibile aggiungere anche istruzioni di registrazione alla funzione. Queste istruzioni vengono visualizzate nella finestra registro della funzione. La finestra registro si trova nel menu a schede a comparsa nella parte inferiore della finestra del codice. Il seguente frammento di codice JavaScript illustra come registrare un messaggio usando il metodo `context.log`.

```javascript
  context.log('Enter your logging statement here');
```  

![Screenshot della sezione dell'editor di codice dell'interfaccia utente di Funzioni di Azure. Viene inoltre evidenziata una riga di codice che usa il metodo context.log() per registrare un messaggio alla finestra registro.](../images/5-log-window.png)

### <a name="errors-and-warnings-window"></a>Finestra degli errori e degli avvisi

La scheda della finestra degli errori e degli avvisi si trova nello stesso menu a comparsa della finestra registro. In questa finestra vengono visualizzati gli errori e gli avvisi di compilazione all'interno del codice. Poiché JavaScript è un linguaggio dinamico e interpretato, l'immagine seguente illustra un errore di compilazione in una funzione C#.  

![Screenshot della sezione dell'editor di codice dell'interfaccia utente di Funzioni di Azure, con la finestra **Registri** evidenziata nella parte inferiore della schermata.](../images/5-errors-window.png)

### <a name="monitoring-dashboard"></a>Dashboard di monitoraggio

Nel menu di spostamento dell'app per le funzioni, quando si espande il nodo della funzione, viene visualizzata la voce di menu **Monitor** (Monitora). Il dashboard di monitoraggio offre un modo rapido per visualizzare la cronologia delle esecuzioni della funzione. Questa visualizzazione mostra il timestamp, il codice di risultato, la durata e l'ID operazione, nonché un messaggio che indica se la funzione è stata completata o meno correttamente. I dati vengono popolati usando Azure Application Insights.  

![Screenshot del dashboard di monitoraggio avviato usando la voce di menu **Monitor** della funzione che visualizza un elenco di chiamate riuscite e non riuscite alla funzione.](../images/5-monitor-function.png)
