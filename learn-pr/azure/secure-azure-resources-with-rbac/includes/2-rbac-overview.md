Supponiamo di dover gestire l'accesso alle risorse di Azure per i team di sviluppo, progettazione e marketing. Si sono già ricevute le prime richieste di accesso, quindi è importante imparare velocemente come funziona la gestione dell'accesso per le risorse in Azure.

## <a name="what-is-rbac"></a>Che cos'è il controllo degli accessi in base al ruolo?

Il controllo degli accessi in base al ruolo è un sistema di autorizzazione basato su Azure Resource Manager che garantisce una gestione degli accessi con granularità fine delle risorse in Azure. Azure dispone di numerose risorse, di cui le macchine virtuali, i siti Web, le reti e le risorse di archiviazione sono solo alcuni esempi.

#### <a name="what-is-role-based-access-control"></a>Che cos'è il controllo degli accessi in base al ruolo?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvk]

## <a name="what-can-i-do-with-rbac"></a>Quali operazioni si possono eseguire con il controllo degli accessi in base al ruolo?

Con il controllo degli accessi in base al ruolo si può concedere l'accesso alle risorse di Azure di cui si ha il controllo.

Di seguito sono riportati alcuni esempi:

- Consentire a un utente di gestire le macchine virtuali in una sottoscrizione e a un altro utente di gestire le reti virtuali
- Consentire a un gruppo di amministratori di database di gestire database SQL all'interno di una sottoscrizione
- Consentire a un utente di gestire tutte le risorse in un gruppo di risorse, ad esempio le macchine virtuali, i siti Web e le subnet
- Consentire a un'applicazione di accedere a tutte le risorse in un gruppo di risorse

## <a name="rbac-in-the-azure-portal"></a>Controllo degli accessi in base al ruolo nel portale di Azure

In diverse aree del portale di Azure è presente un pannello denominato **Controllo di accesso (IAM)**, noto anche come gestione delle identità e degli accessi. Questo pannello mostra chi ha accesso alle varie aree e con quale ruolo. Può essere usato anche per concedere o rimuovere l'accesso.

Di seguito è illustrato un esempio del pannello Controllo di accesso (IAM) per un gruppo di risorse. In questo esempio ad Alain Charon è stato assegnato il ruolo Operatore di backup per questo gruppo di risorse.

![Controllo di accesso (IAM) nel portale di Azure](../media/2-resource-group-access-control.png)

## <a name="how-does-rbac-work"></a>Come funziona il controllo degli accessi in base al ruolo?

Per controllare l'accesso alle risorse mediante il controllo degli accessi in base al ruolo, occorre creare assegnazioni di ruolo, che definiscono la modalità di applicazione delle autorizzazioni. Per creare un'assegnazione di ruolo sono necessari tre elementi: un'entità di sicurezza, una definizione del ruolo e un ambito. Possiamo riferirci a questi elementi come "chi", "cosa" e "dove".

### <a name="1-security-principal-who"></a>1. Entità di sicurezza (chi)

Per *entità di sicurezza* si intende un utente, un gruppo o un'applicazione a cui si vuole concedere l'accesso.

