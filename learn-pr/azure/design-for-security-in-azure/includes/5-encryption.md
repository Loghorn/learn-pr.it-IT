I dati sono le risorse più preziose e insostituibili di un’organizzazione e la crittografia viene utilizzata come la più avanzata e recente linea di difesa in una strategia di sicurezza su più livelli. In quanto provider di servizi sanitari, Lamna Healthcare archivia grandi quantità di dati sensibili. Di recente si è verificata una violazione che ha esposto i dati sensibili non crittografati dei pazienti e ora Lamna Healthcare è pienamente consapevole di avere delle lacune nella protezione dei dati. Lamna Healthcare desidera capire come avrebbe potuto utilizzare la crittografia in modo migliore per proteggere se stessi e i pazienti da questo tipo di evento imprevisto. In questa sezione, vedremo cos’è la crittografia, come implementare la crittografia dei dati e quali funzionalità di crittografia sono disponibili in Azure.

## <a name="what-is-encryption"></a>Che cos'è la crittografia?

La crittografia è il processo che rende illeggibili e inutilizzabili i dati. Per usare o leggere i dati crittografati, devono essere *decrittografati*. Questo processo richiede l'uso di una chiave privata. Esistono due tipi di primo livello di crittografia: **Symmetric** e **asimmetrica**.

La crittografia simmetrica utilizza la stessa chiave per crittografare e decrittografare i dati. Si consideri un'applicazione desktop di gestione delle password. Si immettono le password e vengono crittografate con la propria chiave privata (la chiave spesso deriva dalla password principale). Quando i dati devono essere recuperati, viene usata la stessa chiave e i dati vengono decrittografati.

La crittografia asimmetrica usa una chiave pubblica e una coppia di chiavi private. Entrambe le chiavi possono crittografare, ma non possono decrittografare i propri dati crittografati. Per decrittografare, è necessaria la chiave associata. La crittografia asimmetrica viene usata per operazioni come TLS (usato in https) e la firma dei dati.

Le crittografie simmetriche e asimmetriche rivestono un ruolo fondamentale per la corretta protezione dei dati. 

In genere, la crittografia è eseguita in due modi: crittografia di dati inattivi e crittografia in transito.

### <a name="encryption-at-rest"></a>Crittografia di dati inattivi

I dati inattivi sono i dati che sono stati archiviati su un supporto fisico. Potrebbe trattarsi di dati archiviati sul disco di un server, dati archiviati in un database o dati archiviati in un account di archiviazione. Indipendentemente dal meccanismo di archiviazione, la crittografia dei dati inattivi assicura che i dati archiviati siano illeggibili senza le chiavi e segreti necessari per decrittografare i dati. Se un utente malintenzionato è stato per ottenere un disco rigido con i dati crittografati e non ha accesso alle chiavi di crittografia, l'utente malintenzionato sarebbe non compromettere i dati senza grande difficoltà. In uno scenario di questo tipo, sarebbe necessario tentare attacchi contro i dati crittografati, che sono molto più complessi e con un maggior consumo di risorse rispetto all'accesso ai dati non crittografati su un disco rigido.

I dati effettivi che vengono crittografati possono variare nel contenuto, uso e importanza per l'organizzazione. Potrebbe trattarsi di informazioni finanziarie fondamentali per il business, proprietà intellettuali sviluppate dall’azienda, dati personali sui clienti o dipendenti archiviati dall’azienda e anche di chiavi e segreti usati per la crittografia dei dati stessi. .

![Crittografia di dati inattivi](../media-draft/encryption-at-rest.png)

### <a name="encryption-in-transit"></a>Crittografia in transito

I dati in transito sono dati che vengono spostati attivamente da una posizione a un'altra, ad esempio attraverso la rete internet o tramite una rete privata. Il trasferimento sicuro può essere gestito attraverso la crittografia dei dati prima dell'invio in una rete o attraverso la configurazione di un canale sicuro per trasmettere i dati non crittografati tra due sistemi. La crittografia dei dati in transito consente di proteggere i dati da osservatori esterni e fornisce un meccanismo per trasmettere dati limitando il rischio di esposizione. 

![Crittografia in transito](../media-draft/encryption-in-transit.png)

## <a name="identify-and-classify-data"></a>Identificare e classificare i dati

Rivediamo il problema che Lamna Healthcare sta tentando di risolvere. Avevano avuto incidenti precedenti che avevano esposto dati sensibili, pertanto c'è una divario tra i dati che stanno crittografando e quelli che dovrebbero essere crittografati. Per iniziare, è necessario identificare e classificare i tipi di dati che archiviano e allinearli con i requisiti aziendali e i requisiti normativi riguardanti l'archiviazione dei dati. È utile classificare i dati in relazione all'impatto di esposizione dell'organizzazione, dei relativi clienti o partner. Ecco un esempio di classificazione:

