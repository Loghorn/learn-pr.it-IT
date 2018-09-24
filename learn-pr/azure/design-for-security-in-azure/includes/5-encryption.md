I dati sono le risorse più preziose e insostituibili di un’organizzazione e la crittografia rappresenta l'ultima e più forte linea di difesa in una strategia di sicurezza su più livelli. In quanto operatore sanitario, Lamna Healthcare archivia grandi quantità di dati sensibili. Di recente si è verificata una violazione che ha esposto i dati sensibili non crittografati dei pazienti e ora Lamna Healthcare è pienamente consapevole di avere delle lacune nel proprio approccio alla protezione dei dati. Lamna Healthcare desidera capire come avrebbe potuto utilizzare la crittografia in modo migliore per proteggere l'azienda e i pazienti da questo tipo di incidente. In questa sezione vedremo cos’è la crittografia, come implementare la crittografia dei dati e quali funzionalità di crittografia sono disponibili in Azure.

## <a name="what-is-encryption"></a>Che cos'è la crittografia?

La crittografia è il processo che rende illeggibili e inutilizzabili i dati. Per usare o leggere i dati crittografati, è necessario *decrittografarli*. Questo processo richiede l'uso di una chiave privata. Esistono due tipi principali di crittografia: **simmetrica** e **asimmetrica**.

La crittografia simmetrica usa la stessa chiave per crittografare e decrittografare i dati. Si consideri un'applicazione desktop di gestione delle password. Le password immesse vengono crittografate con la propria chiave personale, che spesso deriva dalla propria password master. Quando i dati devono essere recuperati, viene usata la stessa chiave e i dati vengono decrittografati.

La crittografia asimmetrica usa una coppia di chiavi: una chiave pubblica e una chiave privata. È possibile crittografare i dati con l'una o l'altra chiave, ma i dati non possono essere decrittografati con la stessa chiave. Per decrittografare i dati è necessaria l'altra chiave della coppia. La crittografia asimmetrica viene usata, ad esempio, per il protocollo TLS (usato in https) e per la firma dei dati.

Sia la crittografia simmetrica che quella asimmetrica svolgono una funzione nella corretta protezione dei dati. 

In genere, la crittografia è usata in due ambiti: crittografia di dati inattivi e crittografia in transito.

### <a name="encryption-at-rest"></a>Crittografia di dati inattivi

I dati inattivi sono dati archiviati su un supporto fisico. Può trattarsi di dati archiviati sul disco di un server, in un database o in un account di archiviazione. Indipendentemente dal meccanismo di archiviazione, la crittografia dei dati inattivi assicura che i dati archiviati siano illeggibili senza le chiavi e i segreti necessari per decrittografarli. Se un utente malintenzionato riesce a ottenere un disco rigido con i dati crittografati, ma non ha accesso alle chiavi di crittografia, non sarà in grado di compromettere i dati, se non con grande difficoltà. In uno scenario di questo tipo, dovrebbe tentare attacchi contro i dati crittografati, che sono molto più complessi e con un maggior consumo di risorse rispetto all'accesso a dati non crittografati su un disco rigido.

I dati effettivi che vengono crittografati possono variare nel contenuto, nell'uso e nell'importanza per l'organizzazione. Potrebbe trattarsi di informazioni finanziarie fondamentali per il business, proprietà intellettuale sviluppata dall'azienda, dati personali archiviati dall'azienda relativi a clienti o dipendenti e anche di chiavi e segreti usati per la crittografia dei dati stessi.

![Figura che mostra un esempio di crittografia di dati inattivi.](../media/encryption-at-rest.png)

### <a name="encryption-in-transit"></a>Crittografia di dati in transito

I dati in transito sono dati che vengono spostati attivamente da una posizione a un'altra, ad esempio attraverso Internet o in una rete privata. La sicurezza del trasferimento può essere implementata attraverso la crittografia dei dati prima dell'invio in una rete o attraverso la configurazione di un canale sicuro per trasmettere i dati non crittografati tra due sistemi. La crittografia dei dati in transito consente di proteggere i dati da osservatori esterni e fornisce un meccanismo per trasmettere dati limitando il rischio di esposizione. 

![Figura che mostra un esempio di crittografia di dati in transito. Tutti i dati vengono crittografati prima di essere trasferiti. Quando raggiungono la destinazione, i dati vengono quindi decrittografati.](../media/encryption-in-transit.png)

## <a name="identify-and-classify-data"></a>Identificare e classificare i dati

Rivediamo il problema che Lamna Healthcare sta tentando di risolvere. L'azienda aveva avuto incidenti precedenti che avevano esposto dati sensibili, pertanto i dati che sta crittografando non coincidono con quelli che dovrebbero essere crittografati. Per iniziare, si dovranno identificare e classificare i tipi di dati archiviati e allinearli ai requisiti aziendali e normativi riguardanti l'archiviazione dei dati. È utile classificare i dati in relazione all'impatto della loro esposizione per l'organizzazione e per i suoi clienti o partner. Ecco un esempio di classificazione:

