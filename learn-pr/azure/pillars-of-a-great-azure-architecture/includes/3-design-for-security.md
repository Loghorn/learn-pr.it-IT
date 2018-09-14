Un'organizzazione sanitaria archivia informazioni personali e potenzialmente riservate sui clienti. Un evento imprevisto per la sicurezza potrebbe esporre questi dati sensibili, causando imbarazzo personale o danni finanziari. Cosa si può fare per garantire l'integrità dei dati dei clienti e la sicurezza dei sistemi aziendali? 

In questo articolo vedremo come affrontare la sicurezza di un'architettura.

## <a name="what-should-i-protect"></a>Cosa è necessario proteggere?

Gli archivi di dati dell'organizzazione o di handle è il fulcro delle risorse di entità a protezione diretta. Possono essere dati sensibili sui clienti, informazioni finanziarie sull'organizzazione o informazioni line-of-business critiche che supportano l'organizzazione. Insieme ai dati, è fondamentale proteggere anche l'infrastruttura che li contiene e le identità usate per accedervi.

I dati possono essere soggetti a ulteriori requisiti legali e normativi a seconda della località in cui si trova l'organizzazione, del tipo di dati archiviati o del settore in cui opera l'applicazione. Ad esempio, negli Stati Uniti il settore sanitario è regolato da una legge nota come Health Insurance Portability and Accountability Act (HIPAA). Nel settore finanziario, Payment Card Industry Data Security Standard riguarda la gestione di dati della carta di credito. Le organizzazioni che memorizzano i dati che si trova nell'ambito di tali leggi e standard sono necessari per verificare che determinate misure di sicurezza siano soddisfatti per la protezione dei dati. In Europa il Regolamento generale sulla protezione dei dati (GDPR) stabilisce le norme per la protezione delle informazioni personali e definisce i diritti individuali in relazione ai dati archiviati. Alcuni paesi impongono che determinati tipi di dati rimangano entro i confini nazionali.

Se si verifica una violazione della sicurezza, l'impatto sulle finanze e la reputazione di organizzazioni e clienti può essere notevole. Viene infatti meno la fiducia dei clienti nei confronti dell'organizzazione, con ripercussioni negative sulla sua solidità a lungo termine.

## <a name="defense-in-depth"></a>Difesa avanzata

Un approccio multilivello alla protezione dell'ambiente ne migliorerà le condizioni di sicurezza. Comunemente noto come _difesa in profondità_, suddividere i livelli come indicato di seguito:

* Dati
* Applicazioni
* Macchine virtuali/risorse di calcolo
* Rete
* Perimetro
* Criteri e accesso
* Sicurezza fisica

Ogni livello è incentrato su una diversa area in cui possono verificarsi attacchi e rafforza la protezione nell'eventualità in cui un livello sperimenti un errore o venga aggirato da un utente malintenzionato. Se esistesse un solo livello, un utente malintenzionato avrebbe libero accesso all'ambiente di lavoro, qualora riuscisse a superarlo. L'uso dei livelli di sicurezza aumenta il numero di operazioni che un utente malintenzionato deve eseguire per riuscire ad accedere ai sistemi e ai dati aziendali. Ogni livello avrà controlli, tecnologie e funzionalità di sicurezza specifici. Una volta identificati i meccanismi di protezione da applicare, sarà necessario trovare un equilibrio fra i costi e i requisiti aziendali e il livello di rischio a cui l'organizzazione è esposta.

![Livelli di sicurezza](../media-draft/security-layers.png)

Non esiste un solo sistema, controllo o tecnologia di sicurezza in grado di proteggere l'architettura. La sicurezza non è limitata alla tecnologia, ma riguarda anche le persone e i processi. La creazione di un ambiente che consideri la sicurezza nella sua globalità e l'aggiunta delle sicurezza tra i requisiti predefiniti contribuiranno a rendere l'organizzazione il più sicura possibile.

## <a name="common-attacks"></a>Attacchi più comuni

A ogni livello sono associati alcuni attacchi comuni da cui è opportuno proteggersi. Sono solo un esempio, ma possono dare un'idea del modo in cui ogni livello può essere attaccato e dei tipi di protezioni che può essere necessario considerare.

* **Livello dati**: l'esposizione della chiave di crittografia o l'uso di una crittografia debole può lasciare i dati vulnerabili in caso di accesso non autorizzato.
* **Livello applicazione**: l'aggiunta e l'esecuzione di malware sono le caratteristiche distintive degli attacchi del livello applicazione. Gli attacchi più comuni sono gli attacchi SQL injection e gli attacchi tramite script da altri siti (XSS).
* **Livello macchina virtuale/calcolo**: il malware è un metodo molto usato per attaccare un ambiente e consiste nell'esecuzione di codice dannoso per compromettere un sistema. Una volta che il malware è entrato nel sistema, possono verificarsi altri attacchi che conducono all'esposizione delle credenziali e allo spostamento laterale nell'intero sistema.
* **Livello rete**: le porte aperte a Internet senza necessità costituiscono un metodo di attacco comune. In questo scenario i protocolli SSH o RDP potrebbero rimanere aperti alle macchine virtuali, permettendo agli utenti malintenzionati di eseguire attacchi di forza bruta per accedere ai sistemi aziendali.
* **Livello perimetro**: questo livello è spesso soggetto ad attacchi Denial of Service (DoS). Questi attacchi tentano di sovraccaricare le risorse di rete, obbligandole a passare offline o rendendole incapaci di rispondere alle richieste legittime.
* **Livello criteri e accesso**: è il livello in cui ha luogo l'autenticazione dell'applicazione. Può includere protocolli di autenticazione moderni come OpenID Connect, OAuth o l'autenticazione basata su Kerberos come Active Directory. A questo livello le credenziali esposte rappresentano un rischio ed è importante limitare il numero di autorizzazioni delle identità. È anche opportuno implementare un sistema di monitoraggio che individui i possibili account compromessi, come gli accessi da posizioni insolite.
* **Livello fisico**: questo livello può essere soggetto ad accessi non autorizzati alle strutture aziendali tramite metodi come la progettazione di ingressi e il furto dei badge di sicurezza.

## <a name="shared-security-responsibility"></a>Responsabilità di sicurezza condiviso

Rivedere il modello di responsabilità condivisa, è possibile rivaluti questo nel contesto di sicurezza. A seconda del tipo di servizio che scelto, alcuni meccanismi di protezione verranno compilate al servizio, mentre altre rimangono responsabilità dell'utente. Un'attenta valutazione delle tecnologie che si seleziona servizi e sarà necessaria, verificare di avere specificato i controlli di sicurezza appropriate per l'architettura.

![Responsabilità di sicurezza condiviso](../media-draft/shared_responsibilities.png)