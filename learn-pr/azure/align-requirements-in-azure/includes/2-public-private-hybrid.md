Si supponga di lavorare in un'azienda che opera nel settore sanitario. Sono disponibili sistemi legacy, sistemi line-of-business e piani futuri per nuovi sistemi. Si è sentito parlare dei vantaggi del cloud computing. Come si può scegliere il modello di distribuzione migliore per soluzioni diverse di cloud pubblico, privato o ibrido?

## <a name="what-is-cloud-computing"></a>Cos'è il cloud computing?

Il cloud computing è il provisioning di servizi e applicazioni su richiesta tramite Internet. I server, le applicazioni, i dati e altre risorse vengono forniti come servizio. 

I dettagli dei servizi sono nascosti all'utente. È possibile effettuare rapidamente il provisioning delle risorse di calcolo e usare il servizio con interventi minimi di gestione. Non è corretto pensare al cloud computing come a un data center disponibile tramite Internet. Il cloud computing usa la virtualizzazione, hardware commerciale e processi automatizzati per offrire un'esperienza utente self-service ai clienti, in modo analogo a un servizio di pubblica utilità.

Esistono tre modelli di distribuzione per il cloud computing: cloud pubblico, cloud privato e cloud ibrido. La figura seguente mostra una panoramica di questi modelli di distribuzione.

![Figura che mostra una panoramica generale dei modelli di distribuzione del cloud.](../media/2-cloud-deployment.png)

#### <a name="public-versus-private-versus-hybrid"></a>Pubblico, privato o ibrido

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEv7]

## <a name="public-cloud"></a>Cloud pubblico

I cloud pubblici rappresentano il modo più comune di distribuzione del cloud computing. I servizi vengono offerti attraverso la rete Internet pubblica e sono disponibili per chiunque voglia acquistarli. Le risorse del cloud, ad esempio i server e l'archiviazione, sono di proprietà e gestite da un provider di servizi cloud di terze parti e vengono offerte tramite Internet. I servizi possono essere gratuiti o venduti su richiesta, in modo da consentire ai clienti di pagare solo per i cicli della CPU, lo spazio di archiviazione o la larghezza di banda utilizzati. Microsoft Azure è un esempio di cloud pubblico. 

Si immagini che la propria società che opera nel settore sanitario voglia realizzare un sito Web per la registrazione. Il sito deve adattarsi ed essere reattivo durante i picchi di registrazioni in diversi momenti nel corso dell'anno. I clienti accedono al sito da località globali. È possibile usare il cloud pubblico per la scalabilità automatica per soddisfare le richieste al momento dei picchi di registrazioni. Quando il traffico del sito è basso, il sito può essere ridimensionato riducendo le risorse per risparmiare. Il sito è reattivo ai picchi della domanda e si pagano risorse aggiuntive solo quando necessario. È anche possibile distribuire il sito Web in più aree geografiche per migliorare l'affidabilità e la velocità di risposta.

Durante lo sviluppo del sito Web, gli sviluppatori vogliono creare più ambienti di sviluppo per velocizzare il processo di sviluppo. Gli sviluppatori possono usare il cloud pubblico per effettuare rapidamente il provisioning di macchine virtuali per ambienti sandbox per sviluppare una soluzione. Quando gli sviluppatori non hanno più bisogno di un ambiente, possono eliminarlo.

### <a name="why-public-cloud"></a>Perché scegliere un cloud pubblico?

I cloud pubblici possono essere distribuiti più rapidamente rispetto alle infrastrutture locali e con una piattaforma scalabile quasi all'infinito. Tutti i dipendenti di una società possono usare la stessa applicazione da qualsiasi ufficio o succursale con il dispositivo preferito, purché abbiano accesso a Internet. 

Esempi dei motivi per usare il cloud pubblico:

- **Utilizzo del servizio tramite il modello su richiesta o con sottoscrizione:** il modello su richiesta o con sottoscrizione consente di pagare per la parte di CPU, archiviazione e altre risorse usate o prenotate.
- **Nessun investimento anticipato per l'hardware:** non occorre acquistare, gestire e mantenere aggiornato l'hardware e l'infrastruttura applicativa in locale. Il provider di servizi cloud è interamente responsabile per la gestione e la manutenzione del sistema. 
- **Automazione:** è possibile effettuare velocemente il provisioning delle risorse di infrastruttura tramite un portale Web, script o automaticamente. 
- **Dispersione geografica:** è possibile archiviare i dati vicino agli utenti o in località specifiche, senza la necessità di gestire i propri data center.
- **Manutenzione dell'hardware ridotta:** la manutenzione dell'hardware è una responsabilità del provider di servizi.

## <a name="private-cloud"></a>Cloud privato

