Gli sviluppatori software di un'azienda hanno l'opportunità di arricchire le loro competenze ed entrare a far parte del team di intelligenza artificiale in-house. Pur impegnandosi a raggiungere questo nuovo ruolo, devono comunque svolgere il loro lavoro quotidiano. Il senior engineer di intelligenza artificiale del team ha segnalato alcuni utili notebook Jupyter con lab basati su PyTorch per il training di un modello di classificazione delle immagini. Interessante, ma non è il caso di installare un set di framework nella piattaforma di sviluppo. Al contrario, creare una macchina virtuale basata sull'immagine della Data Science Virtual Machine (DSVM). 

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-the-azure-cli"></a>Che cos'è l'interfaccia della riga di comando di Azure?

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). L'uso di questo strumento è descritto in dettaglio nel modulo **Controllare i servizi di Azure con l'interfaccia della riga di comando**.

## <a name="managing-deployments"></a>Gestione delle distribuzioni

L'interfaccia della riga di comando di Azure include il comando `az group deployment` per gestire le distribuzioni di Azure Resource Manager. È possibile fornire vari sottocomandi per eseguire attività specifiche. Quelli più comuni includono:

| Sottocomando | Descrizione |
|-------------|-------------|
| `create` | Avvia una distribuzione. |
| `list` | Ottiene tutte le distribuzioni di un gruppo di risorse. |
| `export` | Esporta il modello usato per una distribuzione. |

Per un elenco completo dei comandi di distribuzione disponibili, vedere il [riferimento per il comando di distribuzione az](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)

Si userà `az group deployment create` per eseguire il provisioning della macchina virtuale.

## <a name="create-a-json-deployment-parameters-file"></a>Creare un file di parametri di distribuzione JSON

Si creerà una VM con un modello di Azure Resource Manager. Il modello definisce l'immagine di DSVM di Linux di cui si intende eseguire il provisioning. È necessario fornire alcuni parametri al modello, ad esempio le dimensioni della VM che si intende usare, il nome utente amministratore e la password e il nome host del computer. 

1. Eseguire il comando seguente in Azure Cloud Shell a destra di questa unità:

    ```bash
    code parameter_file.json
    ```
    <!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> Questo comando apre un file vuoto denominato `parameter_file.json` nell'editor integrato. 

1. Incollare il frammento JSON seguente nel file vuoto nell'editor di codice.

    ```json
    { 
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "adminUsername": { "value" : "<USERNAME>"},
         "adminPassword": { "value" : "<PASSWORD>"},
         "vmName": { "value" : "<HOSTNAME>"},
         "vmSize": { "value" : "Standard_DS2_v2"},
      }
    }
    ```

1. Aggiornare i parametri seguenti nel frammento JSON incollato nell'editor:

    |Parametro  |Valore corrente  |Valore personalizzato  |
    |---------|---------|---------|
    |adminUsername     |  `<USERNAME>`       |    Scegliere un nome per l'utente amministratore di questo nuovo computer, ad esempio *azuser*.     |
    |adminPassword     |  `<PASSWORD>`       |   Scegliere una password per questo account utente amministratore. Per altre informazioni sui requisiti delle password, vedere [Domande frequenti sulle macchine virtuali Linux](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)     |
    |vmName     |   `<HOSTNAME>`      |  Scegliere un nome per la nuova macchina virtuale. Il nome deve iniziare con una lettera e contenere solo lettere minuscole e numeri. Provare a scegliere un nome univoco, ad esempio uno che includa le iniziali del nome e l'anno di nascita. |
    |vmSize     |  Standard_DS2_v2       |  Queste dimensioni di macchina virtuale sono idonee per questo esercizio, ma è possibile modificarle. Un elenco delle dimensioni di VM disponibili è reperibile in [Dimensioni delle macchine virtuali Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)       |

1. Salvare le modifiche in `parameter_file.json` e chiudere l'editor di testo.

    > [!IMPORTANT]
    > Ricordare i valori scelti per adminUsername, adminPassword e vmName. Verranno usati di nuovo in questo esercizio.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse 

> [!IMPORTANT]
> In genere si crea un gruppo di risorse in un'area a propria scelta. Tuttavia, la sessione sandbox in cui ci si trova fornisce un gruppo di risorse disponibili per l'uso. Il gruppo di risorse per questa sessione è **<rgn>[nome gruppo di risorse sandbox]</rgn>**.

## <a name="deploy-the-dsvm-to-your-resource-group"></a>Distribuire la DSVM nel gruppo di risorse

