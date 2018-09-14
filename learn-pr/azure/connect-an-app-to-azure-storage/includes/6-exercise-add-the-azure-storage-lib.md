::: pivot zona = "csharp" è possibile integrare la libreria client di archiviazione di Azure nell'applicazione console .NET Core.

La libreria client di archiviazione di Azure per .NET viene distribuita con NuGet. È consigliabile aggiungere il **windowsazure. Storage** pacchetto per le applicazioni .NET o .NET Core.

## <a name="add-the-azure-storage-nuget-package"></a>Aggiungere il pacchetto NuGet per Archiviazione di Azure

1. In Cloud Shell, `cd` nella directory PhotoSharingApp se non si è già presente.

1. Aggiungere il **windowsazure. Storage** pacchetto all'applicazione.

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. Ciò deve comportare alcune attività della console mentre la libreria client e tutte le dipendenze necessarie vengono scaricate. Al termine, proseguire e compilare ed eseguire di nuovo l'applicazione per assicurarsi che tutto sia pronto per l'uso.

    ```bash
    dotnet run
    ```

1. Come in precedenza, deve restituire come output "Hello World!".

::: zone-end

::: zone pivot="javascript"

È possibile integrare il **Microsoft Azure Storage Client Library per Node. js e JavaScript** nell'applicazione.

La libreria client per Node. js è disponibile tramite il nodo Package manager (NPM). Si dovranno aggiungere il **azure-storage** del pacchetto per il **Packages** file.

> [!NOTE]
> Il **Microsoft Azure Storage Client Library per Node. js e JavaScript** è destinato a server applicazioni. Se si sta eseguendo JavaScript sul lato client, è consigliabile usare la **Azure Storage Client Library per JavaScript**, che offre le stesse funzionalità ma è stato sviluppato specificamente per l'esecuzione in un browser.

## <a name="add-the-azure-storage-package"></a>Aggiungere il pacchetto di archiviazione di Azure

1. In Cloud Shell, `cd` nella directory PhotoSharingApp se non si è già presente.

1. Aggiungere il **azure-storage** pacchetto all'applicazione. Assicurarsi di specificare il `--save` opzione dove viene conservato per **Packages**.

    ```bash
    npm install azure-storage --save
    ```

1. Ciò deve comportare alcune attività della console mentre la libreria client e tutte le dipendenze necessarie vengono scaricate. Al termine, proseguire e compilare ed eseguire di nuovo l'applicazione per assicurarsi che tutto sia pronto per l'uso.

    ```bash
    node index.js
    ```

1. Come in precedenza, deve restituire come output "Hello, World!"

::: zone-end

Ora che abbiamo le librerie necessarie posto, esaminiamo le comuni attività che verranno eseguite nel codice per usare archiviazione di Azure.
