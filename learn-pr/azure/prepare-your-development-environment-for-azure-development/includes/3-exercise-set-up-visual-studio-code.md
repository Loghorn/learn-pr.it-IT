In questo esercizio verranno installati Visual Studio Code e l'estensione del Servizio app di Azure, in modo da essere pronti per lo sviluppo per Microsoft Azure e per distribuire un'app Web.

## <a name="exercise-steps"></a>Passaggi dell'esercizio

Prima di tutto identificare il sistema operativo in uso e seguire i passaggi nella sezione appropriata di seguito per installare Visual Studio Code.

### <a name="windows"></a>Windows

1. Scaricare il programma di installazione di Visual Studio Code per Windows.
2. Eseguire il programma di installazione. L'operazione non richiederà molto tempo.
3. Aprire Visual Studio Code passando alla cartella di installazione (il percorso predefinito è C:\Programmi\Microsoft VS Code per un computer a 64 bit).

### <a name="macos"></a>macOS

1. Scaricare Visual Studio Code per macOS.
2. Fare doppio clic sull'archivio scaricato per espandere il contenuto.
3. Trascinare Visual Studio Code.app nella cartella Applicazioni, rendendolo disponibile in Launchpad.
4. Aggiungere Visual Studio Code a Dock facendo clic con il pulsante destro del mouse sull'icona e scegliendo Opzioni > Mantieni nel Dock.

### <a name="linux--debian-and-ubuntu"></a>Linux - Debian e Ubuntu

1. Scaricare e installare il [pacchetto DEB (64 bit)](https://go.microsoft.com/fwlink/?LinkID=760868), tramite un centro software con interfaccia grafica, se disponibile, o tramite la riga di comando (sostituendo `<file>` con il nome del file DEB scaricato):

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

### <a name="linux--rhel-fedora-and-centos"></a>Linux - RHEL, Fedora e CentOS

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

### <a name="linux--opensuse-and-sle"></a>Linux - openSUSE e SLE

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

## <a name="install-azure-app-service-extension"></a>Installare l'estensione di Servizio app di Azure

Dopo aver installato Visual Studio Code, aprirlo.

1. Passare alla scheda Estensioni.
2. Cercare Servizio app di Azure.
3. Fare clic su Installa.

    Lo screenshot seguente illustra l'estensione di Servizio app di Azure selezionata nei risultati di ricerca delle estensioni in Visual Studio Code.

    ![Screenshot di Visual Studio Code che mostra la scheda Estensioni con l'estensione di Servizio app di Azure evidenziata nei risultati della ricerca.](../media/3-install-azure-extension.png)

Verrà così installata l'estensione. Sarà poi possibile connettersi alla sottoscrizione di Azure e sviluppare e distribuire un'app Web, per dispositivi mobili o API in Servizio app di Azure.
