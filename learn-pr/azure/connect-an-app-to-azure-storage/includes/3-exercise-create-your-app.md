Stiamo lavorando su un'applicazione di condivisione foto che userà Archiviazione di Azure per gestire le immagini e gli altri dati archiviati per conto degli utenti.

[!include[](../../../includes/azure-sandbox-activate.md)]

::: zone pivot="csharp"

Per semplificare questo scenario, in modo da potersi concentrare sulle API di archiviazione, si creerà una nuova applicazione console .NET Core. Si presupporrà anche che per l'applicazione sia sempre presente la connettività di rete. Tuttavia, è sempre necessario rafforzare l'app per assicurarsi che gli errori di rete non incidano sull'esperienza utente e non causino errori dell'applicazione stessa.

## <a name="create-a-net-core-application"></a>Creare un'applicazione .NET Core

.NET Core è una versione multipiattaforma di .NET che è possibile eseguire in Windows, macOS e Linux. Si possono installare gli strumenti in locale o in alternativa si può usare Cloud Shell sul lato destro della finestra per eseguire i passaggi seguenti.

1. Accedere a Cloud Shell o aprire una sessione della riga di comando e creare una nuova applicazione console .NET Core denominata "PhotoSharingApp". È possibile aggiungere il flag `-o` o `--output` per creare l'app in una cartella specifica.

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. Eseguire l'app per assicurarsi che venga compilata e funzioni correttamente. Deve visualizzare "Hello, World!" sulla console.

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

Per semplificare questo scenario, in modo da potersi concentrare sulle API di archiviazione, si creerà una nuova applicazione Node.js eseguibile dalla console. Si presupporrà anche che l'applicazione disponga sempre di connettività di rete. Tuttavia, è sempre necessario rafforzare l'app per assicurarsi che gli errori di rete non incidano sull'esperienza utente e non risultino in un errore dell'applicazione stessa.

## <a name="create-a-nodejs-application"></a>Creare un'applicazione Node.js

Node.js è un framework diffuso per l'esecuzione di app JavaScript. Viene in genere usato per le app Web, ma si può anche usare per eseguire logica dalla riga di comando. Con gli strumenti installati in locale, è possibile eseguire i passaggi seguenti da una riga di comando. In alternativa, è possibile usare Cloud Shell sul lato destro della finestra.

1. Accedere a Cloud Shell o aprire una sessione della riga di comando e creare una nuova cartella denominata "PhotoSharingApp".

    ```bash
    mkdir PhotoSharingApp
    ```

1. Passare alla nuova cartella e usare `npm` per inizializzare una nuova app Node.js. In questo modo viene creato un file **package.json** contenente i metadati che descrivono l'app.

    ```bash
    cd PhotoSharingApp
    npm init -y
    ```

1. Creare un nuovo file di origine **index.js** in cui salvare il codice.

    ```bash
    touch index.js
    ```

1. Aprire il file **index.js** con un editor. Se si usa Cloud Shell, è possibile digitare `code .` per aprire un editor.

1. Incollare il programma seguente nel file **index.js**. È possibile usare i tasti di scelta rapida **Ctrl+V** o il pulsante destro del mouse per incollare.

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. Salvare il file&mdash; Si può usare il menu "..." nell'angolo superiore destro dell'editor di Cloud Shell.

1. Eseguire l'app per assicurarsi che funzioni correttamente. Dovrebbe visualizzare "Hello, World!" sulla console.

    ```bash
    node index.js
    ```

::: zone-end