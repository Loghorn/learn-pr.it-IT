Le applicazioni distribuite usano le code, ad esempio le code dei bus di servizio, come posizioni di archiviazione temporanea per i messaggi in attesa di essere recapitati a un componente di destinazione. Per inviare e ricevere messaggi tramite una coda, è necessario scrivere codice nei componenti di origine e di destinazione.

Si consideri l'applicazione Contoso Slices. Il cliente effettua l'ordine tramite un sito Web o un'app per dispositivi mobili. Poiché i siti Web e App per dispositivi mobili eseguite su dispositivi del cliente, non è effettivamente alcun limite al numero di ordini può arrivare in una sola volta in. Se l'app per dispositivi mobili e il sito Web depositano gli ordini in una coda, il componente di back-end (un'app Web) può elaborare gli ordini da tale coda in base ai propri tempi.

L'applicazione Contoso Slices prevede diversi passaggi per gestire un nuovo ordine. Ma per tutti i passaggi è prima richiesta l'autorizzazione del pagamento, di conseguenza decidiamo di usare una coda. Il primo processo del componente di ricezione consiste nell'elaborazione del pagamento.

Sia per l'app per dispositivi mobili che per il sito Web, Contoso deve scrivere il codice per aggiungere un messaggio alla coda. Nell'app web back-end, si scriverà il codice che preleva i messaggi dalla coda.

In questa esercitazione verrà spiegato come scrivere tale codice.

## <a name="the-microsoftazureservicebus-nuget-package"></a>Pacchetto NuGet Microsoft.Azure.ServiceBus

Per semplificare la scrittura di codice che invia e riceve i messaggi tramite il bus di servizio, Microsoft offre una libreria di classi .NET, che è possibile usare in qualsiasi linguaggio .NET Framework per interagire con una coda, argomento o inoltro del bus di servizio. È possibile includere questa libreria nell'applicazione aggiungendo il pacchetto NuGet **Microsoft.Azure.ServiceBus**.

La classe più importante di questa libreria è la classe `QueueClient`. Per iniziare, è necessario creare un'istanza di questa classe nei componenti di invio e ricezione.

## <a name="connection-strings-and-keys"></a>Le chiavi e le stringhe di connessione

I componenti di origine e i componenti di destinazione necessarie due informazioni per connettersi a una coda in uno spazio dei nomi del Bus di servizio:

- Il percorso dello spazio dei nomi del Bus di servizio, noto anche come un **endpoint**. Il percorso viene specificato come un nome di dominio completo all'interno di **servicebus.windows.net** dominio. ad esempio **pizzaService.servicebus.windows.net**.
- Una chiave di accesso. Il bus di servizio limita l'accesso a code, argomenti e inoltri richiedendo una chiave di accesso.

Entrambi questi elementi vengono forniti all'oggetto `QueueClient` sotto forma di stringa di connessione. È possibile ottenere la stringa di connessione corretta per lo spazio dei nomi dal portale di Azure.

## <a name="calling-methods-asynchronously"></a>Chiamata dei metodi in modalità asincrona

La coda di Azure può trovarsi a migliaia di chilometri dai componenti di invio e ricezione. Anche se è fisicamente vicina, connessioni lente e la contesa della larghezza di banda possono causare ritardi quando un componente chiama un metodo sulla coda. Per questo motivo, la libreria di client del Bus di servizio rende `async` metodi disponibili per l'interazione con le code. Useremo questi metodi per evitare di bloccare un thread in attesa del completamento delle chiamate.

Quando si invia un messaggio a una coda, ad esempio, usare il metodo `QueueClient.SendAsync()` con la parola chiave `await`.

## <a name="write-code-that-sends-to-queues"></a>Scrivere il codice che invia messaggi alle code 

In qualsiasi invio o ricezione componente, è necessario aggiungere quanto segue `using` istruzioni ai file di codice che chiama una coda del Bus di servizio:

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

Creare quindi un nuovo oggetto `QueueClient` e passarvi la stringa di connessione e il nome della coda:

```C#
queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
```

È possibile inviare un messaggio nella coda chiamando il `QueueClient.SendAsync()` metodo e passando il messaggio sotto forma di un UTF-8 in una stringa codificata:

```C#
string message = "Sure would like a large pepperoni!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await queueClient.SendAsync(encodedMessage);
```

## <a name="receive-messages-from-queue"></a>Ricevere messaggi dalla coda

Per ricevere i messaggi, è prima necessario registrare un gestore di messaggi. Si tratta del metodo nel codice che verrà richiamato quando nella coda è presente un messaggio.

```C#
queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

Eseguire l'elaborazione. Quindi, all'interno del gestore di messaggi, chiamare il `QueueClient.CompleteAsync()` metodo per rimuovere il messaggio dalla coda:

```C#
await queueClient.CompleteAsync(message.SystemProperties.LockToken);
```
    