I dati non sempre è permanenti e talvolta che si desidera eliminarlo in un momento specifico. Ad esempio, nell'applicazione di messaggistica istantanea, gli utenti possono impostare il nome visualizzato della chat di gruppo. Tuttavia, si desidera che il nome reimpostare dopo un'ora. È possibile eseguire questa operazione la scrittura logica personalizzata lato server che impostano un timer per l'ora e il nome. Tuttavia, Cache Redis di Azure supporta la scadenza dei dati, che è una funzionalità che viene eseguito automaticamente senza dover scrivere logica aggiuntiva.

In questo caso, scoprirai i comuni comandi di Redis per implementare scadenza dei dati.

## <a name="what-is-data-expiration"></a>Che cos'è scadenza dei dati?

Scadenza dei dati è una funzionalità in grado di eliminare automaticamente una chiave e il valore nella cache dopo un periodo di tempo.

## <a name="why-use-data-expiration"></a>Perché usare scadenza dei dati?

Scadenza dei dati viene comunemente usato in situazioni in cui i dati archiviati sono di breve durati.  Questo è importante poiché Cache Redis di Azure è un database in memoria e non si dispone della quantità di memoria disponibile per l'uso come si farebbe se sono stati archiviati su disco. Poiché è limitato archiviazione Cache Redis di Azure, è possibile assicurarsi che solo si archiviano dati importanti. Se i dati non è necessaria in circolazione per molto tempo, assicurarsi di che impostare una scadenza.

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a>Come usare la scadenza dei dati in Cache Redis di Azure

Sono disponibili diversi comandi per implementare e gestire la scadenza dei dati in Cache Redis di Azure:

- `EXPIRE`: Imposta il timeout di una chiave in secondi
- `PEXIRE`: Imposta il timeout di una chiave in millisecondi
- `EXPIREAT`: Imposta il timeout di una chiave usando un timestamp Unix dell'ora assolute in pochi secondi
- `PEXPIREAT`: Imposta il timeout di una chiave usando un timestamp Unix dell'ora assolute in millisecondi
- `TTL`: Restituisce il tempo rimanente che dispone di una chiave di durata (TTL) in pochi secondi
- `PTTL`: Restituisce il tempo rimanente che dispone di una chiave di durata (TTL) in millisecondi
- `PERSIST`: Rende una chiave senza scadenza

Il comando più comune è `EXPIRE`, e verrà usato in tutto questo modulo.

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>Esempio di scadenza dei dati con c# e ServiceStack.Redis

Tenere presente che, quando si usa una libreria client, ad esempio **ServiceStack.Redis**, non è necessario preoccuparsi di ricordare i comandi di Cache Redis di Azure a basso livello. In alternativa, è possibile usare i metodi c# semplici.

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.Set(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

Cache Redis di Azure consente di eliminare automaticamente i dati dopo un periodo di tempo Usa la scadenza dei dati. Questo è importante poiché Cache Redis di Azure è un database in memoria e non si dispone di una quantità di memoria disponibile con l'archiviazione dei dati su disco. Se si archiviano dati senza dover essere intorno a lungo, assicurarsi di che impostare una scadenza.