|Classificazione dei dati|Spiegazione|Esempi|
|---|---|---|
|Con restrizioni|I dati classificati con restrizioni pongono rischi significativi se esposti, modificati o eliminati. Per questi dati sono richiesti livelli di protezione elevati. |Dati che contengono numeri di previdenza sociale, numeri di carta di credito, cartelle cliniche|
|Privati| I dati classificati come privati pongono rischi moderati se esposti, modificati o eliminati. Per questi dati sono richiesti livelli di protezione ragionevoli. I dati che non sono classificati con restrizioni o come pubblici saranno classificati come privati.  |Informazioni personali come indirizzo, numero di telefono, risultati accademici, dati sugli acquisti del cliente|
|Pubblici| I dati classificati come pubblici non pongono rischi se esposti, modificati o eliminati. Per questi dati non è richiesta alcuna protezione. |Rendiconti finanziari pubblici, politiche pubbliche, documentazione del prodotto per i clienti|

Passando in rassegna i tipi di dati archiviati, l'azienda può capire meglio dove si trovano i dati sensibili e dove viene attualmente applicata o meno la crittografia.

È inoltre importante una conoscenza approfondita dei requisiti normativi e aziendali applicabili ai dati archiviati dell'organizzazione. I requisiti normativi che un'organizzazione deve rispettare spesso determinano una gran parte dei requisiti di crittografia dei dati. Lamna Healthcare archivia dati sensibili soggetti all’Health Insurance Portability and Accountability Act (HIPAA), che definisce i requisiti per la gestione e l'archiviazione dei dati dei pazienti. Altri settori possono essere soggetti a diversi requisiti normativi. Un istituto finanziario può archiviare informazioni sui conti dei clienti, soggette allo standard PCI (Payment Card Industry). Un'organizzazione che svolge attività commerciali nell'Unione europea può essere soggetta al Regolamento generale sulla protezione dei dati (GDPR), che definisce la gestione dei dati personali nell'Unione europea. I requisiti aziendali possono anche richiedere che siano crittografati tutti i dati che contengono informazioni concorrenziali, che possono esporre l'organizzazione a rischi finanziari.

Dopo aver classificato i dati e definito i requisiti, è possibile sfruttare diversi strumenti e tecnologie per implementare e applicare la crittografia nella propria architettura.

## <a name="encryption-on-azure"></a>Crittografia in Azure

Esaminiamo ora alcuni modi in cui Azure permette di crittografare i dati tra servizi.

### <a name="encrypting-raw-storage"></a>Crittografia di dati archiviati non elaborati

La crittografia del servizio di archiviazione di Azure per dati inattivi consente di proteggere i dati in modo da soddisfare i criteri di sicurezza e conformità dell'organizzazione. Con questa funzionalità, la piattaforma di archiviazione di Azure crittografa automaticamente i dati prima del salvataggio permanente in Azure Managed Disks, Archiviazione BLOB di Azure, File di Azure o Archiviazione code di Azure e li decrittografa prima del recupero. La gestione della crittografia, della crittografia dei dati inattivi, della decrittografia e delle chiavi nella crittografia del servizio di archiviazione è trasparente per le applicazioni che usano il servizio.

Per Lamna Healthcare, ciò significa che ogni volta che usa servizi che supportano la crittografia del servizio di archiviazione, i dati sono crittografati nel supporto fisico di archiviazione. Nell'evento estremamente improbabile di accesso non autorizzato al disco fisico, i dati saranno illeggibili poiché sono stati crittografati al momento della scrittura sul disco fisico.

### <a name="encrypting-virtual-machines"></a>Crittografia delle macchine virtuali

La crittografia del servizio di archiviazione offre una protezione di basso livello dei dati scritti su dischi fisici, ma come è possibile proteggere i dischi rigidi virtuali (VHD) delle macchine virtuali? Se un utente malintenzionato riesce ad accedere a una sottoscrizione di Azure e ad estrarre i dischi rigidi virtuali delle macchine virtuali, come è possibile garantire che non sia in grado di accedere ai dati archiviati su tali dischi?

