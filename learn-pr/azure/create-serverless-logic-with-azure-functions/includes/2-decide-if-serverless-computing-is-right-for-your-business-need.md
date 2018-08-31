Prima di creare un servizio dati di temperatura, è consigliabile verificare che l'elaborazione senza server sia la soluzione più adatta al processo. 

## <a name="what-is-serverless-compute"></a>Cosa significa "elaborazione senza server"?
L'elaborazione senza server usa le funzioni come servizio (FaaS). Le funzioni possono essere considerate come dei microservizi ospitati in una piattaforma cloud. La funzione contiene il codice per la logica di business che può essere eseguito senza eseguire manualmente il provisioning o scalare l'infrastruttura. La piattaforma di hosting è astratta; non sono presenti server a cui accedere direttamente né ridimensionamenti da stimare. 

## <a name="what-is-azure-functions"></a>Che cos'è Funzioni di Azure?
Funzioni di Azure è una piattaforma applicativa senza server, che consente agli sviluppatori di ospitare la logica di business che potrà essere eseguita senza occuparsi del provisioning dell'infrastruttura. Funzioni di Azure offre scalabilità intrinseca e fatturazione solo per le sole risorse usate. Il codice per Funzioni di Azure può essere scritto in diversi linguaggi tra cui C#, F# e JavaScript; supporta NuGet e NPM e consente pertanto di includere le librerie più diffuse. 

## <a name="when-do-you-use-serverless-compute"></a>Quando è consigliata l'elaborazione senza server?
L'elaborazione senza server è una valida opzione per ospitare nel cloud il codice per la logica di business. Offre ridimensionamento automatico, assenza di server da gestire, fatturazione inferiore al secondo, un'ampia possibilità di scelta tra numerosi linguaggi di programmazione per l'implementazione e altre funzionalità illustrate di seguito.

### <a name="avoid-overallocation-of-infrastructure"></a>Evitare la sovrassegnazione dell'infrastruttura
Si supponga di aver eseguito il provisioning di server delle macchine virtuali e di averli configurati con risorse sufficienti a gestire i picchi del carico. Quando il carico diminuisce, l'utente paga per un'infrastruttura che non usa. Grazie al ridimensionamento automatico, l'elaborazione senza server contribuisce a risolvere il problema della sovrassegnazione. Vengono così addebitate solo le funzioni in esecuzione.

### <a name="stateless-logic"></a>Logica senza stato
Le funzioni senza stato sono particolarmente adatte all'elaborazione senza server, in quanto le istanze della funzione vengono create ed eliminate su richiesta. Se lo stato è necessario, può essere archiviato in un servizio di archiviazione associato.

### <a name="event-driven"></a>Basate su eventi
Le funzioni di Azure sono basate su eventi; vengono eseguite solo in risposta a un evento, ad esempio la ricezione di una richiesta HTTP o un messaggio da aggiungere a una coda. La configurazione di Funzioni di Azure semplifica notevolmente la base di codici consentendo di dichiarare la provenienza dei dati (associazione di trigger/input) e la loro destinazione (associazione di output). Non è necessario codice scritto per osservare code, BLOB, hub e così via e l'utente può concentrarsi esclusivamente sulla logica di business.

## <a name="when-do-you-not-use-serverless-compute"></a>Quando non è consigliata l'elaborazione senza server?
L'elaborazione senza server non è sempre la soluzione appropriata per l'hosting la logica di business. Di seguito vengono illustrate alcune caratteristiche di Funzioni di Azure che possono influenzare la decisione di ospitare i servizi usando l'elaborazione senza server. 

### <a name="execution-time"></a>Tempo di esecuzione
Per impostazione predefinita, le funzioni di Azure hanno un timeout di 5 minuti, configurabile fino a un massimo di 10 minuti. Se l'esecuzione della funzione richiede più di 10 minuti, è possibile ospitarla su una macchina virtuale. Se, inoltre, il servizio viene avviato da una richiesta HTTP e si prevede tale valore come risposta HTTP, il timeout viene ulteriormente ridotto a 2,5 minuti.

### <a name="execution-frequency"></a>Frequenza di esecuzione
Il secondo aspetto da valutare è la frequenza di esecuzione. Se si prevede che la funzione venga eseguita in modo continuo da più client, è opportuno stimarne l'utilizzo e calcolare il costo che deriva dall'uso delle funzioni di Azure. Ospitare il servizio in una macchina virtuale potrebbe essere più conveniente.

Durante il ridimensionamento è possibile creare solo un'istanza dell'app per le funzioni ogni 10 secondi, per un massimo di 200 istanze totali. Tenere presente che ogni istanza può prevedere più esecuzioni simultanee, pertanto non esiste alcun limite per quanto riguarda il traffico che una singola istanza può gestire. Tipi di trigger diversi presentano requisiti di ridimensionamento diversi; è consigliabile quindi valutare la gamma di trigger disponibili e analizzarne i limiti.