Un cloud privato è costituito da risorse di calcolo usate esclusivamente da utenti di un'azienda o un'organizzazione. Può essere collocato fisicamente nel data center locale dell'organizzazione oppure ospitato da un provider di servizi di terze parti. Il termine cloud privato non deve essere considerato semplicemente un nuovo nome per definire i data center locali tradizionali. Un cloud privato usa l'infrastruttura e i servizi locali per offrire vantaggi simili a quelli del cloud pubblico. Usa una piattaforma di astrazione per fornire servizi *tipo cloud*, ad esempio i cluster Kubernetes o un ambiente cloud completo, come Azure Stack. L'organizzazione è responsabile di acquisto, configurazione e manutenzione dell'hardware. La comunicazione tra i sistemi avviene in genere con un'infrastruttura di rete di proprietà dell'azienda e gestita dall'azienda. Ad esempio, una rete interna privata o una connessione dedicata in fibra ottica tra edifici.

Si supponga di lavorare in un'azienda del settore sanitario e di avere un'applicazione usata in uno dei data center dell'azienda. L'ambiente operativo non può essere replicato nel cloud pubblico. Subentra una nuova richiesta di accedere ai dati in un altro dei data center dell'azienda. Il database che contiene i dati deve rimanere nell'altro sito, per conformità alle normative. Questo scenario è un cloud privato. Sono disponibili due data center di proprietà dell'organizzazione. È possibile usare una VPN del cloud pubblico tramite Internet per connettere i data center. Questo scenario verrebbe comunque considerato un cloud privato perché la soluzione è privata per l'organizzazione.

### <a name="why-private-cloud"></a>Perché scegliere un cloud privato?

Un cloud privato può offrire una maggiore flessibilità a un'organizzazione. L'organizzazione può personalizzare il proprio ambiente cloud per soddisfare specifiche esigenze aziendali. Dato che le risorse non sono condivise con altri, sono possibili livelli elevati di sicurezza e controllo. Inoltre, i cloud privati possono offrire buoni livelli di efficienza e scalabilità.

Esempi dei motivi per usare il cloud privato:

- **Ambiente pre-esistente:** un ambiente operativo esistente che non può essere replicato nel cloud pubblico. Investimenti hardware notevoli e dipendenti con competenze specifiche. Una grande organizzazione può scegliere di commodotizzare le risorse di calcolo.
- **Applicazioni legacy:** applicazioni legacy business critical che non possono essere facilmente ricollocate fisicamente.
- **Sovranità e sicurezza dei dati:** possono esistere confini politici e requisiti legali che impongono la posizione fisica dei dati.
- **Conformità alle normative/certificazione:** conformità HIPAA o PCI. Data center locale certificato.

## <a name="hybrid-cloud"></a>Cloud ibrido

Un cloud ibrido è un ambiente di elaborazione che combina un cloud pubblico e un cloud privato, consentendo la condivisione tra i cloud di dati e applicazioni. Quando la domanda di elaborazione e calcolo è fluttuante, il cloud computing ibrido offre alle aziende la possibilità di ridimensionare facilmente la propria infrastruttura locale fino al cloud pubblico per gestire eventuali overflow, senza concedere ai data center di terze parti l'accesso a tutti i dati proprietari. Le organizzazioni possono usufruire della flessibilità e della potenza di calcolo del cloud pubblico per attività di calcolo semplici e non sensibili, mantenendo le applicazioni e i dati business critical in locale, ben protetti da un firewall aziendale.

L'uso di un cloud ibrido consente di evitare investimenti per gestire picchi di breve durata della domanda. Offre anche la flessibilità di decidere quali risorse mantenere in locale e quali usare nel cloud. Le aziende pagano solo le risorse che usano temporaneamente invece di dover acquistare, programmare e gestire ulteriori risorse e apparecchiature, che potrebbero rimanere inattive per lunghi periodi di tempo. L'integrazione avviene in genere tramite una VPN sicura tra il provider di servizi cloud, come Azure, e i data center locali.

Si supponga di lavorare in un'azienda del settore sanitario e di avere un'applicazione con la quale i clienti possono accedere alle loro informazioni sanitarie. Una normativa richiede che i dati rimangano in una posizione fisica. Il sito Web del cliente deve essere efficiente nel rispondere a molti utenti globali.  Come soluzione, il database può essere ospitato in un data center locale e il sito Web può essere ospitato nel cloud pubblico. Viene usata una VPN tra il data center locale e il cloud pubblico. Questo scenario viene considerato un cloud ibrido.

### <a name="why-hybrid-cloud"></a>Perché scegliere un cloud ibrido?

Il cloud ibrido consente alle organizzazioni di controllare e mantenere un'infrastruttura privata per gli asset riservati. Offre anche la flessibilità di sfruttare le risorse aggiuntive nel cloud pubblico all'occorrenza. Grazie alla possibilità di ricorrere alle risorse nel cloud pubblico quando servono, si paga per potenza di calcolo aggiuntiva solo quando necessario. La transizione al cloud risulta inoltre facilitata. È possibile eseguire la migrazione in modo graduale per i vari carichi di lavoro nel corso del tempo.

Esempi dei motivi per usare il cloud ibrido:

- **Investimenti hardware esistenti:** per motivi aziendali è richiesto l'uso di un ambiente operativo e di hardware esistenti.
- **Requisiti legali:** una normativa richiede che i dati rimangano in una posizione fisica.
- **Ambiente operativo univoco:** un cloud pubblico non può replicare un ambiente operativo legacy.
- **Migrazione:**: spostare i carichi di lavoro nel cloud nel corso del tempo.
