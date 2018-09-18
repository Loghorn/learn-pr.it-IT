Le impostazioni dell'account di archiviazione illustrate in precedenza si applicano ai servizi dati nell'account. Questo articolo presenterà le tre impostazioni applicabili all'account stesso, anziché ai dati archiviati nell'account:

- Nome
- Modello di distribuzione
- Tipo di account

Queste impostazioni influiscono sulla modalità di gestione dell'account e sul costo dei relativi servizi.

## <a name="name"></a>Nome

Ogni account di archiviazione ha un nome. Il nome deve essere univoco a livello globale, usare solo numeri e lettere minuscole e avere una lunghezza compresa tra 3 e 24.

## <a name="deployment-model"></a>Modello di distribuzione

Un _modello di distribuzione_ è il sistema usato da Azure per organizzare le risorse. Definisce l'API utilizzabile per creare, configurare e gestire tali risorse. Azure offre due modelli di distribuzione:

- **Resource Manager**: il modello corrente che usa l'API di Azure Resource Manager
- **Classica**: un'offerta di tipo legacy che usa l'API Gestione dei servizi di Azure

In genere, è semplice scegliere quale adottare, dal momento che la maggior parte delle risorse di Azure può essere usata solo con Resource Manager. Tuttavia, gli account di archiviazione, le macchine virtuali e le reti virtuali li supportano entrambi, quindi è necessario scegliere l'uno o l'altro quando si crea l'account di archiviazione.

La differenza fondamentale tra i due modelli è il supporto per il raggruppamento. Il modello di Resource Manager aggiunge il concetto di un _gruppo di risorse_ che non è disponibile nel modello classico. Un gruppo di risorse consente di distribuire e gestire una raccolta di risorse come unità singola.

Per tutte le nuove risorse, Microsoft consiglia di usare **Resource Manager**.

## <a name="account-kind"></a>Tipo di account

Il _tipo_ di account di archiviazione è un set di criteri che determinano quali servizi dati è possibile includere nell'account e i relativi prezzi. Sono disponibili tre tipi di account di archiviazione:

- **Archiviazione v2 (utilizzo generico v2)**: l'offerta corrente che supporta tutti i tipi di archiviazione e tutte le funzionalità più recenti
- **Archiviazione (utilizzo generico v1)**: un tipo legacy che supporta tutti i tipi di archiviazione, ma potrebbe non supportare tutte le funzionalità
- **Archiviazione BLOB**: un tipo legacy che consente solo BLOB in blocchi e BLOB di accodamento

Per i nuovi account di archiviazione, Microsoft consiglia di usare l'opzione **Utilizzo generico v2**.

Esistono alcuni casi speciali che possono costituire un'eccezione a questa regola. Ad esempio, i prezzi per le transazioni sono inferiori per gli account per utilizzo generico v1, il che consentirebbe di ridurre leggermente i costi in caso di corrispondenza con il carico di lavoro tipico.

## <a name="summary"></a>Riepilogo

Il consiglio generale è quello di scegliere il modello di distribuzione di **Resource Manager** e il tipo di account di **archiviazione v2 (utilizzo generico v2)** per tutti gli account di archiviazione. Le altre opzioni sono ancora disponibili principalmente per garantire la continuità del funzionamento delle risorse esistenti. Per le nuove risorse, ci sono ben pochi motivi per prendere in considerazione le altre opzioni.