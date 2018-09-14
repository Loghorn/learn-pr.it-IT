I BLOB sono "file per il cloud". Le app funzionano con i BLOB in modo analogo mentre lavorano con i file su disco, ad esempio la lettura e scrittura dei dati. A differenza però di un file locale i BLOB possono essere raggiunti da qualsiasi punto con una connessione Internet.

L'archivio BLOB di Azure *non è strutturato*. Questo significa che non esistono restrizioni ai tipi di dati che un BLOB può contenere. Un BLOB può ad esempio contenere un documento PDF, un'immagine JPG, un file JSON, contenuto video e così via. I BLOB non sono limitati ai formati di file comuni. Un BLOB può contenere gigabyte di dati binari trasmessi da uno strumento scientifico, un messaggio crittografato per un'altra applicazione o può contenere dati in un formato personalizzato per un'app in via di sviluppo.

I BLOB in genere non sono adatti a contenere dati strutturati che sono spesso sottoposti a query. Hanno una latenza maggiore rispetto alla memoria e al disco locale e non hanno le funzionalità di indicizzazione che rendono i database efficienti nell'esecuzione di query. I BLOB vengono comunque usati spesso in *combinazione* con i database per archiviare dati non disponibili per le query. Un'app con un database di profili utente ad esempio può archiviare nei BLOB le immagini dei profili. Ogni record di utente nel database includerà il nome o l'URL del BLOB contenente l'immagine dell'utente.

I BLOB vengono usati per archiviare i dati in molti modi tra tutti i tipi di applicazioni e le architetture:

- App che richiedono la trasmissione di grandi quantità di dati con sistema di messaggistica che supporta solo i messaggi di piccole dimensioni. Queste App possono archiviare dati nei blob e inviare il blob URL nei messaggi.
- L'archivio BLOB viene usato come un file system per l'archiviazione e la condivisione di documenti e altri dati personali.
- Gli asset Web statici come le immagini possono essere archiviati in BLOB ed essere resi disponibili per il download pubblico come se fossero file in un server Web.
- Molti componenti di Azure usano i BLOB dietro le quinte. Azure Cloud Shell ad esempio archivia i file e la configurazione in BLOB e Macchine virtuali di Azure usa i BLOB per l'archiviazione del disco rigido.

Alcune app creano, aggiornano ed eliminano BLOB costantemente come parte del proprio lavoro. Altre usano un piccolo set di BLOB che modificano raramente.

## <a name="storage-accounts-containers-and-metadata"></a>Account di archiviazione, contenitori e metadati

Nell'archivio BLOB ogni BLOB risiede in un *contenitore BLOB*. È possibile archiviare un numero illimitato di BLOB in un contenitore e un numero illimitato di contenitori in un account di archiviazione. I contenitori sono "flat", possono quindi archiviare solo BLOB e non altri contenitori.

I BLOB e i contenitori supportano i metadati nel formato di coppia nome-valore. Le app possono usare i metadati per qualsiasi cosa: una descrizione leggibile del contenuto di un BLOB da visualizzare nell'applicazione, una stringa che l'app usa per determinare per elaborare i dati del BLOB e così via.

> [!TIP]
> L'archiviazione BLOB non include funzionalità per la ricerca e l'ordinamento dei BLOB in base ai metadati. Per informazioni sull'utilizzo di Ricerca di Azure, vedere la sezione Altre informazioni alla fine di questo modulo.

## <a name="the-blob-storage-api-and-client-libraries"></a>API di archiviazione BLOB e librerie client

L'API di archiviazione BLOB è basata su REST e supportata da librerie client in molti linguaggi diffusi. Questa API consente di scrivere app che creano ed eliminano BLOB e contenitori, caricano e scaricano dati di BLOB ed elencano i BLOB in un contenitore.
