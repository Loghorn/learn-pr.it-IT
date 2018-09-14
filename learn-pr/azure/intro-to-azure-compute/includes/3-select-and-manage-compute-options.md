Ora che è stato introdotto il concetto di Azure disponibile di servizi di calcolo, esaminiamo ciascuno più da vicino per decidere quando usare ogni servizio.

## <a name="azure-virtual-machines"></a>Macchine virtuali di Azure

Quando è necessario il controllo totale del sistema operativo e l'ambiente, le macchine virtuali sono una scelta ideale. Consentono infatti di personalizzare tutto il software in esecuzione nella macchina virtuale, proprio come con un computer fisico. Ciò è particolarmente ideale durante l'esecuzione di software personalizzato o personalizzato che ospita le configurazioni.

ma anche quando si passa da un server fisico al cloud. È spesso possibile creare un'immagine del server fisico e ospitarla in una macchina virtuale. Si ha così totale libertà di scelta dei sistemi operativi e degli ambienti di esecuzione dell'applicazione, vale a dire che è possibile sviluppare in qualsiasi linguaggio che usi gli strumenti scelti dall'utente.

Sarà tuttavia necessario gestire la macchina virtuale ovvero aggiornare il sistema operativo e il software che esegue. 

È anche necessario valutare l'eventuale ridimensionamento. È possibile aumentare le prestazioni della macchina virtuale, ad esempio aggiungendo risorse di calcolo e memoria. Ma se è necessario eseguire più istanze in parallelo o con scalabilità orizzontale, si potrebbe essere necessario aggiungere altri servizi, ad esempio servizi di bilanciamento del carico.

Si supponga quindi, che si esegue un sito web che consente a data Scientist caricare le immagini di astronomia che devono essere elaborati. Se è stato duplicato della macchina virtuale, è necessario un servizio aggiuntivo per indirizzare le richieste tra più istanze del sito web della macchina virtuale.

Azure rende disponibili anche servizi di macchine virtuali avanzati:

- Usare la **Virtual Machine Scale Sets** opzione quando è necessario eseguire le istanze disponibili in modo coerente della stessa app o i set di App, scegliere in modo analogo configurate macchine virtuali. In pochi minuti, l'opzione genera automaticamente migliaia di macchine virtuali identiche caricate con l'applicazione, evitando agli utenti qualsiasi attesa. Non essendo necessario il pre-provisioning delle macchine virtuali, vengono sempre usate esclusivamente le risorse di calcolo necessarie all'applicazione.

- Usare la **Batch** opzione quando è necessario pianificazione dei processi su scala cloud e gestione di calcolo con la possibilità di scalare fino a decine, centinaia o migliaia di macchine virtuali. Potrebbero verificarsi situazioni in cui è necessario potenza di calcolo non elaborata o potenza di calcolo a livello di supercomputer &mdash; Azure offre queste funzionalità.

## <a name="azure-containers"></a>Contenitori di Azure

Se si vuole eseguire più istanze di un'applicazione in una singola macchina virtuale, i contenitori sono un'ottima scelta. L'agente di orchestrazione del contenitore è in grado di avviare, arrestare e aumentare il numero di istanze dell'applicazione in base alle esigenze.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

Potrebbe non essere evidente inizialmente ciò che la differenza è tra una macchina virtuale e un contenitore.  Il video seguente tenta di spiegare le differenze.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaq]

I contenitori sono in genere usati per creare soluzioni con un'architettura di microservizi I contenitori vengono spesso usati per creare soluzioni utilizzando un'architettura di microservizi, come vengono spesso usati per suddividere le soluzioni in parti più piccole. Ad esempio, si può suddividere un sito web in un contenitore che ospita il front-end, un altro che ospita il back-end e un terzo per l'archiviazione. Ciò consente di separare i componenti dell'app in sezioni logiche che possono quindi essere gestite, ridimensionate o aggiornate in modo indipendente.

Il video seguente tenta di spiegare che un microservizio è.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yual]

Si supponga il sito web di back-end ha raggiunto la capacità, ma non sono sotto sforzo il front-end e l'archiviazione. È possibile ridimensionare il back-end separatamente per migliorare le prestazioni oppure è possibile decidere di usare un servizio di archiviazione diverso. Oppure è possibile sostituire anche il contenitore di archiviazione senza influire sul resto dell'applicazione.

Se il team ha familiarità con l'orchestrazione dei contenitori Kubernetes, è opportuno valutare l'opzione **Azure Kubernetes Service (AKS)**. La soluzione è in grado di semplificare e automatizzare la gestione, la distribuzione e le operazioni di orchestrazione Kubernetes.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuX]

## <a name="azure-functions"></a>Funzioni di Azure

Quando si è interessati solo sul codice in esecuzione il servizio e non il sottostante piattaforma o dell'infrastruttura, funzioni di Azure sono ideali. Funzioni di Azure consente di eseguire operazioni in risposta a un evento, spesso tramite una richiesta REST, un timer o un messaggio proveniente da un altro servizio di Azure e quando l'operazione può essere completata in pochi secondi al massimo.

Funzioni di Azure ridimensiona automaticamente, in modo che siano una scelta a tinta unita quando la domanda variabile e l'addebito viene effettuato solo quando viene attivata una funzione. Si ipotizzi ad esempio di ricevere messaggi da una soluzione IoT usata per monitorare una flotta di mezzi adibiti alle consegne. È probabile che nelle ore lavorative si avrà un incremento dei dati in arrivo.

Inoltre, le funzioni di Azure sono senza state; si comportano come se essi vengono riavviate ogni volta che rispondono a un evento. Per tale motivo è ideale per l'elaborazione dei dati in ingresso. Se è necessario lo stato, i dati possono essere connessi a un servizio di archiviazione di Azure.
