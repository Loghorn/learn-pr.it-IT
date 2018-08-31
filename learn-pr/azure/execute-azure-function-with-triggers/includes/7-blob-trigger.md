Si supponga di essere un fotografo con un sito Web in cui si pubblicano le immagini della giornata. A causa dei molti impegni non si dispone di una pianificazione di caricamento coerente, ma si desidera inviare una notifica ai fan quando si carica un'immagine. Si decide di creare una funzione di Azure per inviare automaticamente un tweet ogni volta che si carica un'immagine nel contenitore BLOB dell'archiviazione di Azure.

Qui è possibile ottenere informazioni su come creare un trigger del BLOB e fare in modo che monitori un percorso specifico nel contenitore BLOB dell'archiviazione di Azure.

## <a name="what-is-azure-storage"></a>Che cos'è Archiviazione di Azure?

Archiviazione di Azure è la soluzione di archiviazione cloud di Microsoft che supporta tutti i tipi di dati, inclusi BLOB, code e NoSQL. L'obiettivo di Archiviazione di Azure è fornire un'archiviazione dei dati:

- Ad elevata disponibilità
- Sicura
- Scalabile
- Gestita

Il focus tuttavia non verrà posto su Archiviazione di Azure. Verrà usato solo per creare BLOB che attiveranno l'esecuzione della funzione.

## <a name="what-is-azure-blob-storage"></a>Cos'è l'Archiviazione BLOB di Azure?

Archiviazione BLOB di Azure è una soluzione di archiviazione oggetti progettata per archiviare grandi quantità di dati non strutturati. 

Ad esempio, Archiviazione BLOB di Azure è ideale per eseguire operazioni quali:

- Archiviazione di file.
- Gestione di file.
- Streaming di audio e video.
- Registrazione dei dati.

Esistono tre tipi di BLOB: **BLOB in blocchi**, **BLOB di accodamento** e **BLOB di pagine**. I BLOB in blocchi sono i tipi più comuni. Consentono di archiviare in modo efficiente dati binari o testo. I BLOB di accodamento sono simili ai BLOB in blocchi, ma sono stati progettati più che altro per le operazioni di accodamento, quali la creazione di un file di log che viene continuamente aggiornato. Infine, i BLOB di pagine sono costituiti da pagine e sono progettati per operazioni frequenti di lettura e scrittura casuali.

## <a name="what-is-a-blob-trigger"></a>Che cos'è un trigger del BLOB?

Un trigger del BLOB è un trigger che esegue una funzione quando un file viene caricato o aggiornato nell'archivio BLOB di Azure. Per creare un trigger del BLOB, creare un account di Archiviazione di Azure e fornire un percorso che verrà monitorato dal trigger.

## <a name="how-to-create-a-blob-trigger"></a>Come creare un trigger del blob

Proprio come gli altri trigger trattati fino ad ora, il trigger del BLOB viene creato nel portale di Azure. All'interno della funzione di Azure, selezionare **Trigger del BLOB** dall'elenco dei tipi di trigger predefiniti. Immettere quindi la logica da eseguire quando viene creato o aggiornato un BLOB.

Un'impostazione che è opportuno esaminare è il **Percorso**. Il **Percorso** indica al trigger del BLOB la posizione da monitorare per vedere se un BLOB viene caricato o aggiornato. Per impostazione predefinita, il valore di **Percorso** è: 

> samples-workitems/{name}

Se si scompone questo concetto in due parti, si ottiene *samples-workitems* e *{name}*. La prima parte, *samples-workitems*, rappresenta il contenitore BLOB monitorato dal trigger. La seconda parte, *{name}*, significa che tutti i tipi di file determinano la richiamata della funzione da parte del trigger. La funzione viene richiamata perché non è presente alcun filtro. Ad esempio, il trigger può richiamare la funzione solo se viene aggiunto un file PNG utilizzando una sintassi del tipo:

> samples-workitems/{name}.png

L'ultima parte di informazioni significative all'interno di questo concetto è il testo *name*. *name* rappresenta un parametro nella funzione di Azure che riceve il nome del file aggiunto. Ad esempio, se si carica un file denominato *resume.txt*, la funzione di Azure riceve il valore sotto forma di stringa tramite un parametro denominato *name*.

## <a name="summary"></a>Riepilogo

Un trigger del BLOB richiama una funzione di Azure quando rileva attività in una posizione specifica nell'account BLOB di Archiviazione di Azure. Per impostare il percorso da monitorare, modificare il valore **Percorso** nel portale di Azure.
