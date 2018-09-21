Dopo aver eseguito la replica dei dati in più aree, è possibile sfruttare i vantaggi delle soluzioni di failover automatico offerte da Azure Cosmos DB. Il failover automatico è una funzionalità attivata in caso di un'emergenza o di altri eventi che accetta una delle aree di lettura o scrittura offline e reindirizza le richieste dall'area successiva con maggiore priorità. 

Per il sito di abbigliamento online, replicato nelle aree Stati Uniti orientali, Stati Uniti occidentali e Regno Unito occidentale, è possibile assegnare una priorità in modo che se l'area Stati Uniti orientali entra in modalità offline, le letture possano essere reindirizzate all'area Stati Uniti occidentali anziché all'area Regno Unito occidentale per limitare la latenza. 

In questa unità viene descritto il funzionamento del failover e il modo in cui impostare la priorità per le aree in cui sono stati replicati i dati dell'azienda.

## <a name="failover-basics"></a>Nozioni di base sul failover

Nel raro caso di un'interruzione a livello di area di Azure o del data center, Azure Cosmos DB attiva automaticamente i failover di tutti gli account Azure Cosmos DB presenti nell'area interessata.

**Cosa accade se si verifica un'interruzione del servizio in un'area di lettura?**

Gli account Azure Cosmos DB con un'area di lettura in una delle aree interessate vengono automaticamente disconnessi dalla rispettiva area di scrittura e contrassegnati come offline. Gli SDK di Cosmos DB implementano un protocollo di individuazione area che consente di rilevare automaticamente quando un'area è disponibile e reindirizzare le chiamate di lettura all'area successiva disponibile nell'elenco delle aree preferite. Se nessuna delle aree presenti nell'elenco è disponibile, viene eseguito il fallback automatico delle chiamate all'area di scrittura corrente. Non sono necessarie modifiche nel codice dell'applicazione per gestire i failover regionali. Durante l'intero processo, Azure Cosmos DB continua a rispettare garanzie di coerenza.

Dopo il ripristino dell'area interessata da un'interruzione del servizio, tutti gli account Azure Cosmos DB interessati nell'area vengono ripristinati automaticamente dal servizio. Gli account Azure Cosmos DB con un'area di lettura nell'area interessata vengono quindi sincronizzati automaticamente con l'area di scrittura corrente e passano in modalità online. Gli SDK di Azure Cosmos DB rilevano la disponibilità della nuova area e valutano se deve essere selezionata come area di lettura corrente in base all'elenco delle aree preferite configurato dall'applicazione. Le letture successive vengono reindirizzate all'area ripristinata senza richiedere alcuna modifica del codice dell'applicazione.

**Cosa accade se si verifica un'interruzione del servizio in un'area di scrittura?**

Se l'area interessata è l'area di scrittura corrente e per l'account Azure Cosmos DB è abilitato il failover automatico, l'area viene automaticamente contrassegnata come offline. Viene quindi promossa un'area alternativa come area di scrittura per l'account Azure Cosmos DB interessato.

Durante i failover automatici, Azure Cosmos DB sceglie automaticamente l'area di scrittura successiva per un determinato account Azure Cosmos DB in base all'ordine di priorità specificato. Le applicazioni possono usare la proprietà WriteEndpoint della classe DocumentClient per rilevare le modifiche dell'area di scrittura.

Dopo il ripristino dell'area interessata da un'interruzione del servizio, tutti gli account Cosmos DB interessati nell'area vengono ripristinati automaticamente dal servizio.

A questo punto si modifica l'area di lettura per il database.

## <a name="set-read-region-priorities"></a>Impostare le priorità dell'area di lettura

1. Nella schermata **Replica i dati a livello globale** del portale di Azure fare clic su **Failover automatico**. Il failover automatico è abilitato solo se il database è già stato replicato in più di un'area.
2. Nella schermata **Failover automatico** impostare **Abilita failover automatico** su **Attivato**.
3. Nella sezione **Aree di lettura** fare clic sulla parte sinistra della riga **Stati Uniti orientali** e quindi trascinarla nella posizione superiore.
4. Fare clic sulla parte sinistra della riga **Giappone orientale** e quindi trascinarla in seconda posizione.
5. Fare clic su **OK**.

    ![Modificare le aree di lettura nel portale](../media/4-change-priorities/change-read-priorities.gif)

## <a name="summary"></a>Riepilogo

In questa unità sono stati descritti i risultati del failover automatico, il modo in cui è possibile usarlo per la protezione da interruzioni impreviste e il modo in cui modificare le priorità di un'area di lettura.