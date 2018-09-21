Ora si userà l'interfaccia della riga di comando di Azure per creare un gruppo di risorse e quindi distribuire un'app Web in questo gruppo di risorse.

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a>Uso di un gruppo di risorse

Quando si usano il proprio computer e la propria sottoscrizione di Azure, è necessario accedere prima ad Azure tramite il comando `az login`. Questa operazione non è necessaria se si usa l'ambiente Cloud Shell.

In seguito, si crea in genere un gruppo di risorse per tutte le risorse di Azure correlate con un comando `az group create`, ma per questi esercizi il gruppo di risorse è già stato creato. Usare **<rgn>[Nome gruppo di risorse sandbox]</rgn>** per il gruppo di risorse.

1. È possibile elencare tutti i gruppi di risorse presenti in una tabella tramite l'interfaccia della riga di comando di Azure. Poiché si usa un ambiente sandbox di Azure gratuito, deve essere presente un solo gruppo di risorse.

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. Con lo sviluppo di altri elementi di Azure è possibile che vengano aggiunti vari gruppi di risorse. Se nell'elenco di gruppi sono presenti vari elementi, è possibile filtrare i valori restituiti aggiungendo un'opzione `--query`. Provare questo comando:

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    La query è formattata usando **JMESPath**, che è un linguaggio di query standard per le richieste JSON. Altre informazioni su questo linguaggio di filtro avanzato si possono trovare all'indirizzo <http://jmespath.org/>. Nel modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure** vengono analizzate le query in modo più approfondito.

### <a name="steps-to-create-a-service-plan"></a>Procedura per creare un piano di servizio

Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web. I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.

1. Creare un piano di servizio app per eseguire l'app. Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.

    > [!WARNING]
    > I nomi dell'app e del piano devono essere _univoci_. Aggiungere pertanto un suffisso al nome e sostituire il testo `<unique>` nel comando seguente con un set di numeri, con le proprie iniziali o con qualsiasi altro testo per verificare che sia univoco in Azure.

    Per il parametro `--location`, usare uno dei valori di località seguenti.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --location <location>
    ```

    Il completamento di questo comando può richiedere alcuni minuti.

1. Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Procedura per creare un'app Web

A questo punto verrà creata l'app Web nel piano di servizio. È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.

1. Creare l'app Web e specificare il nome del piano creato in precedenza. **Analogamente al nome del piano, anche il nome dell'app deve essere univoco**. Sostituire l'indicatore `<unique>` con un testo per rendere il nome univoco a livello globale.

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.

    ```azurecli
    az webapp list --output table
    ```

1. Prendere nota dell'elemento **DefaultHostName** elencato nella tabella perché si tratta dell'indirizzo Web raggiungibile per il nuovo sito Web. Azure renderà disponibile il sito Web tramite il nome app univoco nel dominio `azurewebsites.net`. Se ad esempio il nome del sito era "popupwebapp-mslearn123", l'URL del sito Web è: `http://popupwebapp-mslearn123.azurewebsites.net`.

1. Nel sito è presente una pagina di avvio rapido creata da Azure che è possibile visualizzare in un browser o tramite CURL, usando solo l'elemento **DefaultHostName**:

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a>Procedura per distribuire il codice da GitHub

1. Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web. Verrà usata una semplice pagina PHP disponibile nel repository GitHub di esempi di Azure che visualizza "HelloWorld!" quando viene eseguita. Assicurarsi di usare il nome dell'app Web creata.

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Dopo la distribuzione, accedere nuovamente al sito con un browser o tramite CURL.

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    Nella pagina viene visualizzato "Hello World!"

    ```output
    Hello World!
    ```

In questo esercizio è stato dimostrato un criterio tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure. È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse. È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse. Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.