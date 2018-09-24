I dati non sempre sono permanenti e talvolta è necessario eliminarli con tempistiche specifiche. Ad esempio, nell'applicazione di messaggistica istantanea gli utenti possono impostare il nome visualizzato della chat di gruppo. Si vuole tuttavia reimpostare il nome dopo un'ora. È possibile eseguire questa operazione scrivendo logica sul lato server personalizzata per impostare un timer di un'ora ed eliminare il nome. Tuttavia, Cache Redis di Azure supporta la scadenza dei dati, una funzionalità che consente di ottenere questo risultato automaticamente senza dover scrivere logica aggiuntiva.

In questa unità verranno presentati i comandi Redis comuni per implementare la scadenza dei dati.

## <a name="what-is-data-expiration"></a>Che cos'è la scadenza dei dati?

La scadenza dei dati è una funzionalità in grado di eliminare automaticamente una chiave e un valore nella cache dopo un determinato periodo di tempo.

## <a name="why-use-data-expiration"></a>Perché usare la scadenza dei dati?

La scadenza dei dati viene usata comunemente nelle situazioni in cui i dati archiviati sono di breve durata.  Questo è importante poiché Cache Redis di Azure è un database in memoria e non è disponibile la stessa quantità di memoria che sarebbe a disposizione se i dati fossero archiviati su disco. Dato che lo spazio di archiviazione con Cache Redis di Azure è limitato, è opportuno assicurarsi di archiviare solo i dati importanti. Se i dati non devono essere disponibili per molto tempo, assicurarsi di impostare una scadenza.

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a>Come usare la scadenza dei dati in Cache Redis di Azure

Sono disponibili diversi comandi per implementare e gestire la scadenza dei dati in Cache Redis di Azure:

- `EXPIRE`: imposta il timeout di una chiave in secondi
- `PEXPIRE`: imposta il timeout di una chiave in millisecondi
- `EXPIREAT`: imposta il timeout di una chiave usando un timestamp Unix assoluto in secondi
- `PEXPIREAT`: imposta il timeout di una chiave usando un timestamp Unix assoluto in millisecondi
- `TTL`: restituisce il tempo rimanente di durata di una chiave in secondi
- `PTTL`: restituisce il tempo rimanente di durata di una chiave in millisecondi
- `PERSIST`: imposta una chiave senza scadenza

Il comando più comune è `EXPIRE`, che verrà usato in questo modulo.

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>Esempio di scadenza dei dati con C# e ServiceStack.Redis

Tenere presente che, quando si usa una libreria client come **ServiceStack.Redis**, non è necessario preoccuparsi di ricordare i comandi di Cache Redis di Azure di basso livello. È invece possibile usare metodi C# semplici.

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.SetValue(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

Cache Redis di Azure consente di eliminare automaticamente i dati dopo un determinato periodo di tempo usando la scadenza dei dati. Questo è importante poiché Cache Redis di Azure è un database in memoria e non è disponibile la stessa quantità di memoria che sarebbe a disposizione se i dati fossero archiviati su disco. Se non è necessario che i dati archiviati siano disponibili per molto tempo, assicurarsi di impostare una scadenza.