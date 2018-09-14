In questo modulo verrà distribuita un'applicazione Web semplice che presenta un'interfaccia utente basata su HTML. Un back-end senza server consente all'applicazione di caricare le immagini e di generare automaticamente sottotitoli in lingua originale descrittivi.

![Esecuzione dell'app Web](../media/0-app-screenshot-finished.png)

Il diagramma seguente illustra i servizi di Azure usati dall'applicazione.

![Diagramma dell'architettura della soluzione](../media/0-architecture.jpg)

1. Archiviazione BLOB di Azure gestisce il contenuto Web statico (HTML, CSS o JS) e archivia le immagini.
2. Funzioni di Azure gestisce i caricamenti delle immagini, il ridimensionamento e l'archiviazione dei metadati.
3. Azure Cosmos DB archivia i metadati delle immagini.
4. Le App per la logica di Azure recupera le didascalie di immagine dal Computer Vision API servizi cognitivi.
5. Azure Active Directory gestisce l'autenticazione degli utenti.

Archiviazione BLOB di Azure è un servizio economico e altamente scalabile che può essere usato per ospitare file statici. In questo modulo, si userà archiviazione Blob per rendere disponibile contenuto statico (ad esempio, HTML, JavaScript o CSS) per un'app web che è compilare.

## <a name="create-an-azure-storage-account"></a>Creare un account di Archiviazione di Azure

[!include[](../../../includes/azure-sandbox-activate.md)]

Un account di Archiviazione di Azure è una risorsa di Azure che consente di archiviare tabelle, code, file, BLOB (oggetti) e dischi delle macchine virtuali.

1. Il contenuto statico, come ad esempio file HTML, CSS e JavaScript, per questa esercitazione è ospitato nell'archiviazione BLOB. L'archiviazione BLOB richiede un account di archiviazione. Creare un utilizzo generico v2 account di archiviazione (per utilizzo generico v2) nel gruppo di risorse. Sostituire `<storage account name>` con un nome univoco.

    ```azurecli
    az storage account create -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 --https-only true --sku Standard_LRS
    ```
    
1. Usare la barra di ricerca nella parte superiore del [portale di Azure](https://portal.azure.com/?azure-portal=true) per trovare l'account di archiviazione appena creato. Aprire l'account.

1. Nel riquadro di spostamento a sinistra selezionare **Sito Web statico (anteprima)** per configurare un contenitore per l'hosting di siti Web statici.
    - Selezionare **Abilitato** per abilitare il sito Web statico.
    - Immettere **index.html** come nome del documento di indice. Nella finestra è già presente *index.html* in grigio, ma si tratta solo di testo di esempio. È comunque necessario immettere **index.html** nella finestra.
    - Fare clic su **Salva**.
    
    ![Immettere le impostazioni del sito Web statico](../media/1-storage-static-website.png)

1. Salvare l'**Endpoint primario** in una posizione da cui poterlo copiare facilmente durante l'esecuzione dell'esercitazione. L'endpoint è l'URL dell'applicazione Web.

## <a name="upload-the-web-application"></a>Caricare l'applicazione Web

1. I file di origine per l'applicazione che si compila in questa esercitazione si trovano in un [repository GitHub](https://github.com/Azure-Samples/functions-first-serverless-web-application). Passare alla home directory in Cloud Shell e Clona questo repository.

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    Il repository viene clonato in `/home/<username>/functions-first-serverless-web-application`.

1. L'applicazione Web sul lato client si trova nella cartella **www** e viene compilata con il framework JavaScript Vue.js. Aprire il **www** cartella ed eseguire **npm** comandi per installare le dipendenze dell'applicazione e compilare l'applicazione. Il completamento dell'ultimo di questi comandi potrebbe richiedere alcuni minuti.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    L'applicazione viene generata nella cartella **dist**.

1. Passare dalla directory corrente alla directory **dist** e caricare l'applicazione nel contenitore BLOB **$web**.

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. Per visualizzare l'applicazione, aprire l'URL dell'endpoint primario del sito Web statico in un web browser.

    ![Home page della prima app Web serverless](../media/1-app-screenshot-new.png)


## <a name="summary"></a>Riepilogo

In questa unità, creato un account di archiviazione. Un contenitore blob denominato **$web** nella risorsa di archiviazione account archivia il contenuto statico per l'applicazione web e rende il contenuto disponibile pubblicamente. Successivamente, si apprenderà come usare una funzione senza server per caricare immagini nell'archiviazione Blob da questa applicazione web.