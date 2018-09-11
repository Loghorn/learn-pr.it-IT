In questo esercizio verrà installato Eclipse nel computer locale e quindi si installerà Azure Toolkit, in preparazione per lo sviluppo di applicazioni Java con l'integrazione di Azure. L'installazione è rapida e semplice. Al termine dell'esercizio, sarà pronto tutto quello che serve per avviare la prima applicazione Java, sfruttando le funzionalità e i servizi di Azure.

## <a name="install-eclipse-ide"></a>Installare l'IDE di Eclipse

1. Scaricare la versione di Eclipse appropriata per il sistema operativo da http://www.eclipse.org/downloads/packages/installer.

1. Avviare il programma di installazione di Eclipse dopo averlo scaricato.

    1. In Windows, fare doppio clic sul file scaricato.

    1. In macOS e Linux, decomprimere il programma di installazione dal file scaricato. Avviare quindi il programma di installazione dopo averlo decompresso.

        > [!NOTE]
        > Il programma di installazione potrebbe richiedere di installare Java Development Kit, se risulta mancante.

1. Selezionare i pacchetti da installare. Per gli sviluppatori Java, scegliere l'opzione per l'IDE di Eclipse Java o Jave EE.

1. Selezionare la destinazione di installazione nel computer.

1. Avviare Eclipse per verificare che sia stato installato correttamente.

## <a name="install-azure-toolkit-for-eclipse"></a>Installare Azure Toolkit for Eclipse

La procedura per l'installazione di Azure Toolkit è identica in Windows, macOS e Linux.

1. Avviare Eclipse.

1. Passare a **Help** > **Install New Software** (Guida, Installa nuovo software).

    Lo screenshot seguente mostra la posizione nel menu della voce **Install New Software** (Installa nuovo software).

    ![Screenshot dell'opzione Install New Software (Installa nuovo software) evidenziata all'interno del menu Help (Guida) di Eclipse.](../media/7-eclipse-install-new-software.png)

1. Verrà aperta la finestra di dialogo **Available Software** (Software disponibile). Nella casella di testo **Work with** (Usa) immettere `http://dl.microsoft.com/eclipse/` e premere INVIO.

1. Nei risultati selezionare l'opzione **Azure Toolkit for Java**. Assicurarsi di deselezionare l'opzione **Contact all update sites during install to find required software** (Contattare tutti i siti di aggiornamento durante l'installazione per trovare il software richiesto).

    La schermata seguente mostra la configurazione di installazione **Available Software** (Software disponibile) sopra descritta.

    ![Screenshot della finestra Available Software (Software disponibile) in Eclipse, con riquadri che evidenziano la configurazione necessaria per trovare e installare Azure Toolkit for Java.](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. Fare clic su **Avanti**.

1. Rivedere e accettare i contratti di licenza quando richiesto e fare clic su **Fine**.

1. Eclipse scarica e installa Azure Toolkit.

1. Se richiesto, riavviare Eclipse.

1. Convalidare l'installazione di Azure Toolkit verificando che sia disponibile la voce di menu **Tools** > **Azure** (Strumenti, Azure) in Eclipse.

## <a name="summary"></a>Riepilogo

In questa unità è stato installato Eclipse per Java e il programma è stato preparato per sfruttare i vantaggi dell'integrazione con i prodotti e i servizi Azure. L'installazione è rapida e semplice e ciò rende Eclipse uno strumento ideale per l'attività di sviluppo Java con l'integrazione di servizi cloud.
