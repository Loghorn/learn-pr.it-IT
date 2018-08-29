
In questo esercizio si userà l'interfaccia della riga di comando di Azure nel computer locale per creare un gruppo di risorse e quindi per distribuire un'app Web in questo gruppo di risorse. 

### <a name="steps-to-create-a-resource-group"></a>Procedura per creare un gruppo di risorse
1. Aprire una shell di bash in Linux o macOS oppure aprire la finestra del prompt dei comandi o PowerShell in ambiente Windows.

1. Avviare l'interfaccia della riga di comando di Azure ed eseguire il comando di accesso.

    ```bash
    az login
    ```
    Se non viene visualizzata la pagina di accesso ad Azure nel Web browser, seguire le istruzioni della riga di comando e immettere un codice di autorizzazione all'indirizzo [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

1. Creare un gruppo di risorse.

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. Verificare che il gruppo di risorse sia stato creato correttamente elencando tutti i gruppi di risorse in una tabella.

    ```bash
    az group list --output table
    ```
1. Facoltativamente, verificare che la risorsa sia stata creata nel portale di Azure. Aprire un Web browser, accedere al portale e passare alla sezione **Gruppi di risorse** (vedere sotto). Il nuovo gruppo di risorse dovrebbe essere visualizzato nell'elenco.

![Uso del portale per elencare i gruppi di risorse](../media-drafts/5-listing-resource-groups.png)

### <a name="steps-to-create-a-service-plan"></a>Procedura per creare un piano di servizio
Quando si eseguono app Web, usando il servizio app di Azure, si paga per le risorse di calcolo di Azure usate dall'app e ciò dipende dal piano di servizio app associato alle app Web. I piani di servizio determinano l'area usata per il data center dell'app, il numero di macchine virtuali usate e il piano tariffario.

1. Creare un piano di servizio app per eseguire l'app. Il nome dell'app e del piano deve essere univoco, quindi modificare la stringa "12345" in un numero casuale. Il comando seguente non specifica un piano tariffario o dettagli sull'istanza della macchina virtuale, quindi per impostazione predefinita si otterrà un piano **Basic** con una istanza di macchina virtuale **Piccola**.

    ```bash
    az appservice plan create --name popupapp12345 --resource-group popupResGroup --location westeurope
    ```

1. Verificare che il piano di servizio sia stato creato correttamente elencando tutti i piani in una tabella.

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a>Procedura per creare un'app Web
A questo punto verrà creata l'app Web nel piano di servizio. È possibile distribuire il codice nello stesso momento, ma per questo esempio l'operazione verrà eseguita in passaggi separati.

1. Creare l'app Web, ricordando di sostituire la stringa "12345" con lo stesso numero casuale usato in precedenza.
    ```bash
    az webapp create --name popupapp12345 --resource-group popupResGroup --plan popupapp12345
    ```

1. Verificare che l'app sia stata creata correttamente elencando tutte le app in una tabella.

    ```bash
    az webapp list --output table
    ```

1. Prendere nota di **DefaultHostName** perché sarà necessario in un secondo momento.

### <a name="steps-to-deploy-code-from-github"></a>Procedura per distribuire il codice da GitHub
1. Il passaggio finale consiste nel distribuire il codice da un repository di GitHub all'app Web, ricordando ancora una volta di sostituire la stringa "12345" con lo stesso numero casuale usato in precedenza.
    ```bash
    az webapp deployment source config --name popupapp12345 --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Copiare l'URL seguente in un browser per visualizzare l'app Web.
http://popupapp12345.azurewebsites.net

1. Nella pagina viene visualizzato "HelloWorld!"

1. Chiudere la finestra del browser.

## <a name="summary"></a>Summary
In questo esercizio è stato dimostrato un modello tipico per una sessione interattiva dell'interfaccia della riga di comando di Azure. È stato prima di tutto usato un comando standard per creare un nuovo gruppo di risorse. È stato quindi usato un set di comandi per distribuire una risorsa (in questo esempio, un'app Web) in questo gruppo di risorse. Questo set di comandi può essere facilmente combinato in uno script della shell da eseguire ogni volta che è necessario creare la stessa risorsa.
