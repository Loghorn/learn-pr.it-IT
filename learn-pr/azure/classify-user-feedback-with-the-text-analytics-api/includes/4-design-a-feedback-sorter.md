Ipotizziamo nostre conoscenze di Analitica di testo per lavorare in una soluzione pratica. La nostra soluzione sarà incentrata sull'analisi del Sentiment di documenti di testo. È possibile impostare il contesto per descrivere il problema che si vuole affrontare.

## <a name="manage-customer-feedback-more-efficiently"></a>Gestione del feedback dei clienti in modo più efficiente

Social network è attivo e parlare del prodotto dell'azienda. L'alias di posta elettronica di commenti e suggerimenti è valido anche con i clienti vogliono condividere il proprio parere del prodotto.

Come avviene con qualsiasi nuovo avvio, vive il motto degli ascolta gli utenti. Tuttavia, il successo del prodotto ha reso mantenere questa promessa più facile dirsi. Gli stessi è un buon problema ma un problema.

Il team non riesce a gestire il volume dei commenti e suggerimenti più. Essi bisogno di aiuto ordinamento i commenti e suggerimenti in modo che i problemi possono essere gestiti nel modo più efficiente possibile. Come sviluppatore all'interno dell'organizzazione, è stato richiesto per compilare una soluzione.

Verranno ora esaminati alcuni requisiti di alto livello:

|Requisito  | Dettagli  |
|---------|---------|
|Classificare i commenti e suggerimenti in modo che è possibile agire di conseguenza.     |   Non tutti i commenti sono uguali. Some è deposizioni brillante. Altri commenti e suggerimenti sono arguzia critiche da un cliente frustrato.  Forse non è possibile stabilire quali il cliente vuole raggiungere in altri casi. <br/><br/>Come minimo, con un'indicazione del sentiment o tono, del feedback potrebbero aiutarci a suddividerlo.     |
|La soluzione deve aumentare o alle esigenze.    |   Siamo un avvio. Sono difficili da giustificare i costi fissi e non è stato determinato out il modello esatto del traffico di commenti e suggerimenti. È necessario una soluzione che è possibile affrontare aumenti improvvisi di attività, ma costi minor durante i periodi non interattiva. <br/><br/> In questo caso, un'architettura senza server, fatturata in un piano a consumo è un buon candidato.     |
| Genera un prodotto valido minimo (MVP), ma rendono la soluzione adattabile.    | Oggi si vuole suddividere in categorie di commenti e suggerimenti in modo che è possibile applicare le nostre risorse limitate per i commenti e suggerimenti che è importante. Se un cliente sia frustrato, desideriamo sapere immediatamente e avvia la chat ad essi.  In futuro, ottimizzeremo questa soluzione per eseguire altre operazioni. Un'idea per una nuova funzionalità consiste nell'esaminare le frasi chiave in commenti e suggerimenti per rilevare i punti deboli prima che raggiungano massa critica con i propri clienti.   Un'altra idea consiste nell'automatizzare le risposte ai clienti che sono positivi o neutro. Anche se preferiscono us e vogliamo che possano conoscere che è accette comunque il feedback degli utenti. <br/><br/>Una soluzione che offre un'architettura plug-and-play è una scelta ideale qui. È ad esempio, potremmo, usare le code come una forma di linea di fabbrica. Eseguire un'attività, quindi il risultato venga inserito in una coda per la parte successiva del sistema per prelevare il backup e processo.   |
|Distribuire in modo rapido.     |   Tutti hanno sentito questo quello precedente. Tenere presente che questa soluzione è un MVP e si vuole testare rapidamente con questo scenario. Per offrire velocità e qualità significa scrivere meno codice. <br/><br/> Sfruttando l'API di Analitica di testo, significa che non abbiamo addestrare un modello per il rilevamento del sentiment.  Usando funzioni di Azure e il binding alle code in modo dichiarativo riduce la quantità di codice da scrivere.  Una soluzione senza server significa anche che non è necessario preoccuparsi della gestione del server.   |

La nostra soluzione proposta per ciascun requisito nella tabella precedente offre una panoramica su come mappare i requisiti per le soluzioni.  Ora vediamo che cosa potrebbe essere simile una soluzione basata su Azure.

## <a name="a-solution-based-on-azure-functions-azure-queue-storage-and-text-analytics-api"></a>Una soluzione basata su funzioni di Azure, archiviazione code di Azure e API traduzione testuale Analitica

Il diagramma seguente è una proposta di progettazione per una soluzione. Usa tre componenti di base di Azure, archiviazione code di Azure, funzioni di Azure e servizi cognitivi Microsoft in Azure.

![Diagramma concettuale di un ordinamento di architettura di feedback.](../media/proposed-solution.PNG)

L'idea è che i documenti di testo contenente i commenti degli utenti vengono inseriti in una coda in cui è stato denominato *nuovi commenti e suggerimenti-q* nel diagramma precedente. L'arrivo di un messaggio contenente il documento di testo nella coda verrà attivare o avviare, l'esecuzione della funzione. La funzione legge i messaggi contenenti i nuovi documenti dalla coda di input e li invia per l'analisi per l'API di Analitica di testo. In base ai risultati che restituisce l'API, un nuovo messaggio contenente il documento viene inserito in una coda di output per un'ulteriore elaborazione.

Il risultato che vengono restituiti per ogni documento è un punteggio del sentiment. Le code di output vengono utilizzate per archiviare commenti e suggerimenti ordinato in positivi, negativi e neutro. Si spera coda negativa sarà sempre vuota. :-)   Dopo che sono stati inseriti in bucket ogni parte in arrivo di commenti e suggerimenti in una coda di output del sentiment in base, è facile immaginare l'aggiunta di logica per eseguire un'azione sui messaggi di ogni coda.

Verrà ora esaminato un diagramma di flusso Avanti per vedere cosa è necessario eseguire la logica della funzione.

![Diagramma di flusso per la logica all'interno della funzione di Azure per ordinare documenti di testo in base alla valutazione nelle code di output.](../media/flow.PNG)

La logica è simile a un router. Accetta input di testo e lo indirizza a una coda di output in base al punteggio del sentiment del testo. Abbiamo una dipendenza sull'API traduzione testuale Analitica. Mentre la logica sembri semplice, questa funzione consente di rimuovere la necessità di persone del team per analizzare manualmente i commenti e suggerimenti.

## <a name="steps-to-implement-our-solution"></a>Passaggi per implementare la nostra soluzione

Per implementare la soluzione descritta in questa unità, è necessario completare i passaggi seguenti.

1. Creare un'app per le funzioni per ospitare la nostra soluzione.

1. Controllare la presenza del sentiment in messaggi in arrivo di commenti e suggerimenti tramite l'API di Analitica di testo. Verrà usata la chiave di accesso nell'esercizio precedente e scrivere il codice per inviare le richieste.

1. Inviare commenti e suggerimenti per l'elaborazione delle code di base del sentiment.

È possibile passare alla creazione di app (funzione).