Si è scelto un gruppo di risorse e sono stati definiti parametri per il modello di Resource Manager della DSVM in un file denominato `parameter_file.json`. Si eseguirà quindi `az group deployment create` per eseguire il provisioning della macchina virtuale.

1. Eseguire il comando seguente in Azure Cloud Shell:

    ```azurecli
    az group deployment create \
    --resource-group  <rgn>[Sandbox resource group name]</rgn> \
    --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json \
    --parameters parameter_file.json
    ```

    Il comando usa il modello di Resource Manager e i parametri per creare la macchina virtuale nel gruppo di risorse. 

2. La distribuzione di una macchina virtuale richiede alcuni minuti. La console visualizza ` - Running ..` e poco altro fino al completamento dell'operazione. Al termine dell'operazione verrà visualizzata una risposta JSON. Scorrere fino alla fine del file JSON e verificare che il campo **"provisioningState"** abbia il valore *Succeeded*.

    > [!TIP]
    > Se si riceve un errore indicante che il record DNS è già usato da un altro indirizzo IP pubblico, provare a modificare **vmName** nel file `parameter_file.json` in un altro nome univoco.

3. Eseguire il comando seguente per ottenere informazioni sulla VM, sostituendo `<HOSTNAME>` con il nome host definito per la VM.

    ```azurecli
    az vm get-instance-view \
    --name <HOSTNAME> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query instanceView.statuses[1] \
    --output table
    ```

    Questo comando visualizza lo stato della VM. Dovrebbe indicare *Esecuzione della macchina virtuale in corso*.

Congratulazioni. È stata creata e distribuita una macchina virtuale Linux basata sull'immagine della DSVM.

## <a name="open-the-vm-to-ssh-traffic-on-port-22"></a>Aprire la VM indirizzare tramite SSH il traffico sulla porta 22

Per impostazione predefinita, la macchina virtuale non ha alcuna porta aperta. L'obiettivo è di connettersi in remoto, avviare un server Jupyter Notebook ed eseguire altri comandi locali nel computer. Per accedere in remoto alla macchina virtuale usando il protocollo Secure Shell (SSH), è necessario aprire una porta. La porta predefinita per SSH è la porta 22.  

1. Eseguire il comando seguente in Azure Cloud Shell, sostituendo `<HOSTNAME>` con il nome assegnato alla DSVM durante l'installazione. 

    ```azurecli
    az vm open-port \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 22 \
    --priority 900
    ```

Il comando può richiedere fino a un minuto. Al termine, il comando restituisce una risposta JSON per la riga di comando. Verificare che il campo **"provisioningState"** abbia il valore *Succeeded*. A breve verrà testato il funzionamento di SSH. Per il momento, verrà aperta un'altra porta.

## <a name="open-the-vm-to-access-the-jupyter-notebook-server-remotely"></a>Aprire la VM per accedere al server Jupyter Notebook in remoto

Come accennato in precedenza, l'immagine della DSVM include software, strumenti ed esempi preinstallati di supporto per i progetti di data science, apprendimento automatico e apprendimento profondo. Jupyter è installato nell'immagine, insieme a notebook di esempio. Avviare un server Jupyter Notebook nella VM e quindi connettersi in remoto al server dal computer locale. Per impostazione predefinita, il server notebook è in esecuzione sulla porta 8888. Per accedere al server in remoto, è necessario aprire la porta nella VM. 

1. Eseguire il comando seguente in Azure Cloud Shell, sostituendo `<HOSTNAME>` con il nome assegnato alla DSVM durante l'installazione.

    ```azurecli
    az vm open-port \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 8888 \
    --priority 901
    ```

Anche questa volta, il comando può richiedere fino a un minuto. Al termine, il comando restituisce una risposta JSON per la riga di comando. Verificare che il campo **"provisioningState"** abbia il valore *Succeeded*.  

## <a name="connect-to-the-vm-with-secure-shell-ssh"></a>Connettersi alla macchina virtuale con Secure Shell (SSH)

1. Eseguire il comando seguente in Azure Cloud Shell per individuare l'indirizzo IP pubblico della VM. Sostituire `<HOSTNAME>` con il nome assegnato alla DSVM durante l'installazione.

    ```azurecli
    az vm list-ip-addresses \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --output table
    ```

1. Eseguire il comando seguente in Cloud Shell per accedere alla VM. Sostituire `<USERNAME>` con il nome utente scelto all'inizio di questo esercizio. Sostituire `<IP>` con il valore della colonna **PublicIPAddresses** del passaggio precedente.

    Ad esempio, se il nome utente scelto è *azuser* e PublicIPAddresses ha un valore di 33.165.103.23, il comando sarà:
    
    `ssh azuser@33.165.103.23`
    
    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 

