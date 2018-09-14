In questo esercizio si creerà una rete virtuale in Microsoft Azure. Verranno quindi create due macchine virtuali e verrà usata la rete virtuale per connettere le macchine virtuali tra loro e a Internet.

Prima di iniziare questa unità, sarà necessario accedere al [Azure Cloud Shell](https://shell.azure.com) con le credenziali della sottoscrizione di valutazione. Si userà il comando di Azure tramite Azure Cloud Shell per creare i gruppi di risorse, le reti virtuali e macchine virtuali.

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

1. Nel **benvenuto in Azure Cloud Shell** finestra, fare clic su **Bash (Linux)**.

1. Nella finestra **You have no storage mounted** (Nessuna risorsa di archiviazione montata) fare clic su **Create Storage** (Crea archiviazione).

1. Nel prompt della riga di comando di Azure PowerShell, digitare il codice seguente e premere INVIO. Sostituire il `<myResourceGroup>` valore con un nome descrittivo per renderlo semplice da ricordare quando pulire tutte le risorse create in un secondo momento. Si userà questo nome mediante il ripristino di questa esercitazione.

    ```azurecli
    az group create --name <myResourceGroup> --location eastus
    ```

## <a name="create-a-virtual-network"></a>Creare una rete virtuale

1. Per creare una rete virtuale, immettere il comando seguente e premere INVIO. Sostituire il `<myVirtualNetwork>` valore con un nome descrittivo per renderlo semplice da ricordare

    ```azurecli
    az network vnet create --name <myVirtualNetwork> --resource-group <myResourceGroup> --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a>Creare due macchine virtuali

1. Per creare la prima macchina virtuale, eseguire il comando seguente per creare una VM Windows con un indirizzo IP pubblico che è accessibile tramite la porta 3389 (Desktop remoto). Nome della macchina virtuale `dataProcStage1`:

    ```azurecli
    az vm create --name dataProcStage1 --resource-group <myResourceGroup> --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. Fornire i valori per la password quando richiesto. Ricordarsi di annotare la password perché sarà necessaria in seguito per accedere al server.

1. A questo punto si creerà la seconda macchina virtuale. Questa macchina virtuale non avrà un indirizzo IP pubblico. Eseguire il comando seguente per creare una VM Windows **senza** un indirizzo IP pubblico usando una stringa vuota. Nome della macchina virtuale `dataProcStage2`:

    ```azurecli
    az vm create -n dataProcStage2 -g <myResourceGroup> --public-ip-address "" --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. Al termine, l'output del secondo comando restituirà un valore per publicIpAddress.  

## <a name="connect-to-dataprocstage1-using-remote-desktop"></a>Connettersi a dataProcStage1 tramite Desktop remoto

1. Nel computer client premere il tasto Windows e digitare RDP.

1. Assicurarsi che **connessione Desktop remoto** app sia selezionata e quindi premere INVIO.

1. Nel **connessione Desktop remoto** nella finestra di dialogo il **Computer** immettere il valore di `dataProcStage1`dell'indirizzo IP pubblico e quindi fare clic su **Connetti**.
    
    Istruzioni per ottenere l'indirizzo ip remoto se non è stato scritto verso il basso

1. Nella finestra di dialogo **La connessione remota è attendibile?** fare clic su **Connetti**.

1. Nel **sicurezza di Windows** finestra di dialogo immettere il nome utente e password usati durante la creazione `dataProcStage1`. 

    È necessario modificare i profili qui

1. Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **OK**.

1. Accedere al computer remoto in Azure.

1. Quando viene visualizzato il messaggio **Reti**, fare clic su **No**.

1. Chiudere Server Manager.

1. Nella sessione remota, fare doppio clic il tasto Windows e fare clic su **prompt dei comandi**.

1. Nella finestra del prompt dei comandi digitare questo comando e premere INVIO.

    ```cmd
    ping dataProcStage2 -4
    ```

1. Non vi sarà alcuna risposta dalla `dataProcStage2`. Infatti, per impostazione predefinita, Windows Firewall impedisce risposte ICMP in `dataProcStage2`.

## <a name="connect-to-dataprocstage2-using-remote-desktop"></a>Connettersi a dataProcStage2 tramite Desktop remoto

Si configurerà il Firewall di Windows su `dataProcStage2` usando un nuovo seesion di desktop remoto. Tuttavia, sarà non è possibile accedere a `dataProcStage2` dal desktop. È importante ricordare, `dataProcStage2` non dispone di un indirizzo IP pubblico. Si apprenderà come tramite desktop remoto dal `dataProcStage1` per connettersi a `dataProcStage2`.

1. Sul `dataProcStage1`, premere il tasto Windows e il tipo **RDP**. Selezionare l'app **Connessione Desktop remoto** e premere INVIO.

1. Nel **Computer** immettere `dataProcStage2`, quindi fare clic su **Connect**. In base alla configurazione di rete predefinito`dataProcStage1` è in grado di risolvere l'indirizzo per `dataProcStage2` usando il nome del computer.

1. Nella finestra di dialogo **La connessione remota è attendibile?** fare clic su **Connetti**.

1. Nel **sicurezza di Windows** finestra di dialogo immettere il nome utente e password usati durante la creazione `dataProcStage2`.

1. Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **OK**. È ora possibile accedere al computer remoto in Azure.

1. Quando viene visualizzato il messaggio **Reti**, fare clic su **No**.

1. Chiudere Server Manager.

1. Sul `dataProcStage2`, premere il tipo di chiave, Windows **Firewall**, quindi premere INVIO. Viene visualizzata la console di **Windows Firewall con sicurezza avanzata**.

1. Nel riquadro a sinistra fare clic su **Regole in ingresso**.

1. Nel riquadro di destra, scorrere verso il basso e fare doppio clic su **condivisione File e stampanti (richiesta Echo - ICMPv4-In)**, quindi fare clic su **Abilita regola**.

1. Tornare al `dataProcStage1` console e nella finestra del prompt dei comandi, digitare il comando seguente e premere INVIO.

    ```cmd
    ping dataProcStage2 -4
    ```

1. `dataProcStage2` risponde con quattro risposte, dimostrando la connettività tra le due macchine virtuali.

## <a name="summary"></a>Riepilogo

Avere creato una rete virtuale, creata due macchine virtuali che sono collegate alla rete virtuale, connessa a una delle VM e illustrate la connettività di rete per l'altra VM nella stessa rete virtuale. È possibile usare rete virtuale di Azure per connettere le risorse all'interno della rete di Azure. Queste risorse devono essere tuttavia incluse nello stesso gruppo di risorse e nella stessa sottoscrizione. Successivamente, si esaminerà i gateway VPN, che consentono di connettere la rete virtuale in diversi gruppi di risorse, sottoscrizioni e anche aree geografiche.
