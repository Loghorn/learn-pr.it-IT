Si supponga di dover pianificare l'architettura di un'applicazione per la condivisione di musica. È necessario assicurarsi che i file musicali vengano caricati nell'API Web in modo affidabile dall'app per dispositivi mobili.

Dopo aver stabilito che questa comunicazione deve essere un messaggio, è necessario scegliere se usare le code di archiviazione di Azure o il bus di servizio di Azure, entrambi servizi che possono essere usati per archiviare code di messaggi in attesa dell'elaborazione.

## <a name="delivery-guarantees"></a>Garanzie di recapito

I sistemi di accodamento in genere garantiscono il recapito di ogni messaggio nella coda a un componente di destinazione. Per queste garanzie si possono adottare tuttavia approcci diversi:

- **Recapito At-Least-Once.** Con questo approccio, è garantito il recapito di ogni messaggio ad almeno uno dei componenti che recuperano i messaggi dalla coda. Si noti, tuttavia, che in determinate circostanze, è possibile che lo stesso messaggio venga recapitato più di una volta. Ad esempio, se sono presenti due istanze di un'app Web che recuperano i messaggi da una coda, in genere ogni messaggio viene recapitato a una sola di tali istanze. Tuttavia, se un'istanza richiede molto tempo per elaborare il messaggio e scade un timeout, il messaggio potrebbe essere inviato anche all'altra istanza. Il codice dell'app Web deve essere progettato tenendo in considerazione questa eventualità.
- **Recapito At-Most-Once.** Con questo approccio, il recapito di ogni messaggio non è garantito ed esiste una probabilità molto piccola che il messaggio possa non arrivare. Tuttavia, a differenza del recapito At-Least-Once, non vi è alcuna possibilità che il messaggio venga recapitato due volte.
- **First-In-First-Out (FIFO).** Nella maggior parte dei sistemi di messaggistica, i messaggi in genere lasciano la coda nello stesso ordine in cui sono stati aggiunti, ma è necessario considerare se quest'ordine è garantito. Se l'applicazione distribuita richiede che i messaggi vengano elaborati con precisione nell'ordine corretto, è necessario scegliere un sistema di accodamento che include una garanzia FIFO.

## <a name="transactions"></a>Transazioni

Alcuni gruppi di messaggi strettamente correlati possono causare problemi quando si verifica un errore di recapito per un messaggio nel gruppo.

Si consideri, ad esempio, un'applicazione di e-commerce. Quando l'utente fa clic sul pulsante "Acquista", viene inviato un messaggio con i dettagli degli ordini oltre a un messaggio con i dettagli della carta di credito. Se il messaggio per la carta di credito non viene recapitato, l'ordine potrebbe essere spedito senza pagamento.

È possibile evitare questi tipi di problemi raggruppando i due messaggi in una transazione. Le transazioni devono avere esito positivo o negativo come singola unità. Se il recapito del messaggio dei dettagli della carta di credito non riesce, anche il messaggio dei dettagli dell'ordine non verrà recapitato.

## <a name="when-to-use-storage-queues"></a>Quando usare le code di archiviazione

Le code vengono usate per i messaggi, ma non per gli eventi.

Gli account di archiviazione di Azure includono le code, che possono essere usate dalle applicazioni distribuite come semplici posizioni di archiviazione temporanea per i messaggi. Un componente di origine può aggiungere un messaggio alla coda. I componenti di destinazione recuperano il messaggio all'inizio della coda per l'elaborazione. Queste code migliorano l'affidabilità dello scambio di messaggi poiché, nei momenti di picco, i messaggi possono attendere semplicemente che un componente di destinazione sia pronto per elaborarli.

Le code vengono fornite anche come parte della messaggistica del bus di servizio. Se si è deciso di usare una coda in Azure per una comunicazione specifica, è necessario stabilire se usare una coda di archiviazione o una coda del bus di servizio.

Per effettuare questa scelta, considerare le domande seguenti:

- È necessaria una garanzia di recapito At-Most-Once?
- È necessaria una garanzia FIFO?
- È necessario raggruppare i messaggi in transazioni?
- È necessario archiviare messaggi di dimensioni maggiori di 64 KB?

Se la risposta a una di queste domande è affermativa, è necessario scegliere di usare una coda del bus di servizio.

Tenere in considerazione anche le domande seguenti:

- Sono necessari log sul lato server per tutti i messaggi che passano attraverso la coda?
- Si prevede che le dimensioni della coda possano superare gli 80 GB?

Se la risposta a una di queste domande è affermativa, è necessario scegliere di usare una coda di archiviazione.

## <a name="summary"></a>Riepilogo

Una coda è una semplice posizione di archiviazione temporanea per i messaggi inviati tra i componenti di un'applicazione distribuita. Usare una coda per organizzare i messaggi e gestire correttamente i picchi nella domanda non prevedibili.

Usare le code di archiviazione quando si vuole un sistema di accodamento semplice e facile da programmare. Per esigenze più avanzate, usare le code del bus di servizio.