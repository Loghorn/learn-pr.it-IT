Memoria è la risorsa più critica per la Cache Redis di Azure, poiché si tratta di un database in memoria. Si verificano problemi quando si inizia aggiungendo i dati che superano la quantità di memoria disponibile. Cache Redis di Azure supporta i criteri di eliminazione, che indica come gestire i dati quando si esegue memoria insufficiente.

In questo caso, impostare un criterio di rimozione per determinare cosa devono fare i dati quando si supera la quantità massima di memoria.

## <a name="what-is-an-eviction-policy"></a>Che cos'è un criterio di rimozione?

Un criterio di rimozione è un piano che determina come i dati devono essere gestiti quando si supera la quantità massima di memoria disponibile. Ad esempio, usando un criterio di rimozione, si potrebbe indicare Cache Redis di Azure per eliminare una chiave casuale per liberare spazio per i nuovi dati da inserire.

### <a name="types-of-eviction-policies"></a>Tipi di criteri di rimozione

Esistono sei i criteri di rimozione diversi forniti da Cache Redis di Azure. Tutti questi valori eseguire un'azione quando si tenta di inserire i dati quando si è esaurito la memoria.

* **noeviction:** alcun criterio di rimozione. Restituisce un messaggio di errore se si prova a inserire i dati.

* **AllKeys-lru:** rimuove la chiave usata meno di recente.

* **AllKeys-random:** rimuove una chiave casuale.

* **volatile-lru:** rimuove la chiave usata meno di recente all'esterno di tutte le chiavi con un set di scadenza.

* **volatile-ttl:** rimuove la chiave con il tempo più breve durata (TTL) in base allo scadere impostate per esso.

* **volatile-random:** rimuove una chiave casuale con una scadenza impostata.

## <a name="how-to-set-an-eviction-policy"></a>Come impostare un criterio di rimozione

Azure offre un semplice menu elenco a discesa per impostare i criteri di rimozione per Cache Redis di Azure. Selezionare **impostazioni avanzate**e utilizzare il **maxmemory-policy** dal menu a discesa.

Poiché la memoria è fondamentale per Cache Redis di Azure, è supportata per i criteri di rimozione. Un criterio di rimozione determina le azioni da intraprendere con i dati esistenti quando si ha esaurito la memoria e tenta di inserire nuovi dati.