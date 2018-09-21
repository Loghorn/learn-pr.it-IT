Dopo aver creato e configurato l'hub eventi, è necessario configurare le applicazioni per inviare e ricevere i flussi dei dati di eventi.

Una soluzione di elaborazione dei pagamenti, ad esempio, userà un'applicazione mittente di un certo tipo per raccogliere i dati della carta di credito del cliente e un'applicazione ricevitore per verificare la validità della carta di credito.

Anche se la configurazione di un'applicazione Java presenta differenze rispetto a un'applicazione .NET, esistono alcuni principi generali per consentire alle applicazioni di connettersi a un hub eventi e inviare o ricevere correttamente i messaggi. Pertanto, anche se il processo di modifica dei file di testo di configurazione Java è diverso dalla preparazione di un'applicazione .NET con Visual Studio, i principi sono gli stessi.

## <a name="what-are-the-minimum-event-hub-application-requirements"></a>Quali sono i requisiti minimi di un'applicazione per un hub eventi?

Per configurare un'applicazione per l'invio di messaggi a un hub eventi, è necessario specificare le informazioni seguenti, in modo da consentire all'applicazione di creare le credenziali di connessione:

- Nome dello spazio dei nomi dell'hub eventi
- Nome dell'hub eventi
- Nome dei criteri di accesso condiviso
- Chiave di accesso condiviso primaria

Per configurare un'applicazione per la ricezione di messaggi da un hub eventi, specificare le informazioni seguenti, in modo da consentire all'applicazione di creare le credenziali di connessione:

- Nome dello spazio dei nomi dell'hub eventi
- Nome dell'hub eventi
- Nome dei criteri di accesso condiviso
- Chiave di accesso condiviso primaria
- Nome dell'account di archiviazione
- Stringa di connessione dell'account di archiviazione
- Nome del contenitore dell'account di archiviazione

Se si dispone di un'applicazione ricevitore che archivia i messaggi in Archiviazione BLOB di Azure, è anche necessario configurare prima un account di archiviazione.

## <a name="azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a>Comandi dell'interfaccia della riga di comando di Azure per la creazione di un account di archiviazione standard per uso generico

Nell'interfaccia della riga di comando di Azure è disponibile un set di comandi che consentono di creare e gestire un account di archiviazione. Tali comandi vengono usati nell'unità successiva, ma un riepilogo dei comandi di base viene indicato di seguito. 

> [!TIP]
> Sono disponibili numerosi moduli MS Learn che riguardano gli account di archiviazione, a partire dal modulo **Introduzione ad Archiviazione di Azure**.

| Comando | Descrizione |
|---------|-------------|
| `storage account create` | Crea un account di archiviazione per uso generico v2 |
| `storage account key list` | Recupera la chiave dell'account di archiviazione. |
| `storage account show-connection-string` | Recupera la stringa di connessione per un account di archiviazione di Azure. |
| `storage container create` | Crea un nuovo contenitore in un account di archiviazione. |

## <a name="shell-command-for-cloning-an-application-github-repository"></a>Comando della shell per clonare un repository GitHub dell'applicazione

Git è uno strumento di collaborazione che usa un modello di controllo della versione distribuito ed è progettato per la collaborazione su progetti software e di documentazione. I client Git sono disponibili per più piattaforme, tra cui Windows, e la riga di comando di Git è inclusa nella cloud shell Bash di Azure. GitHub è un servizio di hosting basato sul Web per i repository Git. 

Se si dispone di un'applicazione ospitata come progetto in GitHub, è possibile creare una copia locale del progetto clonandone il repository tramite il comando **git clone**. Questa operazione viene eseguita nell'unità successiva.

## <a name="editing-files-in-the-cloud-shell"></a>Modifica di file in Cloud SHell

È possibile usare uno degli editor predefiniti in Cloud Shell per modificare tutti i file che costituiscono l'applicazione e aggiungere lo spazio dei nomi e il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria. 

Cloud Shell supporta **nano**, **vim** ed **emacs** nonché un editor analogo a Visual Studio Code denominato **code**. È sufficiente digitare il nome dell'editor desiderato per avviarlo nell'ambiente. Nell'unità successiva si userà l'editor **code**.

## <a name="summary"></a>Riepilogo

Le applicazioni mittente e ricevitore devono essere configurate con informazioni specifiche sull'ambiente dell'hub eventi. Creare un account di archiviazione se l'applicazione ricevitore archivia i messaggi in Archiviazione BLOB. Se l'applicazione è ospitata in GitHub, è necessario clonarla nella directory locale. Per aggiungere lo spazio dei nomi all'applicazione, è possibile usare un editor di testo come **nano**.