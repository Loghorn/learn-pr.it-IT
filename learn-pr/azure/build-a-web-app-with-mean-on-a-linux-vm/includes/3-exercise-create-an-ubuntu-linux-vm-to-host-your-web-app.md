Lo stack MEAN di componenti richiede un server. Può essere un computer Linux o una macchina virtuale in esecuzione nella sala server oppure può essere configurato in una macchina virtuale basata sul cloud. In questo modulo lo stack verrà configurato per l'esecuzione in una macchina virtuale Ubuntu Linux in esecuzione in Azure.

In questa unità si creerà una nuova macchina virtuale Ubuntu Linux ospitata in Azure. È anche possibile installare i componenti dello stack MEAN in una macchina virtuale esistente o in un computer host fisico. Creandone uno nuovo con questo esercizio, è possibile collegare tra loro tutti i componenti in un gruppo di risorse di Azure per semplificare la gestione e la pulizia dopo aver completato gli esercizi.

## <a name="provision-an-ubuntu-linux-vm"></a>Effettuare il provisioning di una macchina virtuale Ubuntu Linux

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="creating-a-resource-group"></a>Creazione di un gruppo di risorse

In genere, la prima cosa che occorre fare quando si crea un nuovo set di risorse è creare un _gruppo di risorse_ per contenerle tutte. Questo è un passaggio non necessario nell'ambiente sandbox di Azure, ma quando si lavora nella propria sottoscrizione usare il comando seguente per creare un gruppo di risorse in una località nelle vicinanze.

```azurecli
az group create --name <resource-group-name> --location <resource-group-location>
```

> [!IMPORTANT]
> Non si deve creare un gruppo di risorse quando si usa l'ambiente sandox di Azure. Usare invece il gruppo di risorse creato in precedenza denominato **<rgn>[gruppo di risorse sandbox]</rgn>**.

1. In Cloud Shell a destra, digitare il comando seguente per creare una nuova macchina virtuale Ubuntu Linux. Sostituire `<vm-admin-username>` e `<vm-admin-password>` con il nome utente e la password preferiti dell'amministratore.

    ```azurecli
    az vm create \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    Prendere nota del nome utente e della password amministratore per potersi connettere a questa macchina virtuale in un secondo momento.

    Il completamento di questo comando richiede circa 2 minuti. Al termine del comando, l'output risultante sarà simile al seguente.

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[Sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    È consigliabile salvare anche l'indirizzo IP pubblico della VM appena creata per potersi connettere alla VM.

1. Provare a connettersi alla nuova macchina virtuale.

    In Cloud Shell eseguire il comando seguente. Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della VM precedenti.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    La prima volta che ci si connette al computer, verrà chiesto se si considera attendibile il computer remoto. Rispondendo `yes`, l'impronta digitale della chiave ECDSA del computer verrà salvata in locale, quindi le connessioni successive saranno considerate attendibili. Verrà quindi richiesta la password, che verrà visualizzata ogni volta che ci si connette.

    Se è tutto corretto, digitare `exit` per chiudere la sessione SSH.

1. Aprire la porta 80 nella macchina virtuale per consentire il traffico HTTP in ingresso verso la nuova applicazione Web che verrà creata.

    ``` bash
    az vm open-port --port 80 --resource-group <rgn>[Sandbox resource group name]</rgn> --name MeanDemo
    ```

    Il comando aprirà la porta HTTP nella macchina virtuale denominata "MeanDemo" al momento della creazione.

## <a name="summary"></a>Riepilogo

Quando la nuova VM Ubuntu Linux è pronta, è possibile connettersi a essa per avviare l'installazione dei diversi componenti dello stack MEAN.