Prima di analizzare alcune delle pratiche associate a SRE, è utile applicare in contesto le idee esposte nell'unità precedente. In questa breve unità si riassumerà l'evoluzione della metodologia SRE e la sua relazione con altre procedure operative che potrebbero risultare familiari. Questo sarà utile in seguito, perché tali procedure avranno più senso se considerate in contesto. Si avrà anche una risposta pronta alle domande del tipo "Che cosa rende SRE diverso da...?".

## <a name="history"></a>Cronologia

Una cronologia riassuntiva di SRE inizia dalle origini in Google nel 2003. Ben Treynor, ora Treynor Sloss, assunse la leadership del "Production team" di Google (allora composto da soli sette ingegneri software) e diede forma all'idea "Che cosa accade quando si richiede a un ingegnere software di progettare una funzione operativa". Questo precedente spiega perché SRE può risultare molto "software engineering" ai professionisti del settore operativo che la usano per la prima volta. La metodologia ha adottato valori del settore dell'ingegneria software, come l'importanza della creazione di codice e i sistemi di controllo del codice sorgente come strumento fondamentale. L'implementazione iniziale e corrente di Google SRE è ben documentata nei due libri pubblicati da O' Reilly (vedere l'unità Introduzione).

Man mano che dipendenti di Google lasciavano l'azienda e un numero crescente di dipendenti della società citava pubblicamente questo approccio, la metodologia SRE iniziò ad espandersi ad altre organizzazioni del settore. Tali organizzazioni adottavano e adattavano i principi SRE alle impostazioni cultura locali. Questo processo di espansione ha generato nel settore varie implementazioni di SRE diverse tra loro. 

## <a name="devops-and-sre"></a>DevOps e SRE

Il settore in generale si trovava di fronte alle stesse sfide in termini di scalatura, velocità di sviluppo o stabilità funzionale e relative ad altri aspetti di delivery del software che hanno contribuito a promuovere il movimento Site Reliability Engineering. Attività parallele per affrontare queste sfide al di fuori di Google (e alcune aziende di grandi dimensioni in quel periodo) hanno dato origine a DevOps. 

Per informazioni dettagliate su DevOps, vedere [https://docs.microsoft.com/azure/devops/learn/](https://docs.microsoft.com/azure/devops/learn/).

> [!NOTE]
> È importante sottolineare che DevOps e SRE sono due tentativi diversi di risolvere lo stesso problema. SRE non è il passaggio evolutivo successivo a DevOps. SRE non è stato creato per essere "il futuro della metodologia DevOps".

Le differenze tra SRE e DevOps sono a tutt'oggi un argomento in fase di definizione nel settore. Vi sono tuttavia alcune differenze ampiamente concordate, tra cui:

- SRE è una disciplina di progettazione incentrata sull'affidabilità, DevOps è un movimento emerso per l'esigenza di rompere l'isolamento tra i settori sviluppo software e operativo.
- SRE può essere il nome di un ruolo, ad esempio "Sono un Site Reliability Engineer (SRE)", ma non così DevOps. Nessuno, in termini precisi, assolve una mansione di "DevOps".
- La metodologia SRE tende a essere più prescrittiva, DevOps è intenzionalmente il contrario. L'adozione quasi universale dell'integrazione continua/recapito continuo e dei principi Agile sono gli aspetti più prescrittivi di DevOps.

Le due metodologie del settore operativo, DevOps e SRE, condividono un'attenzione speciale per il monitoraggio e l'osservabilità (possibilmente per motivi diversi). Di conseguenza può risultare facile importare prassi e principi SRE in un'organizzazione che implementa una metodologia DevOps. Questo processo va tuttavia eseguito con attenzione e con obiettivi ben chiari. Inoltre può e deve essere implementato in modo incrementale, non mediante un cambio improvviso.

> [!WARNING]
> Il semplice scambio di qualifiche tra i membri dell'organizzazione è una strategia di implementazione che raramente ha esito positivo. Non produce i vantaggi che SRE può offrire. Per suggerimenti utili, vedere la sezione Introduzione di questa unità.

## <a name="conclusion"></a>Conclusioni

Questa breve unità ha cercato di offrire un minimo di contesto in merito a SRE e DevOps. SRE e DevOps sono scuole di pensiero contigue per il settore operativo ed è opportuno considerarle in questi termini. 

Ora che è stato esaminato brevemente il background di SRE si passerà direttamente a illustrare alcuni principi di base.
