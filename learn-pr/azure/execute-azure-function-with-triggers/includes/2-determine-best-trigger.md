Una funzione di Azure non funziona finché un elemento non indicherà di eseguirla. Ad esempio, è possibile creare una funzione di Azure per inviare un promemoria tramite SMS ai clienti prima di un appuntamento. Se non viene abilitata la funzione quando deve essere eseguita, i clienti non riceveranno mai un messaggio.

In questo caso, verranno esaminati i trigger di alto livello ed esplorati i tipi più comuni di trigger.

## <a name="what-is-a-trigger"></a>Che cos'è un trigger?

Un trigger è un servizio che definisce il modo in cui viene richiamata una funzione di Azure. Ad esempio, se si desidera che una funzione venga eseguita ogni 10 minuti, è possibile usare un trigger timer.

Una funzione deve avere esattamente un trigger associato a essa. Se si desidera eseguire una parte di logica in più condizioni, è necessario creare più funzioni.

## <a name="what-is-a-binding"></a>Che cos'è un'associazione?

Un'associazione è una connessione ai dati all'interno della funzione. Le associazioni sono facoltative e sono disponibili sotto forma di associazioni di input e output. Un'associazione di input è costituita dai dati ricevuti dalla funzione. Un'associazione di output è costituita dai dati inviati dalla funzione.

A differenza di un trigger, una funzione può avere più associazioni di input e output.

## <a name="types-of-triggers"></a>Tipi di trigger

Le funzioni di Azure supportano un'ampia gamma di tipi di trigger. Ecco alcuni dei tipi più comuni:

| Tipo | Scopo |
| --- | --- |
| **Timer** | Eseguire una funzione in un intervallo specificato. |
| **HTTP** | Eseguire una funzione quando viene ricevuta una richiesta HTTP. |
| **Blob** | Eseguire una funzione quando un file viene caricato o aggiornato nell'archivio BLOB di Azure. |
| **Coda** | Eseguire una funzione quando un messaggio viene aggiunto a una coda di Archiviazione di Azure. |
| **Cosmos DB** | Eseguire una funzione quando un documento viene modificato in una raccolta. |
| **Hub eventi** | Eseguire una funzione quando un Hub eventi riceve un nuovo evento. |

In questo modulo si approfondiranno i tipi di **timer**, **HTTP** e **blob**.

## <a name="summary"></a>Riepilogo

Per eseguire una funzione di Azure, è necessario usare un trigger. I trigger timer, HTTP e di blob sono tre tipi di trigger tra i più comuni che verranno usati per eseguire la logica senza server.
