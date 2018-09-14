Per la maggior parte delle organizzazioni, i dati sono l'asset più prezioso e non sostituibili. La crittografia viene utilizzato come la più avanzata e l'ultima linea di difesa in una strategia di sicurezza su più livelli. 

Contoso Shipping sa che la crittografia è la protezione sola i propri dati dispone di una volta lascia il Data Center come andava alle App per dispositivi mobili.

## <a name="what-is-encryption"></a>Che cos'è la crittografia?

La crittografia è il processo che rende illeggibili e inutilizzabili i dati. Per usare o leggere i dati crittografati, devono essere *decrittografati*. Questo processo richiede l'uso di una chiave privata. Esistono due tipi di primo livello di crittografia: **simmetrica** e **asimmetrica**.

La crittografia simmetrica utilizza la stessa chiave per crittografare e decrittografare i dati. Si consideri un'applicazione desktop di gestione delle password. Si immettono le password e vengono crittografate con la propria chiave privata (la chiave spesso deriva dalla password principale). Quando i dati devono essere recuperati, viene usata la stessa chiave e i dati vengono decrittografati.

La crittografia asimmetrica usa una chiave pubblica e una coppia di chiavi private. Entrambe le chiavi in grado di crittografare, ma una sola chiave non può decrittografare i propri dati crittografati. Per decrittografare, è necessaria la chiave associata. La crittografia asimmetrica viene usata per elementi quali sicurezza TLS (Transport Layer) (usato in HTTPS) e la firma dei dati.

Le crittografie simmetriche e asimmetriche rivestono un ruolo fondamentale per la corretta protezione dei dati. 

In genere, la crittografia è eseguita in due modi: crittografia di dati inattivi e crittografia in transito.

## <a name="encryption-in-transit"></a>Crittografia in transito

I dati in transito sono dati che vengono spostati attivamente da una posizione a un'altra, ad esempio attraverso la rete internet o tramite una rete privata. Trasferimento sicuro può essere gestito da diversi livelli. È possibile eseguirla crittografando i dati a livello di applicazione prima di inviarli in rete. HTTPS è un esempio del livello di applicazione della crittografia di transito. 

È anche possibile configurare un canale protetto, ad esempio una rete privata virtuale (VPN), a un livello di rete, per trasmettere dati tra due sistemi. 

La crittografia dei dati in transito consente di proteggere i dati da osservatori esterni e fornisce un meccanismo per trasmettere dati limitando il rischio di esposizione. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Crittografia in transito](../media-COPIED-FROM-DESIGNFORSECURITY/encryption-in-transit.png)


## <a name="encryption-at-rest"></a>Crittografia di dati inattivi

I dati inattivi sono i dati che sono stati archiviati su un supporto fisico. Potrebbe trattarsi di dati archiviati sul disco di un server, dati archiviati in un database o dati archiviati in un account di archiviazione. Indipendentemente dal meccanismo di archiviazione, la crittografia dei dati inattivi assicura che i dati archiviati siano illeggibili senza le chiavi e segreti necessari per decrittografare i dati. Se un utente malintenzionato è stato per ottenere un disco rigido con i dati crittografati e non ha accesso alle chiavi di crittografia, l'utente malintenzionato sarebbe non compromettere i dati senza grande difficoltà.

I dati effettivi che vengono crittografati possono variare nel contenuto, uso e importanza per l'organizzazione. Ciò potrebbe essere critici per l'azienda, proprietà intellettuale che è stato sviluppato da business, i dati personali sui clienti o dipendenti che archivia il business, le informazioni finanziarie e anche le chiavi e segreti usati per la crittografia dei dati stessi .

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Crittografia dei dati inattivi](../media-COPIED-FROM-DESIGNFORSECURITY/encryption-at-rest.png)

## <a name="encryption-on-azure"></a>Crittografia in Azure

Esaminiamo ora alcuni modi in cui Azure ti permette di crittografare i dati tra servizi.

### <a name="encrypt-raw-storage"></a>Crittografare l'archiviazione non elaborato

La crittografia del servizio di archiviazione di Azure per dati inattivi consente di proteggere i dati in modo da soddisfare i criteri di sicurezza e conformità dell'organizzazione. Con questa funzionalità, la piattaforma di archiviazione di Azure è in grado di crittografare automaticamente i dati prima del salvataggio permanente in Azure Managed Disks, Archiviazione BLOB di Azure, File di Azure o Archiviazione code di Azure e di decrittografarli prima del recupero. La gestione della crittografia, la crittografia di dati inattivi, la decrittografia e la gestione delle chiavi in crittografia del servizio di archiviazione sono attività completamente trasparenti per le applicazioni che usano il servizio.

