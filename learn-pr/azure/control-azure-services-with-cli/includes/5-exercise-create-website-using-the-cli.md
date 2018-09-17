Ora si userà l'interfaccia della riga di comando di Azure per creare un gruppo di risorse e quindi distribuire un'app Web in questo gruppo di risorse.

È possibile usare l'interfaccia della riga di comando di Azure installata con la sottoscrizione di Azure, ma per questo esercizio è disponibile un ambiente sandbox gratuito con Azure Cloud Shell in cui l'interfaccia della riga di comando di Azure è già installata. Le istruzioni seguenti presuppongono che venga usato l'ambiente sandbox gratuito.

### <a name="create-a-resource-group"></a>Creare un gruppo di risorse

[!include[](../../../includes/azure-sandbox-activate.md)]

> [!NOTE]
> In genere si crea un gruppo di risorse per tutte le risorse di Azure correlate con un comando `az group create ...`, ma per questi esercizi è già stato creato il gruppo di risorse. Il nome di questo gruppo di risorse verrà inserito nei comandi successivi in questo esercizio.

<!-- TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well. -->

<!-- 1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows. -->

<!-- 1. Start the Azure CLI and run the login command.

    ```azurecli
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin). -->

<!-- 1. Create a resource group.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ``` -->

1. Verificare che il gruppo di risorse sia stato creato correttamente elencando tutti i gruppi di risorse in una tabella.

    ```azurecli
    az group list --output table
    ```

    Viene visualizzato solo il gruppo di risorse `<rgn>[Sandbox resource group name]</rgn>` che è stato generato per l'uso del sandbox gratuito.

<!-- > [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list. -->

1. Man mano che si sviluppano altri elementi di Azure è possibile che vengano aggiunti vari gruppi di risorse. Se nell'elenco di gruppi sono presenti vari elementi, è possibile filtrare i valori restituiti aggiungendo un'opzione `--query`. Provare questo comando:

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    La query è formattata usando **JMESPath**, che è un linguaggio di query standard per le richieste JSON. Altre informazioni su questo linguaggio di filtro avanzato si possono trovare all'indirizzo <http://jmespath.org/>. Nel modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** vengono analizzate le query in modo più approfondito.

### <a name="steps-to-create-a-service-plan"></a>Procedura per creare un piano di servizio

Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web. I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Creare un piano di servizio app per eseguire l'app. Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.

    > [!WARNING]
    > I nomi dell'app e del piano devono essere _univoci_. Aggiungere pertanto un suffisso al nome e sostituire il testo `<unique>` nel comando seguente con un set di numeri, con le proprie iniziali o con qualsiasi altro testo per assicurarsi che sia univoco in Azure.

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --location eastus
    ```

    Il completamento di questo comando può richiedere alcuni minuti.

1. Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Procedura per creare un'app Web

A questo punto verrà creata l'app Web nel piano di servizio. È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.

1. Creare l'app Web e specificare il nome del piano creato in precedenza. **Analogamente al nome del piano, anche il nome dell'app deve essere univoco. Sostituire l'indicatore `<unique>` con un testo per rendere il nome univoco a livello globale.**
    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group popupResGroup --plan popupappplan-<unique>
    ```

1. Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.

    ```azurecli
    az webapp list --output table
    ```

1. Prendere nota di **DefaultHostName** perché sarà necessario in un secondo momento.

### <a name="steps-to-deploy-code-from-github"></a>Procedura per distribuire il codice da GitHub

1. Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web. Verrà usata una semplice pagina PHP disponibile nel repository GitHub di esempi di Azure che visualizza "HelloWorld!" quando viene eseguita. Assicurarsi di usare il nome dell'app Web creato.

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Al termine della distribuzione, Azure renderà disponibile il sito Web tramite il nome app univoco nel dominio `azurewebsites.net`. Se, ad esempio, il nome dell'app fosse "popupapp-mslearn123", l'indirizzo del sito Web sarebbe: <http://popupapp-mslearn123.azurewebsites.net>. Sarà necessario digitare l'URL corretto per accedere all'istanza specifica.

1. Nella pagina viene visualizzato "Hello World!"

1. Chiudere la finestra del browser.

In questo esercizio è stato dimostrato un criterio tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure. È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse. È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse. Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.
