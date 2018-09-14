Dopo aver creato e configurato l'hub eventi, è necessario configurare le applicazioni per inviare e ricevere i flussi di dati di eventi.

Ad esempio, una soluzione di elaborazione dei pagamenti userà un'applicazione mittente di qualche tipo per raccogliere i dati della carta di credito del cliente e un'applicazione ricevitore per verificare la validità della carta di credito.

Anche se la configurazione di un'applicazione Java presenta differenze rispetto a un'applicazione .NET, esistono alcuni principi generali per consentire alle applicazioni di connettersi a un hub eventi e inviare o ricevere correttamente i messaggi. Pertanto, anche se il processo di modifica dei file di testo di configurazione Java è diverso dalla preparazione di un'applicazione .NET con Visual Studio, i principi sono gli stessi.

## <a name="what-are-the-minimum-event-hub-application-requirements"></a>Quali sono i requisiti minimi di un'applicazione per Hub eventi?

Per configurare un'applicazione per l'invio di messaggi a un hub eventi, è necessario fornire le informazioni seguenti, in modo da consentire all'applicazione di creare le credenziali di connessione:

- Nome dello spazio dei nomi di Hub eventi
- Nome dell'hub eventi
- Nome dei criteri di accesso condiviso
- Chiave di accesso condiviso primaria

Per configurare un'applicazione per la ricezione di messaggi da un hub eventi, fornire le informazioni seguenti, in modo da consentire all'applicazione di creare le credenziali di connessione:

- Nome dello spazio dei nomi di Hub eventi
- Nome dell'hub eventi
- Nome dei criteri di accesso condiviso
- Chiave di accesso condiviso primaria
- Nome dell'account di archiviazione
- Stringa di connessione dell'account di archiviazione
- Nome del contenitore dell'account di archiviazione

Se si dispone di un'applicazione ricevitore che archivia i messaggi in Archiviazione BLOB di Azure, è anche necessario configurare prima un account di archiviazione.

## <a name="the-azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a>Comandi dell'interfaccia della riga di comando di Azure per la creazione di un account di archiviazione standard per utilizzo generico

1. Creare un account di archiviazione V2 generico nel gruppo di risorse e la stessa posizione Data Center di Azure che è stato utilizzato durante la creazione del gruppo di risorse.

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name> --location <location> --sku Standard_RAGRS --encryption blob
    ```

1. Per accedere a questa risorsa di archiviazione, è necessaria una chiave di accesso dell'account di archiviazione. Visualizzare le chiavi di accesso associate all'account di archiviazione.

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```

1. Salvare il valore associato a **key1**.

1. Saranno anche necessari i dettagli della connessione per l'account di archiviazione. Visualizzare la stringa di connessione per l'account di archiviazione.

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```

1. Salvare il valore associato a **connectionString**.

1. I messaggi verranno archiviati in un contenitore nell'account di archiviazione. Creare un contenitore nell'account di archiviazione usando `<connection string>` con la stringa di connessione ottenuta nel passaggio precedente.

    ```azurecli
    az storage container create -n <container> --connection-string "<connection string>"
    ```

## <a name="shell-command-for-cloning-an-application-github-repository"></a>Comando della shell per clonare un repository GitHub dell'applicazione

Git è uno strumento di collaborazione che usa un modello di controllo della versione distribuito ed è progettato per la collaborazione su progetti software e di documentazione. I client Git sono disponibili per più piattaforme, tra cui Windows, e la riga di comando di Git è inclusa nella cloud shell Bash di Azure. GitHub è un servizio di hosting basato sul Web per i repository Git. 

Se si dispone di un'applicazione ospitata come progetto in GitHub, è possibile creare una copia locale del progetto clonandone il repository tramite il comando **git clone**.

1. Clonare un repository nella home directory.

    ```azurecli
      git clone https://github.com/<project repository>
    ```

## <a name="shell-commands-for-preparing-an-application"></a>Comandi della shell per la preparazione di un'applicazione

1. A questo punto, è possibile usare un editor come **nano** per modificare l'applicazione e aggiungere lo spazio dei nomi di Hub eventi, il nome dell'hub eventi, il nome dei criteri di accesso condiviso e la chiave primaria. Nano è un semplice editor di testo disponibile per Linux e altri sistemi operativi di tipo Unix. È anche disponibile nella cloud shell Bash di Azure. Ad esempio, usare questo comando per aprire un file di applicazione Java nell'editor **nano**.

    ```azurecli
    nano <application>.java
    ```

1. A seconda del modo in cui sono sviluppate le applicazioni, potrebbe essere necessario compilare l'applicazione dopo aver aggiunto le informazioni di configurazione di Hub eventi. Ad esempio, le applicazioni Java usate nell'unità successiva devono essere compilate tramite uno strumento come Apache **Maven**. Maven compila i file Java in file con estensione class o li comprime in file con estensione jar. Maven è disponibile nella cloud shell Bash. Per compilare l'applicazione Java, usare i comandi **mvn**. Usare questo comando mvn per compilare un'applicazione Java.

    ```azurecli
    mvn clean package -DskipTests
    ```

## <a name="summary"></a>Riepilogo

Le applicazioni mittente e ricevitore devono essere configurate con informazioni specifiche sull'ambiente di Hub eventi. Creare un account di archiviazione se l'applicazione ricevitore archivia i messaggi in Archiviazione BLOB. Se l'applicazione è ospitata in GitHub, è necessario clonarla nella directory locale. Per aggiungere lo spazio dei nomi all'applicazione, è possibile usare un editor di testo come **nano**.