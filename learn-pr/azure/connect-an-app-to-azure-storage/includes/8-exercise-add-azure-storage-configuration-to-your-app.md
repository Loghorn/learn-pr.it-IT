::: zone pivot="csharp" 

In questo esercizio si aggiunge all'applicazione .NET core il supporto per recuperare una stringa di connessione da un file di configurazione. Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione in un file JSON.

## <a name="create-a-json-configuration-file"></a>Creare un file di configurazione JSON

1. Passare con il comando `cd` alla directory PhotoSharingApp, se ci si trova in un'altra directory.

1. Usare lo strumento `touch` nella riga di comando per creare un file denominato **appsettings.json**.

    ```bash
    touch appsettings.json
    ```

1. Aprire il progetto in un editor. Se si lavora in locale, è possibile usare l'editor preferito. È consigliabile usare **Visual Studio Code**, un IDE multipiattaforma estendibile. Se si lavora in Cloud Shell, è consigliabile usare l'editor di Cloud Shell. Il comando seguente funziona in entrambi gli editor.

    ```bash
    code .
    ```

1. Selezionare il file **appsettings.json** nell'editor e aggiungere il testo seguente. Salvare il file. Nell'angolo superiore destro dell'editor di Cloud Shell è disponibile un menu con le operazioni più comuni eseguite sui file.

    ```json
    {
      "StorageAccountConnectionString": "<value>"
    }
    ```

1. È ora necessario ottenere la stringa di connessione dell'account di archiviazione e inserirla nella configurazione per l'app. In Cloud Shell eseguire il comando seguente.

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. Copiare la stringa di connessione restituita dal comando e sostituire `"<value>"` nel file **appsettings.json** con questa stringa di connessione. Salvare il file.

1. Aprire quindi il file di progetto (**PhotoSharingApp.csproj**) nell'editor.

1. Aggiungere il blocco di configurazione seguente per includere il nuovo file nel progetto e copiarlo nella cartella di output. In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Salvare il file. Assicurarsi di eseguire questa operazione. In caso contrario le modifiche andranno perse quando si aggiunge il pacchetto più avanti.

## <a name="add-support-to-read-a-json-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione JSON

Un'applicazione .NET Core richiede pacchetti NuGet aggiuntivi per leggere un file di configurazione JSON.

1. Nella sezione della finestra relativa al prompt dei comandi aggiungere un riferimento al pacchetto NuGet **Microsoft.Extensions.Configuration.Json**.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere codice per leggere il file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.

1. Selezionare **Program.cs** nell'editor.

1. Nella parte superiore del file è presente la riga **using System;**. Al di sotto di questa riga aggiungere le righe di codice seguenti:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Sostituire il contenuto del metodo **Main** con il codice seguente. Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

Il file **Program.cs** avrà ora un aspetto simile al seguente:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
        }
    }
}
```

::: zone-end

::: zone pivot="javascript"

Si procederà ad aggiungere all'applicazione Node.js il supporto per recuperare una stringa di connessione da un file di configurazione. Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione dal file JavaScript.

## <a name="create-a-env-configuration-file"></a>Creare un file di configurazione con estensione env

1. Assicurarsi di essere nella directory di lavoro corretta per il progetto.

1. Usare lo strumento `touch` nella riga di comando per creare un file con estensione **env**.

    ```bash
    touch .env
    ```

1. Aprire il progetto nell'editor di Cloud Shell.

    ```bash
    code .
    ```

1. Selezionare il file con estensione **env** nell'editor e aggiungere il testo seguente. Può essere necessario fare clic sul pulsante Aggiorna nel codice per visualizzare i nuovi file. Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu con le operazioni più comuni eseguite sui file.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > Il valore **AZURE_STORAGE_CONNECTION_STRING** è una variabile di ambiente hardcoded usata dalle API di archiviazione per cercare le chiavi di accesso. Se si preferisce, è possibile usare un nome personalizzato, ma è necessario specificare il nome quando si crea l'oggetto `BlobService` nell'app Node.js.

1. È ora necessario ottenere la stringa di connessione dell'account di archiviazione e inserirla nella configurazione per l'app. In Cloud Shell eseguire il comando seguente.

    ```azurecli
    az storage account show-connection-string \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name <name> \
        --query connectionString
    ```

1. Copiare la stringa di connessione restituita dal comando, senza le virgolette, e sostituire `<value>` nel file con estensione **env** con questa stringa di connessione.

1. Salvare il file.

## <a name="add-support-to-read-an-environment-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione dell'ambiente

Per includere nelle app Node.js il supporto per la lettura del file con estensione **env**, è possibile aggiungere il pacchetto **dotenv**.

1. Nella sezione relativa al prompt dei comandi della finestra aggiungere una dipendenza al pacchetto *dotenv** tramite `npm`.

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere codice per la lettura del file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione.

1. Selezionare *index.js** nell'editor.

1. Nella parte superiore del file è presente la riga **#!/usr/bin/env node**. Al di sotto di questa riga aggiungere un'istruzione `require` per caricare il pacchetto **dotenv**. In questo modo si rendono le variabili di ambiente definite nel file **.env** disponibili per il programma.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

Ora che tutto è pronto, si può iniziare ad aggiungere codice per usare l'account di archiviazione.
