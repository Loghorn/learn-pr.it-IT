Le conoscenze apprese su Analisi del testo verranno ora applicate nell'ambito di una soluzione pratica. Questa soluzione sarà incentrata su Analisi del sentiment per i documenti di testo. Il contesto viene definito descrivendo il problema che si vuole affrontare. 

## <a name="manage-customer-feedback-more-efficiently"></a>Gestire il feedback dei clienti in modo più efficiente

I clienti esprimono la loro opinione sul prodotto di un'azienda sui social media. Anche all'indirizzo di posta elettronica per i commenti e i suggerimenti dei clienti arrivano numerosi messaggi con pareri sul prodotto.

Come qualsiasi altra nuova startup, l'azienda si impegna a tenere in grande considerazione l'opinione dei clienti. Tuttavia, il successo del prodotto potrebbe rendere difficile mantenere questo impegno. Come risolvere questo problema? 

Il team non è più in grado di gestire il volume dei commenti e ha bisogno di uno strumento per ordinare il feedback ricevuto in modo che i problemi possano essere gestiti nel modo più efficiente possibile. Per questo motivo, è stato chiesto al responsabile dello sviluppo di realizzare una soluzione. 

Ecco alcuni dei requisiti principali:


|Requisito  | Dettagli  |
|---------|---------|
|Classificare il feedback in modo da poter agire di conseguenza.     |   Non tutti i commenti sono uguali. Alcuni sono particolarmente lusinghieri. Altri sono critiche di clienti insoddisfatti.  In altri casi ancora è impossibile stabilire di cosa ha bisogno il cliente. <br/><br/>Un'indicazione del sentiment, o del tono, del feedback potrebbe essere utile per classificarlo.     |
|La soluzione dovrebbe essere scalabile in base alle esigenze.    |   L'azienda è una startup. I costi fissi sono difficili da giustificare e non è stato ancora individuato il modello esatto del traffico del feedback. È necessaria una soluzione che consenta di affrontare i picchi di attività, costando il meno possibile durante i periodi di inattività. <br/><br/> In questo caso, una valida soluzione potrebbe essere un'architettura serverless fatturata con un piano a consumo.     |
| Generare un prodotto minimo funzionante (MVP; Minimal Viable Product), rendendo la soluzione adattabile.    | Al momento è indispensabile classificare il feedback in modo da poter applicare le risorse limitate dell'azienda ai commenti realmente importanti. Se un cliente è insoddisfatto, l'azienda vuole saperlo immediatamente per poterlo contattare.  In futuro, questa soluzione verrà ottimizzata per eseguire altre operazioni. Un'idea per una nuova funzionalità consiste nell'esaminare le frasi chiave all'interno del feedback per rilevare i punti deboli prima che diventino problemi critici per i clienti.   Un'altra idea è quella di automatizzare le risposte per i clienti positivi o neutrali. Anche se un cliente è soddisfatto, è opportuno fargli sapere che il suo feedback viene preso in considerazione. <br/><br/>Una soluzione in grado di offrire un'architettura plug-and-play è un'ottima scelta in questo caso. Ad esempio, è possibile usare le code come modello di produzione tipo catena di montaggio. Si esegue un'attività, quindi il risultato viene inserito in una coda dove il componente successivo del sistema lo preleva e lo elabora.   |
|Distribuire in modo rapido.     |   Questo è un obiettivo comune. Tenere presente che questa soluzione è un MVP e si vuole testarla rapidamente con questo scenario. Una distribuzione rapida e qualitativamente elevata implica la scrittura di meno codice. <br/><br/> Sfruttare l'API Analisi del testo significa che non occorre eseguire il training di un modello per rilevare il sentiment.  L'uso di Funzioni di Azure e l'associazione alle code riduce in modo dichiarativo la quantità di codice da scrivere.  Una soluzione serverless significa anche che non è necessario preoccuparsi della gestione del server.   |

La soluzione proposta per ciascun requisito nella tabella precedente offre una rapida panoramica di come associare i requisiti alle soluzioni.  Si passerà ora a esaminare una possibile soluzione basata su Azure.

## <a name="a-solution-based-on-azure-functions-azure-queue-storage-and-text-analytics-api"></a>Soluzione basata su Funzioni di Azure, Archiviazione code di Azure e API Analisi del testo

Il diagramma seguente è una proposta di progettazione per una soluzione. Usa tre componenti di base di Azure, Archiviazione code di Azure, Funzioni di Azure e Servizi cognitivi Microsoft in Azure.

![Diagramma concettuale di un'architettura per l'ordinamento del feedback.](../media-draft/proposed-solution.PNG)

L'idea è che i documenti di testo contenenti il feedback degli utenti vengano inseriti in una coda denominata *new-feedback-q* nel diagramma precedente. L'arrivo di un documento di testo nella coda attiva, o avvia, una funzione di Azure. La funzione legge i nuovi documenti dalla coda di input e li invia per l'analisi all'API Analisi del testo. In base ai risultati restituiti dall'API, il documento viene inserito in una coda di output per un'ulteriore elaborazione.

Il risultato che viene restituito per ogni documento è un punteggio del sentiment. Le code di output vengono usate per archiviare i commenti ordinati in positivi, neutrali e negativi. Ovviamente si spera che la coda dei commenti negativi sia sempre vuota. Dopo che ogni commento in arrivo è stato inserito in un bucket in una coda di output in base al sentiment, il passo successivo è quello di aggiungere la logica per eseguire un'azione sui messaggi in ogni coda. 

Verrà ora esaminato un diagramma di flusso per vedere le operazioni che la logica della funzione deve eseguire.

![Diagramma di flusso della logica all'interno della funzione di Azure per ordinare i documenti di testo in base al sentiment nelle code di output.](../media-draft/flow.PNG)

La logica è simile a un router. Accetta l'input di testo e lo indirizza a una coda di output in base al punteggio del sentiment del testo. È presente una dipendenza dall'API Analisi del testo. Per quanto la logica sembri semplice, questa funzione consentirà di evitare ai membri del team di analizzare manualmente il feedback.

## <a name="steps-to-implement-our-solution"></a>Passaggi per l'implementazione della soluzione

Per implementare la soluzione descritta in questa unità, sarà necessario completare i passaggi seguenti.

1. Creare un'app per le funzioni per l'hosting della soluzione.

1. Rilevare il sentiment nei messaggi di feedback in arrivo usando l'API Analisi del testo. Verrà usata la chiave di accesso dell'esercizio precedente e verrà scritto il codice per inviare le richieste.

1. Inviare il feedback alle code di elaborazione sulla base del sentiment.


Si passerà ora alla creazione della funzione e dell'app per le funzioni. 