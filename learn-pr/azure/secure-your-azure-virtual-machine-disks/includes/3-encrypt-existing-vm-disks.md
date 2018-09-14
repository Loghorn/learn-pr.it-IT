Si supponga che l'azienda abbia deciso di implementare Crittografia dischi di Azure in tutte le macchine virtuali. È necessario valutare come implementare la crittografia nei volumi delle macchine virtuali esistenti.

In questo modulo si esamineranno i requisiti per Crittografia dischi di Azure e i passaggi necessari per la crittografia dei dischi nelle macchine virtuali Windows esistenti.

## <a name="azure-disk-encryption-prerequisites"></a>Prerequisiti di Crittografia dischi di Azure

Prima che sia possibile crittografare il primo disco della macchina virtuale, è necessario:

1. Creare un insieme di credenziali delle chiavi.
1. Configurare un'applicazione Azure Active Directory (Azure AD) e un'entità servizio.
1. Configurare i criteri di accesso per l'insieme di credenziali delle chiavi per l'app Azure AD.
1. Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi.

### <a name="azure-key-vault"></a>Azure Key Vault

Le chiavi di crittografia utilizzate da ADE possono essere archiviate in Azure Key Vault. Azure Key Vault è uno strumento che consente di archiviare i segreti e accedervi in modo sicuro. Un segreto è qualsiasi elemento per cui si vuole controllare rigorosamente l'accesso, ad esempio chiavi API, password o certificati. Ciò fornisce archiviazione protetta altamente disponibile e scalabile, in FIPS Federal Information Processing Standards () 140-2 livello 2 convalidati moduli di protezione Hardware (HSM). Tramite Key Vault, è possibile avere il controllo completo delle chiavi usate per crittografare i dati ed è possibile gestire e controllare l'utilizzo delle chiavi. È possibile configurare e gestire l'insieme di credenziali delle chiavi usando il portale di Azure, Azure PowerShell e la CLI di Azure.

>[!NOTE]
> Crittografia dischi di Azure richiede che l'insieme di credenziali delle chiavi e le macchine virtuali nella stessa area di Azure; Ciò garantisce che i segreti di crittografia non devono attraversare i limiti a livello di area.

### <a name="azure-ad-application-and-service-principal"></a>Applicazione Azure AD ed entità servizio

Per accedere alle risorse o modificarle, ad esempio per la configurazione della crittografia per una macchina virtuale con script o codice, è prima necessario configurare un'**applicazione Azure Active Directory (AD)**. Azure AD è una directory di multi-tenant, basato sul cloud e il servizio di gestione di identità. Combina servizi directory di base, gestione dell'accesso alle applicazioni e protezione delle identità in un'unica soluzione.

È necessaria anche un'**entità servizio** di Azure. Le entità servizio sono gli account del servizio usato per eseguire lo script o codice. Consentono di assegnare le autorizzazioni specifiche e l'ambito necessari per eseguire l'attività in una risorsa di Azure specifica.

Sono presenti due elementi in Azure AD: l'oggetto applicazione è il **_definizione_** dell'applicazione (cosa) e il servizio di entità è la **_specifica istanza_**  dell'applicazione.

Questo approccio è in linea con il principio dei **privilegi minimi**, in base a cui le autorizzazioni assegnate all'app sono limitate al minimo richiesto per consentire all'app di svolgere le attività previste.

È possibile configurare e gestire applicazioni di Azure AD e le entità servizio usando il portale di Azure, Azure PowerShell e la CLI di Azure.

### <a name="key-vault-access-policies"></a>Criteri di accesso per gli insiemi di credenziali delle chiavi

Prima di archiviare le chiavi di crittografia in un insieme di credenziali delle chiavi, ADE richiede informazioni dettagliate sul **ID Client** e il **privata del Client** dell'applicazione Azure AD che ha la possibilità di scrittura per l'insieme di credenziali delle chiavi.

È anche necessario fornire ad Azure l'accesso alle chiavi di crittografia nell'insieme di credenziali delle chiavi, in modo che vengano rese disponibili alla macchina virtuale per l'avvio e la decrittografia dei volumi.

## <a name="set-key-vault-advanced-access-policies"></a>Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi

I **criteri di accesso avanzati** consentono la crittografia dei dischi nell'insieme di credenziali delle chiavi e in loro assenza le distribuzioni della crittografia hanno esito negativo. 

Ci sono tre criteri che devono essere abilitati:

- **Insieme di credenziali delle chiavi per crittografia dischi di**. Obbligatorio per crittografia dischi di Azure.
- **Key Vault per la distribuzione**. Consente al provider di risorse Microsoft. COMPUTE a recuperare segreti dall'insieme di credenziali chiave. Questo criterio è necessaria quando si crea una macchina virtuale.
- **Key Vault per la distribuzione del modello, se necessario**. Consente a Azure Resource Manager per ottenere i segreti dall'insieme di credenziali chiave. Questo criterio è necessario quando si usa modelli di Azure Resource Manager per la distribuzione della macchina virtuale.

I criteri di accesso dell'insieme di credenziali delle chiavi possono essere configurati e gestiti usando il portale di Azure, Azure PowerShell o l'interfaccia della riga di comando di Azure.

### <a name="what-is-the-azure-disk-encryption-prerequisites-configuration-script"></a>Che cos'è lo script di configurazione dei prerequisiti di crittografia dischi di Azure

Il **script di configurazione dei prerequisiti di crittografia dischi di Azure** configura tutte (o il numero desiderato) i prerequisiti di crittografia. Lo script assicura anche che l'insieme di credenziali delle chiavi sia nella stessa area della macchina virtuale si desidera crittografare. Verranno creare un gruppo di risorse e un insieme di credenziali delle chiavi e impostare i criteri di accesso dell'insieme di credenziali chiave. Lo script crea anche un blocco delle risorse nell'insieme di credenziali delle chiavi per la protezione da eliminazioni accidentali.

## <a name="encrypting-an-existing-vm-disk"></a>Crittografia di un disco di una macchina virtuale esistente

Esistono due passaggi per crittografare un disco di macchina virtuale esistente, quando si usa lo script di configurazione dei prerequisiti di crittografia dischi di Azure:

1. Eseguire lo script di configurazione dei prerequisiti di crittografia dischi di Azure.
1. Crittografare la macchina virtuale di Azure PowerShell.
