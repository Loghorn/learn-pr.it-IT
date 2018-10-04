Per iniziare nel migliore dei modi è spesso utile partire dai concetti base. Ad esempio è utile porre la domanda base: "Che cos'è Site Reliability Engineering (SRE)?"
Esistono diverse risposte a questa domanda, tra cui [quella spesso citata](https://landing.google.com/sre/book/chapters/introduction.html) della persona che ha coniato il termine (Ben Treynor Sloss di Google), ma forse la risposta più pratica è la seguente:

> Site Reliability Engineering (SRE) è una disciplina di ingegneria informatica dedicata ad assistere le organizzazioni nell'ottenimento dei livelli di affidabilità appropriati per i sistemi, i servizi e i prodotti.

In un secondo momento si potranno aggiungere altre definizioni, ma per ora è opportuno iniziare da qui. Questa definizione è costituita da due parti cruciali che vanno separate e conducono alla domanda successiva: "Perché è importante?" .

## <a name="reliability"></a>Affidabilità

Al centro della definizione (e del nome "SRE") sta la parola Reliability (affidabilità). La definizione non recita "livello appropriato di prestazioni" o "livello appropriato di efficienza" o "livello appropriato di stabilità" e nemmeno "ottenimento del livello appropriato di utili". Dice "livelli di affidabilità appropriati". Perché?

Si consideri una rapida dimostrazione. Di seguito è riportata una schermata. Che cosa visualizza? Evitare di procedere fino a quando non si capisce di che si tratta o si rinuncia. Nota: se i dettagli non sono particolarmente chiari, non dipende dal browser, ma dalla qualità dell'immagine originale.

   ![Screenshot 1](../media/02_blank-screenshot.png)

Questa immagine è uno screenshot di un'app PHP (senza supporto per il debug aggiuntivo) quando l'esecuzione non riesce. Per un'app Java la visualizzazione sarà simile alla seguente:

   ![Screenshot 2](../media/02_java-screenshot.png)

Perché si visualizzano questi esempi? Ognuno di essi rappresenta un'applicazione la cui creazione può avere richiesto all'organizzazione grandi quantità di tempo, energie e risorse. Se tuttavia l'applicazione non è attiva, ovvero non è operativa quando un cliente deve accedervi, ovvero non è affidabile, non è utile a nessuno, men che meno all'organizzazione stessa. Di fatto la mancanza di affidabilità può essere dannosa per l'organizzazione (a livello di reputazione, economico, contrattuale, di morale del personale e così via).

Questo è il motivo per cui l'affidabilità è così importante e anche il motivo per cui la metodologia SRE si incentra sull'affidabilità come una proprietà fondamentale, forse LA proprietà fondamentale del servizio, del sistema o del prodotto. L'affidabilità può includere molti aspetti (che verranno esaminati più avanti), ma ora è necessario concentrarsi sulla seconda parte essenziale della definizione.

## <a name="appropriate-levels-of-reliability"></a>Livelli di affidabilità appropriati

Una parola molto importante potrebbe essere sfuggita alla prima lettura della definizione. Eccola sottolineata:

> Site Reliability Engineering (SRE) è una disciplina di ingegneria informatica dedicata ad assistere le organizzazioni nell'ottenimento dei livelli di affidabilità *appropriati* per i sistemi, i servizi e i prodotti.

Perché questa parola è così importante?

Un'osservazione importante formulata nell'ambito SRE è il fatto che solo pochi sistemi e servizi devono offrire un'affidabilità del 100%. Eccezioni degne di nota sono le situazioni con rischio per la vita umana, quali l'aviazione, le strumentazioni medicali e così via.

In effetti, esistono anche poche situazioni in cui l'affidabilità del 100% è auspicabile. Le risorse, il lavoro (e quindi i costi) necessari per aumentare l'affidabilità crescono in modo brusco man mano che aumenta il grado di affidabilità desiderato. In altri termini la ricerca di affidabilità non necessaria può comportare sprechi in termini di tempo e denaro. _Si vuole ottenere il livello appropriato di affidabilità del sistema, dei servizi e dei prodotti._ 

Il livello deve corrispondere alle esigenze aziendali e avere un carattere pragmatico. Se ad esempio i clienti si connettono all'azienda attraverso una rete che non è affidabile al 100% (si supponga che sia disponibile per il 90% del tempo), il lavoro e i costi necessari per portare l'affidabilità al 95% non sono un investimento opportuno. _Si vuole ottenere il livello appropriato di affidabilità del sistema, dei servizi e dei prodotti._

SRE porta questo pragmatismo a un grado più elevato. Se ora siamo in grado di definire un livello di affidabilità auspicabile, cosa fare quando si riesce a raggiungere o superare tale livello? Analogamente, cosa fare se si non riesce a raggiungere tale livello?

A queste domande verrà data risposta dopo una breve interruzione, per l'aggiunta di un po' di contesto.