|Classificazione dei dati|Spiegazione|
|---|---|
|Con restrizioni|I dati classificati con restrizioni pongono rischi significativi se esposti, modificati o eliminati. Per questi dati sono richiesti elevati livelli di protezione. |
|Privati| I dati classificati come privati pongono rischi moderati se esposti, modificati o eliminati. Per questi dati sono richiesti livelli di protezione ragionevoli. I dati che non sono classificati con restrizioni o come pubblici saranno classificati come privati.  |
|Pubblici| I dati classificati come pubblici non pongono rischi se esposti, modificati o eliminati. Per questi dati non è richiesta alcuna protezione. |

Grazie all'uso di un inventario dei tipi di dati archiviati, delineano un quadro generale delle posizioni in cui possono essere archiviati i dati sensibili e dove la crittografia dei dati esistente può essere applicata o meno.

Inoltre, è importante una conoscenza approfondita dei requisiti normativi e aziendali applicabili ai dati archiviati dell'organizzazione. I requisiti normativi che un'organizzazione deve rispettare spesso determinano una gran parte dei requisiti di crittografia dei dati. Lamna Healthcare, archivia dati sensibili che rientrano sotto l’Health Insurance Portability and Accountability Act (HIPAA), che contiene i requisiti sulla gestione e archiviazione dei dati dei pazienti. Altri settori possono essere soggetti a diversi requisiti normativi. Un istituto finanziario può archiviare le informazioni sul conto che rientrano negli standard Payment Card Industry (PCI). Un'organizzazione che intrattiene attività commerciali nell'Unione europea può essere soggetta al Regolamento generale sulla protezione dei dati (GDPR), che definisce la gestione dei dati personali nell'Unione europea. I requisiti aziendali possono anche richiedere che tutti i dati che contengono informazioni concorrenziali, che possono esporre l'organizzazione a rischi finanziari, siano crittografati.

Dopo aver classificato i dati e definito i requisiti, è possibile sfruttare diversi strumenti e tecnologie per implementare e applicare la crittografia nell'architettura.

## <a name="encryption-on-azure"></a>Crittografia in Azure

Esaminiamo ora alcuni modi in cui Azure ti permette di crittografare i dati tra servizi.

### <a name="encrypting-raw-storage"></a>Crittografia di archiviazione non elaborata

La crittografia del servizio di archiviazione di Azure per dati inattivi consente di proteggere i dati in modo da soddisfare i criteri di sicurezza e conformità dell'organizzazione. Con questa funzionalità, la piattaforma di archiviazione di Azure è in grado di crittografare automaticamente i dati prima del salvataggio permanente in Azure Managed Disks, Archiviazione BLOB di Azure, File di Azure o Archiviazione code di Azure e di decrittografarli prima del recupero. La gestione della crittografia, la crittografia di dati inattivi, la decrittografia e la gestione delle chiavi in crittografia del servizio di archiviazione sono attività completamente trasparenti per le applicazioni che usano il servizio.

Per Lamna Healthcare, ciò significa che ogni volta che usa servizi che supportano la crittografia del servizio di archiviazione, i dati sono crittografati nel supporto fisico di archiviazione. Nell'evento estremamente improbabile che l'accesso al disco sia violato, i dati saranno illeggibili poiché sono stati crittografati al momento della scrittura sul disco fisico.

### <a name="encrypting-virtual-machines"></a>Crittografia delle macchine virtuali

La crittografia del servizio di archiviazione fornisce una protezione di crittografia di basso livello per i dati scritti sul disco fisico, ma come è possibile proteggere i dischi rigidi virtuali (VHD) delle macchine virtuali? Se un utente malintenzionato riesca ad accedere alla sottoscrizione di Azure ed exfiltrated i dischi rigidi virtuali delle macchine virtuali, come si garantirà non sarebbe in grado di accedere ai dati archiviati sul disco rigido virtuale?

Crittografia dischi di Azure (ADE) è una funzionalità che consente di crittografare i dischi di macchine virtuali IaaS Windows e Linux. Crittografia dischi di Azure applica la funzionalità standard di settore BitLocker di Windows e la funzionalità DM-Crypt di Linux per offrire la crittografia del volume per i dischi dati e del sistema operativo. La soluzione è integrata con Azure Key Vault per consentire di controllare e gestire le chiavi di crittografia dei dischi e i segreti (ed è possibile usare identità del servizio gestite per accedere all’insieme di credenziali delle chiavi).

 Lamna Healthcare può applicare ADE alle macchine virtuali per avere la sicurezza che tutti i dati archiviati nei dischi rigidi virtuali siano protetti secondo i requisiti dell'organizzazione e normativi. Perché vengono anche crittografati i dischi di avvio, è possibile controllare e controllare l'utilizzo.

