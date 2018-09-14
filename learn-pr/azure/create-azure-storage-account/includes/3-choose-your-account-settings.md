Le impostazioni dell'account di archiviazione illustrate in precedenza si applicano ai servizi dati nell'account. Questo articolo, illustreremo le tre impostazioni applicabili per l'account stesso, anziché per i dati archiviati nell'account:

- Nome
- Modello di distribuzione
- Tipo di account

Queste impostazioni influiscono sulla modalità di gestione di account e il costo dei servizi in esso contenuti.

## <a name="name"></a>Nome

Ogni account di archiviazione ha un nome. Il nome deve essere globalmente univoco, usare solo lettere minuscole e numeri ed essere compresa tra 3 e 24 caratteri.

## <a name="deployment-model"></a>Modello di distribuzione

Un _modello di distribuzione_ è il sistema usato da Azure per organizzare le risorse. Definisce l'API utilizzabile per creare, configurare e gestire tali risorse. Azure offre due modelli di distribuzione:

- **Resource Manager**: il modello corrente che usa l'API di Azure Resource Manager
- **Classico**: un'offerta di tipo legacy che usa l'API di gestione di servizi di Azure

La decisione su quale scegliere è generalmente semplice perché le risorse di Azure più compatibili solo con Resource Manager. Tuttavia, gli account di archiviazione, le macchine virtuali e reti virtuali supportano entrambi, pertanto è necessario scegliere uno o l'altro quando si crea l'account di archiviazione.

La differenza fondamentale tra i due modelli è il supporto per il raggruppamento. Il modello di Resource Manager aggiunge il concetto di una _gruppo di risorse_, che non è disponibile nel modello classico. Un gruppo di risorse consente di distribuire e gestire una raccolta di risorse come unità singola.

Microsoft consiglia di usare **Resource Manager** per tutte le nuove risorse.

## <a name="account-kind"></a>Tipo di account

Il _tipo_ di account di archiviazione è un set di criteri che determinano quali servizi dati è possibile includere nell'account e i relativi prezzi. Sono disponibili tre tipi di account di archiviazione:

- **Archiviazione v2 (utilizzo generico v2)**: l'offerta corrente che supporta tutti i tipi di archiviazione e tutte le funzionalità più recenti
- **Archiviazione (utilizzo generico v1)**: un tipo legacy che supporta tutti i tipi di archiviazione, ma potrebbe non supportare tutte le funzionalità
- **Archivio BLOB**: un tipo legacy che consente solo i BLOB in blocchi e BLOB di aggiunta

Microsoft consiglia di usare la **per utilizzo generico v2** opzione per i nuovi account di archiviazione.

Esistono alcuni casi speciali che possono costituire un'eccezione a questa regola. Ad esempio, i prezzi per le transazioni sono inferiori in utilizzo generico v1, che consentirebbe di ridurre leggermente i costi se che corrisponde al tipico carico di lavoro.

## <a name="summary"></a>Riepilogo

Il consiglio generale è quello di scegliere il modello di distribuzione di **Resource Manager** e il tipo di account di **archiviazione v2 (utilizzo generico v2)** per tutti gli account di archiviazione. Le altre opzioni sono ancora disponibili principalmente per garantire la continuità del funzionamento delle risorse esistenti. Per le nuove risorse, esistono alcuni motivi da prendere in considerazione le altre opzioni.