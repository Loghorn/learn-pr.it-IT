In questa unità, verrà installato **PowerShell** nel computer locale.

> [!NOTE]
> Questa esercitazione descrive l'installazione gli strumenti di PowerShell in locale. Nella parte restante del modulo verrà usato Azure Cloud Shell in modo da sfruttare il supporto gratuito per la sottoscrizione in Microsoft Learn. Se si preferisce, è possibile considerare questo esercizio come un'attività facoltativa e limitarsi a leggere le istruzioni.

::: zone pivot="linux"

## <a name="linux"></a>Linux

Installazione di PowerShell per Linux comporterà l'uso di gestione pacchetti. In questo esempio verrà usato **Ubuntu 18.04**, ma sono disponibili [istruzioni dettagliate per le altre versioni e distribuzioni nella documentazione](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

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

In macOS, il primo passaggio consiste nell'installare **PowerShell Core**. Questa operazione viene eseguita tramite Gestione pacchetti Homebrew.

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

::: zone-end

::: zone pivot="windows"

## <a name="windows"></a>Windows
PowerShell è incluso in Windows, potrebbe essere tuttavia presenti un aggiornamento disponibile per la macchina. Il supporto tecnico di Azure che si intende usare richiede PowerShell versione 5.0 o versioni successive. È possibile controllare la versione installata seguendo questa procedura:

1. Aprire il menu **Start** e digitare **Windows PowerShell**. Potrebbero essere presenti più collegamenti di scelta rapida disponibili:
    - Windows PowerShell - questa è la versione a 64 bit e a livello generale ciò che è consigliabile scegliere.
    - Windows PowerShell (x86)-Questa è una versione a 32 bit installata in Windows a 64 bit.
    - Windows PowerShell ISE - Integrated Scripting Environment (ISE) viene usato per la scrittura di script di PowerShell. 
    - Windows PowerShell ISE (x86)-versione A 32 bit di ISE.

1. Selezionare l'icona di Windows PowerShell.

1. Digitare il comando seguente per determinare la versione di PowerShell installata.

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
Se il numero di versione è inferiore alla versione 5.0, seguire queste istruzioni per la [aggiornamento di PowerShell di Windows esistente](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

::: zone-end

## <a name="summary"></a>Riepilogo
Aver configurato il computer locale per il supporto di PowerShell. Successivamente, si discuterà comandi aggiuntivi, che è possibile aggiungere tra cui il modulo di Azure.