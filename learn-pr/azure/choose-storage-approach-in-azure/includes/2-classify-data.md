In un'azienda di vendita online, possono esserci diversi tipi di dati. Alcuni dati potrebbero essere strutturati, ad esempio le informazioni di contatto dei clienti, mentre altri dati potrebbero essere non strutturati, ad esempio un'immagine di un prodotto. Ci sono tre categorie di dati: strutturati, semistrutturati e non strutturati. Per ogni categoria di dati può essere più vantaggiosa una soluzione di archiviazione specifica.

In questo modulo si apprenderà come classificare i dati nelle categorie appropriate, in modo che sia possibile scegliere la soluzione di archiviazione appropriata.

## <a name="structured-data"></a>Dati strutturati

I dati strutturati sono dati che rispettano uno schema e quindi hanno tutti gli stessi campi o proprietà. I dati strutturati possono essere archiviati in una tabella di database con righe e colonne. I dati strutturati si basano sulle chiavi per mettere in correlazione una riga in una tabella con i dati correlati in un'altra riga di un'altra tabella. I dati strutturati sono anche noti come dati relazionali, in quanto lo schema definisce le tabelle di dati e i campi nelle tabelle, oltre che una chiara relazione tra i due elementi.

I dati strutturati sono semplici, nel senso che l'immissione, l'esecuzione di query e l'analisi risultano facilitate, in quanto tutti i dati hanno lo stesso formato.

Esempi di dati strutturati includono:

- Dati di sensori
- Dati finanziari

## <a name="semi-structured-data"></a>Dati semistrutturati

I dati semistrutturati sono meno organizzati rispetto ai dati strutturati e non vengono archiviati in database relazionali, in quanto i campi non si adattano perfettamente a tabelle, righe e colonne. I dati semistrutturati contengono tag che creano un'organizzazione e una gerarchia apparenti all'interno dei dati.  

I dati semistrutturati sono definiti anche dati non relazionali o dati NoSQL.

Esempi di dati semistrutturati includono:

- File JSON
- File XML

## <a name="unstructured-data"></a>Dati non strutturati

L'organizzazione dei dati non strutturati è in genere ambigua quando si osservano i dati. I dati non strutturati vengono spesso immessi sotto forma di file, ad esempio foto o video. Il file video stesso può presentare una struttura complessiva e includere metadati semistrutturati, ma i dati che costituiscono il video stesso sono non strutturati. Di conseguenza, le foto, i video e altri file simili sono classificati come dati non strutturati.

Esempi di dati non strutturati includono:

- File multimediali, come foto, video e file audio
- File di Office, come documenti di Word
- File di testo
- File di log

Ora che si conoscono le differenze tra le diverse tipologie di dati, è possibile esaminare i set di dati usati in un'app di vendita online e classificarli.

### <a name="product-catalog-data"></a>Dati del catalogo prodotti

I dati del catalogo prodotti per un rivenditore sono piuttosto strutturati di natura, perché ogni prodotto ha un codice SKU, una descrizione, una quantità, un prezzo, opzioni di misure, opzioni di colore, una foto e in alcuni casi un video. Si tratta quindi di dati relazionali, in quanto hanno tutti la stessa struttura. Man mano che si introducono nuovi prodotti, tuttavia, potrebbe essere necessario aggiungere altri campi. Ad esempio, si potrebbero introdurre nuove scarpe da tennis con una funzionalità Bluetooth per l'inoltro dei dati dei sensori dalla scarpa a un'app di fitness nel telefono dell'utente con funzione di contapassi. Si tratta di una tendenza sempre più diffusa, quindi in futuro si vuole consentire ai clienti di applicare un filtro per cercare le scarpe con "funzionalità Bluetooth". Non si vuole tornare indietro e aggiornare tutti i dati delle scarpe esistenti con la proprietà relativa alla funzionalità Bluetooth, ma si vuole semplicemente aggiungere questa caratteristica alle nuove scarpe.

Con l'aggiunta del campo relativo alla funzionalità Bluetooth, i dati delle scarpe non sono più eterogenei, in quanto sono state introdotte differenze nello schema. Se si trattasse di un caso unico e si pensasse che sarà l'unica eccezione, si potrebbe tornare indietro e normalizzare i dati esistenti in modo che tutti i prodotti includano un campo relativo alla funzionalità Bluetooth. Tuttavia, questo è solo uno dei molti campi di specializzazione che si prevede di supportare in futuro. Di conseguenza, questi dati sono classificati come semistrutturati. I dati sono organizzati in base a tag, ma ogni prodotto nel catalogo può contenere campi univoci.

Classificazione dei dati: **semistrutturati**

### <a name="photos-and-videos"></a>Foto e video

La foto e i video visualizzati nelle pagine dei prodotti sono dati non strutturati. Anche se il file multimediale può contenere metadati, il corpo del file multimediale è non strutturato.

Classificazione dei dati: **non strutturati**

### <a name="business-data"></a>Dati di business

I business analyst desiderano implementare la business intelligence per eseguire valutazioni delle pipeline di inventario e revisioni dei dati di vendita. Per eseguire queste operazioni, i dati di più mesi devono essere aggregati e quindi è necessario eseguire query su di essi. Vista la necessità di aggregare i dati, questi dati devono essere strutturati in modo che un mese possa essere confrontato con il successivo.

Classificazione dei dati: **strutturati**

## <a name="summary"></a>Riepilogo

Ci sono tre categorie di dati: strutturati, semistrutturati e non strutturati. È utile comprendere le differenze e determinare la categoria dei dati in uso per scegliere la soluzione di archiviazione appropriata. I dati strutturati sono dati organizzati che si adattano perfettamente alle righe e alle colonne delle tabelle. I dati semistrutturati sono comunque organizzati e hanno proprietà e valori ben definiti, ma possono variare. I dati non strutturati non si adattano alle tabelle e non hanno uno schema.