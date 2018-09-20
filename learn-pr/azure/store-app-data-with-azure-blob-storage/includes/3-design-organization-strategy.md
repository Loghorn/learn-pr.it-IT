Quando si progetta un'app che richiede l'archiviazione di dati, è importante pensare al modo in cui l'app organizzerà i dati tra gli account di archiviazione, i contenitori e i BLOB.

## <a name="storage-accounts"></a>Account di archiviazione

Un singolo account di archiviazione è abbastanza flessibile per organizzare i BLOB nel modo desiderato, ma è consigliabile usare altri account di archiviazione, a seconda delle esigenze, per controllare l'accesso ai dati e separare i costi in modo logico.

## <a name="containers-and-blobs"></a>Contenitori e BLOB

La strategia per la denominazione e l'organizzazione di contenitori e BLOB dovrebbe essere determinata dalla natura dell'applicazione e dai dati archiviati.

Le app che usano i BLOB come parte di uno schema di archiviazione con database non devono normalmente basarsi sull'organizzazione, la denominazione o i metadati per fornire indicazioni sui propri dati. Le app di questo tipo usano in genere come nomi di BLOB degli identificatori, ad esempio GUID, e fanno riferimento a tali identificatori nei record di database. Per determinare la posizione di archiviazione del BLOB e il tipo di dati contenuti viene usato il database.

Altre app possono usare l'archiviazione BLOB di Azure in modo analogo a un sistema di file personali, in cui i nomi di contenitori e BLOB vengono usati per indicare il significato e la struttura. I nomi di BLOB in questi tipi di app hanno spesso un aspetto simile ai nomi di file tradizionali e includono le estensioni dei nomi di file, ad esempio `.jpg`, per indicare il tipo di dati che contengono. Per organizzare i BLOB vengono usate directory virtuali (vedere di seguito) e per archiviare le informazioni su BLOB e contenitori vengono spesso usati tag di metadati.

Vi sono alcuni aspetti da considerare quando si decide come organizzare e archiviare BLOB e contenitori.

### <a name="naming-limitations"></a>Limitazioni di denominazione

I nomi di contenitori e BLOB devono essere conformi a un set di regole, incluse le limitazioni di lunghezza e di caratteri. Vedere la sezione Altre informazioni alla fine di questo modulo per informazioni più dettagliate sulle regole di denominazione.

### <a name="public-access-and-containers-as-security-boundaries"></a>Accesso pubblico e contenitori come limiti di sicurezza

Per impostazione predefinita, tutti i BLOB richiedono l'autenticazione per l'accesso. È tuttavia possibile configurare singoli contenitori in modo da consentire il download pubblico dei BLOB senza autenticazione. Questa funzionalità supporta molti casi d'uso, ad esempio l'hosting di asset di sito Web statici e la condivisione di file. Il download del contenuto di BLOB funziona infatti come la lettura di qualsiasi altro tipo di dati sul Web. È sufficiente puntare un browser o qualsiasi elemento che può eseguire una richiesta GET all'URL del BLOB.

L'abilitazione dell'accesso pubblico è importante per la scalabilità perché i dati scaricati direttamente da un'archiviazione BLOB non generano traffico nell'app sul lato server. Anche se non si sfrutta immediatamente l'accesso pubblico o se si prevede di usare un database per controllare l'accesso ai dati tramite l'applicazione, pianificare l'uso di contenitori separati per i dati da rendere disponibili pubblicamente.

> [!CAUTION]
> I BLOB presenti in un contenitore configurato per l'accesso pubblico possono essere scaricati senza alcun tipo di autenticazione o controllo da qualsiasi utente che conosca i relativi URL di archiviazione. Non inserire mai i dati dei BLOB in un contenitore pubblico che non si prevede di condividere pubblicamente.

Oltre all'accesso pubblico, Azure offre una funzionalità di firma di accesso condiviso che consente un controllo dettagliato delle autorizzazioni sui contenitori. Una maggiore precisione nel controllo degli accessi consente un ulteriore miglioramento della scalabilità. È pertanto utile pensare in generale ai contenitori come limiti di sicurezza.

### <a name="blob-name-prefixes-virtual-directories"></a>Prefissi dei nomi di BLOB (directory virtuali)

Tecnicamente, i contenitori sono "piatti" e non supportano alcun tipo di nidificazione o gerarchia. Se tuttavia si assegnano ai BLOB nomi gerarchici simili a percorsi di file, ad esempio `finance/budgets/2017/q1.xls`, l'operazione dell'API che genera l'elenco dei nomi può filtrare i risultati in base a prefissi specifici. È così possibile esaminare l'elenco in modo analogo a un sistema gerarchico di file e cartelle.

Questa funzionalità è spesso indicata con il termine *directory virtuali* perché viene usata da alcuni strumenti e librerie client per visualizzare ed esplorare le risorse di archiviazione BLOB come un file system. L'esplorazione di ogni cartella attiva una chiamata separata per elencare i BLOB presenti in tale cartella.

L'uso di nomi simili ai file per i BLOB è una tecnica comune per l'organizzazione e l'esplorazione di dati di BLOB complessi.

### <a name="blob-types"></a>Tipi di BLOB

Esistono tre diversi tipi di BLOB in cui è possibile archiviare i dati:

- I **BLOB in blocchi** sono costituiti da blocchi di dimensioni diverse che possono essere caricati in modo indipendente e in parallelo. La scrittura in un BLOB in blocchi comporta il caricamento dei dati nei blocchi e l'esecuzione del commit nel BLOB.
- I **BLOB di accodamento** sono BLOB in blocchi specializzati che supportano solo l'aggiunta di nuovi dati (non l'aggiornamento o l'eliminazione di dati esistenti), ma con un notevole grado di efficienza. I BLOB di accodamento sono ideali per scenari quali l'archiviazione di log o la scrittura di dati di streaming.
- I **BLOB di pagine** sono progettati per gli scenari che prevedono operazioni di lettura e scrittura ad accesso casuale. I BLOB di pagine vengono usati per archiviare i file di disco rigido virtuale (VHD) usati da Macchine virtuali di Azure, ma sono efficaci per qualsiasi scenario che comporta l'accesso casuale.

I BLOB in blocchi sono la soluzione ideale per la maggior parte degli scenari che non richiedono specificamente BLOB di accodamento o BLOB di pagine. La struttura basata su blocchi supporta operazioni di caricamento e download molto rapide e accesso efficiente a singoli elementi di un BLOB. La gestione e il commit dei blocchi sono gestiti automaticamente dalla maggior parte delle librerie client che, in alcuni casi, gestiscono le operazioni di caricamento e download in parallelo per ottimizzare le prestazioni.