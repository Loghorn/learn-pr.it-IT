A questo punto la macchina virtuale Windows è stata distribuita ed è in esecuzione, ma non è configurata per eseguire alcuna operazione.

È importante ricordare che questo scenario è un sistema di elaborazione video. La piattaforma riceve i file tramite FTP. Le telecamere del traffico caricano i clip video in un URL noto di cui è stato eseguito il mapping a una cartella nel server. Il software personalizzato in ogni macchina virtuale Windows viene eseguito come servizio, controlla la cartella ed elabora ogni clip caricato. Passa quindi il video normalizzato agli algoritmi in esecuzione in altri servizi di Azure.

Sono necessarie alcune configurazioni per supportare questo scenario:

- Installare FTP e aprire le porte necessarie per comunicare.
- Installare il codec video proprietario univoco per il sistema di telecamere della città.
- Installare il servizio di transcodifica che elabora i video caricati.

Molte di queste sono tipiche attività amministrative che di fatto non verranno illustrate di seguito e quindi non richiedono software da installare. Verrà invece illustrato come è _possibile_ installare software personalizzato o di terze parti usando Desktop remoto e verranno esaminati i passaggi necessari. Si inizierà ottenendo le informazioni di connessione.

## <a name="connect-to-the-vm-with-remote-desktop-protocol"></a>Connettersi alla macchina virtuale con Remote Desktop Protocol

Per connettersi a una macchina virtuale di Azure con un client RDP, sono necessari:

- L'indirizzo IP pubblico della macchina virtuale (o privato se la macchina virtuale è configurata per connettersi alla rete).
- Il numero della porta.

È possibile immettere queste informazioni nel client RDP o scaricare un file **RDP** preconfigurato.

### <a name="download-the-rdp-file"></a>Scaricare il file RDP

