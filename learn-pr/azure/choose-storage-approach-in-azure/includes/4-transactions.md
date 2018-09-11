Spesso è necessario raggruppare una serie di aggiornamenti ai dati perché una modifica a un tipo di dati comporta una modifica a un altro tipo di dati. Le transazioni consentono di raggruppare questi aggiornamenti in modo che, se un evento in una serie di aggiornamenti ha esito negativo, è possibile eseguire il rollback dell'intera serie o annullarla. Un rivenditore online potrebbe ad esempio usare una transazione per l'invio di un ordine e la verifica del pagamento. Raggruppando gli eventi correlati, è possibile evitare di ridurre i livelli di inventario fino a quando non si riceve un metodo di pagamento approvato.

In questo modulo si apprenderà che cos'è una transazione e quando è necessario usarla per i dati.

## <a name="what-is-a-transaction"></a>Che cos'è una transazione?

Una transazione è un'unità logica che viene eseguita in modo indipendente per il recupero o gli aggiornamenti dei dati.

Ecco la domanda da porsi per determinare se è necessario un database transazionale: una modifica a un tipo di dati nel set di dati influirà su un altro tipo di dati? Se la risposta è affermativa, nel servizio di database sarà necessario il supporto per le transazioni.

Le transazioni vengono spesso definite da un set di quattro requisiti, detti anche garanzie ACID. ACID è l'acronimo di Atomicità, Coerenza, Isolamento e Durabilità:

* L'atomicità indica che tutti i dati vengono aggiornati oppure viene eseguito il rollback di tutti i dati allo stato originale.
* La coerenza garantisce che se accade qualcosa nel corso della transazione, non venga aggiornata solo una parte dei dati, mentre l'altra no. La transazione viene applicata in modo coerente a tutti i dati oppure non viene applicata.
* L'isolamento garantisce che una transazione non influisca su un'altra transazione.
* La durabilità significa che le modifiche apportate nell'ambito della transazione vengono salvate in modo permanente nel sistema. I dati di cui viene eseguito il commit vengono salvati dal sistema in modo che, anche in caso di un errore con conseguente riavvio del sistema, i dati saranno disponibili nello stato corretto.

Quando un database ha le garanzie ACID, questi principi vengono applicati alle transazioni e si può avere la certezza che le transazioni verranno applicate in modo coerente.

## <a name="oltp-vs-olap"></a>OLTP e OLAP

I database transazionali vengono spesso denominati sistemi OLTP (Online Transaction Processing, elaborazione di transazioni online). I sistemi OLTP supportano in genere numerosi utenti, hanno tempi di risposta rapidi, gestiscono volumi elevati di dati, offrono disponibilità elevata, ovvero hanno tempi di inattività minimi, e in genere gestiscono le transazioni di piccole dimensioni o relativamente semplici.

Al contrario, i sistemi OLAP (Online Analytical Processing, elaborazione analitica online) supportano in genere un numero inferiore di utenti, hanno tempi di risposta più lunghi, possono offrire una minore disponibilità e in genere gestiscono le transazioni di grandi dimensioni e complesse.

I sistemi OLTP e OLAP non vengono usati tanto frequentemente quanto in passato, ma il confronto semplifica la classificazione delle esigenze dell'applicazione, pertanto si tratta di concetti importanti da conoscere. 

Ora che è stata acquisita familiarità con le transazioni e con i concetti di OLTP e OLAP, verranno esaminati i set di dati dello scenario di vendita online per determinare la necessità di transazioni.

### <a name="product-catalog-data"></a>Dati del catalogo prodotti

I dati del catalogo prodotti devono essere archiviati in un database transazionale. Quando gli utenti effettuano un ordine e il pagamento viene verificato, l'inventario per l'articolo deve venire aggiornato. Analogamente, se la carta di credito del cliente viene rifiutata, deve essere eseguito il rollback dell'ordine e l'inventario non deve venire aggiornato. Queste relazioni richiedono tutte le transazioni.

### <a name="photos-and-videos"></a>Foto e video

Le foto e i video in un catalogo prodotti non richiedono il supporto delle transazioni. L'unico motivo per cui può venire apportata una modifica a una foto o a un video è la presenza di un aggiornamento o l'aggiunta di nuovi file. Anche se c'è una relazione tra l'immagine e i dati effettivi del prodotto, la natura non è transazionale.

### <a name="business-data"></a>Dati di business

Per i dati di business, poiché tutti i dati sono cronologici e non cambiano, il supporto delle transazioni non è richiesto. I business analyst che usano i dati hanno esigenze specifiche, in quanto spesso richiedono l'uso delle aggregazioni nelle query, in modo da poter lavorare con i totali di gruppi di dati più piccoli.

## <a name="summary"></a>Riepilogo

Garantire che i dati siano nello stato corretto non è sempre facile. Le transazioni possono risultare utili in quanto permettono di applicare requisiti di integrità dei dati. Se i principi ACID possono essere utili per i dati, è consigliabile scegliere una soluzione di archiviazione che supporti le transazioni.