### <a name="encrypt-virtual-machines"></a>Crittografare le macchine virtuali

Crittografia del servizio di archiviazione fornisce la protezione di crittografia di basso livello per i dati scritti sul disco fisico, ma come proteggere i dischi rigidi virtuali (VHD) delle macchine virtuali? Se un utente malintenzionato riesca ad accedere alla sottoscrizione di Azure ed exfiltrated i dischi rigidi virtuali delle macchine virtuali, come si garantirà non sarebbe in grado di accedere ai dati archiviati sul disco rigido virtuale?

Crittografia dischi di Azure è una funzionalità che consente di crittografare i dischi delle macchine virtuali IaaS Windows e Linux. Crittografia dischi di Azure sfrutta la funzionalità BitLocker standard del settore di Windows e la funzionalità dm-crypt di Linux per fornire la crittografia del volume per i dischi del sistema operativo e dati. La soluzione è integrata con Azure Key Vault per consentire di controllare e gestire le chiavi di crittografia dei dischi e i segreti e le identità del servizio gestito è possibile usare per l'accesso a Key Vault.

Per la spedizione di Contoso, usando macchine virtuali era una delle loro primo si sposta verso il cloud. Tutti i dischi rigidi virtuali crittografati è un modo molto semplice, basso impatto per assicurarsi che è possibile eseguire tutti i che loro disposizione per proteggere i propri dati.

### <a name="encrypt-databases"></a>Crittografare i database

La funzionalità Transparent Data Encryption (TDE) consente di proteggere il database SQL di Azure e Azure Data Warehouse dalla minaccia di attività dannose. Esegue in tempo reale la crittografia e la decrittografia del database, dei backup associati e dei file di log delle transazioni inattivi, senza richiedere modifiche dell'applicazione. Per impostazione predefinita, Transparent Data Encryption è abilitato per tutte le istanze di Database SQL di Azure appena distribuite.

TDE esegue la crittografia dell'archiviazione di un intero database usando una chiave simmetrica detta "chiave di crittografia del database". Per impostazione predefinita, Azure fornisce una chiave di crittografia univoca per ogni istanza di SQL Server logico e gestisce tutti i dettagli. Trasferire la propria chiave (BYOK) è supportato anche usando chiavi archiviate in Azure Key Vault.

Poiché TDE è abilitato per impostazione predefinita, Contoso Shipping può essere certi che hanno la protezione appropriata per i dati archiviati nei database.

### <a name="encrypt-secrets"></a>Crittografare i segreti

Si è osservato che i servizi di crittografia che tutte utilizzano le chiavi per crittografare e decrittografare i dati, così come è assicurarsi che le chiavi siano sicure? Le aziende possono avere anche le password, stringhe di connessione o altri informazioni sensibili che devono archiviare in modo sicuro.

Azure Key Vault è un servizio cloud che funziona come archivio protetto dei segreti. Key Vault consente di creare più contenitori sicuri denominati insiemi di credenziali. Questi insiemi di credenziali sono supportati da moduli di protezione hardware. Gli insiemi di credenziali consentono di ridurre le probabilità di perdita accidentale di informazioni di sicurezza centralizzando l'archiviazione dei segreti delle applicazioni. Insiemi di credenziali chiave anche controllare e registrare l'accesso a tutti gli elementi archiviati in essi contenuti. Azure Key Vault può gestire la richiesta e rinnovo dei certificati TLS, offrendo le funzionalità necessarie per una soluzione di gestione del ciclo di vita certificato affidabile. Key Vault è progettato per supportare qualsiasi tipo di segreto. Questi segreti potrebbe essere le password, credenziali del database, le chiavi API e i certificati.

Poiché le identità di Azure AD possono essere concesso l'accesso per usare i segreti di Azure Key Vault, le applicazioni con le identità del servizio gestito abilitate può automaticamente e acquisire facilmente i segreti che necessari.

La crittografia è spesso l'ultimo livello di difesa da utenti malintenzionati e rappresenta un importante di un approccio a più livelli alla protezione dei sistemi. Azure offre funzionalità incorporate e servizi per crittografare e proteggere i dati dall'esposizione non intenzionale. Protezione dei dati dei clienti archiviati all'interno di servizi di Azure è di fondamentale importanza per Microsoft e deve essere incluse in qualsiasi progetto. Servizi fondamentali come archiviazione di Azure, macchine virtuali di Azure, Database SQL di Azure e Azure Key Vault consentono di proteggere l'ambiente tramite la crittografia.