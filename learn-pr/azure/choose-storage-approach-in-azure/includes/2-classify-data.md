Un'azienda di vendita online ha diversi tipi di dati. Ogni tipo di dati può trarre vantaggio da una soluzione di archiviazione diverso. 

I dati dell'applicazione possono essere classificati in tre modi diversi: strutturati, semistrutturati e strutturati. In questo caso, verrà illustrato come classificare i dati in modo che sia possibile scegliere la soluzione di archiviazione appropriato.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuY]

## <a name="structured-data"></a>Dati strutturati

I dati strutturati sono dati che rispettano uno schema e quindi hanno tutti gli stessi campi o proprietà. I dati strutturati possono essere archiviati in una tabella di database con righe e colonne. I dati strutturati si basa sulle chiavi per indicare come una riga in una tabella è correlato ai dati in un'altra riga di un'altra tabella. I dati strutturati sono detto anche ai dati relazionali, come schema di dati definisce la tabella di dati, i campi della tabella e di chiara relazione tra i due.

I dati strutturati sono semplici, nel senso che l'immissione, l'esecuzione di query e l'analisi risultano facilitate, in quanto tutti i dati hanno lo stesso formato.

Esempi di dati strutturati includono:

- Dati di sensori
- Dati finanziari

## <a name="semi-structured-data"></a>Dati semistrutturati

Dati semistrutturati sono organizzati minore rispetto a dati strutturati e non viene archiviati in un formato relazionale, come i campi non rientrano esattamente in tabelle, righe e colonne. Dati semistrutturati contengono tag che rendono evidente l'organizzazione e della gerarchia dei dati. I dati semistrutturati sono definiti anche dati non relazionali o dati NoSQL.

### <a name="what-is-nosql"></a>Che cos'è NoSQL?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvd]

Esempi di dati semistrutturati includono:

- Chiave / valore coppie
- Dati a grafo
- File JSON
- File XML

## <a name="unstructured-data"></a>Dati non strutturati

L'organizzazione dei dati non strutturati è ambiguo a livello generale. Dati non strutturati vengono inviati spesso nei file, ad esempio foto o video. Il file video stesso può presentare una struttura complessiva e includere metadati semistrutturati, ma i dati che costituiscono il video stesso sono non strutturati. Di conseguenza, le foto, i video e altri file simili sono classificati come dati non strutturati.

Esempi di dati non strutturati includono:

- File multimediali, come foto, video e file audio
- File di Office, come documenti di Word
- File di testo
- File di log

Dopo avere appreso le differenze tra ogni tipo di dati, è possibile esaminare i set di dati usati in un'azienda di vendita online e averli classificati.

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

Dati del catalogo di prodotti per un'azienda di vendita online sono piuttosto strutturati in natura, perché ogni prodotto dispone di un SKU del prodotto, una descrizione, una quantità, un prezzo, opzioni di dimensioni, le opzioni di colore, una foto e possibilmente un video. Si tratta quindi di dati relazionali, in quanto hanno tutti la stessa struttura. Tuttavia, quando si introducono nuovi prodotti o tipi diversi di prodotti, è possibile aggiungere campi diversi come passare del tempo. Nuovo scarpe tennis in esecuzione, ad esempio, sono abilitato Bluetooth, per inoltrare i dati di sensori dal weekend a un'app di idoneità al telefono dell'utente. Questa sembra essere una tendenza in crescita, e si desidera consentire ai clienti di filtrare in base a "Funzione Bluetooth attivata" shoes in futuro. Non si desidera tornare indietro e aggiornare tutti i dati di casa esistente con una proprietà di funzione Bluetooth attivata, si desidera semplicemente aggiungerlo al nuovo shoes.

Con l'aggiunta della proprietà funzione Bluetooth attivata, i dati di casa non sono più omogenei, come è stato introdotto il concetto le differenze nello schema. Se questo è l'unica eccezione che si prevede di verifica, è possibile tornare indietro e normalizzare i dati esistenti in modo che tutti i prodotti inclusi un campo "Funzione Bluetooth attivata" per mantenere un'organizzazione strutturata e relazionale. Tuttavia, se questo è solo uno dei molti campi specializzati che hai immaginato che supportano in futuro, la classificazione dei dati è semi-strutturata. I dati sono organizzati per tag, ma ogni prodotto nel catalogo può contenere campi univoci.

Classificazione dei dati: **semistrutturati**

## <a name="photos-and-videos"></a>Foto e video

La foto e i video visualizzati nelle pagine dei prodotti sono dati non strutturati. Anche se il file multimediale può contenere metadati, il corpo del file multimediale è non strutturato.

Classificazione dei dati: **non strutturati**

## <a name="business-data"></a>Dati di business

I business analyst desiderano implementare la business intelligence per eseguire valutazioni delle pipeline di inventario e revisioni dei dati di vendita. Per eseguire queste operazioni, i dati di più mesi devono essere aggregati e quindi è necessario eseguire query su di essi. A causa della necessità di aggregare i dati simili, questi dati devono essere strutturati, in modo che un mese può essere confrontato con quella successiva.

Classificazione dei dati: **strutturati**

## <a name="summary"></a>Riepilogo

I dati possono essere classificati in tre modi diversi: strutturati, semistrutturati e strutturati. Comprendere le differenze in modo che è possibile classificare i propri dati, sarà possibile scegliere la soluzione di archiviazione corretto. 

Ricapitolando, dati strutturati sono organizzati i dati che si adatta perfettamente in righe e colonne nelle tabelle. I dati semistrutturati sono comunque organizzati e hanno proprietà e valori ben definiti, ma possono variare. I dati non strutturati non si adattano alle tabelle e non hanno uno schema.