Crittografia dischi di Azure è una funzionalità che consente di crittografare i dischi di macchine virtuali IaaS Windows e Linux. Crittografia dischi di Azure applica la funzionalità standard di settore BitLocker di Windows e la funzionalità DM-Crypt di Linux per offrire la crittografia dei volumi dei dischi contenenti i dati e il sistema operativo. La soluzione è integrata con Azure Key Vault per consentire di controllare e gestire i segreti e le chiavi di crittografia dei dischi (ed è possibile usare identità gestite per servizi di Azure per accedere all'insieme di credenziali delle chiavi).

 Lamna Healthcare può applicare Crittografia dischi di Azure alle proprie macchine virtuali per garantire che tutti i dati archiviati nei dischi rigidi virtuali siano protetti secondo i requisiti dell'organizzazione e normativi. Poiché anche i dischi di avvio sono crittografati, è possibile controllarne l'utilizzo.

### <a name="encrypting-databases"></a>Crittografia dei database

Lamna Healthcare dispone di diversi database distribuiti che archiviano dati che necessitano di protezione aggiuntiva. Ha spostato molti database al database SQL di Azure e vuole assicurarsi che i dati siano crittografati nel database. In caso di violazione dei file di dati, dei file di log o dei file di backup, vuole assicurarsi che siano illeggibili senza avere accesso alle chiavi di crittografia.

La funzionalità Transparent Data Encryption (TDE) consente di proteggere il database SQL di Azure e Azure Data Warehouse dalla minaccia di attività dannose. Esegue in tempo reale la crittografia e la decrittografia del database, dei backup associati e dei file di log delle transazioni inattivi, senza richiedere modifiche dell'applicazione. Per impostazione predefinita, Transparent Data Encryption è abilitata per tutti i database SQL di Azure appena distribuiti.

TDE esegue la crittografia dell'archiviazione di un intero database usando una chiave simmetrica detta "chiave di crittografia del database". Per impostazione predefinita, Azure fornisce una chiave di crittografia univoca per ogni SQL Server logico e gestisce tutti i dettagli. È inoltre supportato il servizio Bring Your Own Key tramite chiavi archiviate in Azure Key Vault.

Poiché TDE è abilitata per impostazione predefinita, Lamna Healthcare può essere certa di avere la protezione appropriata per i dati archiviati nei propri database.

### <a name="encrypting-secrets"></a>Crittografia dei segreti

Come già visto, tutti i servizi di crittografia usano chiavi per crittografare e decrittografare i dati, quindi in che modo è possibile assicurarsi che le chiavi stesse siano protette? Lamna Healthcare potrebbe avere anche password, stringhe di connessione o altre informazioni sensibili da archiviare in modo sicuro.

Azure Key Vault è un servizio cloud che funziona come archivio sicuro dei segreti. Key Vault consente di creare più contenitori sicuri denominati insiemi di credenziali. Questi insiemi di credenziali sono supportati da moduli di protezione hardware. Gli insiemi di credenziali consentono di ridurre le probabilità di perdita accidentale di informazioni di sicurezza centralizzando l'archiviazione dei segreti delle applicazioni. Gli insiemi di credenziali delle chiavi controllano e registrano anche l'accesso a tutti gli elementi archiviati al loro interno. Azure Key Vault può gestire la richiesta e il rinnovo dei certificati TLS (Transport Layer Security), offrendo le funzionalità necessarie per una soluzione affidabile di gestione del ciclo di vita dei certificati. Key Vault è progettato per supportare qualsiasi tipo di segreto. I segreti possono essere password, credenziali del database, chiavi API e certificati.

Poiché è possibile concedere alle identità di Azure AD l’accesso per usare i segreti di Azure Key Vault, le applicazioni che usano identità gestite per i servizi Azure possono acquisire automaticamente e facilmente i segreti necessari.

Lamna Healthcare può usare Key Vault per l'archiviazione di tutte le informazioni sensibili delle applicazioni, inclusi i certificati TLS usati per proteggere la comunicazione tra i sistemi.

## <a name="encryption-at-lamna-healthcare"></a>Crittografia in Lamna Healthcare

Lamna Healthcare ha eseguito il processo di identificazione e classificazione di tutti i dati che archivia. Ha allineato queste classificazioni con i requisiti normativi e aziendali e ha constatato di avere molti più dati da crittografare. Ha crittografato tutte le macchine virtuali che archiviano dati sensibili e sta eseguendo la crittografia di tutte le informazioni sensibili dei pazienti che si trovano nell'archiviazione BLOB. Transparent Data Encryption (TDE) è abilitato in tutti i database di Lamna Healthcare e pertanto i loro database relazionali rispondono ai requisiti di crittografia aziendali indipendentemente dalla classificazione. Lamna Healthcare ha anche esteso a tutta l'organizzazione l'uso di Key Vault per archiviare tutti i certificati e le informazioni sulle credenziali che possono servire al funzionamento delle applicazioni.

## <a name="summary"></a>Riepilogo

La crittografia spesso è l'ultimo livello di difesa da utenti malintenzionati e rappresenta una parte importante dell'approccio a più livelli per la protezione dell'architettura. Azure offre funzionalità e servizi integrati per crittografare e proteggere i dati dall'esposizione non intenzionale. La protezione dei dati dei clienti archiviati nei servizi di Azure è di importanza fondamentale per Microsoft e deve essere inclusa nella progettazione di qualsiasi architettura. Servizi di base come archiviazione di Azure, macchine virtuali di Azure, Database SQL di Azure e Azure Key Vault consentono di proteggere l'ambiente aziendale tramite la crittografia.
