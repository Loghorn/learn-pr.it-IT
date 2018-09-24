Il rivenditore online per cui si sta lavorando prevede di espandersi presto in una nuova area geografica. Questo passaggio aumenterà la base clienti e il volume delle transazioni. È necessario assicurarsi che il database sia predisposto per gestire l'espansione secondo le necessità.

Disporre di una strategia di partizione assicura che nel momento in cui il database dovrà aumentare le sue dimensioni, sarà in grado di farlo facilmente e continuare a eseguire transazioni e query efficienti.

## <a name="what-is-a-partition-strategy"></a>Cosa si intende per strategia di partizione?

Se si continua ad aggiungere nuovi dati a un singolo server o a una singola partizione, potrebbe esaurirsi lo spazio. Per prepararsi a ciò, è necessaria una strategia di partizionamento di **scalabilità orizzontale** e non verticale. La scalabilità orizzontale consente di aggiungere altre partizioni nel database quando necessarie all'applicazione.

La strategia di partizione e scalabilità orizzontale in Azure Cosmos DB è determinata dalla chiave di partizione, ovvero un valore impostato durante la creazione di una raccolta. Dopo aver impostato la chiave di partizione, non può essere modificata senza ricreare la raccolta, quindi selezionare la chiave di partizione corretta è una decisione importante da eseguire all'inizio del processo di sviluppo.  

In questa unità, si apprenderà come scegliere la chiave di partizione più adatta a uno scenario e che potrà sfruttare la scalabilità automatica che Azure Cosmos DB è in grado di offrire.

## <a name="what-is-a-partition-key"></a>Che cos'è una chiave di partizione?

Una chiave di partizione è il valore con cui Azure organizza i dati in suddivisioni logiche. In questo scenario di vendita online, l'uso del valore `userID` o `productId` come chiave di partizione è un'ottima scelta perché è univoco e idoneo per la ricerca dei record. `userID` è un'ottima scelta, poiché l'applicazione deve spesso recuperare le impostazioni di personalizzazione, il carrello, la cronologia degli ordini e le informazioni di profilo per l'utente, solo per citarne alcune. Anche `productId` è un'ottima scelta, poiché l'applicazione deve eseguire query su livelli di inventario, costi di spedizione, opzioni di colore, posizione dei negozi e molto altro.

Una chiave di partizione deve tendere a distribuire le operazioni tra il database. L'obiettivo è distribuire le richieste per evitare partizioni critiche. Una partizione critica è una partizione singola che riceve molte più richieste rispetto alle altre, che possono creare un collo di bottiglia della velocità effettiva. Ad esempio, per l'applicazione di e-commerce, l'ora corrente sarebbe una pessima scelta per una chiave di partizione, perché tutti i dati in ingresso sarebbero indirizzati a una singola chiave di partizione. I valori `userID` o `productId` sono più indicati, dal momento che tutti gli utenti nel sito probabilmente aggiungono e aggiornano i dati del carrello o del profilo circa con la stessa frequenza, così che le letture e le scritture vengono distribuite in tutte le partizioni dell'utente e dei prodotti.

Lo spazio di archiviazione massimo per i dati associati a ogni chiave di partizione è di 10 GB, ovvero le dimensioni di una partizione fisica in Azure Cosmos DB. Pertanto, se il singolo record `userID` o `productId` è superiore a 10 GB, può essere necessario usare invece una chiave composta in modo che ogni record sia inferiore. Un esempio di una chiave composta sarebbe `userID-date`, che sarà simile a **CustomerName-08072018**. Questo approccio consentirebbe di creare una nuova partizione per ogni giorno in cui un utente visita il sito.

## <a name="best-practices"></a>Procedure consigliate

Quando si tenta di determinare la chiave di partizione corretta e la soluzione non è ovvia, ecco alcuni suggerimenti da tenere presenti.

- Non temere di scegliere una chiave di partizione con un numero elevato di valori. Più sono i valori della chiave di partizione, maggiore è la scalabilità.
- Per determinare la migliore chiave di partizione per un carico di lavoro con intensa attività di lettura, esaminare le migliori 3-5 query che si intende usare. Il valore incluso più di frequente nella clausola WHERE è un buon candidato per la chiave di partizione.
- Per carichi di lavoro con intensa attività di scrittura, è necessario capire le esigenze di un carico di lavoro transazionale, poiché la chiave di partizione è l'ambito delle transazioni in più documenti.
