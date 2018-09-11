In questa unità si installerà **Azure PowerShell** nel computer locale. Scegliere la sezione appropriata per il sistema operativo.

## <a name="linux-and-mac"></a>Linux e Mac
In Linux e macOS, il primo passaggio consiste nell'installare **PowerShell Core**.

### <a name="linux"></a>Linux
Come indicato nell'ultima unità, l'installazione di PowerShell per Linux comporterà l'uso di uno strumento di gestione pacchetti. In questo esempio verrà usato **Ubuntu 18.04**, ma sono disponibili [istruzioni dettagliate per le altre versioni e distribuzioni nella documentazione](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

PowerShell Core verrà installato in Ubuntu Linux tramite Advanced Packaging Tool (**apt**) e la riga di comando di Bash. 

1. Importare la chiave di crittografia per il repository Ubuntu di Microsoft. Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto di PowerShell Core da installare sia fornito da Microsoft.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Registrare il **repository Ubuntu di Microsoft**, in modo da consentire allo strumento di gestione pacchetti di individuare il pacchetto di PowerShell Core.

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. Aggiornare l'elenco dei pacchetti.

    ```bash
    sudo apt-get update
    ```

1. Installare PowerShell Core.

    ```bash
    sudo apt-get install -y powershell
    ```

1. Avviare PowerShell per verificare che sia installato correttamente.

    ```bash
    pwsh
    ```

### <a name="macos"></a>macOS
Installare quindi **PowerShell Core** su macOS tramite la gestione pacchetti Homebrew.

> [!IMPORTANT]
> Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti Homebrew. Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).

1. Installare Homebrew-Cask per ottenere altri pacchetti, incluso il pacchetto di PowerShell Core:

    ```bash
    brew tap caskroom/cask
    ```

1. Installare PowerShell Core:

    ```bash
    brew cask installs powershell
    ```

1. Avviare PowerShell Core per verificare che sia installato correttamente:

    ```bash
    pwsh
    ```

## <a name="install-azure-powershell"></a>Installare Azure PowerShell
Dopo l'installazione del prodotto **PowerShell** di base, installare **Azure PowerShell** per aggiungere i comandi specifici di Azure.

### <a name="windows"></a>Windows
Installare Azure PowerShell in Windows usando il comando `Install-Module` PowerShell.

> [!IMPORTANT]
> Per installare Azure PowerShell, è necessario PowerShell versione 5.0 o superiore. Per controllare la versione di PowerShell, usare il comando seguente: 
>
> `$PSVersionTable.PSVersion` 
>
>Se il numero di versione è inferiore alla versione 5.0, seguire le istruzioni per l'[aggiornamento di Windows PowerShell esistente](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

1. Aprire il menu **Start** e digitare **Windows PowerShell**.

1. Fare clic con il pulsante destro del mouse sull'icona di **Windows PowerShell** e scegliere **Esegui come amministratore**.

1. Nella finestra di dialogo **Controllo account utente** selezionare **Sì**.

1. Digitare il comando seguente e quindi premere INVIO:

    ```powershell
    Install-Module -Name AzureRM
    ```

1. Se viene richiesto se considerare attendibili i moduli di PSGallery, rispondere **Sì** oppure **Sì a tutti**.

> [!TIP]
> Se viene visualizzato un messaggio di errore che indica che è già installata una versione del modulo Azure PowerShell, è possibile eseguire l'aggiornamento alla versione _più recente_ eseguendo il comando:
> 
> `Update-Module -Name AzureRM`
> 
> Come nel caso del comando `Install-Module`, rispondere **Sì** o **Sì a tutti** quando viene richiesto se considerare attendibile il modulo.

### <a name="linux-or-macos"></a>Linux o macOS
Per installare Azure PowerShell in Linux o macOS, viene usato lo stesso processo di base. La procedura è la stessa per entrambi i sistemi operativi. La differenza consiste nel metodo per l'avvio di una sessione di PowerShell Core con privilegi elevati.

1. In un terminale, digitare il comando seguente per avviare PowerShell Core con privilegi elevati.

    ```bash
    sudo pwsh
    ```

1. Per installare Azure PowerShell, eseguire il comando seguente nel prompt di PowerShell Core.

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. Se viene richiesto se considerare attendibili i moduli di **PSGallery**, rispondere **Sì** oppure **Sì a tutti**.

## <a name="summary"></a>Riepilogo
È stato configurato il computer locale per amministrare le risorse di Azure con Azure PowerShell. Ora è possibile usare Azure PowerShell in locale per immettere comandi o eseguire script. Azure PowerShell inoltrerà i comandi ai data center di Azure, in cui verranno eseguiti all'interno della sottoscrizione di Azure.