Si supponga che l'azienda abbia deciso di implementare Crittografia dischi di Azure in tutte le macchine virtuali. È necessario valutare come implementare la crittografia nei volumi delle macchine virtuali esistenti.

In questo modulo si esamineranno i requisiti per Crittografia dischi di Azure e i passaggi necessari per la crittografia dei dischi nelle macchine virtuali Windows esistenti.

## <a name="azure-disk-encryption-prerequisites"></a>Prerequisiti di Crittografia dischi di Azure

Per poter crittografare il primo disco di una macchina virtuale, è necessario

1. Creare un insieme di credenziali delle chiavi
1. Configurare un'applicazione Azure AD e un'entità servizio
1. Impostare i criteri di accesso per l'insieme di credenziali delle chiavi per l'app Azure AD
1. Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi

### <a name="azure-key-vault"></a>Azure Key Vault

Le chiavi di crittografia usate da Crittografia dischi di Azure possono essere archiviate in Azure Key Vault. Azure Key Vault è uno strumento che consente di archiviare i segreti e accedervi in modo sicuro. Un segreto è qualsiasi elemento per cui si vuole controllare rigorosamente l'accesso, ad esempio chiavi API, password o certificati. Questa soluzione fornisce archiviazione sicura, scalabile e a disponibilità elevata in moduli di protezione hardware conformi a FIPS (Federal Information Processing Standards) 140-2, livello 2. Tramite Key Vault, è possibile avere il controllo completo delle chiavi usate per crittografare i dati ed è possibile gestire e controllare l'utilizzo delle chiavi. È possibile configurare e gestire un insieme di credenziali delle chiavi usando il portale di Azure, Azure PowerShell e l'interfaccia della riga di comando di Azure.

>[!NOTE]
> Crittografia dischi di Azure richiede che l'istanza di Key Vault e le macchine virtuali si trovino nella stessa area di Azure, in modo che i segreti di crittografia non debbano attraversare limiti a livello di area.

### <a name="azure-ad-application-and-service-principal"></a>Applicazione Azure AD ed entità servizio

Per accedere alle risorse o modificarle, ad esempio per la configurazione della crittografia per una macchina virtuale con script o codice, è prima necessario configurare un'**applicazione Azure Active Directory (AD)**. Azure Active Directory (Azure AD) è un servizio di gestione delle identità e directory basato sul cloud multi-tenant, che combina i principali servizi directory, la gestione dell'accesso alle applicazioni e la protezione delle identità in un'unica soluzione.

È necessaria anche un'**entità servizio** di Azure. Le entità servizio sono gli account del servizio usati per eseguire lo script o il codice e consentono di assegnare le autorizzazioni specifiche e l'ambito necessari per eseguire l'attività su una risorsa di Azure specifica.

Ci sono due elementi in Azure AD: l'oggetto applicazione è la **_definizione_** dell'applicazione (la sua funzione) e l'entità servizio è l'**_istanza specifica_** dell'applicazione.

Questo approccio è in linea con il principio dei **privilegi minimi**, in base a cui le autorizzazioni assegnate all'app sono limitate al minimo richiesto per consentire all'app di svolgere le attività previste.

È possibile configurare e gestire entità servizio e applicazioni Azure AD usando il portale di Azure, Azure PowerShell e l'interfaccia della riga di comando di Azure.

### <a name="key-vault-access-policies"></a>Criteri di accesso per gli insiemi di credenziali delle chiavi

Per poter archiviare le chiavi di crittografia in un'istanza di Key Vault, Crittografia dischi di Azure richiede le informazioni relative a **ID client** e **Segreto client** dell'applicazione Azure Active Directory che ha le autorizzazioni per scrivere in Key Vault.

È anche necessario fornire ad Azure l'accesso alle chiavi di crittografia nell'insieme di credenziali delle chiavi, in modo che vengano rese disponibili alla macchina virtuale per l'avvio e la decrittografia dei volumi.

## <a name="set-key-vault-advanced-access-policies"></a>Impostare i criteri di accesso avanzati per l'insieme di credenziali delle chiavi

I **criteri di accesso avanzati** consentono la crittografia dei dischi nell'insieme di credenziali delle chiavi e in loro assenza le distribuzioni della crittografia hanno esito negativo. 

Ci sono tre criteri che devono essere abilitati:

- **Key Vault per la crittografia dei dischi** Obbligatorio per Crittografia dischi di Azure.
- **Key Vault per la distribuzione** Per consentire al provider di risorse Microsoft.Compute di recuperare i segreti dall'insieme di credenziali delle chiavi. Questo criterio è necessario quando si crea una macchina virtuale.
- **Key Vault per la distribuzione di modelli, se necessario** Per consentire ad Azure Resource Manager di recuperare i segreti dall'insieme di credenziali delle chiavi. Questo criterio è necessario quando si usano modelli ARM per la distribuzione delle macchine virtuali.

I criteri di accesso dell'insieme di credenziali delle chiavi possono essere configurati e gestiti usando il portale di Azure, Azure PowerShell o l'interfaccia della riga di comando di Azure.

### <a name="what-is-the-azure-disk-encryption-prerequisites-configuration-script"></a>Informazioni sullo script di configurazione dei prerequisiti di Crittografia dischi di Azure

Lo **script di configurazione dei prerequisiti di Crittografia dischi di Azure** consente di configurare tutti i prerequisiti di crittografia (o quelli desiderati). Lo script assicura anche che Key Vault si trovi nella stessa area della macchina virtuale da crittografare. Lo script crea un gruppo di risorse e un insieme di credenziali delle chiavi e imposta i criteri di accesso dell'insieme di credenziali delle chiavi. Lo script crea anche un blocco delle risorse nell'insieme di credenziali delle chiavi per la protezione da eliminazioni accidentali.

## <a name="encrypting-an-existing-vm-disk"></a>Crittografia di un disco di una macchina virtuale esistente

Per la crittografia di un disco di una macchina virtuale esistente usando lo **script di configurazione dei prerequisiti di Crittografia dischi di Azure** sono necessari due passaggi:

1. Eseguire lo script di configurazione dei prerequisiti di Crittografia dischi di Azure.
1. Crittografare la macchina virtuale di Azure in PowerShell