1. Nel [portale di Azure](https://portal.azure.com?azure-portal=true) verificare che il pannello **Panoramica** per la macchina virtuale creata in precedenza sia aperto. La macchina virtuale è disponibile in **Tutte le risorse** se è necessario aprirla. Il pannello di panoramica presenta numerose informazioni sulla macchina virtuale.

    - È possibile vedere se la macchina virtuale è in esecuzione.
    - Arrestarla o riavviarla.
    - Ottenere l'indirizzo IP pubblico per connettersi alla macchina virtuale.
    - Visualizzare l'attività di CPU, disco e rete.

1. Fare clic sul pulsante **Connetti** nella parte superiore del riquadro.

1. Nel pannello **Connetti a macchina virtuale** prendere nota delle impostazioni **Indirizzo IP** e **Numero di porta**, quindi fare clic su **Scarica file RDP** e salvarlo nel computer.

1. Prima di connettersi, verranno modificate alcune impostazioni. In Windows trovare il file usando Esplora risorse, fare clic con il pulsante destro del mouse e scegliere **Modifica**. In MacOS sarà necessario aprire prima il file con il client RDP e quindi fare clic con il pulsante destro del mouse sull'elemento nell'elenco visualizzato e scegliere **Modifica**.

1. È possibile modificare diverse impostazioni per controllare l'esperienza di connessione alla macchina virtuale di Azure. Le impostazioni che è possibile esaminare sono:

    - **Visualizzazione**: Per impostazione predefinita sarà a schermo intero. È possibile sostituirla con una risoluzione inferiore o usare tutti i monitor se ne è disponibile più di uno.
    - **Risorse locali**: è possibile condividere le unità locali con la macchina virtuale per poter copiare i file dal PC alla macchina virtuale. Fare clic sul pulsante **Altro** in **Dispositivi e risorse locali** per selezionare gli elementi da condividere.
    - **Esperienza**: regolare l'esperienza visiva in base alla qualità della rete.

1. Condividere l'unità C: locale in modo che sia visibile alla macchina virtuale.

1. Tornare alla scheda **Generale** e fare clic su **Salva** per salvare le modifiche. È sempre possibile tornare indietro e modificare questo file in un secondo momento per provare altre impostazioni.

### <a name="connect-to-the-windows-vm"></a>Connettersi alla macchina virtuale Windows

1. Fare clic sul pulsante **Connetti** per avviare la connessione alla macchina virtuale.

1. Nella finestra di dialogo **Connessione Desktop remoto** prendere nota dell'avviso di sicurezza e dell'indirizzo IP del computer remoto, quindi fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** immettere il nome utente e la password usati nei passaggi 6 e 7.
    
    > [!NOTE]
    > Se si usa un client Windows per connettersi alla macchina virtuale, per impostazione predefinita verranno usate le identità note nel computer. È possibile fare clic sull'opzione **Altre opzioni** e selezionare "Usa un account diverso" per poter immettere una combinazione diversa di nome utente/password.
    
1. Nella seconda finestra di dialogo **Connessione Desktop remoto** prendere nota degli errori di certificato e fare clic su **Sì**.

### <a name="install-worker-roles"></a>Installare i ruoli di lavoro

La prima volta che ci si connette a una macchina virtuale Windows Server, verrà avviato Server Manager. In questo modo è possibile assegnare un ruolo di lavoro per le attività di dati o Web comuni. È anche possibile avviare Server Manager tramite il menu Start.

È a questo punto che si aggiungerebbe il ruolo del server Web al server. In questo modo verrebbe installato IIS e durante la configurazione si disattiverebbero le richieste HTTP e si abiliterebbe il server FTP. È anche possibile ignorare IIS e installare un server FTP di terze parti. Si configurerebbe quindi il server FTP per consentire l'accesso a una cartella nell'unità dei Big Data aggiunta alla macchina virtuale.

Poiché questo server non verrà effettivamente configurato qui, chiudere Server Manager.

## <a name="install-custom-software"></a>Installare software personalizzato

Esistono due approcci che è possibile usare per installare il software. In primo luogo, questa macchina virtuale viene connessa a Internet. Se il software necessario include un programma di installazione scaricabile, è possibile aprire un Web browser nella sessione RDP, scaricare il software e installarlo. In secondo luogo, se il software è personalizzato, come il servizio usato, è possibile copiarlo dal computer locale nella macchina virtuale per installarlo. Ora verrà esaminato questo secondo approccio.

1. Aprire Esplora file. Fare clic su **Questo PC** nella barra laterale. Verranno visualizzate più unità:

    - Unità Windows (C:) che rappresenta il sistema operativo.
    - Unità di archiviazione temporanea (D:).
    - L'unità C: locale, che avrà un nome diverso da quello visualizzato sotto.

    ![Unità locale condivisa con Azure](../media-drafts/6-drive-list.png)

Con l'accesso all'unità locale, è possibile copiare i file per il software personalizzato nella macchina virtuale e installare il software. Questa operazione non verrà effettivamente eseguita perché si tratta solo di uno scenario simulato, ma si può immaginare come funzionerebbe.

L'aspetto più interessante da osservare nell'elenco delle unità è cosa _manca_. Si noti che l'unità **dati** non è presente. Azure ha aggiunto un disco rigido virtuale, ma non l'ha inizializzato.

## <a name="initialize-data-disks"></a>Inizializzare i dischi dati

Tutte le unità aggiuntive create da zero dovranno essere inizializzate e formattate. Il processo per eseguire questa operazione è identico a quello valido per un'unità fisica.

1. Avviare lo strumento **Gestione disco** dal menu Start.

1. Verrà visualizzato un avviso che informa che è stato rilevato un disco non inizializzato.

    ![Inizializzare il disco dati nella macchina virtuale](../media-drafts/6-disk-management.png)

1. Fare clic su **OK** per inizializzare il disco. Verrà quindi visualizzato nell'elenco di volumi in cui è possibile formattarlo e assegnare una lettera di unità.

1. Aprire Esplora File. Ora l'unità dati è visualizzata.

1. Proseguire e chiudere il client RDP per uscire dalla macchina virtuale. Il server continuerà a venire eseguito.

RDP consente di lavorare con la macchina virtuale di Azure come con un computer locale. Con l'accesso all'interfaccia utente del desktop, è possibile amministrare questa macchina virtuale come qualsiasi computer Windows: installare software, configurare ruoli, mettere a punto le funzionalità ed eseguire altre attività comuni. Si tratta tuttavia di processi manuali. Se è necessario installare sempre alcuni software, valutare la possibilità di automatizzare il processo tramite scripting.