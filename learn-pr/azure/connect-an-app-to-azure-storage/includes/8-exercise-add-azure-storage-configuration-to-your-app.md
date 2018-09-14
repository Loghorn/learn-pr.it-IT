::: pivot zona = "csharp" è possibile aggiungere il supporto per l'applicazione .NET core per recuperare una stringa di connessione da un file di configurazione. Si inizierà aggiungendo il plumbing necessarie per gestire la configurazione in un file JSON.

## <a name="create-a-json-configuration-file"></a>Creare un file di configurazione JSON

1. Assicurarsi che si è nella directory di lavoro corretto per il progetto.

1. Usare la `touch` strumento della riga di comando per creare un file denominato **appSettings. JSON**.

    ```bash
    touch appsettings.json
    ```

1. Aprire il progetto con l'editor interattivo. Se si lavora in locale, usare l'editor preferito. È consigliabile **Visual Studio Code**, che è un IDE estendibile multipiattaforma. I comandi seguenti sono per l'editor di Cloud Shell, ma sono molto simili a Visual Studio Code.

    ```bash
    code .
    ```

1. Selezionare il **appSettings. JSON** file nell'editor e aggiungere il testo seguente. Salvare il file. Nell'editor online, è presente un menu in alto a destra con operazioni su file comuni.

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. Selezionare quindi il file di progetto (**PhotoSharingApp.csproj**) per aprirlo nell'editor.

1. Aggiungere il seguente blocco di configurazione per includere il nuovo file nel progetto e copiarlo nella cartella di output. In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.

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

1. Salvare il file. (Assicurarsi che si esegue questa operazione oppure si perderà la modifica quando si aggiunge il pacchetto seguente).

## <a name="add-support-to-read-a-json-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione JSON

Un'applicazione .NET Core richiede altri pacchetti NuGet per leggere un file di configurazione JSON.

1. Nella sezione della finestra del prompt dei comandi, aggiungere un riferimento per la **Microsoft.Extensions.Configuration.Json** pacchetto NuGet.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere il codice per leggere il file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.

1. Selezionare **Program.cs** nell'editor.

1. Nella parte superiore del file è presente la riga **using System;**. Al di sotto di questa riga aggiungere le righe di codice seguenti:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Sostituire il contenuto del **Main** metodo con il codice seguente. Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.

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

È possibile aggiungere il supporto per l'applicazione Node. js per recuperare una stringa di connessione da un file di configurazione. Si inizierà aggiungendo il plumbing necessarie per gestire la configurazione dal file JavaScript.

## <a name="create-a-env-configuration-file"></a>Creare un file di configurazione con estensione env

1. Assicurarsi che si è nella directory di lavoro corretto per il progetto.

1. Usare la `touch` strumento della riga di comando per creare un file denominato **env**.

    ```bash
    touch .env
    ```

1. Aprire il progetto con l'editor interattivo, se si lavora in locale, usare l'editor preferito, è consigliabile **Visual Studio Code** che è un IDE estendibile multipiattaforma. I comandi seguenti sono per l'editor di Cloud Shell, ma sono molto simili a Visual Studio Code.
    
    ```bash
    code .
    ```

1. Selezionare il **env** file nell'editor e aggiungere il testo seguente. Salvare il file, nell'editor online, è un menu nell'angolo superiore destro con operazioni su file comuni.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > Il **AZURE_STORAGE_CONNECTION_STRING** valore è una variabile di ambiente hard-coded utilizzata per le API di archiviazione per individuare le chiavi di accesso. È possibile usare il proprio nome se si preferisce, ma è necessario specificare il nome per il momento in cui si crea il `BlobService` oggetto.

1. Salvare il file.

## <a name="add-support-to-read-an-environment-configuration-file"></a>Aggiungere il supporto per leggere un file di configurazione Ambiente

App Node. js può includere il supporto per la lettura dal **env** file aggiungendo le **dotenv** pacchetto.

1. Nella sezione della finestra del prompt dei comandi, aggiungere una dipendenza per il *dotenv** creare un pacchetto usando `npm`.

    ```bash
    npm install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere il codice per leggere il file di configurazione

Ora che sono state aggiunte le librerie necessarie per abilitare la lettura della configurazione, è necessario abilitare tale funzionalità all'interno dell'applicazione.

1. Selezionare *index. js** nell'editor.

1. Nella parte superiore del file, un **&! usr/bin/env/nodo** riga è presente. Sotto tale riga, aggiungere un `require` istruzione per caricare il **dotenv** pacchetto. In questo modo le variabili di ambiente definite nel nostro **env** file disponibili per il programma.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

## <a name="add-the-connection-string-to-the-configuration-file"></a>Aggiungere la stringa di connessione nel file di configurazione

È ora necessario ottenere la stringa di connessione dell'account di archiviazione e aggiungerla nella configurazione per l'app.

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Passare all'account di archiviazione. È possibile usare la **tutte le risorse** sezione per trovare l'account di archiviazione, oppure è possibile eseguire ricerche in base al nome dalle _casella di ricerca_ nella parte superiore della finestra del portale.

1. Selezionare il **chiavi di accesso** pannello dell'account di archiviazione nel portale.

1. Copia il **key1** stringa di connessione.

1. Incollare il contenuto della chiave di accesso copiato dal portale come valore per la variabile di configurazione di stringa di connessione.

La configurazione sarà ora simile alla seguente:

::: zone pivot="csharp"
```json
{
    "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
}
```
::: zone-end

::: zone pivot="javascript"
```
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
```
::: zone-end

Ora che abbiamo che tutte le reti cablate, è possibile iniziare ad aggiungere codice per usare l'account di archiviazione.