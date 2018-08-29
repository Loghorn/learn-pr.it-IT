## <a name="motivation"></a>Motivazione
L'interfaccia della riga di comando di Azure consente di scrivere i comandi ed eseguirli immediatamente. Ricordare che l'obiettivo complessivo dell'esempio di sviluppo software è distribuire nuove build di un'app Web per i test. Il primo passaggio consiste nel creare un gruppo di risorse. Tenere presente che l'obiettivo è creare queste risorse tramite un'installazione locale dell'interfaccia della riga di comando di Azure. 

Questa unità mostra come usare l'interfaccia della riga di comando di Azure per accedere alla sottoscrizione di Azure e creare una nuova risorsa.

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a>Quali risorse di Azure possono essere gestite tramite l'interfaccia della riga di comando di Azure?
L'interfaccia della riga di comando di Azure consente di controllare quasi tutti gli aspetti di ogni risorsa di Azure. È possibile lavorare con i gruppi di risorse, l'archiviazione, le macchine virtuali, Azure Active Directory (Azure AD), i contenitori, Machine Learning e così via.

I comandi nell'interfaccia della riga di comando sono strutturati in gruppi e sottogruppi. Ogni gruppo rappresenta un servizio offerto da Azure e i sottogruppi dividono i comandi per tali servizi in raggruppamenti logici. Ad esempio, il gruppo **storage** include sottogruppi, tra i quali **account**, **blob**, **storage** e **queue**.

Quindi, come si possono trovare i comandi specifici necessari? Un modo consiste nell'usare **az find**. Ad esempio, se si vogliono trovare i comandi utili per gestire un **BLOB** di archiviazione, si potrebbe usare il comando find seguente:

```bash
az find -q blob
```

Se si conosce già il nome del comando desiderato, l'argomento **--help** per il comando può essere più utile. Si ottengono informazioni dettagliate sul comando e, per un gruppo di comandi, un elenco dei sottocomandi disponibili. Ritornando all'esempio di archiviazione, ecco come è possibile ottenere un elenco dei sottogruppi e dei comandi per la gestione dell'archiviazione BLOB:

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a>Come creare una risorsa di Azure
Quando si crea una nuova risorsa di Azure, in genere esistono tre passaggi: connettersi alla sottoscrizione di Azure, creare la risorsa e verificare che la creazione sia stata completata (vedere sotto).

![Procedura per creare una risorsa con l'interfaccia della riga di comando di Azure](../media-drafts/4-create-resources-overview.png)

Ogni passaggio corrisponde a un comando diverso dell'interfaccia della riga di comando di Azure.

### <a name="connect"></a>Connettere
Dato che si sta lavorando con un'installazione locale dell'interfaccia della riga di comando di Azure, è necessario eseguire l'autenticazione prima di poter eseguire i comandi di Azure, tramite il comando **login** dell'interfaccia della riga di comando di Azure. 

```bash
az login
```

L'interfaccia della riga di comando di Azure in genere avvierà il browser predefinito per aprire la pagina di accesso di Azure. Se ciò non accade, seguire le istruzioni della riga di comando e immettere un codice di autorizzazione all'indirizzo [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

Quando l'accesso riesce, si verrà connessi alla sottoscrizione di Azure. 

### <a name="create"></a>Create
È spesso necessario creare un nuovo gruppo di risorse prima di creare un nuovo servizio di Azure, quindi i gruppi di risorse verranno usati come esempio per mostrare come creare le risorse di Azure dall'interfaccia della riga di comando.

Il comando **group create** dell'interfaccia della riga di comando di Azure crea un gruppo di risorse. È necessario specificare un nome e una posizione. Il nome deve essere univoco all'interno della sottoscrizione. La posizione determina dove vengono archiviati i metadati per il gruppo di risorse. Per specificare la posizione si usano stringhe come "West US", "North Europe" o "West India". In alternativa, è possibile usare gli equivalenti composti da una parola singola, ad esempio westus, northeurope o westindia. La sintassi di base è:

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a>Verificare
Per molte risorse di Azure, l'interfaccia della riga di comando di Azure include un sottocomando **list** per visualizzare i dettagli della risorsa. Ad esempio, il comando **group list** dell'interfaccia della riga di comando di Azure elenca i gruppi di risorse di Azure. Questo comando è utile in questo contesto per verificare se è stata completata la creazione del gruppo di risorse:

```bash
az group list
```

Per ottenere una visualizzazione più concisa, è possibile formattare l'output come una tabella semplice:

```bash
az group list --output table
```
