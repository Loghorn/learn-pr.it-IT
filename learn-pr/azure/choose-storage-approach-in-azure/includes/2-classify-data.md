Un'azienda di vendita online presenta diversi tipi di dati. Per ogni tipo di dati può essere utile una soluzione di archiviazione specifica. 

I dati delle applicazioni possono essere classificati in uno di tre modi: strutturati, semistrutturati e non strutturati. In questo modulo si apprenderà come classificare i dati in modo da poter scegliere la soluzione di archiviazione appropriata.

## <a name="structured-data"></a>Dati strutturati

I dati strutturati sono dati che rispettano uno schema e quindi hanno tutti gli stessi campi o proprietà. I dati strutturati possono essere archiviati in una tabella di database con righe e colonne. I dati strutturati si basano sulle chiavi per indicare che una riga in una tabella è correlata ai dati presenti in un'altra riga di un'altra tabella. I dati strutturati sono anche noti come dati relazionali, in quanto lo schema dei dati definisce la tabella di dati, i campi nelle tabelle e la chiara relazione tra i due elementi.

I dati strutturati sono semplici, nel senso che l'immissione, l'esecuzione di query e l'analisi risultano facilitate, in quanto tutti i dati hanno lo stesso formato.

Esempi di dati strutturati includono:

- Dati di sensori
- Dati finanziari

## <a name="semi-structured-data"></a>Dati semistrutturati

I dati semistrutturati sono meno organizzati rispetto ai dati strutturati e non sono archiviati in un formato relazionale, in quanto i campi non si adattano perfettamente a tabelle, righe e colonne. I dati semistrutturati contengono tag che ne evidenziano l'organizzazione e la gerarchia. I dati semistrutturati sono anche definiti dati non relazionali o NoSQL.

Esempi di dati semistrutturati includono:

- File JSON
- File XML

## <a name="unstructured-data"></a>Dati non strutturati

L'organizzazione dei dati non strutturati è in genere ambigua. I dati non strutturati vengono spesso distribuiti in file, ad esempio foto o video. Il file video può presentare una struttura complessiva e includere metadati semistrutturati, ma i dati che costituiscono il video sono non strutturati. Di conseguenza, le foto, i video e altri file simili sono classificati come dati non strutturati.

Esempi di dati non strutturati includono:

- File multimediali, come foto, video e file audio
- File di Office, come documenti di Word
- File di testo
- File di log

Ora che si conoscono le differenze tra le diverse tipologie di dati, è possibile esaminare i set di dati usati in un'azienda di vendita online e classificarli.

## <a name="product-catalog-data"></a>Dati del catalogo prodotti

I dati del catalogo prodotti di un'azienda di vendita online sono piuttosto strutturati di natura, perché ogni prodotto ha un codice SKU, una descrizione, una quantità, un prezzo, opzioni di misure, opzioni di colore, una foto e in alcuni casi un video. Si tratta quindi di dati relazionali, in quanto hanno tutti la stessa struttura. Man mano che si introducono nuovi prodotti o tipi di prodotti diversi, tuttavia, potrebbe essere necessario aggiungere campi diversi. Ad esempio, si potrebbero introdurre nuove scarpe da tennis con una funzionalità Bluetooth per l'inoltro dei dati dei sensori dalla scarpa a un'app di fitness nel telefono dell'utente. Si tratta di una tendenza sempre più diffusa, quindi in futuro si vuole consentire ai clienti di applicare un filtro per cercare le scarpe con "funzionalità Bluetooth". Non si vuole tornare indietro e aggiornare tutti i dati delle scarpe esistenti con una proprietà relativa alla funzionalità Bluetooth, ma si vuole semplicemente aggiungere questa caratteristica alle nuove scarpe.

Con l'aggiunta della proprietà relativa alla funzionalità Bluetooth, i dati delle scarpe non sono più omogenei, in quanto sono state introdotte differenze nello schema. Se si trattasse dell'unica eccezione prevista, si potrebbe tornare indietro e normalizzare i dati esistenti in modo che tutti i prodotti includano un campo relativo alla funzionalità Bluetooth al fine di mantenere un'organizzazione relazionale strutturata. Tuttavia, se questo è solo uno dei molti campi di specializzazione che si prevede di supportare in futuro, questi dati vengono classificati come semistrutturati. I dati sono organizzati in base a tag, ma ogni prodotto nel catalogo può contenere campi univoci.

Classificazione dei dati: **semistrutturati**

## <a name="photos-and-videos"></a>Foto e video

La foto e i video visualizzati nelle pagine dei prodotti sono dati non strutturati. Anche se il file multimediale può contenere metadati, il corpo del file multimediale è non strutturato.

Classificazione dei dati: **non strutturati**

## <a name="business-data"></a>Dati di business

I business analyst desiderano implementare la business intelligence per eseguire valutazioni delle pipeline di inventario e revisioni dei dati di vendita. Per eseguire queste operazioni, è necessario aggregare i dati di più mesi sui quali verranno successivamente eseguite le query. Vista la necessità di aggregare dati simili, questi dati devono essere strutturati in modo che un mese possa essere confrontato con il successivo.

Classificazione dei dati: **strutturati**

## <a name="summary"></a>Riepilogo

I dati possono essere classificati in uno di tre modi: strutturati, semistrutturati e non strutturati. La conoscenza delle differenze per poter classificare i propri dati consente di scegliere la soluzione di archiviazione corretta. 

In sintesi, i dati strutturati sono dati organizzati che si adattano perfettamente alle righe e alle colonne delle tabelle. I dati semistrutturati sono comunque organizzati e hanno proprietà e valori ben definiti, ma possono variare. I dati non strutturati non si adattano alle tabelle e non hanno uno schema.