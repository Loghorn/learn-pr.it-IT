Per la maggior parte delle organizzazioni, i dati costituiscono un asset prezioso e insostituibile. La crittografia è l'ultima, nonché la più potente, linea di difesa in una strategia di sicurezza a più livelli. 

Contoso Shipping sa che la crittografia è l'unica protezione per i dati una volta che questi lasciano il data center diretti alle app per dispositivi mobili.

## <a name="what-is-encryption"></a>Che cos'è la crittografia?

La crittografia è il processo che rende illeggibili e inutilizzabili i dati per gli utenti non autorizzati a visualizzarli. Per usare o leggere i dati crittografati, è necessario *decrittografarli*. Questo processo richiede l'uso di una chiave privata. Esistono due tipi principali di crittografia: **simmetrica** e **asimmetrica**.

La crittografia simmetrica usa la stessa chiave per crittografare e decrittografare i dati. Si consideri un'applicazione desktop di gestione delle password. Le password immesse vengono crittografate con la propria chiave personale, che spesso deriva dalla propria password master. Quando i dati devono essere recuperati, viene usata la stessa chiave e i dati vengono decrittografati.

La crittografia asimmetrica usa una coppia di chiavi: una chiave pubblica e una chiave privata. È possibile crittografare i dati con l'una o l'altra chiave, ma i dati non possono essere decrittografati con una sola chiave. Per decrittografare i dati, è necessaria l'altra chiave della coppia. La crittografia asimmetrica viene usata, ad esempio, per il protocollo TLS (Transport Layer Security), usato in HTTPS, e per la firma dei dati.

Sia la crittografia simmetrica che quella asimmetrica svolgono una funzione nella corretta protezione dei dati. 

In genere, la crittografia è usata in due ambiti: crittografia di dati inattivi e crittografia in transito.

## <a name="encryption-in-transit"></a>Crittografia in transito

I dati in transito sono dati che vengono spostati attivamente da una posizione a un'altra, ad esempio attraverso Internet o in una rete privata. Il trasferimento sicuro può essere gestito da vari livelli diversi. Ad esempio, tramite la crittografia dei dati al livello dell'applicazione prima dell'invio attraverso una rete. HTTPS è un esempio di crittografia in transito a livello dell'applicazione. 

È anche possibile configurare un canale sicuro, come una rete privata virtuale (VPN), a livello della rete per trasmettere i dati tra due sistemi. 

La crittografia dei dati in transito consente di proteggere i dati da osservatori esterni e fornisce un meccanismo per trasmettere dati limitando il rischio di esposizione. 

![Crittografia in transito](../media/encryption-in-transit.png)


## <a name="encryption-at-rest"></a>Crittografia di dati inattivi

I dati inattivi sono dati archiviati su un supporto fisico. Può trattarsi di dati archiviati sul disco di un server, in un database o in un account di archiviazione. Indipendentemente dal meccanismo di archiviazione, la crittografia dei dati inattivi assicura che i dati archiviati siano illeggibili senza le chiavi e i segreti necessari per decrittografarli. Se un utente malintenzionato riesce a ottenere un disco rigido con dati crittografati, ma non ha accesso alle chiavi di crittografia, non sarà in grado di compromettere i dati, se non con grande difficoltà.

I dati effettivi che vengono crittografati possono variare nel contenuto, nell'uso e nell'importanza per l'organizzazione. Potrebbe trattarsi di informazioni finanziarie fondamentali per il business, proprietà intellettuale sviluppata dall'azienda, dati personali archiviati dall'azienda relativi a clienti o dipendenti e anche di chiavi e segreti usati per la crittografia dei dati stessi.

![Crittografia di dati inattivi](../media/encryption-at-rest.png)

## <a name="encryption-on-azure"></a>Crittografia in Azure

Esaminiamo ora alcuni modi in cui Azure permette di crittografare i dati tra servizi.

:::row:::
  :::column:::
    ![Immagine che rappresenta dati archiviati crittografati](../media/4-encrypt-raw-storage.png)
  :::column-end:::
    :::column span="3"::: **Crittografare dati archiviati non elaborati**

La crittografia del servizio di archiviazione di Azure per dati inattivi consente di proteggere i dati in modo da soddisfare i criteri di sicurezza e conformità dell'organizzazione. Con questa funzionalità, la piattaforma di archiviazione di Azure crittografa automaticamente i dati prima del salvataggio permanente in Azure Managed Disks, Archiviazione BLOB di Azure, File di Azure o Archiviazione code di Azure e li decrittografa prima del recupero. La gestione della crittografia, della crittografia dei dati inattivi, della decrittografia e delle chiavi nella crittografia del servizio di archiviazione è trasparente per le applicazioni che usano il servizio.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Immagine che rappresenta una macchina virtuale crittografata](../media/4-encrypt-virtual-machines.png)
  :::column-end:::
    :::column span="3"::: **Crittografare le macchine virtuali**

