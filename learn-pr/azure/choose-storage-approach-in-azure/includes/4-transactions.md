Può essere necessario, per le applicazioni, raggruppare una serie di aggiornamenti dei dati perché la modifica apportata ad alcuni dati comporta la modifica di altri dati. Le transazioni consentono di raggruppare questi aggiornamenti in modo che, se un evento in una serie di aggiornamenti ha esito negativo, sia possibile eseguire il rollback dell'intera serie oppure annullarla. 

Un rivenditore online può ad esempio usare una transazione per l'invio di un ordine e la verifica del pagamento. Raggruppando gli eventi correlati, è possibile evitare di ridurre le scorte di magazzino fino a quando non si riceve una forma di pagamento approvata.

In questo modulo si apprenderà cosa sono le transazioni e se è necessario usarle per i dati.

## <a name="what-is-a-transaction"></a>Cos'è una transazione?

Una transazione è un'unità logica che viene eseguita in modo indipendente per il recupero o gli aggiornamenti dei dati.

La domanda da porsi per determinare se è necessario usare le transazioni nella propria applicazione è la seguente: la modifica di alcuni dati nel set di dati influirà su altri dati? Se la risposta è affermativa, il servizio di database dovrà supportare le transazioni.

Le transazioni vengono spesso definite da un set di quattro requisiti, detti anche garanzie ACID. ACID è l'acronimo di Atomicità, Coerenza, Isolamento e Durabilità:

- L'atomicità indica l'aggiornamento di tutti i dati oppure il relativo rollback allo stato originale.
- La coerenza garantisce che se accade qualcosa nel corso della transazione, una parte dei dati non sia esclusa dagli aggiornamenti. In generale, la transazione viene applicata in modo coerente o non viene applicata affatto.
- L'isolamento garantisce che una transazione non influisca su un'altra transazione.
- La durabilità significa che le modifiche apportate nell'ambito della transazione vengono salvate in modo permanente nel sistema. I dati di cui viene eseguito il commit vengono salvati dal sistema in modo che siano disponibili nello stato corretto anche in caso di errore con conseguente riavvio del sistema.

Quando un database offre garanzie ACID, questi principi vengono applicati a tutte le transazioni in modo coerente.

## <a name="oltp-vs-olap"></a>OLTP e OLAP

I database transazionali vengono spesso denominati sistemi OLTP (Online Transaction Processing, elaborazione delle transazioni online). I sistemi OLTP supportano in genere numerosi utenti, garantiscono tempi di risposta rapidi e possono gestire volumi elevati di dati. Offrono inoltre una disponibilità elevata (ovvero un tempo di inattività minimo) e gestiscono transazioni di piccole dimensioni o relativamente semplici.

Al contrario, i sistemi OLAP (Online Analytical Processing) supportano in genere un numero inferiore di utenti, hanno tempi di risposta più lunghi, possono offrire una minore disponibilità e normalmente gestiscono transazioni complesse di grandi dimensioni.

I termini OLTP e OLAP vengono usati con minore frequenza rispetto al passato, ma la conoscenza di questi concetti semplifica la categorizzazione delle esigenze dell'applicazione. 

Ora che è stata acquisita familiarità con le transazioni e i concetti di OLTP e OLAP, verranno esaminati i singoli set di dati nello scenario di vendita online e verrà determinata la necessità di usare le transazioni.

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

I dati del catalogo prodotti devono essere archiviati in un database transazionale. Quando gli utenti effettuano un ordine e il pagamento viene verificato, l'inventario per l'articolo deve venire aggiornato. Analogamente, se la carta di credito del cliente viene rifiutata, deve essere eseguito il rollback dell'ordine e l'inventario non deve venire aggiornato. Queste relazioni richiedono tutte le transazioni.

## <a name="photos-and-videos"></a>Foto e video

Le foto e i video in un catalogo prodotti non richiedono il supporto delle transazioni. Questi file vengono modificati solo quando viene eseguito un aggiornamento o vengono aggiunti nuovi file. Anche se esiste una relazione tra l'immagine e i dati effettivi del prodotto, questa non è di natura transazionale.

## <a name="business-data"></a>Dati di business

Per i dati di business, poiché tutti i dati sono cronologici e non cambiano, il supporto delle transazioni non è richiesto. I business analyst che usano i dati hanno inoltre esigenze specifiche, nel senso che nelle proprie query spesso devono usare le aggregazioni, in modo da poter lavorare con i totali di altri punti dati più piccoli.

## <a name="summary"></a>Riepilogo

Garantire che i dati siano nello stato corretto non è sempre facile. Le transazioni possono risultare utili in quanto permettono di applicare requisiti di integrità dei dati. Se i principi ACID risultano utili per i dati, è consigliabile scegliere una soluzione di archiviazione che supporti le transazioni.