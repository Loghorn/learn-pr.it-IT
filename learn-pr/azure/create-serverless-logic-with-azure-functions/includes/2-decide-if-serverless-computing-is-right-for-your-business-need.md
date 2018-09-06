Per stabilire se l'informatica serverless è adatta alle proprie esigenze, considerare innanzitutto in che cosa consiste l'elaborazione serverless.

## <a name="what-is-serverless-compute"></a>Che cos'è l'elaborazione serverless?

L'elaborazione serverless usa le funzioni come servizio (FaaS). Le funzioni possono essere considerate come dei microservizi ospitati in una piattaforma cloud. La logica di business viene eseguita come funzioni e non è necessario eseguire manualmente il provisioning o ridimensionare l'infrastruttura. Il provider del cloud gestisce l'infrastruttura. L'app viene automaticamente ridimensionata o ridotta a seconda del carico. Azure offre diversi modi per compilare questo tipo di architettura. I due approcci più comuni sono le App per la logica di Azure e le Funzioni di Azure, che vengono trattate in questo modulo.

## <a name="what-is-azure-functions"></a>Che cos'è Funzioni di Azure?

Funzioni di Azure è una piattaforma applicativa senza server, che consente agli sviluppatori di ospitare la logica di business che potrà essere eseguita senza occuparsi del provisioning dell'infrastruttura. Funzioni offre scalabilità intrinseca e all'utente vengono fatturate solo le risorse usate. È possibile scrivere il codice della funzione nel linguaggio preferito, inclusi C#, F# e JavaScript. È incluso anche il supporto per NuGet e NPM, in modo che sia possibile usare librerie comuni nella logica di business.

## <a name="benefits-of-a-serverless-compute-solution"></a>Vantaggi di una soluzione di calcolo serverless

L'elaborazione serverless è una valida opzione per ospitare nel cloud il codice per la logica di business. Con le offerte serverless, come Funzioni di Azure, è possibile scrivere la logica di business nel linguaggio di propria scelta. Si ottiene un ridimensionamento automatico, non ci sono server da gestire e la fatturazione è basata su ciò che viene utilizzato, non sul tempo riservato. Di seguito sono riportate alcune caratteristiche aggiuntive di una soluzione serverless da valutare.

### <a name="avoids-over-allocation-of-infrastructure"></a>Evita la sovrassegnazione dell'infrastruttura

Si supponga di aver eseguito il provisioning di server delle macchine virtuali e di averli configurati con risorse sufficienti a gestire i picchi del carico. Quando il carico diminuisce, l'utente paga potenzialmente per un'infrastruttura che non usa. Grazie al ridimensionamento automatico, l'elaborazione serverless contribuisce a risolvere il problema dell'assegnazione. Vengono così addebitate solo le funzioni in esecuzione.

### <a name="stateless-logic"></a>Logica senza stato

Le funzioni senza stato sono particolarmente adatte all'elaborazione senza server, in quanto le istanze della funzione vengono create ed eliminate su richiesta. Se lo stato è necessario, può essere archiviato in un servizio di archiviazione associato.

### <a name="event-driven"></a>Basate su eventi

Le funzioni sono _basate su eventi_. Ciò significa che vengono eseguite solo in risposta a un evento (detto "trigger"), ad esempio la ricezione di una richiesta HTTP o un messaggio da aggiungere a una coda. Il trigger viene configurato come parte della definizione di funzione. Questo approccio semplifica notevolmente il codice consentendo di dichiarare la provenienza dei dati (associazione di trigger/input) e la loro destinazione (associazione di output). Non è necessario scrivere codice per osservare code, BLOB, hub e così via e l'utente può concentrarsi esclusivamente sulla logica di business.

### <a name="functions-can-be-used-in-traditional-compute-environments"></a>Le funzioni possono essere usate negli ambienti di calcolo tradizionale

Le funzioni sono un componente fondamentale dell'elaborazione serverless, ma sono anche una piattaforma di calcolo generale per l'esecuzione di qualsiasi tipo di codice. Qualora le esigenze della vostra applicazione dovessero cambiare, sarà possibile prendere il proprio progetto e distribuirlo in un ambiente non serverless, che offra la flessibilità di gestirne il ridimensionamento, eseguirlo su reti virtuali e persino isolare completamente le proprie funzioni.

## <a name="drawbacks-of-a-serverless-compute-solution"></a>Svantaggi di una soluzione di calcolo serverless

L'elaborazione serverless non è sempre la soluzione appropriata per l'hosting della logica di business. Di seguito vengono illustrate alcune caratteristiche di funzioni che possono influenzare la decisione di ospitare i servizi usando l'elaborazione serverless. 

### <a name="execution-time"></a>Tempo di esecuzione

Per impostazione predefinita, le funzioni hanno un timeout di 5 minuti, configurabile fino a un massimo di 10 minuti. Se l'esecuzione della funzione richiede più di 10 minuti, è possibile ospitarla su una macchina virtuale. Se, inoltre, il servizio viene avviato da una richiesta HTTP e si prevede tale valore come risposta HTTP, il timeout viene ulteriormente ridotto a 2,5 minuti. Infine, è inoltre disponibile un'opzione denominata **Durable Functions** che consente di orchestrare le esecuzioni di più funzioni senza alcun timeout.

### <a name="execution-frequency"></a>Frequenza di esecuzione

Il secondo aspetto da valutare è la frequenza di esecuzione. Se si prevede che la funzione venga eseguita in modo continuo da più client, è opportuno stimarne l'utilizzo e calcolare il costo che deriva dall'uso delle funzioni. Ospitare il servizio in una macchina virtuale potrebbe essere più conveniente.

Durante il ridimensionamento è possibile creare solo un'istanza dell'app per le funzioni ogni 10 secondi, per un massimo di 200 istanze totali. Tenere presente che ogni istanza può prevedere più esecuzioni simultanee, pertanto non esiste alcun limite per quanto riguarda il traffico che una singola istanza può gestire. Tipi di trigger diversi presentano requisiti di ridimensionamento diversi; è consigliabile quindi valutare la gamma di trigger disponibili e analizzarne i limiti.


