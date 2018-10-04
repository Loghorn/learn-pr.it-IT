Un processo di operations con esito positivo, ovvero in grado di raggiungere e mantenere l'affidabilità desiderata, dipende tanto dal modo in cui vengono trattate le macchine quanto dal modo in cui vengono trattati gli umani responsabili dell'ambiente. L'ingegneria dell'affidabilità del sito (Site Reliability Engineering) riconosce questa verità in diversi modi fondamentali per la sua applicazione.

## <a name="toil"></a>Fatica

Il primo modo è costituito da un'attenzione al concetto di “fatica”. Nel contesto SRE, la fatica si riferisce a un lavoro su operations svolto da un umano con determinate caratteristiche. La fatica non ha un valore di riscatto a lungo termine. Non fa avanzare il servizio in modo significativo. È spesso ripetitiva e in gran parte manuale (sebbene possa essere automatizzata). Man mano che il servizio o il sistema cresce di dimensione, è probabile che anche il numero di richieste del sistema aumenti in modo proporzionale e necessiti di maggior lavoro manuale.

Ad esempio, se un servizio richiede al team SRE di eseguire un azzeramento ogni settimana, offrire nuovi account e spazio su disco manualmente o eseguire ripetutamente riavvii manuali, tutto ciò è un carico operativo che è fatica. L'esecuzione di queste azioni non ha migliorato il servizio in modo persistente a lungo termine. È probabile che queste azioni debbano essere ripetute più volte.

> [!NOTE]
> Anche se le richieste di questo tipo vengono mantenute in un sistema di ticket come spesso avviene, l'esecuzione dell'azione e la risoluzione del ticket è ancora fatica. Si tratta semplicemente di fatica registrata dettagliatamente.

Gli SRE odiano la fatica. Lavorano per eliminarla ogni volta che è possibile e appropriato. Questo è il caso in cui entra in gioco l'automazione in SRE. Se le richieste possono essere gestite automaticamente, il team può dedicarsi ad attività più appaganti ed efficaci dell'evasione della coda di richieste.

Tenere presente che l'uso del termine “appropriato” in questo contesto è simile all'uso dello stesso termine nel contesto dell'affidabilità. In alcune situazioni il lavoro di eliminazione della fatica è meno prioritario rispetto ad altri lavori, ma nel complesso la rimozione della fatica da un servizio rimane uno degli obiettivi principali degli SRE.

## <a name="project-work-vs-reactive-ops-work"></a>Lavoro su progetto e lavoro su operations reattivo

Per compiere il lavoro necessario per rimuovere la fatica o migliorare l'affidabilità di un sistema, il tempo degli SRE deve essere allocato in modo che non venga totalmente impiegato in emergenze, risposte alle pagine o elaborazione di una coda di richieste. Gli SRE devono avere tempo a disposizione per scrivere codice per l'eliminazione della fatica, costruire un'automazione self-service che elimini la necessità dei ticket, creare progetti che incrementino l'efficienza del servizio e delle persone. Solitamente viene indicata una percentuale, proveniente dal modello originale Google, che non supera il 50% di carico operativo per team.

> [!NOTE]
> Sebbene il 50% possa essere un numero arbitrario, nella pratica questa percentuale sembra essere un obiettivo ragionevole per molte persone.

In alcuni momenti il tempo degli SRE viene totalmente dedicato alle emergenze, ma questo stato non può essere costante. Se il lavoro su "operations" reattivo (in gran parte fatica) occupa più del 50% del tempo del team per un periodo prolungato, questa condizione porta a esaurimento e a una bassa affidabilità. In questa situazione, i cicli virtuosi descritti in precedenza non possono essere applicati o creati. SRE pone la stessa attenzione ai carichi su chiamata non equilibrati che possono potenzialmente avere un forte impatto negativo sul team.

Dopo aver esaminato alcune delle procedure e dei principi SRE, di seguito viene descritto come iniziare ad applicarli.
