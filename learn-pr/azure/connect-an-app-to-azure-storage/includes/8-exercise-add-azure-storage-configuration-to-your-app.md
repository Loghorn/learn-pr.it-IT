::: zone pivot="csharp" In questo esercizio si aggiunge all'applicazione .NET core il supporto per recuperare una stringa di connessione da un file di configurazione. Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione in un file JSON.

## <a name="create-a-json-configuration-file"></a>Creare un file di configurazione JSON

1. Assicurarsi di essere nella directory di lavoro corretta per il progetto.

1. Usare lo strumento `touch` sulla riga di comando per creare un file denominato **appsettings. JSON**.

    ```bash
    touch appsettings.json
    ```

1. Aprire il progetto con l'editor interattivo, se si lavora in locale, usare il proprio editor preferito. È consigliabile usare **Visual Studio Code**, un ambiente di sviluppo integrato estendibile multipiattaforma. I comandi seguenti sono per l'editor di Cloud Shell, ma sono molto simili a quelli di Visual Studio Code.
    
    ```bash
    code .
    ```

1. Selezionare il file **appsettings.json** file nell'editor e aggiungere il testo seguente. Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu relativo alle operazioni comuni per i file.

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. Selezionare quindi il file di progetto (**PhotoSharingApp.csproj**) per aprirlo nell'editor.

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

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere il codice per leggere il file di configurazione

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

::: zone-pivot="javascript"

Aggiungiamo all'applicazione Node.js il supporto per recuperare una stringa di connessione da un file di configurazione. Per iniziare si aggiungeranno i componenti di base necessari per gestire la configurazione dal file JavaScript.

## <a name="create-a-env-configuration-file"></a>Creare un file di configurazione con estensione env

1. Assicurarsi di essere nella directory di lavoro corretta per il progetto.

1. Usare lo strumento `touch` sulla riga di comando per creare un file denominato **.env**.

    ```bash
    touch .env
    ```

1. Aprire il progetto con l'editor interattivo, se si lavora in locale, usare il proprio editor preferito. È consigliabile usare **Visual Studio Code**, un ambiente di sviluppo integrato estendibile multipiattaforma. I comandi seguenti sono per l'editor di Cloud Shell, ma sono molto simili a quelli di Visual Studio Code.
    
    ```bash
    code .
    ```

1. Selezionare il file **.env** nell'editor e aggiungere il testo seguente. Salvare il file. Nell'angolo superiore destro dell'editor online è disponibile un menu relativo alle operazioni comuni per i file.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > Il valore **AZURE_STORAGE_CONNECTION_STRING** è una variabile di ambiente hardcoded usata dalle API di archiviazione per cercare le chiavi di accesso. Se si preferisce si può usare un nome personalizzato, ma è necessario fornire il nome quando si crea l'oggetto `BlobService`.

1. Salvare il file.

## <a name="add-support-to-read-an-environment-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione dell'ambiente

Si può includere nelle app Node.js il supporto per la lettura dal file **.env** aggiungendo il pacchetto **dotenv**.

1. Nella della finestra relativa al prompt dei comandi aggiungere una dipendenza dal pacchetto *dotenv**.

    ```bash
    node install dotenv --save
    ```

## <a name="add-code-to-read-the-configuration-file"></a>Aggiungere il codice per leggere il file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione.

1. Selezionare *index.js** nell'editor.

1. Nella parte superiore del file è presente la riga **#!/usr/bin/env node**. Al di sotto di questa riga aggiungere un'istruzione `require` per caricare il pacchetto **dotenv**. In questo modo si rendono le variabili di ambiente definite nel file **.env** disponibili per il programma.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

## <a name="add-the-connection-string-to-the-configuration-file"></a>Aggiungere la stringa di connessione al file di configurazione

È ora necessario ottenere la stringa di connessione dell'account di archiviazione e aggiungerla nella configurazione per l'app.

1. Accedere al [portale di Azure](https://portal.azure.com/?azure-portal=true).

1. Passare all'account di archiviazione. È possibile usare la sezione **Tutte le risorse** per trovare l'account di archiviazione oppure eseguire una ricerca per nome nella _casella di ricerca_ nella parte superiore della finestra del portale. 

1. Selezionare il pannello **Chiavi di accesso** dell'account di archiviazione nel portale.

1. Copiare la stringa di connessione **key1**.

1. Incollare il contenuto della chiave di accesso copiata dal portale come valore per la variabile di configurazione della stringa di connessione.

La configurazione sarà ora simile alla seguente:

::: zone pivot="csharp"
    ```json
    {
        "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
    }
    ```
::: zone-end

::: zone-pivot="javascript"
    ```
    AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
    ```
::: zone-end

Ora che tutto è pronto, si può iniziare ad aggiungere codice per usare l'account di archiviazione.