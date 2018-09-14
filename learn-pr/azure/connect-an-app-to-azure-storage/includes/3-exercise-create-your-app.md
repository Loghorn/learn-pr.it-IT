È importante ricordare che Microsoft si impegna in un'applicazione di condivisione di foto che verrà utilizzata l'archiviazione di Azure per gestire le immagini e gli altri bit di dati archiviati per conto degli utenti.

::: zone pivot="csharp"

Per semplificare questo scenario, in modo che sono tutti concentrati sulle API di archiviazione, si creerà una nuova applicazione Console .NET Core. Si presupporrà anche ha sempre la connettività di rete. Tuttavia, è sempre necessario rafforzare la protezione dell'app per verificare che gli errori di rete non influisce sull'esperienza dell'utente o generare un errore dell'applicazione stessa.

## <a name="create-a-net-core-application"></a>Creare un'applicazione .NET Core

.NET core è una versione multipiattaforma di .NET in esecuzione su Windows, macOS e Linux. È possibile installare gli strumenti in locale o usare Cloud Shell sul lato destro della finestra per eseguire i passaggi seguenti riportati di seguito.

1. Accedere a Cloud Shell o aprire una sessione della riga di comando e creare una nuova applicazione Console .NET Core con il nome "PhotoSharingApp". È possibile aggiungere il `-o` o `--output` flag per creare l'app in una cartella specifica.

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. Eseguire l'app per assicurarsi che si basa e viene eseguita correttamente. Viene visualizzato "Hello, World!" Nella console.

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

Per semplificare questo scenario, in modo che sono tutti concentrati sulle API di archiviazione, si creerà una nuova applicazione Node. js che è possibile eseguire dalla console. Si presupporrà anche ha sempre la connettività di rete. Tuttavia, è sempre necessario rafforzare la protezione dell'app per verificare che gli errori di rete non influisce sull'esperienza dell'utente, o generare un errore dell'applicazione stessa.

## <a name="create-a-nodejs-application"></a>Creare un'applicazione Node.js

Node. js è un framework più diffusi per l'esecuzione di App JavaScript. Viene in genere utilizzato per le app web, ma è possibile utilizzarlo per eseguire la logica da anche la riga di comando. Se si dispone di strumenti installati in locale, è possibile eseguire i passaggi seguenti dalla riga di comando. In alternativa, è possibile usare Cloud Shell sul lato destro della finestra per eseguire la procedura seguente.

1. Accedere a Cloud Shell o aprire una sessione della riga di comando e creare una nuova cartella denominata "PhotoSharingApp".

    ```bash
    mkdir PhotoSharingApp
    ```

1. Modificare nella nuova cartella e creare un **package. JSON** file con Node Package Manager (NPM) che descrive la nuova app.
    - Denominarla "PhotoSharingApp".
    - È possibile sfruttare le impostazioni predefinite per tutte le altre richieste.

    ```bash
    cd PhotoSharingApp
    npm init
    ```

1. Creare un nuovo file di origine **index. js**, ovvero in cui verrà salvato il nostro codice.

    ```bash
    touch index.js
    ```

1. Aprire il **index. js** file con un editor. Se si usa Cloud Shell, è possibile digitare `code .` per aprire un editor.

1. Inserire il seguente programma nel **index. js** file.

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. Salvare il file&mdash;è possibile usare il menu "..." nell'angolo superiore destro dell'editor di Cloud Shell.

1. Eseguire l'app per assicurarsi che viene eseguita correttamente. Viene visualizzato "Hello, World!" Nella console.

    ```bash
    node index.js
    ```

::: zone-end