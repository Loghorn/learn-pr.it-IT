L'applicazione di condivisione foto richiede l'archiviazione di diversi tipi di dati. È necessario archiviare dati binari come una foto e dati testuali come un nome utente o un indirizzo di posta elettronica. Ogni sviluppatore abile deve aver tenuto conto di considerazioni sulla progettazione appropriate nello scegliere la forma di archiviazione da usare.

Qui vengono fornite alcune considerazioni legate alla progettazione di cui tenere conto prima di scegliere una strategia di archiviazione. Sarà necessario creare una soluzione semplice da usare, valida per il linguaggio scelto, affidabile e veloce.

## <a name="design-considerations"></a>Considerazioni sulla progettazione

Archiviazione di Azure espone funzionalità usando un endpoint HTTP tramite Internet. Che cosa succede in caso di problemi di connettività Internet? È necessario scrivere codice per creare una richiesta, comunicare con l'endpoint e analizzare qualsiasi risposta?

## <a name="connectivity-and-networking-issues"></a>Problemi di connettività e di rete

In un mondo ideale una rete è disponibile al 100% del tempo ed è sempre affidabile. Tuttavia, questa non è una prospettiva realistica. Poiché l'archiviazione è una componente di quasi tutte le applicazioni e le API di Archiviazione di Azure vengono esposte in rete, è necessario considerare l'impatto di un'interruzione di rete. Di seguito sono riportate alcune considerazioni di cui tenere conto nel determinare se Archiviazione di Azure sia la scelta corretta per le proprie esigenze:

* L'applicazione deve poter mantenere sempre la connettività Web?

Se si crea un'applicazione Web, si presuppone generalmente che la connessione Web sia sempre disponibile, di certo almeno, in primo luogo, per accedere all'applicazione. La possibilità di presupporre questo aspetto semplifica certamente la scrittura di applicazioni, in quanto sono necessari meno codice e complessità per gestire gli scenari offline. Le applicazioni non Web, come quelle installate in un desktop o un dispositivo mobile, possono non essere in grado di dare per scontato questo aspetto, di cui è necessario tenere conto durante la progettazione dell'applicazione. Le app che prevedono connettività Web costante devono essere almeno in grado di gestire occasionalmente errori di connettività, come le interruzioni di rete.

* La progettazione e l'implementazione delle applicazioni supporterà un set di funzionalità ridotte se non è disponibile connettività Web?

Se l'applicazione può essere disconnessa regolarmente o almeno operare in stato disconnesso, sarà in grado di ridurre le proprie funzionalità senza arrestarsi in modo anomalo o generare un errore e diventare inutilizzabile? Questo è un aspetto importante da considerare, in quanto gli scenari disconnessi richiedono spesso livelli elevati di impegno e complessità. Potrebbe essere possibile archiviare determinate parti di dati necessari in locale sul dispositivo per permettere il funzionamento offline. Alcuni elementi dell'applicazione devono essere disabilitati fino al ripristino della connettività? Queste considerazioni riguardano nello specifico l'applicazione ed è importante tenerne conto nelle prima fasi di progettazione e implementazione a causa dell'impegno richiesto per gestire questo scenario.

## <a name="communicating-with-azure-storage"></a>Comunicazione con Archiviazione di Azure

Quando si scrive un'applicazione, in genere si preferisce evitare di occuparsi della scrittura del codice alla base di elementi di basso livello come la formattazione dei dati da inviare ai servizi per l'elaborazione. Spesso, vengono create librerie per la comunicazione dei client con servizi come Archiviazione di Azure per semplificare notevolmente questa attività. Tuttavia, non sono disponibili librerie client per tutti i linguaggi a questo scopo.

* È disponibile una libreria client per lo stack di tecnologie scelto?

In caso contrario, si è disposti a creare metodi di accesso agli endpoint personalizzati, che in genere richiedono un impegno maggiore?

Se sono disponibili librerie, assicurarsi che ve ne sia una per lo stack di tecnologie selezionato. Se si usano stack di tecnologie tra i più diffusi, come .NET o Java, sono in genere disponibili librerie client, mentre se si usano stack meno diffusi come Haskel o Scala, questo potrebbe non avvenire. L'entità dell'impegno aggiuntivo richiesto quando non si usa una libreria client predefinita può spesso essere significativa e deve essere tenuta in considerazione nella progettazione e nell'implementazione dell'applicazione.

Ai fini del modulo e per motivi di semplicità, si presuppone di creare un'applicazione console .NET Core che avrà sempre connettività Internet e che non è progettata per ridurre uniformemente le proprie funzionalità in caso contrario.

## <a name="summary"></a>Riepilogo

Archiviazione di Azure è un meccanismo di archiviazione flessibile e potente che espone le proprie funzionalità tramite una serie di endpoint API HTTP. È importante tenere conto dell'impatto di eventuali problemi di connettività di rete e di disponibilità delle librerie client nel processo di progettazione e implementazione dell'applicazione per garantire che Archiviazione di Azure sia la scelta ideale per l'applicazione.


