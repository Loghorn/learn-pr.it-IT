L'estensione Azure Cosmos DB per Visual Studio Code semplifica la creazione di account, database e raccolte grazie alla possibilità di creare le risorse usando la finestra di comando.

In questa unità si vedrà come installare l'estensione Azure Cosmos DB per Visual Studio e quindi come usarla per creare un account, un database e una raccolta.

## <a name="install-the-azure-cosmos-db-extension-for-visual-studio"></a>Installare l'estensione Azure Cosmos DB per Visual Studio

1. Passare a [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) e installare l'estensione **Azure Cosmos DB** per Visual Studio Code.

1. Quando la scheda dell'estensione viene caricata in Visual Studio Code, fare clic su **Installa**.

1. Al termine dell'installazione, fare clic su **Ricarica**.

    Visual Studio Code visualizza l'icona di Azure ![Icona di Azure](../media/2-setup/visual-studio-code-explorer-icon.png) sul lato sinistro della schermata dopo l'installazione e il ricaricamento dell'estensione.

## <a name="create-an-azure-cosmos-db-account-in-visual-studio-code"></a>Creare un account di Azure Cosmos DB in Visual Studio Code

[!include[](../../../includes/azure-sandbox-activate.md)]

1. In Visual Studio Code accedere ad Azure facendo clic su **Visualizza** > **Riquadro comandi** e digitando **Azure: Accedi**. È necessario aver installato l'estensione [Account Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) per usare Azure: Accedi.

    > [!IMPORTANT]
    > Accedere ad Azure con lo stesso account usato per creare la sandbox. La sandbox consente di accedere a una sottoscrizione Concierge.

    Seguire le istruzioni per copiare e incollare il codice fornito nel Web browser, che autentica la sessione di Visual Studio Code.

1. Fare clic sull'icona di **Azure** ![Icona di Azure](../media/2-setup/visual-studio-code-explorer-icon.png) nel menu a sinistra e quindi fare clic con il pulsante destro del mouse su **Concierge Subscription** (Sottoscrizione Concierge) e scegliere **Crea account**.

    Se la sottoscrizione Concierge non è inclusa nell'elenco, assicurarsi di avere eseguito l'accesso ad Azure in Visual Studio Code con lo stesso account usato per creare la sandbox. Inoltre, se le sottoscrizioni di Azure sono state filtrate nell'estensione Account Azure, verificare che la sottoscrizione Concierge sia selezionata nel comando `> Azure: Select Subscriptions`.

1. Fare clic sul pulsante __+__ per iniziare a creare un account Cosmos DB. Se sono disponibili più sottoscrizioni, verrà richiesto di selezionarne una.

1. Nella casella di testo nella parte superiore della schermata, immettere un nome univoco per l'account di Azure Cosmos DB e quindi premere INVIO. Il nome dell'account può contenere solo lettere minuscole, numeri e il carattere '-' e deve avere una lunghezza compresa tra 3 e 31 caratteri.

1. Selezionare **SQL (DocumentDB)** > **<rgn>[Nome gruppo di risorse sandbox]</rgn>** e quindi selezionare una località.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    La scheda di output in Visual Studio Code visualizza lo stato di avanzamento della creazione dell'account, operazione il cui completamento richiede alcuni minuti.

1. Dopo aver creato l'account, espandere la sottoscrizione di Azure nel riquadro **Azure: Cosmos DB** e l'estensione visualizza il nuovo account di Azure Cosmos DB. Nell'immagine seguente, il nuovo account è denominato **learning-modules**.

    ![Estensione di Visual Studio Code per Azure Cosmos DB](../media/2-setup/azure-cosmos-db-vs-code-extension.png)

## <a name="create-an-azure-cosmos-db-database-and-collection-in-visual-studio-code"></a>Creare un database e una raccolta di Azure Cosmos DB in Visual Studio Code

Si vedrà ora come creare un nuovo database e una raccolta per i clienti.

1. Nel riquadro Azure: Cosmos DB fare clic con il pulsante destro del mouse sul nuovo account e quindi scegliere **Crea database**.
1. Nel riquadro di input nella parte superiore della schermata digitare `Users` per il nome del database e premere INVIO.
1. Immettere `WebCustomers` per il nome della raccolta e premere INVIO.
1. Immettere `userId` per la chiave di partizione e premere INVIO.
1. Infine, confermare `1000` per la capacità unità elaborate iniziale e premere INVIO.
1. Espandere l'account nel riquadro **Azure: Cosmos DB**. Verranno visualizzati il nuovo database **Users** e la nuova raccolta **WebCustomers**.

    ![Animazione che mostra le istruzioni riportate sopra eseguite tramite l'estensione Azure Cosmos DB in Visual Studio Code.](../media/2-setup/vs-code-azure-cosmos-db-extension.gif)

Ora che è disponibile l'account di Azure Cosmos DB, è possibile procedere in Visual Studio Code.
