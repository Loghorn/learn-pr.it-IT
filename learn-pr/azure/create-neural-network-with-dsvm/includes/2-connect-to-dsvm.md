### <a name="connect-to-the-data-science-vm"></a>Connettersi alla Data Science VM

In questa unità si stabilirà una connessione remota al desktop di Ubuntu nella macchina virtuale creata nell'esercizio precedente. A tale scopo è necessario un client che supporta [Xfce](https://xfce.org/), ovvero un ambiente desktop leggero per Linux. Per informazioni di base e per una panoramica dei diversi modi per connettersi a una DSVM, vedere [Come accedere alla macchina virtuale per l'analisi scientifica dei dati per Linux](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro#how-to-access-the-data-science-virtual-machine-for-linux).

1. Se non è già disponibile un client Xfce installato, scaricare il [client X2Go](https://wiki.x2go.org/doku.php/download:start) e installarlo prima di continuare con questo esercizio. X2Go è una soluzione Xfce gratuita e open source che funziona su una vasta gamma di sistemi operativi, tra cui Windows e OS X. Le istruzioni riportate in questa unità presuppongono l'uso di X2Go, ma è possibile usare qualsiasi client che supporta Xfce.

1. Tornare al gruppo di risorse **data-science-rg** nel portale di Azure. Fare clic sulla risorsa **data-science-vm** per aprirla nel portale.

    ![Apertura della Data Science VM](../media-draft/2-open-data-science-vm.png)

1. Passare il mouse sull'indirizzo IP visualizzato per la macchina virtuale e fare clic sul pulsante **Copy** (Copia) visualizzato per copiare l'indirizzo IP negli Appunti.

    ![Copia dell'indirizzo IP della macchina virtuale](../media-draft/2-copy-ip-address.png)

1. Avviare il client X2Go e connettersi alla Data Science VM usando l'indirizzo IP negli Appunti e il nome utente specificato nell'esercizio precedente. Connettersi tramite la porta **22** (la porta standard usata per le connessioni SSH) e specificare **XFCE** come tipo di sessione. Fare clic sul pulsante **OK** per confermare le preferenze.

    ![Connessione con X2Go](../media-draft/2-new-session-1.png)

1. Nel pannello "New session" (Nuova sessione) a destra selezionare la risoluzione che si vuole usare per il desktop remoto. Fare quindi clic su **New session** (Nuova sessione) nella parte superiore del pannello.

    ![Avvio di una nuova sessione](../media-draft/2-new-session-2.png)

1. Immettere la password specificata in [Esercizio 1](#Exercise1) e quindi fare clic sul pulsante **OK**. Se viene richiesto se si considera attendibile la chiave host, rispondere **Yes** (Sì). Ignorare inoltre eventuali messaggi di errore che segnalano che non è stato possibile avviare il daemon SSH.

    ![Accesso alla macchina virtuale](../media-draft/2-new-session-3.png)

1. Attendere che il desktop remoto vengano visualizzato e verificare che sia simile a quello riportato di seguito.

    > Se il testo e le icone sul desktop sono troppo grandi, terminare la sessione. Fare clic sull'icona nell'angolo in basso a destra del pannello "New Session" (Nuova sessione) e scegliere **Session preferences** (Preferenze della sessione) dal menu. Passare alla scheda "Input/Output" nella finestra di dialogo "New session" (Nuova sessione) e modificare il valore DPI dello schermo, quindi avviare una nuova sessione. Iniziare con 96 DPI e regolare in base alle esigenze.

    ![Connessione effettuata](../media-draft/2-ubuntu-desktop.png)

Ora che si è connessi, dedicare un po' di tempo a esplorare i collegamenti sul desktop. Si tratta dei collegamenti ai numerosi strumenti di Data Science preinstallati nella macchina virtuale, tra cui [Jupyter](http://jupyter.org/), [R Studio](https://www.rstudio.com/) e [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).