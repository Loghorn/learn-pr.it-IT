In questo esercizio verrà creata una rete virtuale in Microsoft Azure. Verranno quindi create due macchine virtuali e verrà usata la rete virtuale per connettere le macchine virtuali tra loro e a Internet.

Prima di iniziare l'esercizio, sarà necessario accedere ad [Azure Cloud Shell](https://shell.azure.com) con le credenziali della versione di valutazione gratuita. Azure Cloud Shell verrà usato per creare i gruppi di risorse, le reti virtuali e le macchine virtuali.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

1. Nella finestra **Benvenuto in Azure Cloud Shell** fare clic su **PowerShell (Linux)**.

1. Nella finestra **You have no storage mounted** (Nessuna risorsa di archiviazione montata) fare clic su **Create Storage** (Crea archiviazione).

1. Nel prompt \> di Azure PowerShell digitare il codice seguente e premere INVIO.

```PowerShell
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-network"></a>Creare una rete virtuale

1. Per creare una rete virtuale, immettere il comando seguente e premere INVIO.

```PowerShell
az network vnet create --name myVirtualNetwork --resource-group myResourceGroup --subnet-name default
```

## <a name="create-two-virtual-machines"></a>Creare due macchine virtuali

1. Per creare la prima macchina virtuale, eseguire il comando seguente per creare una VM Windows con un indirizzo IP pubblico accessibile tramite la porta 3389 (Desktop remoto):

    ``` PowerShell
    az vm create --name dataProcessingStage1 --resource-group MyResourceGroup --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. Fornire i valori per la password quando richiesto.

1. Eseguire questo comando per creare una VM Windows senza indirizzo IP pubblico:

    ```PowerShell
    az vm create -n dataProcessingStage2 -g MyResourceGroup --public-ip-address '' --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. Al termine, l'output del secondo comando restituirà un valore per publicIpAddress. 

## <a name="connect-to-dataprocessingstage1-using-remote-desktop"></a>Connettersi a dataProcessingStage1 mediante Desktop remoto

1. Nel computer client premere il tasto Windows e digitare RDP.

1. Assicurarsi che l'app **Connessione Desktop remoto** sia selezionata e quindi premere INVIO.

1. Nella finestra di dialogo **Desktop remoto** immettere nel campo **Computer** il valore di dataProcessingStage1PublicIPAddress, quindi fare clic su **Connetti**.

1. Nella finestra di dialogo **La connessione remota è attendibile?** fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati durante la creazione di dataProcessingStage1

1. Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **OK**.

1. Accedere al computer remoto in Azure.

1. Quando viene visualizzato il messaggio **Reti**, fare clic su **No**.

1. Chiudere Server Manager.

1. Nella sessione remota fare clic con il pulsante destro del mouse sul tasto Windows e quindi scegliere **Prompt dei comandi**.

1. Nella finestra del prompt dei comandi digitare questo comando e premere INVIO.

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. Non dovrebbe essere restituita alcuna risposta dal computer remoto. Windows Firewall impedisce infatti per impostazione predefinita le risposte ICMP.

## <a name="connect-to-dataprocessingstage2-using-remote-desktop"></a>Connettersi a dataProcessingStage2 mediante Desktop remoto

1. Nel computer client premere il tasto Windows e digitare **RDP**. Selezionare l'app **Connessione Desktop remoto** e premere INVIO.

1. Nel campo **Computer** immettere il valore di dataProcessingStage2PublicIPAddress, quindi fare clic su **Connetti**.

1. Nella finestra di dialogo **La connessione remota è attendibile?** fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati durante la creazione di dataProcessingStage2

1. Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **OK**. È ora possibile accedere al computer remoto in Azure.

1. Quando viene visualizzato il messaggio **Reti**, fare clic su **No**.

1. Chiudere Server Manager.

1. In dataProcessingStage2 premere il tasto Windows, quindi digitare **Firewall** e premere INVIO. Viene visualizzata la console di **Windows Firewall con sicurezza avanzata**.

1. Nel riquadro a sinistra fare clic su **Regole in ingresso**.

1. Nel riquadro a destra scorrere verso il basso e fare clic con il pulsante destro del mouse su **Condivisione file e stampanti (richiesta echo - ICMPv4-In)**, quindi scegliere **Abilita regola**.

1. Tornare alla console di dataProcessingStage1, quindi nella finestra del prompt dei comandi digitare questo comando e premere INVIO.

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. dataProcessingStage2 risponde con quattro risposte, dimostrando la connettività tra le due VM.

## <a name="summary"></a>Riepilogo

È stata creata una rete virtuale, sono state create due VM collegate a tale rete virtuale, è stata eseguita la connessione a una VM ed è stata dimostrata la connettività di rete all'altra VM nella stessa rete virtuale. È possibile usare le reti virtuali di Azure per connettere risorse entro la rete di Azure. Queste risorse devono essere tuttavia incluse nello stesso gruppo di risorse e nella stessa sottoscrizione. Nel prossimo esercizio verranno esaminati i gateway VPN, che consentono di connettere reti virtuali in gruppi di risorse, sottoscrizioni e addirittura aree geografiche diverse.