La crittografia del servizio di archiviazione offre una protezione di basso livello dei dati scritti su dischi fisici, ma come è possibile proteggere i dischi rigidi virtuali delle macchine virtuali? Se un utente malintenzionato riesce ad accedere a una sottoscrizione di Azure e a estrarre i dischi rigidi virtuali delle macchine virtuali, come è possibile garantire che non riesca ad accedere ai dati archiviati su tali dischi?

Crittografia dischi di Azure è una funzionalità che consente di crittografare i dischi delle macchine virtuali IaaS Windows e Linux. Crittografia dischi di Azure sfrutta la funzionalità standard di settore BitLocker di Windows e la funzionalità dm-crypt di Linux per fornire la crittografia del volume per i dischi dati e il sistema operativo. La soluzione è integrata con Azure Key Vault per consentire di controllare e gestire i segreti e le chiavi di crittografia dei dischi. È anche possibile usare le identità del servizio gestite per accedere a Key Vault.

Per Contoso Shipping, usare le macchine virtuali è stato uno dei primi passaggi della migrazione al cloud. La crittografia dei dischi rigidi virtuali è un modo semplice e a impatto ridotto per garantire la migliore protezione dei dati aziendali.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Immagine che rappresenta un database crittografato](../media/4-encrypt-databases.png)
  :::column-end:::
    :::column span="3"::: **Crittografare i database**

La funzionalità Transparent Data Encryption (TDE) consente di proteggere il database SQL di Azure e Azure Data Warehouse dalla minaccia di attività dannose. Esegue in tempo reale la crittografia e la decrittografia del database, dei backup associati e dei file di log delle transazioni inattivi, senza richiedere modifiche dell'applicazione. Per impostazione predefinita, Transparent Data Encryption è abilitata per tutte le istanze di database SQL di Azure appena distribuite.

TDE esegue la crittografia dello spazio di archiviazione di un intero database usando una chiave simmetrica detta "chiave di crittografia del database". Per impostazione predefinita, Azure fornisce una chiave di crittografia univoca per ogni istanza di SQL Server logica e gestisce tutti i dettagli. È supportato anche il servizio BYOK (Bring Your Own Key) tramite chiavi archiviate in Azure Key Vault.

Poiché TDE è abilitata per impostazione predefinita, è possibile essere certi che Contoso Shipping abbia la protezione appropriata per i dati archiviati nei propri database.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Immagine che rappresenta un segreto crittografato](../media/4-encrypt-secrets.png)
  :::column-end:::
    :::column span="3"::: **Crittografare i segreti**

Come già visto, tutti i servizi di crittografia usano chiavi per crittografare e decrittografare i dati, quindi in che modo è possibile assicurarsi che le chiavi stesse siano protette? Le aziende potrebbero inoltre avere anche password, stringhe di connessione o altre informazioni sensibili da archiviare in modo sicuro.

Azure Key Vault è un servizio cloud che funziona come archivio sicuro dei segreti. Key Vault consente di creare più contenitori sicuri denominati insiemi di credenziali. Questi insiemi di credenziali sono supportati da moduli di protezione hardware. Gli insiemi di credenziali consentono di ridurre le probabilità di perdita accidentale di informazioni di sicurezza centralizzando l'archiviazione dei segreti delle applicazioni. Gli insiemi di credenziali delle chiavi controllano e registrano anche l'accesso a tutti gli elementi archiviati al loro interno. Azure Key Vault può gestire la richiesta e il rinnovo dei certificati TLS, offrendo le funzionalità necessarie per una soluzione affidabile di gestione del ciclo di vita dei certificati. Key Vault è progettato per supportare qualsiasi tipo di segreto. I segreti possono essere password, credenziali del database, chiavi API e certificati.

Poiché è possibile concedere alle identità di Azure AD l'accesso per usare i segreti di Azure Key Vault, le applicazioni con identità del servizio gestite abilitate possono acquisire automaticamente e facilmente i segreti necessari.
  :::column-end:::
:::row-end:::

## <a name="summary"></a>Riepilogo

Come forse è già noto, la crittografia spesso è l'ultimo livello di difesa da utenti malintenzionati e rappresenta una parte importante dell'approccio a più livelli per la protezione dei sistemi. Azure offre funzionalità e servizi integrati per crittografare e proteggere i dati dall'esposizione non intenzionale. La protezione dei dati dei clienti archiviati nei servizi di Azure è di importanza fondamentale per Microsoft e deve essere inclusa in qualsiasi tipo di progettazione. Servizi di base come Archiviazione di Azure, Macchine virtuali di Azure, Database SQL di Azure e Azure Key Vault consentono di proteggere l'ambiente aziendale tramite la crittografia.