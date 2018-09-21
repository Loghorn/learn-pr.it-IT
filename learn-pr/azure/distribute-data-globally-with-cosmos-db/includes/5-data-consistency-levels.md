Azure Cosmos DB consente agli sviluppatori di scegliere tra cinque modelli di coerenza ben definiti nell'ambito della coerenza: assoluta, decadimento ristretto, sessione, prefisso coerente e finale. Questi livelli di coerenza consentono di ottimizzare la disponibilità e prestazioni del database, in base alle esigenze. Per i casi in cui i dati devono essere elaborati in un ordine specifico, la coerenza assoluta potrebbe essere la scelta giusta. In altri casi, se i dati non devono essere subito coerenti, la coerenza finale potrebbe essere la scelta ideale. 

In questa unità verranno descritti i livelli di coerenza disponibili in Azure Cosmos DB e verrà determinato il livello di coerenza corretto per un sito di abbigliamento online.

## <a name="consistency-basics"></a>Nozioni di base di coerenza

Ogni account di database ha un livello di coerenza predefinito, che determina la coerenza dei dati all'interno dell'account. A un'estremità dello spettro si colloca la coerenza assoluta, che offre una garanzia di linearità, ovvero la garanzia che le letture restituiscano la versione più recente di un elemento. All'altra estremità del spettro si colloca la coerenza finale, che garantisce che, in assenza di altre scritture, alla fine le repliche all'interno del gruppo convergeranno. Al centro troviamo la coerenza di sessione, la più diffusa, poiché garantisce letture uniformi, scritture uniformi e letture delle proprie scritture.

![I cinque livelli di coerenza in Azure Cosmos DB](../media/5-change-consistency/five-consistency-levels.png)

Nella tabella seguente sono elencate le garanzie su ogni livello di coerenza.
 
**Livelli di coerenza e garanzie**

| Livello di coerenza | Garanzie |
| --- | --- |
| Assoluta | Linearità. Le letture restituiscono sempre la versione più recente di un elemento.|
| Decadimento ristretto | Prefisso coerente. Ritardo delle letture rispetto alle scritture almeno di prefissi k o intervallo t. |
| Sessione   | Prefisso coerente. Letture uniformi, scritture uniformi, lettura delle proprie scritture, scrittura basata sulle letture. |
| Prefisso coerente | Gli aggiornamenti restituiti sono un prefisso di tutti gli aggiornamenti, senza interruzioni. |
| Finale  | Letture non in ordine. |

È possibile configurare il livello di coerenza predefinito per l'account Azure Cosmos DB (e successivamente ignorare l'impostazione del livello di coerenza per una specifica richiesta di lettura) nel portale di Azure. Internamente, il livello di coerenza predefinito si applica ai dati all'interno dei set di partizioni che possono estendersi a più aree.

In Azure Cosmos DB le operazioni di lettura servite con coerenza di sessione, prefisso coerente e finale sono due volte più economiche, in termini di utilizzo di unità richiesta, delle operazioni di lettura con coerenza assoluta o decadimento ristretto.

### <a name="use-of-consistency-levels"></a>Uso dei livelli di coerenza

Circa il 73% dei tenant di Azure Cosmos DB usa il livello di coerenza sessione e il 20% il livello decadimento ristretto. Circa il 3% dei clienti di Azure Cosmos DB sperimenta i vari livelli di coerenza inizialmente prima di decidere quale sia il livello di coerenza specifico più adatto alla propria applicazione. Solo il 2% dei tenant di Azure Cosmos DB esegue l'override dei livelli di coerenza per ogni richiesta.

## <a name="consistency-levels-in-detail"></a>Livelli di coerenza in dettaglio

Per altre informazioni sui livelli di coerenza, vedere gli esempi di coerenza basati su note musicali disponibili nel portale di Azure, quindi esaminare le informazioni seguenti su ogni livello.

1. Nel portale di Azure fare clic su **Coerenza predefinita**.
2. Fare clic su ogni modello di coerenza e osservare gli esempi musicali. Notare come i dati vengono scritti nelle varie aree e in che modo la scelta del livello di coerenza influisce sulle modalità di scrittura dei dati di nota. La coerenza assoluta è disattivata, in quanto è disponibile solo per i dati scritti in una singola area.

    ![Informazioni sulle impostazioni di coerenza nel portale](../media/5-change-consistency/consistency.gif)

Di seguito sono riportate altre informazioni sui livelli di coerenza. Considerare il modo in cui ognuno di questi livelli di coerenza potrebbe operare per i dati utente e di prodotto del sito di abbigliamento.

### <a name="strong-consistency"></a>Coerenza assoluta

