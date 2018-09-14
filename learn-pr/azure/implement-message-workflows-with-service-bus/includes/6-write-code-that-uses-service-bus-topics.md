In un'applicazione distribuita, alcuni messaggi devono essere inviati a un singolo componente destinatario. Altri messaggi devono raggiungere più di una destinazione.

Si esaminerà ora cosa accade quando un utente annulla un ordine di una pizza. Si tratta di uno scenario un po' diverso rispetto all'esecuzione dell'ordine iniziale. In tal caso, era necessario attendere l'elaborazione del pagamento per inviare l'ordine per i passaggi successivi (ad esempio la preparazione e la cottura nel negozio locale). Per l'operazione di annullamento, è invece necessario inviare una notifica sia al negozio locale *sia* al componente di elaborazione dei pagamenti. Questo approccio riduce al minimo le probabilità di sprecare ingredienti o il tempo dell'autista addetto alla consegna.

Per consentire a più componenti di ricevere lo stesso messaggio, si userà un argomento del bus di servizio di Azure.

## <a name="how-code-that-uses-topics-differs-from-queues"></a>Differenze del codice che usa argomenti rispetto alle code

Se si vuole che ogni messaggio inviato all'argomento venga recapitato a tutti i componenti che hanno eseguito la sottoscrizione, è necessario usare gli argomenti. Scrittura di codice che usa argomenti è un modo per sostituire le code. Si usa lo stesso pacchetto NuGet **Microsoft.Azure.ServiceBus**, si configurano le stringhe di connessione e si usano modelli di programmazione asincrona.

Tuttavia, si usa la classe `TopicClient` invece della classe `QueueClient` per inviare i messaggi e la classe `SubscriptionClient` per ricevere i messaggi.

## <a name="setting-filters-on-subscriptions"></a>Impostazione di filtri nelle sottoscrizioni

Se si vuole controllare quali messaggi inviati all'argomento vengono recapitati a sottoscrizioni specifiche, è possibile inserire filtri in ogni sottoscrizione dell'argomento. Nell'applicazione di pizza, ad esempio, i negozi sono in esecuzione applicazioni Universal Windows Platform (UWP). Ogni negozio può sottoscrivere l'argomento "OrderCancellation", ma applicare un filtro per il proprio valore di StoreId. In questo modo, si risparmia larghezza di banda Internet in quanto non vengono inviati messaggi non necessari a negozi distanti. Nel frattempo, il componente di elaborazione dei pagamenti sottoscrive tutti i messaggi di annullamento.

I filtri possono essere di tre tipi:

- **Filtri booleani.** `TrueFilter` garantisce che tutti i messaggi inviati all'argomento vengano recapitati alla sottoscrizione corrente. Il `FalseFilter` garantisce che nessuno dei messaggi vengano recapitati alla sottoscrizione corrente. (Questa in modo efficace i blocchi o disattiva la sottoscrizione).
- **Filtri SQL.** Un filtro SQL specifica una condizione usando la stessa sintassi di una clausola `WHERE` in una query SQL. Solo i messaggi che restituiscono `True` se valutata rispetto a questa sottoscrizione verrà recapitato ai sottoscrittori.
- **Filtri di correlazione.** Un filtro di correlazione contiene un set di condizioni che vengono confrontate con le proprietà di ogni messaggio. Se la proprietà nel filtro e la proprietà del messaggio hanno lo stesso valore, si considera una corrispondenza.

Per il filtro StoreId, *sarebbe possibile* usare un filtro SQL. Questi filtri sono le più flessibili, ma sono anche i più dispendiosa e può rallentare la velocità effettiva del Bus di servizio. In questo caso, si sceglie invece un filtro di correlazione. 

## <a name="topicclient-example"></a>Esempio TopicClient

In qualsiasi invio o ricezione componente, è necessario aggiungere quanto segue `using` istruzioni ai file di codice che chiama un argomento del Bus di servizio:

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

Per inviare un messaggio, iniziare creando un nuovo oggetto `TopicClient` e passare la stringa di connessione e il nome dell'argomento:

```C#
topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
```

È possibile inviare un messaggio all'argomento chiamando il `TopicClient.SendAsync()` metodo e passando il messaggio. Come con le code, il messaggio deve essere sotto forma di stringa con codificata UTF-8:

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