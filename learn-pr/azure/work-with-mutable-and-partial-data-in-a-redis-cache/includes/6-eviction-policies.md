Trattandosi di un database in memoria, la memoria è la risorsa più critica per Cache Redis di Azure. Possono verificarsi problemi quando si inizia ad aggiungere dati che superano la quantità di memoria disponibile. Cache Redis di Azure supporta i criteri di rimozione, che indicano come gestire i dati in caso di memoria insufficiente.

In questa unità si vedrà come impostare i criteri di rimozione per determinare come devono essere gestiti i dati quando si supera la quantità massima di memoria.

## <a name="what-is-an-eviction-policy"></a>Che cosa sono i criteri di rimozione?

I criteri di rimozione sono un piano che determina come devono essere gestiti i dati quando si supera la quantità massima di memoria disponibile. Ad esempio, usando i criteri di rimozione è possibile indicare a Cache Redis di Azure di eliminare una chiave casuale per fare spazio ai nuovi dati inseriti.

### <a name="types-of-eviction-policies"></a>Tipi di criteri di rimozione

Cache Redis di Azure offre sei diversi tipi di criteri di rimozione. Tutti questi valori eseguono un'azione quando si cerca di inserire i dati in condizioni di memoria insufficiente.

* **noeviction:** nessun criterio di rimozione. Restituisce un messaggio di errore se si tenta di inserire i dati.

* **allkeys-lru:** rimuove l'ultima chiave usata.

* **allkeys-random:** rimuove una chiave casuale.

* **volatile-lru:** rimuove l'ultima chiave usata tra tutte le chiavi con una scadenza impostata.

* **volatile-ttl:** rimuove la chiave con la durata più breve in base alla scadenza impostata.

* **volatile-random:** rimuove una chiave casuale con una scadenza impostata.

## <a name="how-to-set-an-eviction-policy"></a>Come impostare i criteri di rimozione

Azure offre un semplice menu a discesa per impostare i criteri di rimozione per Cache Redis di Azure. Selezionare **Impostazioni avanzate** e usare il menu a discesa **Criterio per la memoria massima**.

Dato che la memoria è fondamentale per Cache Redis di Azure, è previsto il supporto dei criteri di rimozione. I criteri di rimozione determinano le azioni da intraprendere con i dati esistenti quando la memoria è insufficiente e si cerca di inserire nuovi dati.