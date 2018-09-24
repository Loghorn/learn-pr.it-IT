In questa unità si installerà **PowerShell** nel computer locale.

> [!NOTE]
> Questo esercizio illustra tutte le operazioni necessarie per installare gli strumenti di PowerShell in locale. Nella parte restante del modulo verrà usato Azure Cloud Shell in modo da sfruttare il supporto della sottoscrizione gratuita in Microsoft Learn. Se si preferisce, è possibile considerare questo esercizio come un'attività facoltativa e limitarsi a leggere le istruzioni.

::: zone pivot="linux"

## <a name="linux"></a>Linux

L'installazione di PowerShell per Linux comporterà l'uso di una gestione pacchetti. In questo esempio verrà usato **Ubuntu 18.04**, ma sono disponibili [istruzioni dettagliate per le altre versioni e distribuzioni nella documentazione](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

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
::: zone-end

::: zone pivot="macos"

## <a name="macos"></a>MacOS

In macOS, il primo passaggio consiste nell'installare **PowerShell Core**. Questa operazione viene eseguita tramite la gestione pacchetti Homebrew.

> [!IMPORTANT]
> Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti Homebrew. Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).

1. Installare Homebrew-Cask per ottenere altri pacchetti, incluso il pacchetto di PowerShell Core:

    ```bash
    brew tap caskroom/cask
    ```

1. Installare PowerShell Core:

    ```bash
    brew cask install powershell
    ```

1. Avviare PowerShell Core per verificare che sia installato correttamente:

    ```bash
    pwsh
    ```

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows
PowerShell è incluso in Windows, ma potrebbe essere disponibile un aggiornamento per il computer in uso. Per il supporto di Azure che verrà usato è richiesto PowerShell versione 5.0 o versioni successive. È possibile controllare la versione installata seguendo questa procedura:

1. Aprire il menu **Start** e digitare **Windows PowerShell**. Potrebbero essere disponibili più collegamenti:
    - Windows PowerShell - questa è la versione a 64 bit e a livello generale quella da scegliere.
    - Windows PowerShell (x86) - questa è una versione a 32 bit installata in Windows a 64 bit.
    - Windows PowerShell ISE - Integrated Scripting Environment (ISE) viene usato per la scrittura di script di PowerShell. 
    - Windows PowerShell ISE (x86) - Versione a 32 bit di ISE.

1. Selezionare l'icona di Windows PowerShell.

1. Digitare il comando seguente per determinare la versione di PowerShell installata.

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
Se il numero di versione è inferiore a 5.0, seguire queste istruzioni per l'[aggiornamento della versione esistente di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

::: zone-end

Il computer locale è stato configurato per il supporto di PowerShell. Verranno poi presentati altri comandi che è possibile aggiungere includendo il modulo di Azure.