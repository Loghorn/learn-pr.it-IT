
In questo esercizio si installerà l'interfaccia della riga di comando di Azure nel computer locale e quindi si eseguirà un semplice comando per verificare l'installazione. 

## <a name="installing-the-azure-cli"></a>Installazione dell'interfaccia della riga di comando di Azure
Il metodo usato per l'installazione dell'interfaccia della riga di comando di Azure dipende dal sistema operativo del computer in uso. Scegliere la procedura corretta per il sistema operativo.

### <a name="linux"></a>Linux
L'interfaccia della riga di comando di Azure verrà installata in Ubuntu Linux tramite Advanced Packaging Tool (**apt**) e la riga di comando di Bash.

> [!NOTE]
> I comandi elencati di seguito sono per Ubuntu versione 18.04. Se si usa una versione diversa di Ubuntu, è necessario aggiungere un altro repository. Per informazioni dettagliate, vedere [Installare l'interfaccia della riga di comando di Azure 2.0 con APT](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).

1. Modificare l'elenco di origini in modo che il repository di Microsoft venga registrato e la gestione dei pacchetti possa individuare il pacchetto dell'interfaccia della riga di comando di Azure.

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```
1. Importare la chiave di crittografia per il repository Microsoft Ubuntu. Questo consente allo strumento di gestione pacchetti di verificare che il pacchetto dell'interfaccia della riga di comando di Azure da installare sia fornito da Microsoft.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```
1. Installare l'interfaccia della riga di comando di Azure.

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

### <a name="macos"></a>macOS
Di seguito si installerà l'interfaccia della riga di comando di Azure in macOS tramite la gestione pacchetti Homebrew.

> [!IMPORTANT]
> Se il comando **brew** non è disponibile, potrebbe essere necessario installare la gestione pacchetti HomeBrew. Per informazioni dettagliate, vedere il [sito Web di Homebrew](https://brew.sh/).

1. Aggiornare il repository brew per assicurarsi di ottenere il pacchetto più recente dell'interfaccia della riga di comando di Azure.

    ```bash
    brew update
    ```
1. Installare l'interfaccia della riga di comando di Azure.

    ```bash
    brew install azure-cli
    ```

### <a name="windows"></a>Windows
Di seguito si installerà l'interfaccia della riga di comando di Azure in Windows tramite il programma di installazione MSI.

1. Passare a [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) e nella finestra di dialogo della sicurezza del browser fare clic su **Esegui**.
1. Nel programma di installazione accettare le condizioni di licenza e quindi fare clic su **Installa**.
1. Nella finestra di dialogo **Controllo account utente** selezionare **Sì**.

## <a name="running-the-azure-cli"></a>Esecuzione dell'interfaccia della riga di comando di Azure
Per eseguire l'interfaccia della riga di comando di Azure, aprire una shell bash (Linux e macOS) o usare il prompt dei comandi o PowerShell (Windows).

1. Avviare l'interfaccia della riga di comando di Azure e verificare l'installazione eseguendo il controllo della versione.

    ```bash
    az --version
    ```

> [!NOTE]
> In Windows, l'esecuzione dell'interfaccia della riga di comando di Azure da PowerShell presenta alcuni vantaggi rispetto all'esecuzione dal prompt dei comandi. Ad esempio, PowerShell offre funzionalità di completamento con tasto TAB aggiuntive rispetto a quelle disponibili dal prompt dei comandi. 

## <a name="summary"></a>Summary
I computer locali sono stati configurati per amministrare le risorse di Azure con l'interfaccia della riga di comando di Azure. È ora possibile usare l'interfaccia della riga di comando di Azure in locale per immettere comandi o eseguire script. L'interfaccia della riga di comando di Azure inoltrerà i comandi ai data center di Azure dove verranno eseguiti all'interno della sottoscrizione di Azure.