* La coerenza assoluta offre una garanzia di [linearità](https://aphyr.com/posts/313-strong-consistency-models) ovvero la garanzia che le letture restituiscano la versione più recente di un elemento.
* la coerenza assoluta garantisce che una scrittura sia visibile solo dopo che ne è stato eseguito il commit in modo permanente dal quorum di maggioranza delle repliche. Una scrittura può ottenere o il commit sincrono e permanente da parte della replica primaria e della maggioranza delle repliche secondarie o l'interruzione. Una lettura viene sempre confermata dal quorum di maggioranza per le letture: un client non potrà mai vedere una scrittura parziale o di cui non sia stato eseguito il commit e leggerà sempre la più recente scrittura confermata. 
* Gli account Azure Cosmos DB configurati per usare la coerenza assoluta non possono associare più di un'area di Azure con il loro account Azure Cosmos DB.  
* Il costo di un'operazione di lettura (in termini di unità richiesta usate) con il livello di coerenza assoluta è più alto rispetto ai livelli sessione e finale, ma uguale a quello del livello con decadimento ristretto.

### <a name="bounded-staleness-consistency"></a>Coerenza con decadimento ristretto

* La coerenza con decadimento ristretto garantisce che il ritardo delle letture sulle scritture sia al massimo pari a *K* versioni o prefissi di un elemento o all'intervallo di tempo *t*.
* Pertanto, quando si sceglie il decadimento ristretto, il "decadimento" può essere configurato in due modi: numero di versioni *K* dell'elemento del ritardo delle operazioni di lettura sulle operazioni di scrittura e l'intervallo di tempo *t*.
* Il decadimento ristretto offre un ordine globale totale tranne all'interno della "finestra di decadimento". La garanzia di lettura uniforme esiste in un'area sia all'interno che all'esterno della "finestra di decadimento".
* Il decadimento ristretto offre una maggiore garanzia di coerenza rispetto alla coerenza di sessione, con prefisso coerente o finale. Per le applicazioni distribuite a livello globale, è consigliabile usare il decadimento ristretto per gli scenari in cui si vuole una coerenza assoluta, ma anche il 99,99% di disponibilità e bassa latenza.
* Gli account Azure Cosmos DB configurati con la coerenza con decadimento ristretto possono associare qualsiasi numero di aree di Azure con il proprio account Azure Cosmos DB. 
* Il costo di un'operazione di lettura (in termini di unità richiesta usate) con il decadimento ristretto è più alto rispetto ai livelli sessione e finale, ma uguale a quello del livello assoluto.

### <a name="session-consistency"></a>Coerenza di sessione

* A differenza dei modelli di coerenza globale offerti dai livelli di coerenza assoluta e con decadimento ristretto, la coerenza di "sessione" ha come ambito una sessione del client.
* La coerenza di sessione è ideale per tutti gli scenari in cui è coinvolto un dispositivo o una sessione utente poiché garantisce letture monotone, scritture monotone e garanzie di lettura di ciò che si scrive (RYW).
* La coerenza di sessione offre una coerenza prevedibile per una sessione e la massima velocità di scrittura, con latenza minima per scrittura e lettura.
* Gli account Azure Cosmos DB configurati con la coerenza di sessione possono associare qualsiasi numero di aree di Azure con il proprio account Azure Cosmos DB.
* Il costo di un'operazione di lettura (in termini di unità richiesta usate) con il livello di coerenza di sessione è minore rispetto ai livelli assoluto e con decadimento ristretto, ma maggiore rispetto al livello finale.

### <a name="consistent-prefix-consistency"></a>Coerenza con prefisso coerente

* Il prefisso coerente garantisce che, in assenza di altre operazioni di scrittura, alla fine le repliche convergeranno all'interno del gruppo. 
* Il prefisso coerente garantisce che le operazioni di lettura non vedano mai il fuori sequenza delle operazioni di scrittura. Se queste sono state eseguite nell'ordine `A, B, C`, il client vede `A`, `A,B` o `A,B,C`, ma mai il fuori sequenza ad esempio `A,C` o `B,A,C`.
* Gli account Azure Cosmos DB configurati con la coerenza con prefisso coerente possono associare qualsiasi numero di aree di Azure con il proprio account Azure Cosmos DB. 

### <a name="eventual-consistency"></a>Coerenza finale

* La coerenza finale garantisce che, in assenza di altre scritture, alla fine le repliche all'interno del gruppo convergeranno.
* La coerenza finale è la forma più debole di coerenza, in cui un client può ottenere nel tempo valori obsoleti rispetto a quelli già visualizzati in passato.
* La coerenza finale rappresenta il livello più debole, ma offre la latenza più bassa sia per le letture sia le per scritture.
* Gli account Azure Cosmos DB configurati con la coerenza finale possono associare qualsiasi numero di aree di Azure con il proprio account Azure Cosmos DB. 
* Il costo di un'operazione di lettura, in termini di unità richiesta usate, con il livello di coerenza finale è il più basso fra tutti i livelli di coerenza di Azure Cosmos DB.

## <a name="summary"></a>Riepilogo

In questa unità si è appreso come usare i livelli di coerenza per ottimizzare la disponibilità elevata e ridurre al minimo la latenza.