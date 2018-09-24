In un'applicazione distribuita alcuni messaggi devono essere inviati a un singolo componente destinatario. Altri messaggi devono raggiungere più di una destinazione.

Si esaminerà ora cosa accade quando un utente annulla un ordine di una pizza. Si tratta di uno scenario un po' diverso rispetto all'esecuzione dell'ordine iniziale. In tal caso, era necessario attendere l'elaborazione del pagamento per inviare l'ordine per i passaggi successivi (ad esempio la preparazione e la cottura nel negozio locale). Per l'operazione di annullamento, è invece necessario inviare una notifica sia al negozio locale *sia* al componente di elaborazione dei pagamenti. Questo approccio riduce al minimo le probabilità di sprecare ingredienti o il tempo dell'autista addetto alla consegna.

Per consentire a più componenti di ricevere lo stesso messaggio, si userà un argomento del bus di servizio di Azure.

## <a name="code-with-topics-vs-code-with-queues"></a>Codice con argomenti e codice con code

Se si vuole che ogni messaggio inviato all'argomento venga recapitato a tutti i componenti che hanno eseguito la sottoscrizione, è necessario usare gli argomenti. La scrittura di codice che usa gli argomenti rappresenta un modo per sostituire le code. Si usa lo stesso pacchetto NuGet **Microsoft.Azure.ServiceBus**, si configurano le stringhe di connessione e si usano modelli di programmazione asincrona.

Tuttavia, si usa la classe `TopicClient` invece della classe `QueueClient` per inviare i messaggi e la classe `SubscriptionClient` per ricevere i messaggi.

## <a name="setting-filters-on-subscriptions"></a>Impostazione di filtri nelle sottoscrizioni

Se si vuole controllare quali messaggi inviati all'argomento vengono recapitati a sottoscrizioni specifiche, è possibile inserire filtri in ogni sottoscrizione dell'argomento. Nell'applicazione per la pizza, ad esempio, i negozi eseguono applicazioni Universal Windows Platform (UWP). Ogni negozio può sottoscrivere l'argomento "OrderCancellation", ma applicare un filtro per il proprio valore di StoreId. In questo modo, si risparmia larghezza di banda Internet in quanto non vengono inviati messaggi non necessari a negozi distanti. Nel frattempo, il componente di elaborazione dei pagamenti sottoscrive tutti i messaggi di annullamento.

I filtri possono essere di tre tipi:

- **Filtri booleani.** `TrueFilter` garantisce che tutti i messaggi inviati all'argomento vengano recapitati alla sottoscrizione corrente. `FalseFilter` garantisce che nessuno dei messaggi vengano recapitati alla sottoscrizione corrente. (Questa scelta blocca o disattiva a tutti gli effetti la sottoscrizione.)
- **Filtri SQL.** Un filtro SQL specifica una condizione usando la stessa sintassi di una clausola `WHERE` in una query SQL. Solo i messaggi che restituiscono `True` quando valutati rispetto alla sottoscrizione vengono recapitati ai sottoscrittori.
- **Filtri di correlazione.** Un filtro di correlazione contiene un set di condizioni che vengono confrontate con le proprietà di ogni messaggio. Se la proprietà nel filtro e la proprietà del messaggio hanno lo stesso valore, si considera una corrispondenza.

Per il filtro StoreId, *sarebbe possibile* usare un filtro SQL. Questi filtri sono i più flessibili, ma anche i più impegnativi dal punto di vista del calcolo e possono rallentare la velocità effettiva del bus di servizio. In questo caso è preferibile scegliere un filtro di correlazione. 

## <a name="topicclient-example"></a>Esempio TopicClient

In qualsiasi componente di invio o di ricezione è necessario aggiungere le istruzioni `using` seguenti a qualsiasi file di codice che chiama un argomento del bus di servizio:

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

Per inviare un messaggio, iniziare creando un nuovo oggetto `TopicClient` e passare la stringa di connessione e il nome dell'argomento:

```C#
topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
```

È possibile inviare un messaggio all'argomento, chiamando il metodo `TopicClient.SendAsync()` e passando il messaggio. Come nel caso delle code, il messaggio deve essere in formato di stringa con codifica UTF8:

```C#
string message = "Cancel! I can't believe you use canned mushrooms!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await topicClient.SendAsync(encodedMessage);
```

Per ricevere messaggi, è necessario creare un oggetto `SubscriptionClient` e non un oggetto `TopicClient` e passare la stringa di connessione, il nome dell'argomento **e** il nome della sottoscrizione:

```C#
subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
```

Registrare quindi un gestore di messaggi, ovvero il metodo asincrono nel codice che elabora il messaggio recuperato.

```C#
subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

Nel gestore di messaggi chiamare il metodo `SubscriptionClient.CompleteAsync()` per rimuovere il messaggio dalla coda:

```C#
await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
```