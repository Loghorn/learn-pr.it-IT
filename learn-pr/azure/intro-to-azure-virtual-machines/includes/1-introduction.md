Si supponga di lavorare nel reparto IT di una società di ricerca farmaceutica e di amministrare un gruppo di server che eseguono tutta l'infrastruttura aziendale, dai server Web ai database. L'hardware sta tuttavia diventando vecchio e non riesce a tenere il passo con alcune delle nuove applicazioni di analisi dei dati che sono state distribuite.

Si potrebbe aggiornare tutto l'hardware, ma questa soluzione non sembra appropriata per diversi motivi:

1. I server sono fisicamente dislocati in tutto il mondo con poco personale in ogni sede. Si vuole centralizzare l'aggiornamento nella sede centrale.

1. La società usa un software di analisi dei dati personalizzato in diverse versioni di Windows e Linux, in alcuni casi con configurazioni particolari non del tutto chiare. Serve un modo per testare le distribuzioni in modo esaustivo e provare configurazioni diverse al fine di verificare che tutto funzioni correttamente prima della transizione.

1. L'attività è in forte espansione e la società sta crescendo rapidamente. È probabile che il carico sui server interni, in particolare sui database, continui a crescere rendendo necessario l'acquisto di soluzioni per il futuro o la definizione di un piano di scalabilità per gestire la crescita.

Per questi motivi, si decide che è il momento di esaminare il cloud per verificare se può aiutare a risolvere il problema relativo al carico e alla scalabilità. In presenza di un gruppo di server misti e software personalizzato, si può esplorare la possibilità di spostare i server uno alla volta in Azure usando macchine virtuali di Azure.

Le macchine virtuali di Azure sono uno dei vari tipi di risorse di calcolo scalabili e su richiesta offerti da Azure. Con le macchine virtuali, si ha il controllo totale sulla configurazione e si può installare tutto il necessario per la propria attività. Se occorre scalare o estendere il data center, non è necessario acquistare hardware fisico. Azure offre infine servizi aggiuntivi per monitorare, proteggere e gestire aggiornamenti e patch del sistema operativo.

Verranno ora esaminate le decisioni prese prima di creare una VM, le opzioni per creare e gestire la macchina virtuale e le estensioni e i servizi usati per gestirla.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

Questo modulo descrive:

- Compilare un elenco di controllo per creare una macchina virtuale
- Descrivere le opzioni per creare e gestire le macchine virtuali
- I servizi aggiuntivi disponibili per amministrare le macchine virtuali