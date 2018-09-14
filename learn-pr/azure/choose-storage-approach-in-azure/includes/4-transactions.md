Le applicazioni potrebbe essere necessario raggruppare una serie di aggiornamenti dei dati, perché una modifica a un pezzo di dati deve comportare una modifica a un altro blocco di dati. Le transazioni consentono di raggruppare questi aggiornamenti in modo che, se un evento in una serie di aggiornamenti ha esito negativo, è possibile eseguire il rollback dell'intera serie o annullarla. 

Ad esempio, come un rivenditore online è possibile utilizzare una transazione per il posizionamento di una verifica ordine e il pagamento. Raggruppando gli eventi correlati, è possibile evitare di ridurre le scorte di magazzino fino a quando non si riceve un metodo di pagamento approvato.

In questo caso, si apprenderà sulle transazioni e se sono necessarie per i dati.

## <a name="what-is-a-transaction"></a>Che cos'è una transazione?

Una transazione è un'unità logica che viene eseguita in modo indipendente per il recupero o gli aggiornamenti dei dati.

Ecco la domanda da porsi sulla necessità di utilizzare le transazioni nell'applicazione: una modifica a un pezzo di dati nel set di dati influirà un'altra? Se la risposta è affermativa, allora sarà necessario il supporto per le transazioni del servizio di database.

Le transazioni vengono spesso definite da un set di quattro requisiti, detti anche garanzie ACID. ACID è l'acronimo di Atomicità, Coerenza, Isolamento e Durabilità:

- L'atomicità indica che tutti i dati vengono aggiornati o che tutti i dati viene eseguito il rollback allo stato originale.
- La coerenza garantisce che se si verifica un evento solo tramite la transazione, una parte dei dati non viene escluso dagli aggiornamenti. Tutti i livelli, la transazione viene applicata in modo coerente o non ereditarli affatto.
- L'isolamento garantisce che una transazione non influisca su un'altra transazione.
- La durabilità significa che le modifiche apportate nell'ambito della transazione vengono salvate in modo permanente nel sistema. Il commit dei dati viene salvati dal sistema in modo che anche in caso di un riavvio del sistema e di errore, i dati sono disponibili nello stato corretto.

Quando un database offre garanzie ACID, questi principi vengono applicati a tutte le transazioni in modo coerente.

## <a name="oltp-vs-olap"></a>OLTP e OLAP

I database transazionali vengono spesso denominati sistemi OLTP (Online Transaction Processing, elaborazione delle transazioni online). I sistemi OLTP supportano in genere numerosi utenti, garantiscono tempi di risposta rapidi e possono gestire volumi elevati di dati. Offrono inoltre una disponibilità elevata (ovvero un tempo di inattività minimo) e gestiscono transazioni di piccole dimensioni o relativamente semplici.

Al contrario, i sistemi OLAP (Online Analytical Processing) supportano in genere un numero inferiore di utenti, hanno tempi di risposta più lunghi, possono offrire una minore disponibilità e normalmente gestiscono transazioni di grandi dimensioni e complesse.

Con la frequenza alle precedenti, ma comprendere li rende più semplice classificare le esigenze dell'applicazione non vengono utilizzati i termini OLTP e OLAP. 

Ora che si ha familiarità con le transazioni, OLTP e OLAP, verrà ora illustrato ognuno dei set di dati nello scenario di vendita online e determinare la necessità di transazioni.

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

I dati del catalogo prodotti devono essere archiviati in un database transazionale. Quando gli utenti effettuano un ordine e il pagamento viene verificato, l'inventario per l'articolo deve venire aggiornato. Analogamente, se la carta di credito del cliente viene rifiutata, deve essere eseguito il rollback dell'ordine e l'inventario non deve venire aggiornato. Queste relazioni richiedono tutte le transazioni.

## <a name="photos-and-videos"></a>Foto e video

Foto e video in un catalogo di prodotti non richiedono il supporto delle transazioni. Questi file vengono modificati solo quando viene eseguito un aggiornamento oppure vengono aggiunti nuovi file. Anche se esiste una relazione tra l'immagine e i dati effettivi del prodotto, la natura non è transazionale.

## <a name="business-data"></a>Dati aziendali

Per i dati di business, perché tutti i dati cronologici e non modificabile, il supporto delle transazioni non è obbligatorio. I business analyst che usano i dati hanno esigenze specifiche, in quanto spesso richiedono l'uso delle aggregazioni nelle query, in modo da poter lavorare con i totali di gruppi di dati più piccoli.

## <a name="summary"></a>Riepilogo

Garantire che i dati siano nello stato corretto non è sempre facile. Le transazioni possono risultare utili in quanto permettono di applicare requisiti di integrità dei dati. Se i dati trae vantaggio dai principi ACID, quindi scegliere una soluzione di archiviazione che supporta le transazioni.