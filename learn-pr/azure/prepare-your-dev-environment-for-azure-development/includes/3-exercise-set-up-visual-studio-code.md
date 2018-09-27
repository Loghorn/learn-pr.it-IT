Per usare Visual Studio Code per lo sviluppo in Azure, è necessario installare Visual Studio Code in locale e una o più estensioni di Azure. In questo esercizio si aggiungerà l'estensione **Servizio app di Azure**.

## <a name="install-visual-studio-code"></a>Installare Visual Studio Code

::: zone pivot="windows"

### <a name="windows"></a>Windows

1. [Scaricare il programma di installazione di Visual Studio Code per Windows.](https://code.visualstudio.com/)

1. Eseguire il programma di installazione.

1. Aprire Visual Studio Code premendo il tasto Windows oppure facendo clic sull'icona di Windows nella barra delle applicazioni digitando "Visual Studio Code" e facendo clic sulla voce **Visual Studio Code** visualizzata.

::: zone-end

::: zone pivot="macos"

### <a name="macos"></a>macOS

1. [Scaricare Visual Studio Code per macOS](https://code.visualstudio.com/).

1. Fare doppio clic sull'archivio scaricato per espandere il contenuto.

1. Trascinare Code.app di Visual Studio nella cartella Applicazioni.

1. Aprire Visual Studio Code facendo clic sull'icona nella sezione App o eseguendo una ricerca di Visual Studio Code in Spotlight.

::: zone-end

::: zone pivot="linux"

### <a name="linux"></a>Linux 

#### <a name="debian-and-ubuntu"></a>Debian e Ubuntu

1. Scaricare e installare il [pacchetto DEB (64 bit)](https://go.microsoft.com/fwlink/?LinkID=760868) tramite un centro software con interfaccia grafica, se disponibile, o tramite la riga di comando (sostituendo `<file>` con il nome del file DEB scaricato):

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

#### <a name="rhel-fedora-and-centos"></a>RHEL, Fedora e CentOS

1. Usare lo script seguente per installare la chiave e il repository:

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. Aggiornare la cache dei pacchetti e installare il pacchetto usando dnf (Fedora 22 e versioni successive):

    ```bash
    dnf check-update
    sudo dnf install code
    ```

#### <a name="opensuse-and-sle"></a>openSUSE e SLE

1. Il repository yum funziona anche per sistemi basati su openSUSE e SLE. Lo script seguente installerà la chiave e il repository:

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. Aggiornare la cache dei pacchetti e installare il pacchetto con:

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> Per altri dettagli sull'installazione o l'aggiornamento di Visual Studio Code in varie distribuzioni Linux, vedere la [documentazione per l'esecuzione di Visual Studio Code in Linux](https://code.visualstudio.com/docs/setup/linux).

::: zone-end

## <a name="install-azure-app-service-extension"></a>Installare l'estensione Servizio app di Azure

1. Se non è già aperto, aprire Visual Studio Code.

1. Aprire il browser Estensioni. È accessibile tramite il menu a sinistra.

1. Cercare **Servizio app di Azure**.

1. Selezionare **Servizio app di Azure** tra i risultati e fare clic su **Installa**.

    Lo screenshot seguente illustra l'estensione di Servizio app di Azure selezionata nei risultati di ricerca delle estensioni in Visual Studio Code.

    ![Screenshot di Visual Studio Code che mostra la scheda Estensioni con l'estensione di Servizio app di Azure evidenziata nei risultati della ricerca.](../media/3-install-azure-extension.png)

Verrà così installata l'estensione. È ora possibile connettersi alla sottoscrizione di Azure e distribuire un'app Web, per dispositivi mobili o API in Servizio app di Azure.
