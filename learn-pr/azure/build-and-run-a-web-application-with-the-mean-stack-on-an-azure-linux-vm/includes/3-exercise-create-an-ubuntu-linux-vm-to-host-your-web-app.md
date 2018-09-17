Lo stack MEAN di componenti richiede un server. Può essere un computer Linux o una macchina virtuale in esecuzione nella sala server oppure può essere configurato in una macchina virtuale basata sul cloud. In questo modulo lo stack verrà configurato per l'esecuzione in una macchina virtuale Ubuntu Linux in esecuzione in Azure.

In questa unità si creerà una nuova macchina virtuale Ubuntu Linux ospitata in Azure. È anche possibile installare i componenti dello stack MEAN in una macchina virtuale esistente o in un computer host fisico. Creandone uno nuovo con questo esercizio, è possibile collegare tra loro tutti i componenti in un gruppo di risorse di Azure per semplificare la gestione e la pulizia dopo aver completato gli esercizi.

Per creare la macchina virtuale Linux, verrà usata la riga di comando di Cloud Shell integrata nel portale di Azure.

## <a name="provision-an-ubuntu-linux-vm"></a>Effettuare il provisioning di una macchina virtuale Ubuntu Linux

1. Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).
1. Aprire Cloud Shell dall'icona con la parentesi acuta (>_) nella barra degli strumenti del portale di Azure.
1. In Cloud Shell eseguire il comando per creare un gruppo di risorse di Azure, che includerà la macchina virtuale. Sostituire `<resource-group-name>` con il nome del proprio gruppo di risorse e `<resource-group-location>` con la località di Azure desiderata (ad esempio, `westus`).


    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    Ricordare il nome del gruppo di risorse perché verrà usato in altri comandi.

1. In Cloud Shell eseguire questo comando per creare una nuova macchina virtuale Ubuntu Linux. Sostituire `<resource-group-name>` con il nome del proprio gruppo di risorse e `<vm-admin-username>` e `<vm-admin-password>` con il nome utente e la password amministratore preferiti.

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
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
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    È consigliabile salvare anche l'indirizzo IP pubblico della VM appena creata per potersi connettere alla VM.

1. Provare a connettersi alla nuova macchina virtuale.

    Aprire un prompt dei comandi o una finestra del terminale ed eseguire il comando seguente. Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della VM precedenti.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    La prima volta che ci si connette al computer, verrà chiesto se si considera attendibile il computer remoto. Rispondendo `yes`, l'impronta digitale della chiave ECDSA del computer verrà salvata in locale, quindi le connessioni successive saranno considerate attendibili.

    Se è tutto corretto, digitare `exit` per chiudere la sessione SSH.

1. Aprire la porta 80 per consentire il traffico HTTP in ingresso nella nuova applicazione Web che verrà creata.

    Tornare a Cloud Shell nel portale di Azure. Eseguire il comando seguente usando il nome del gruppo di risorse originale per `<resource-group-name>`.

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    Il comando aprirà la porta HTTP nella macchina virtuale denominata "MeanDemo" al momento della creazione.

## <a name="summary"></a>Riepilogo

Quando la nuova VM Ubuntu Linux è pronta, è possibile connettersi a essa per avviare l'installazione dei diversi componenti dello stack MEAN.