### <a name="encrypting-databases"></a>Crittografia dei database

Lamna Healthcare dispone di numerosi database distribuiti che archiviano i dati che necessitano di protezione aggiuntiva. Ha spostato molti database al database SQL di Azure e desidera assicurarsi che i dati siano crittografati nel rispettivo database. Se il file di dati, i file di log o file di backup sono stati rubati, vuole assicurarsi che siano leggibili senza disporre dell'accesso alle chiavi di crittografia.

La funzionalità Transparent Data Encryption (TDE) consente di proteggere il database SQL di Azure e Azure Data Warehouse dalla minaccia di attività dannose. Esegue in tempo reale la crittografia e la decrittografia del database, dei backup associati e dei file di log delle transazioni inattivi, senza richiedere modifiche dell'applicazione. Per impostazione predefinita, Transparent Data Encryption è abilitata per tutti i database SQL di Azure appena distribuiti.

TDE esegue la crittografia dell'archiviazione di un intero database usando una chiave simmetrica detta "chiave di crittografia del database". Per impostazione predefinita, Azure fornisce una chiave di crittografia univoca per ogni SQL Server logico e gestisce tutti i dettagli. Bring-your-own-key è supportato anche usando chiavi archiviate in Azure Key Vault.

Poiché TDE è abilitata per impostazione predefinita, Lamna Healthcare può essere certa di avere la protezione appropriata per i dati archiviati nei database.

### <a name="encrypting-secrets"></a>Crittografia dei segreti

Si è osservato che i servizi di crittografia che tutte utilizzano le chiavi per crittografare e decrittografare i dati, così come è assicurarsi che le chiavi siano sicure? Lamna Healthcare potrebbe avere anche password, stringhe di connessione o altri informazioni sensibili da archiviare in modo sicuro.

Azure Key Vault è un servizio cloud che funziona come archivio protetto dei segreti. Key Vault consente di creare più contenitori sicuri denominati insiemi di credenziali. Questi insiemi di credenziali sono supportati da moduli di protezione hardware. Gli insiemi di credenziali consentono di ridurre le probabilità di perdita accidentale di informazioni di sicurezza centralizzando l'archiviazione dei segreti delle applicazioni. Gli insiemi di credenziali delle chiavi controllano e registrano anche l'accesso a tutti gli elementi archiviati al loro interno. Azure Key Vault può gestire la richiesta e il rinnovo dei certificati TLS (Transport Layer Security), offrendo le funzionalità necessarie per una soluzione affidabile di gestione del ciclo di vita dei certificati. Key Vault è progettato per supportare qualsiasi tipo di segreto. Questi segreti potrebbero essere password, credenziali del database, chiavi API e certificati.

Poiché è possibile concedere alle identità di Azure AD l’accesso per usare i segreti di Azure Key Vault, le applicazioni con identità del servizio gestito abilitata possono acquisire automaticamente e facilmente i segreti necessari.

Lamna Healthcare può usare Key Vault per l'archiviazione di tutte le informazioni sensibili dell'applicazione, inclusi i certificati TLS usati per proteggere la comunicazione tra i sistemi.

## <a name="encryption-at-lamna-healthcare"></a>Crittografia in Lamna Healthcare

Lamna Healthcare ha eseguito il processo di identificazione e classificazione per tutti i dati che archivia. Ha allineato queste classificazioni con i requisiti normativi e aziendali e ha realizzato di avere molti più dati da crittografare. Ha crittografato tutte le macchine virtuali che archiviano dati sensibili e sta eseguendo la crittografia di tutte le informazioni sensibili dei pazienti che vengono archiviati nell'archivio BLOB. Transparent Data Encryption è abilitato in tutti i database, in modo che i database relazionali soddisfano i requisiti di crittografia indipendentemente dalla classificazione. Inoltre, Lamna Healthcare ha lavorato sull'uso di Key Vault da parte dell’organizzazione per archiviare tutti i certificati e le informazioni sulle credenziali che potrebbero servire alle applicazioni per l'esecuzione.

## <a name="summary"></a>Riepilogo

La crittografia spesso è l'ultimo livello di difesa da utenti malintenzionati e rappresenta un importante approccio a più livelli per la protezione dell'architettura. Azure offre funzionalità e servizi integrati per crittografare e proteggere i dati dall'esposizione non intenzionale. La protezione dei dati dei clienti archiviati nei servizi di Azure è di importanza fondamentale per Microsoft e dovrebbe essere inclusa in ogni progetto di architettura. Servizi fondamentali come archiviazione di Azure, macchine virtuali di Azure, Database SQL di Azure e Azure Key Vault consentono di proteggere l'ambiente tramite la crittografia.