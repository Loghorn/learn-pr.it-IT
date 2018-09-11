Supponiamo di dover gestire l'accesso alle risorse di Azure per i team di sviluppo, progettazione e marketing. Si sono già ricevute le prime richieste di accesso, quindi è importante apprendere velocemente il funzionamento della gestione dell'accesso per le risorse in Azure.

## <a name="what-is-rbac"></a>Che cos'è il controllo degli accessi in base al ruolo?

Il controllo degli accessi in base al ruolo è un sistema di autorizzazione basato su [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) che garantisce una gestione degli accessi con granularità fine delle risorse in Azure. Azure dispone di numerose risorse, di cui le macchine virtuali, i siti Web, le reti e le risorse di archiviazione sono solo alcuni esempi.

## <a name="what-can-i-do-with-rbac"></a>Quali operazioni si possono eseguire con il controllo degli accessi in base al ruolo?

Con il controllo degli accessi in base al ruolo si può concedere a singoli utenti o a gruppi di utenti l'accesso alle risorse di Azure di cui si ha il controllo.

Ecco alcuni esempi:
- Consentire a un utente di gestire le macchine virtuali in una sottoscrizione e a un altro utente di gestire le reti virtuali
- Consentire a un gruppo di amministratori di database di gestire database SQL all'interno di una sottoscrizione
- Consentire a un utente di gestire tutte le risorse in un gruppo di risorse, ad esempio le macchine virtuali, i siti Web e le subnet
- Consentire a un'applicazione di accedere a tutte le risorse in un gruppo di risorse

## <a name="rbac-in-the-azure-portal"></a>Controllo degli accessi in base al ruolo nel portale di Azure

In diverse aree del portale di Azure è presente un pannello denominato **Controllo di accesso (IAM)**, noto anche come gestione delle identità e degli accessi. Questo pannello mostra chi ha accesso alle varie aree e con quale ruolo. Può essere usato anche per concedere o rimuovere l'accesso.

Di seguito è illustrato un esempio del pannello Controllo di accesso (IAM) per un gruppo di risorse. In questo esempio ad Alain Charon è stato assegnato il ruolo Operatore di backup su questo gruppo di risorse.

![Controllo di accesso (IAM) nel portale di Azure](../media-draft/2-resource-group-access-control.png)

## <a name="how-does-rbac-work"></a>Come funziona il controllo degli accessi in base al ruolo?

Per controllare l'accesso alle risorse mediante il controllo degli accessi in base al ruolo, occorre creare assegnazioni di ruolo, che definiscono la modalità di applicazione delle autorizzazioni. Per creare un'assegnazione di ruolo sono necessari tre elementi: un'entità di sicurezza, una definizione del ruolo e un ambito. Possiamo riferirci a questi elementi come "chi", "cosa" e "dove".

### <a name="1-security-principal-who"></a>1. Entità di sicurezza (chi)

Per *entità di sicurezza* si intende un utente, un gruppo o un'applicazione a cui si vuole concedere l'accesso.

![Entità di sicurezza](../media-draft/2-rbac-security-principal.png)

### <a name="2-role-definition-what-you-can-do"></a>2. Definizione del ruolo (cosa si può fare)

Una *definizione del ruolo* è una raccolta di autorizzazioni, talvolta semplicemente chiamata ruolo. Una definizione del ruolo elenca le autorizzazioni che definiscono le operazioni che è possibile eseguire, ad esempio lettura, scrittura ed eliminazione. I ruoli possono essere di livello superiore, come Proprietario, o specifici, come Collaboratore Macchina virtuale.

![Definizione del ruolo](../media-draft/2-rbac-role-definition.png)

Azure include diversi [ruoli predefiniti](/azure/role-based-access-control/built-in-roles) che è possibile usare. I quattro ruoli predefiniti fondamentali sono i seguenti:

