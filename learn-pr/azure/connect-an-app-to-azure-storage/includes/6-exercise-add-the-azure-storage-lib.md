::: zone pivot="csharp"
In questa unità si integrerà la libreria client di Archiviazione di Azure nell'applicazione console .NET Core.

La libreria client di Archiviazione di Azure per .NET viene distribuita con NuGet. Occorrerà aggiungere il pacchetto **WindowsAzure.Storage** alle applicazioni .NET o .NET Core.

## <a name="add-the-azure-storage-nuget-package"></a>Aggiungere il pacchetto NuGet per Archiviazione di Azure

1. Nel terminale passare con il comando `cd` alla directory PhotoSharingApp, se ci si trova in un'altra directory.

1. Aggiungere il pacchetto **WindowsAzure.Storage** all'applicazione.

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. Questo dovrebbe tradursi in alcune attività della console durante il download della libreria client e di tutte le dipendenze necessarie. Al termine, compilare ed eseguire di nuovo l'applicazione per assicurarsi che sia tutto pronto.

    ```bash
    dotnet run
    ```

1. Come prima, dovrebbe restituire l'output "Hello World!".

::: zone-end

::: zone pivot="javascript"

Ora si integrerà la **libreria client di Archiviazione di Microsoft Azure per Node.js e JavaScript** nell'applicazione.

La libreria client per Node.js è disponibile tramite Node Package manager (NPM). Occorrerà aggiungere il pacchetto **azure-storage** al file **packages.json**.

> [!NOTE]
> La **libreria client di Archiviazione di Microsoft Azure per Node.js e JavaScript** è destinata alle applicazioni server. Se si sta creando codice JavaScript lato client, è consigliabile usare la **libreria client di Archiviazione di Azure per JavaScript**, che offre le stesse funzionalità ma è stata sviluppata specificamente per l'esecuzione in un browser.

## <a name="add-the-azure-storage-package"></a>Aggiungere il pacchetto di Archiviazione di Azure

1. In Cloud Shell passare con il comando `cd` alla directory PhotoSharingApp, se ci si trova in un'altra directory.

1. Aggiungere il pacchetto **azure-storage** all'applicazione. Assicurarsi di specificare l'opzione `--save`, per salvarlo in modo permanente in **packages.json**.

    ```bash
    npm install azure-storage --save
    ```

1. Questo dovrebbe tradursi in alcune attività della console durante il download della libreria client e di tutte le dipendenze necessarie. Al termine, compilare ed eseguire di nuovo l'applicazione per assicurarsi che sia tutto pronto.

    ```bash
    node index.js
    ```

1. Come prima, dovrebbe restituire l'output "Hello World!"

::: zone-end

Ora che sono disponibili le librerie necessarie, è possibile esaminare le attività comuni da eseguire nel codice per usare Archiviazione di Azure.