![Illustrazione che mostra un'entità di sicurezza che include utente, gruppo ed entità di sicurezza.](../media/2-rbac-security-principal.png)

### <a name="2-role-definition-what-you-can-do"></a>2. Definizione del ruolo (cosa si può fare)

Una *definizione del ruolo* è una raccolta di autorizzazioni, talvolta semplicemente chiamata ruolo. Una definizione del ruolo elenca le autorizzazioni che definiscono le operazioni che è possibile eseguire, ad esempio lettura, scrittura ed eliminazione. I ruoli possono essere di livello superiore, come Proprietario, o specifici, come Collaboratore Macchina virtuale.

![Illustrazione dell'elenco dei diversi ruoli predefiniti e personalizzati con zoom sulla definizione del ruolo Collaboratore.](../media/2-rbac-role-definition.png)

Azure include diversi ruoli predefiniti che è possibile usare. Di seguito sono elencati quattro fondamentali ruoli predefiniti.

- Proprietario: ha accesso completo a tutte le risorse, oltre al diritto di delegare l'accesso ad altri utenti.
- Collaboratore: può creare e gestire tutti i tipi di risorse di Azure, ma non può concedere l'accesso ad altri.
- Lettore: può visualizzare le risorse di Azure esistenti.
- Amministratore Accesso utenti: consente di gestire l'accesso degli utenti alle risorse di Azure.

Se i ruoli predefiniti non soddisfano le esigenze specifiche dell'organizzazione, è possibile creare ruoli personalizzati.

### <a name="3-scope-where"></a>3. Ambito (dove)

L'*ambito* definisce dove viene applicato l'accesso. È utile se si vuole assegnare a qualcuno il ruolo Collaboratore Sito Web, ma solo per un gruppo di risorse.

In Azure è possibile specificare un ambito su più livelli: gruppo di gestione, sottoscrizione, gruppo di risorse o risorsa. Gli ambiti sono strutturati in una relazione padre-figlio. Quando si concede l'accesso in un ambito padre, le stesse autorizzazioni vengono ereditate dagli ambiti figlio. Ad esempio, se si assegna il ruolo Collaboratore a un gruppo nell'ambito di una sottoscrizione, tale ruolo viene ereditato da tutti i gruppi di risorse e le risorse della sottoscrizione.

![Illustrazione che mostra una rappresentazione gerarchica dei diversi livelli di Azure a cui applicare l'ambito. La gerarchia, partendo dal livello più elevato, è in questo ordine: gruppo di gestione, sottoscrizione, gruppo di risorse e risorsa.](../media/2-rbac-scope.png)

### <a name="role-assignment"></a>Assegnazione del ruolo

Una volta determinato il chi, il cosa e il dove, è possibile unire questi elementi per concedere l'accesso. Un'*assegnazione di ruolo* è il processo di associazione di un ruolo a un'entità di sicurezza in un particolare ambito ai fini della concessione dell'accesso. Per concedere l'accesso, si crea un'assegnazione di ruolo. Per revocare l'accesso, si rimuove l'assegnazione di ruolo.

Nell'esempio seguente, al gruppo Marketing è stato assegnato il ruolo Collaboratore nell'ambito del gruppo di risorse Sales.

![Illustrazione che mostra un processo di assegnazione del ruolo di esempio per il gruppo Marketing, che è una combinazione di entità di sicurezza, definizione del ruolo e ambito. Il gruppo Marketing rientra nell'entità di sicurezza Group e dispone del ruolo di Collaboratore per l'ambito del gruppo Resource.](../media/2-rbac-overview.png)

## <a name="rbac-is-an-allow-model"></a>Il controllo degli accessi in base al ruolo è un modello di autorizzazione

Il controllo degli accessi in base al ruolo è un modello di autorizzazione. Questo significa che quando a un utente viene assegnato un ruolo, il controllo degli accessi in base al ruolo lo autorizza ad eseguire determinate azioni, come la lettura, la scrittura o l'eliminazione. Pertanto, se un'assegnazione di ruolo concede all'utente le autorizzazioni di lettura su un gruppo di risorse e un'altra assegnazione di ruolo concede le autorizzazioni di scrittura sullo stesso gruppo di risorse, l'utente avrà le autorizzazioni di scrittura su tale gruppo.

Le autorizzazioni del controllo degli accessi in base al ruolo sono di tipo `NotActions`. `NotActions` non è una regola di negazione. È semplicemente un modo comodo per creare un set di autorizzazioni consentite quando è necessario escludere autorizzazioni specifiche.

In questa unità si sono apprese le nozioni di base sul funzionamento del controllo degli accessi in base al ruolo. A questo punto è possibile iniziare a usare concretamente questa funzionalità. Il modo più semplice per iniziare è tramite il portale di Azure. Il resto di questo modulo contiene esercizi pratici correlati al controllo degli accessi in base al ruolo.