In questo modulo verrà creata una semplice un'app console tramite il terminale integrato, verranno installati i pacchetti NuGet e si userà l'estensione di Azure Cosmos DB per visualizzare i database e le raccolte creati nel modulo precedente. Si recupererà la stringa di connessione di Azure Cosmos DB dall'estensione e quindi si inizierà a configurare la connessione ad Azure Cosmos DB per creare il database utente.

## <a name="create-a-console-app"></a>Creare un'app console

1. Aprire Visual Studio Code e quindi selezionare **File** > **Apri cartella**.

2. Creare una nuova cartella denominata **modulo di apprendimento** in cui si posizionare il nuovo progetto C# e quindi fare clic su **Seleziona cartella**.

2. Aprire il terminale integrato da Visual Studio Code selezionando **Visualizza** > **Terminale integrato** nel menu principale.

3. Nella finestra del terminale digitare **dotnet new console**.

    Questo comando crea un file **Program.cs** nella cartella con un semplice programma "Hello World" già scritto, oltre a un file di progetto C# denominato **learning-module.csproj**.

4. Nella finestra del terminale digitare il comando seguente per eseguire il programma "Hello World". 

    ```
    dotnet run
    ```

    La finestra del terminale visualizza "Hello world!" come output.

## <a name="connect-the-app-to-azure-cosmos-db"></a>Connettere l'app ad Azure Cosmos DB

1. Accedere ad Azure facendo clic su **Visualizza** > **Riquadro comandi** e digitando **Azure: Accedi**.

    Seguire le istruzioni per copiare e incollare il codice fornito nel Web browser, che autentica la sessione di Visual Studio Code.

2. Fare clic sull'icona ![icona di Esplora risorse](../media/2-setup/visual-studio-code-explorer-icon.png) **Esplora risorse** nel menu a sinistra e quindi espandere **Azure Cosmos DB**.

3. Espandere la sottoscrizione di Azure > account Azure Cosmos DB. Se nei moduli precedenti sono stati creati il database **Products** e la raccolta **Clothing**, l'estensione li visualizza.

   ![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

4. Verranno ora creati un nuovo database e una raccolta per i clienti.

    Nella finestra Esplora risorse fare clic con il pulsante destro del mouse sul proprio account e quindi fare clic su **Crea database**. 
    
    Nella casella di testo nella parte superiore della schermata, digitare **Users** per il nome del database > **INVIO** > **WebCustomers** per il nome della raccolta >  **INVIO** > **userId** per la chiave di partizione > **INVIO** > **1000** per la capacità di unità elaborate iniziale > **INVIO**.

    ![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing-->

    Il nuovo database Users e la raccolta WebCustomers vengono visualizzati nella finestra Esplora risorse.

5. Nel terminale integrato eseguire ognuno dei comandi seguenti da un nuovo prompt dei comandi per installare i pacchetti NuGet necessari.

    ```
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet add package Bogus
    dotnet restore
    ```

6. Nella parte superiore del riquadro Esplora risorse fare clic su **Program.cs** per aprire il file.

7. Aggiungere le istruzioni using seguenti dopo `using System;`.

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

8. Creare un nuovo file denominato App.config nella cartella del modulo di apprendimento e aggiungere il codice seguente.
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

9. Copiare la stringa di connessione dall'estensione Azure Cosmos DB facendo clic con il pulsante destro del mouse sull'account del modulo di apprendimento e scegliendo **Copy Connection String** (Copia stringa di connessione).

    ![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/vs-code-copy-connection-string.gif) 

10. Incollare la stringa di connessione in un file di testo e quindi copiare la parte **AccountEndpoint** dal file di testo in **accountEndpoint** nel file App.config.

    AccountEndpoint dovrebbe avere un aspetto analogo al codice seguente:

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

12. A questo punto copiare il valore **AccountKey** dal valore di testo nel valore **accountKey** in App.config.

12. Nel terminale integrato digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

13. Aggiungere una nuova attività asincrona per creare un nuovo client e verificare se il database Users esiste aggiungendo il metodo seguente dopo il metodo principale.
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpointUrl"]), ConfigurationManager.AppSettings["primaryKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

14. Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

15. Copiare e incollare il codice seguente nel metodo **Main**, sovrascrivendo la riga `Console.WriteLine("Hello World!");` corrente.

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

16. Di nuovo nel terminale integrato, digitare il comando seguente per eseguire il programma per assicurarsi che funzioni.

    ```csharp
    dotnet run
    ```

    La console visualizza l'output seguente.
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a>Riepilogo

In questo modulo sono state impostate le fondamenta per l'applicazione di Azure Cosmos DB. È stato configurato l'ambiente di sviluppo in Visual Studio Code, creato un semplice progetto HelloWorld, connesso il progetto all'endpoint di Azure Cosmos DB e verificato che la raccolta e il database esistano.