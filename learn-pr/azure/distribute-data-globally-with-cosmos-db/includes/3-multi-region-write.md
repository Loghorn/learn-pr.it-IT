Garantire un'esperienza utente eccezionale in uno scenario di e-commerce richiede disponibilità elevata e bassa latenza.

Abilitando il supporto multimaster nell'account Azure Cosmos DB appena creato, sono numerosi i vantaggi per questi due aspetti delle prestazioni e tutti i vantaggi sono garantiti dai contratti di servizio con supporto finanziario offerti da Azure Cosmos DB.

## <a name="what-is-multi-master-support"></a>Che cos'è il supporto multimaster?

Il supporto multimaster è un'opzione che può essere abilitata nei nuovi account Azure Cosmos DB. Una volta che l'account viene replicato in più aree, ogni area è un'area master che partecipa in ugual misura a un modello di scrittura ovunque, anche noto come modello attivo-attivo.

Le aree di Azure Cosmos DB che fungono da aree master in una configurazione multimaster operano automaticamente in modo da convergere i dati scritti in tutte le repliche e assicurare la coerenza globale e l'integrità dei dati.

Con il supporto multimaster di Azure Cosmos DB è possibile eseguire operazioni di scrittura in qualsiasi contenitore in un'area abilitata per la scrittura a livello globale. I dati scritti vengono propagati immediatamente a tutte le altre aree.  

## <a name="what-are-the-benefits-of-multi-master-support"></a>Quali sono i vantaggi offerti dal supporto multimaster?

I vantaggi offerti dal supporto multimaster sono:

* Latenza di scrittura a singole unità: gli account multimaster hanno una latenza di scrittura migliorata di <10 ms per il 99% delle operazioni di scrittura, fino a <15 ms per gli account non multimaster.
* Disponibilità del 99,999% nelle operazioni di lettura/scrittura: la disponibilità nelle operazioni di scrittura degli account multimaster aumenta al 99,999%, dal 99,99% per gli account non multimaster.
* Scalabilità delle operazioni di scrittura e velocità effettiva illimitate: con gli account multimaster è possibile scrivere in ogni area, garantendo scalabilità delle operazioni di scrittura e velocità effettiva illimitate per supportare miliardi di dispositivi.
* Risoluzione integrata dei conflitti: gli account multimaster hanno tre metodi per la risoluzione dei conflitti per garantire coerenza e integrità dei dati a livello globale. 

## <a name="conflict-resolution"></a>Risoluzione dei conflitti

Con l'aggiunta del supporto multimaster potrebbero verificarsi conflitti per la scrittura in aree diverse. I conflitti sono estremamente rari in Azure Cosmos DB e possono verificarsi solo quando un elemento viene modificato contemporaneamente in più aree, prima che sia stata eseguita la propagazione tra le aree. Considerata la velocità con cui la replica si verifica a livello globale, questi conflitti non dovrebbero verificarsi quasi mai. Tuttavia, Azure Cosmos DB fornisce modalità di risoluzione dei conflitti che consentono agli utenti di gestire scenari in cui lo stesso record viene aggiornato simultaneamente da writer diversi in due o più aree.  

Azure Cosmos DB offre tre tipi di modelli di risoluzione dei conflitti. 
* **Priorità ultima scrittura**, in cui i conflitti vengono risolti in base al valore di una proprietà integer definita dall'utente nel documento. Per impostazione predefinita viene usata la proprietà _ts per determinare l'ultimo documento scritto. Priorità ultima scrittura è il meccanismo predefinito di gestione dei conflitti.
* **Personalizzata: procedura definita dall'utente**, in cui è possibile controllare completamente la risoluzione dei conflitti tramite la registrazione di una procedura definita dall'utente nella raccolta. Una procedura definita dall'utente è un tipo speciale di stored procedure con una firma molto specifica. Se la procedura definita dall'utente ha esito negativo o non esiste, Azure Cosmos DB aggiungerà tutti i conflitti nei feed dei conflitti di sola lettura per essere elaborati in modo asincrono.  
* **Personalizzata: asincrona**, in cui Azure Cosmos DB esclude il commit per tutti i conflitti e li registra nel feed dei conflitti di sola lettura per la risoluzione posticipata dall'applicazione dell'utente. L'applicazione può eseguire la risoluzione dei conflitti in modo asincrono e usare qualsiasi logica o fare riferimento a qualsiasi origine esterna, applicazione o servizio per risolvere il conflitto.

## <a name="summary"></a>Riepilogo

In questa unità sono stati descritti gli account multimaster, che consentono di scrivere dati in più aree per migliorare la disponibilità e le prestazioni.