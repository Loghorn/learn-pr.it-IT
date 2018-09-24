Visual Studio Code consente di creare un'applicazione console usando il terminale integrato e alcuni brevi comandi.

In questa unità si creerà una semplice app console usando il terminale integrato, dall'estensione si recupererà la stringa di connessione di Azure Cosmos DB e dall'applicazione si configurerà la connessione ad Azure Cosmos DB.

## <a name="create-a-console-app"></a>Creare un'app console

1. In Visual Studio Code selezionare **File** > **Apri cartella**.

1. Creare una nuova cartella denominata `learning-module` in un percorso a scelta e quindi fare clic su **Seleziona cartella**.

1. Assicurarsi che il salvataggio automatico dei file sia abilitato. Fare quindi clic sul menu File e selezionare **Salvataggio automatico** se l'opzione non è selezionata. Verranno copiati vari blocchi di codice e ciò garantirà di operare sempre sulle modifiche più recenti dei file.

1. Aprire il terminale integrato da Visual Studio Code selezionando **Visualizza** > **Terminale** nel menu principale.

1. Copiare e incollare il comando seguente nella finestra del terminale.

    ```bash
    dotnet new console
    ```

    Questo comando crea un file **Program.cs** nella cartella con un semplice programma "Hello World" già scritto, oltre a un file di progetto C# denominato **learning-module.csproj**.

1. Nella finestra del terminale copiare e incollare il comando seguente per eseguire il programma "Hello World".

    ```bash
    dotnet run
    ```

    La finestra del terminale visualizza "Hello world!" come output.

## <a name="connect-the-app-to-azure-cosmos-db"></a>Connettere l'app ad Azure Cosmos DB

1. Al prompt del terminale, copiare e incollare il blocco dei comandi seguente per installare i pacchetti NuGet necessari.

    ```bash
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet restore
    ```

1. Nella parte superiore del riquadro Esplora risorse fare clic su **Program.cs** per aprire il file.

1. Aggiungere le istruzioni using seguenti dopo `using System;`.

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    Se viene visualizzato un messaggio in cui si chiede di aggiungere gli asset mancanti necessari, fare clic su **Sì**.

1. Creare un nuovo file denominato App.config nella cartella del modulo di apprendimento e aggiungere il codice seguente.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. Copiare la stringa di connessione facendo clic sull'![icona di Azure](../media/2-setup/visual-studio-code-explorer-icon.png) a sinistra, espandere la sottoscrizione Concierge, fare clic con il pulsante destro del mouse sul nuovo account di Azure Cosmos DB e scegliere **Copy Connection String** (Copia stringa di connessione).

1. Incollare la stringa di connessione alla fine del file App.config, quindi copiare la parte **AccountEndpoint** dalla stringa di connessione in **accountEndpoint** nel file App.config.

    AccountEndpoint dovrebbe avere un aspetto analogo al codice seguente:

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. A questo punto copiare il valore **AccountKey** dalla stringa di connessione nel valore **accountKey**, quindi eliminare la stringa di connessione originale che è stata copiata.

1. Al prompt del terminale copiare e incollare il comando seguente per eseguire il programma.

    ```csharp
    dotnet run
    ```

    Il programma visualizzerà Hello World! nel terminale.

## <a name="create-the-documentclient"></a>Creare un'istanza di DocumentClient

A questo punto è possibile creare un'istanza di DocumentClient, ovvero la rappresentazione lato client del servizio Azure Cosmos DB. Questo client viene usato per configurare ed eseguire richieste nel servizio.

1. Aggiungere quanto segue all'inizio della classe Program in Program.cs.

    ```csharp
    private DocumentClient client;
    ```

1. Aggiungere una nuova attività asincrona per creare un nuovo client e verificare se il database Users esiste aggiungendo il metodo seguente dopo il metodo `Main`.

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. Sempre nel terminale integrato copiare e incollare il comando seguente per eseguire il programma e assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

1. Copiare e incollare il codice seguente nel metodo **Main**, sovrascrivendo la riga `Console.WriteLine("Hello World!");` corrente.

    ```csharp
    try
    {
        Program p = new Program();
        p.BasicOperations().Wait();
    }
    catch (DocumentClientException de)
    {
        Exception baseException = de.GetBaseException();
        Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
    }
    catch (Exception e)
    {
        Exception baseException = e.GetBaseException();
        Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
    }
    finally
    {
        Console.WriteLine("End of demo, press any key to exit.");
        Console.ReadKey();
    }
    ```

1. Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

    La console visualizza l'output seguente.

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

In questa unità sono state poste le basi per creare l'applicazione di Azure Cosmos DB. Dopo aver configurato l'ambiente di sviluppo in Visual Studio Code, è stato creato un semplice progetto HelloWorld, è stato connesso il progetto all'endpoint di Azure Cosmos DB ed è stata verificata l'esistenza di raccolta e database.
