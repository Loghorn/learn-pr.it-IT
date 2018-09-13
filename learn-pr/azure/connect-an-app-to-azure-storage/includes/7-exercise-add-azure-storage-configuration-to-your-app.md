In questa unità verrà recuperata la chiave di accesso e la si aggiungerà alla configurazione dell'app. Inoltre, poiché è stata creata un'applicazione console .NET Core, è necessario aggiungere all'applicazione il supporto per la lettura da file di configurazione.

## <a name="create-a-json-configuration-file"></a>Creare un file di configurazione JSON

1. Nel progetto di Visual Studio creato nell'unità precedente fare clic con il pulsante destro del mouse sul progetto, scegliere **Aggiungi**, quindi **Nuovo elemento** (oppure premere CTRL+MAIUSC+A).

1. Verrà visualizzata una finestra di dialogo modale. Selezionare **File di configurazione JSON per JavaScript** e digitare **appsettings.json** nella casella di testo *Nome*, quindi fare clic su **Aggiungi**.

  ![Impostazioni app](..\media-draft\7-appsettings.png)

1. Viene aggiunto un file al progetto con contenuto simile al seguente:

    ```json
    {
      "exclude": [
        "**/bin",
        "**/bower_components",
        "**/jspm_packages",
        "**/node_modules",
        "**/obj",
        "**/platforms"
      ]
    }
    ```
1. Rimuovere la voce **exclude** e il contenuto associato e sostituirla con una singola voce **StorageAccountConnectionString** con un valore di stringa vuoto. La voce deve essere simile alla seguente:

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```
1. Selezionare il file **appsettings.json**, fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** (in alternativa, premere ALT+INVIO o F4).

1. Modificare il valore di **Copia nella directory di output** in **Copia se più recente**.

  ![Azione di compilazione](..\media-draft\7-build-action.png)

  > In questo modo, il file di configurazione dell'app viene inserito nella directory di output alla creazione/compilazione dell'app.

## <a name="add-support-to-read-a-json-configuration-file"></a>Aggiungere il supporto per la lettura di un file di configurazione JSON

Un'applicazione console .NET Core richiede l'aggiunta di determinate librerie per permettere la lettura da un file di configurazione JSON in tutta semplicità.

1. Fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.

![Gestisci pacchetti NuGet](..\media-draft\5-manage-nuget-packages.png)

1. Verrà visualizzata una pagina che mostra i pacchetti NuGet attualmente installati. Fare clic sull'opzione **Sfoglia** e digitare **Microsoft.Extensions.Configuration.Json** nel campo di ricerca. Nei risultati della ricerca viene visualizzato il pacchetto **Microsoft.Extensions.Configuration.Json**.

1. Selezionare il pacchetto **Microsoft.Extensions.Configuration.Json** e nel riquadro destro selezionare **Installa**.

1. Verrà visualizzata la finestra di dialogo **Anteprima modifiche**. Fare clic su **OK**.

1. Verrà visualizzata una finestra di dialogo per l'accettazione della licenza. Fare clic su **Accetto**.


## <a name="read-from-the-configuration-file"></a>Lettura dal file di configurazione

Ora che sono state aggiunte le librerie necessarie per permettere la lettura della configurazione, è necessario abilitare questa funzionalità nell'applicazione console.

1. Individuare il file **Program.cs** nel progetto e aprirlo in Visual Studio.

1. Nella parte superiore del file è presente la riga **using System;**. Al di sotto di questa riga aggiungere le righe di codice seguenti:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Sostituire il contenuto del metodo **Main** con il codice seguente:

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

1. Il file **Program.cs** avrà ora un aspetto simile al seguente:

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;

    namespace DemoConsoleApp
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

> Questo codice inizializza il sistema di configurazione per la lettura dal file **appsettings.json**.

## <a name="add-your-connection-string-to-the-configuration-file"></a>Aggiungere la stringa di connessione al file di configurazione

È ora necessario ottenere la stringa di connessione dell'account di archiviazione e aggiungerla nella configurazione per l'app.

1. Nel portale di Azure passare alla sottoscrizione che contiene l'account di archiviazione.

1. Passare all'account di archiviazione.

1. Passare al pannello Chiavi dell'account dell'account di archiviazione nel portale.

1. Copiare la stringa di connessione **key1**.

1. Incollare il contenuto della chiave di accesso copiata dal portale come valore per **StorageAccountConnectionString**. La configurazione sarà ora simile alla seguente:

    ```json
    {
      "StorageAccountConnectionString": "your-access-key-connection-string-goes-here"
    }
    ```