1. Quando richiesto, immettere la password per l'utente amministratore scelto all'inizio di questo esercizio. Una volta eseguito l'accesso, il prompt dovrebbe cambiare nel formato `username@hostname`, ad esempio `azuser@js1982`.

Il passaggio successivo è quello per avviare il server Jupyter Notebook nella VM e aprire un notebook in remoto.

## <a name="start-the-jupyter-notebook-server-on-the-vm"></a>Avviare il server Jupyter Notebook nella VM

È disponibile un set di notebook nella cartella `~/notebooks` della macchina virtuale. Presupponendo che si è ancora connessi tramite una sessione SSH, avviare il server notebook e aprire uno di questi notebook per verificare che tutto funzioni correttamente.


1. Al prompt dei comandi della VM eseguire questo comando:

    ```bash
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
    ```

> [!CAUTION]
> L'accesso al server notebook in questo esercizio avviene tramite `http://`. Per eseguire un server notebook in pubblico, è consigliabile proteggerlo. Per altre informazioni sulla protezione di un server notebook, vedere la documentazione ufficiale di Jupyter online. 

Nel comando precedente è stato avviato il server Jupyter Notebook con il comando `jupyter notebook`. Sono disponibili tre importanti argomenti della riga di comando. Tenere presente che è stato eseguito l'accesso al computer in remoto tramite una console. I notebook vengono gestiti in un browser. 

 - `--ip=0.0.0.0` Per impostazione predefinita, un server notebook viene eseguito in locale all'indirizzo 127.0.0.1:8888 ed è accessibile solo da localhost. È possibile accedere al server notebook dal browser in locale usando http://127.0.0.1:8888. Impostare l'indirizzo IP su 0.0.0.0 per indica al server di ascoltare il traffico su tutti gli indirizzi IP. Se il server notebook è in ascolto su 0.0.0.0, sarà raggiungibile attraverso l'indirizzo IP del computer host.  
 - `--no-browser`  Poiché si intende connettersi al notebook da un altro computer tramite internet, configurare il server notebook per impedire l'apertura del browser (comportamento predefinito). 
 - `--allow-root`  In questo esercizio è disponibile solo un account amministratore nella VM, pertanto si vuole essere in grado di eseguire i notebook come radice.

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a>Connettersi al server Jupyter Notebook da un browser remoto

Quando il comando riportato sopra viene eseguito nella macchina virtuale, il server notebook si avvia e la console mostra un URL da usare in un browser. 

![Screenshot del messaggio visualizzato nella console da un server notebook in esecuzione, contenente l'URL per accedere al server dal computer host](../media/notebook-url.png)

1. Copiare l'URL visualizzato dal server notebook nella barra degli indirizzi del browser preferito. È anche possibile fare clic sull'URL per l'apertura nel browser predefinito. 

    Si riceverà il messaggio "Impossibile raggiungere il sito" perché l'URL assegnato è la connessione al server notebook dal computer host.

1. Per accedere in remoto al server, sostituire il nome host nell'URL con l'indirizzo IP della macchina virtuale salvata in precedenza. 

    Di seguito è riportato un URL di un server notebook di esempio.

    "http://**ab-dsvm-4**:8888/?token={some token}"

    In questo caso si sostituirà **ab-dsvm-4** con l'indirizzo IP del computer. Se l'indirizzo IP è `52.175.199.43`, l'URL diventa:

    "http://**52.175.199.43**:8888/?token={some token}"

    Assicurarsi che l'indirizzo della porta `:8888` venga mantenuto nell'URL.

    > [!TIP]
    > Se non si intende usare l'indirizzo IP, è possibile usare anche il nome completo del server nel formato `<HOST NAME>.<REGION>.cloudapp.azure.com`

    Lo screenshot seguente mostra l'aspetto del dashboard Jupyter nel browser.

    ![Screenshot che mostra il dashboard dei Jupyter Notebook. ](../media/jupyter-in-browser.png)

1. Passare a **notebooks/IntroToJupyterPython.ipynb** e selezionarlo. Testare questo notebook per verificare che tutto funzioni come previsto.

    Congratulazioni. La macchina virtuale basata su DSVM è ora in esecuzione e può lavorare in remoto con Jupyter. In questo esercizio è stato eseguito il software installato nella macchina virtuale. Nel prossimo esercizio il software verrà isolato in un contenitore nella macchina virtuale in modo da potersi esercitare in tutta sicurezza.

4. Una volta terminato l'uso del notebook, sarà possibile arrestare il server Jupyter con `Control-C` in Cloud Shell.