- Proprietario: ha accesso completo a tutte le risorse, oltre al diritto di delegare l'accesso ad altri utenti.
- Collaboratore: può creare e gestire tutti i tipi di risorse di Azure, ma non può concedere l'accesso ad altri.
- Lettore: può visualizzare le risorse di Azure esistenti.
- Amministratore Accesso utenti: consente di gestire l'accesso degli utenti alle risorse di Azure.

Se i ruoli predefiniti non soddisfano le esigenze specifiche dell'organizzazione, è possibile creare [ruoli personalizzati](/azure/role-based-access-control/custom-roles).

### <a name="3-scope-where"></a>3. Ambito (dove)

L'*ambito* definisce dove viene applicato l'accesso. È utile se si vuole assegnare a qualcuno il ruolo Collaboratore Sito Web, ma solo per un gruppo di risorse.

In Azure è possibile specificare un ambito su più livelli: gruppo di gestione, sottoscrizione, gruppo di risorse o risorsa. Gli ambiti sono strutturati in una relazione padre-figlio. Quando si concede l'accesso in un ambito padre, le stesse autorizzazioni vengono ereditate dagli ambiti figlio. Ad esempio, se si assegna il ruolo Collaboratore a un gruppo nell'ambito di una sottoscrizione, tale ruolo viene ereditato da tutti i gruppi di risorse e le risorse della sottoscrizione.

![Ambito](../media-draft/2-rbac-scope.png)

### <a name="role-assignment"></a>Assegnazione di ruolo

Una volta determinato il chi, il cosa e il dove, è possibile unire questi elementi per concedere l'accesso. Un'*assegnazione di ruolo* è il processo di associazione di un ruolo a un'entità di sicurezza in un particolare ambito ai fini della concessione dell'accesso. Per concedere l'accesso, si crea un'assegnazione di ruolo. Per revocare l'accesso, si rimuove l'assegnazione di ruolo.

Nell'esempio seguente, al gruppo Marketing è stato assegnato il ruolo Collaboratore nell'ambito del gruppo di risorse Sales.

![Assegnazione di ruolo](../media-draft/2-rbac-overview.png)

## <a name="rbac-is-allow-only-with-no-deny"></a>Applicazione del controllo degli accessi in base al ruolo

Attualmente il controllo degli accessi in base al ruolo può essere usato solo per concedere l'accesso e non per negarlo. Questo significa che quando a un utente viene assegnato un ruolo, il controllo degli accessi in base al ruolo gli consente di eseguire determinate azioni, come la lettura, la scrittura o l'eliminazione, ma non può negare esplicitamente l'accesso. Pertanto, se un'assegnazione di ruolo concede le autorizzazioni di lettura su un gruppo di risorse e un'altra assegnazione di ruolo concede le autorizzazioni di scrittura sullo stesso gruppo di risorse, si avranno le autorizzazioni di scrittura su tale gruppo.

Le autorizzazioni del controllo degli accessi in base al ruolo sono di tipo `NotActions`. `NotActions` non è una regola di negazione. È semplicemente un modo comodo per creare un set di autorizzazioni consentite quando è necessario escludere autorizzazioni specifiche.

## <a name="other-roles-in-azure"></a>Altri ruoli in Azure

In Azure sono disponibili anche altri ruoli, come Amministratore globale, Amministratore dell'account e molti altri. Molti di questi altri ruoli vengono usati per l'amministrazione di Azure Active Directory, ad esempio per la creazione di utenti, la reimpostazione di password e la gestione delle licenze utente e dei domini. Sono disponibili diversi articoli con informazioni dettagliate su questi ruoli, ma la cosa importante da ricordare è che i ruoli del controllo degli accessi in base al ruolo vengono usati per gestire l'accesso alle risorse di Azure.

## <a name="summary"></a>Riepilogo

In questa unità si sono apprese le nozioni di base sul funzionamento del controllo degli accessi in base al ruolo. A questo punto è possibile iniziare a usare concretamente questa funzionalità. Il modo più semplice per iniziare è tramite il portale di Azure. Il resto di questo modulo contiene esercizi pratici correlati al controllo degli accessi in base al ruolo.
