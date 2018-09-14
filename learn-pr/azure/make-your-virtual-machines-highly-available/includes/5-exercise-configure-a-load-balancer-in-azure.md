Le risorse chiave è già stato creato per l'architettura di banca online. In questa esercitazione si completerà il programma di installazione per la configurazione delle impostazioni di sicurezza, un pool di indirizzi back-end, regole di integrità del probe e regole di bilanciamento del carico. Al termine dell'installazione, si testerà il bilanciamento del carico, rimuovere una macchina virtuale dal pool e verificare che le richieste client non vengono più inviate a questa macchina virtuale.

## <a name="inbound-rules"></a>Regole in ingresso

Creare una regola di sicurezza in ingresso per consentire le connessioni HTTP in ingresso che usano la porta 80.

1. Nel [portale di Azure](https://portal.azure.com/?azure-portal=true), nel menu a sinistra, fare clic su **tutte le risorse**. Quindi, nell'elenco di risorse, fare clic su **woodgrove-NSG**.

1. Nel **woodgrove-NSG** pannello, in **impostazioni**, fare clic su **regole di sicurezza in ingresso**e quindi fare clic su **Aggiungi**.

1. Nel **Aggiungi regola di sicurezza in ingresso** pannello, immettere le informazioni seguenti:
    - Origine: **Tag del servizio**
    - Tag di servizio di origine: **Internet**
    - Gli intervalli di porte di origine: **\***
    - Destinazione: **qualsiasi**
    - Gli intervalli di porte di destinazione: **80**
    - Protocollo: **TCP**
    - Azione: **consentire**
    - Priority: **100**
    - Nome: **woodgrove-HTTP-rule**

1. Fare clic su **Aggiungi**.

Creare una regola di sicurezza in ingresso per consentire le connessioni RDP in ingresso che usano la porta 3389.

1. Nel **woodgrove-NSG - regole di sicurezza in ingresso** pannello, fare clic su **Add**.

1. Nel **Aggiungi regola di sicurezza in ingresso** pannello, immettere le informazioni seguenti:
    - Origine: **Tag del servizio**
    - Tag di servizio di origine: **Internet**
    - Gli intervalli di porte di origine: **\***
    - Destinazione: **qualsiasi**
    - Gli intervalli di porte di destinazione: **3389**
    - Protocollo: **TCP**
    - Azione: **consentire**
    - Priority: **200**
    - Nome: **woodgrove-RDP-rule**

1. Fare clic su **Aggiungi**.

## <a name="prepare-a-script-to-install-and-configure-iis-on-the-vms"></a>Preparare uno script per installare e configurare IIS nelle macchine virtuali

1. Copiare i comandi di PowerShell seguenti in un editor di testo:

    ```powershell
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # remove default htm file
    remove-item  C:\inetpub\wwwroot\iisstart.htm

    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Web page from <b>" + $env:computername + "</b>")
    ```

1. Salvare il file in locale come **ConfigureIIS.ps1**.

### <a name="install-and-configure-iis-on-vms"></a>Installare e configurare IIS nelle macchine virtuali

1. Nel portale di Azure, nel menu a sinistra, fare clic su **tutte le risorse**. Quindi, nell'elenco di risorse, fare clic su **woodgrove SVR01**.

1. Nella pagina Panoramica fare clic su **Connect**.

1. Nel **Connetti a macchina virtuale** pannello, fare clic su **Scarica File RDP**. Quindi, nella finestra popup, fare clic su **aperto**.

1. Nella finestra **Connessione Desktop remoto** fare clic su **Connetti**.

1. Nel **sicurezza di Windows** della finestra di dialogo fare clic su **altre scelte**e quindi fare clic su **Usa un account diverso**.

1. Nel **sicurezza di Windows** della finestra di dialogo immettere le credenziali usate durante il provisioning di macchine virtuali e quindi fare clic su **OK**.

1. Se si verifica una **connessione Desktop remoto** avviso del certificato, fare clic su **Yes**.

1. Nel computer locale, aprire la cartella che contiene lo script.

1. Copia **ConfigureIIS.ps1** negli Appunti.

1. Passare a una sessione RDP. Quindi aprire l'Explorer di Windows e incollare **ConfigureIIS.ps1** al **c:\\**.

1. Nella sessione RDP, fare clic su **avviare**, quindi aprire **Windows PowerShell**.

1. Al prompt di PowerShell, digitare il comando seguente e quindi premere **invio:**

    ```powershell
    cd \
    ```

1. Al prompt di PowerShell, digitare il comando seguente e quindi premere **invio:**

    ```powershell
    .\ConfigureIIS.ps1
    ```

1. Chiudere la connessione RDP.

1. Ripetere i passaggi da 1 a 13 per **woodgrove SVR02** e **woodgrove SVR03**.

## <a name="create-a-back-end-address-pool"></a>Creare un pool di indirizzi back-end

Successivamente, usare il portale per creare un pool di indirizzi back-end per il servizio di bilanciamento del carico pubblico.

1. Nel portale di Azure, nel menu a sinistra, fare clic su **tutte le risorse**. Quindi, nell'elenco di risorse, fare clic su **woodgrove-LB**.

1. Nel **woodgrove-LB** pannello, in **impostazioni**, fare clic su **pool back-end**e quindi fare clic su **Aggiungi**.

1. Nel **Aggiungi pool back-end** pannello, aggiungere le informazioni seguenti:
    - Nome: **woodgrove BEP**
    - Associato a: selezionare **set di disponibilità**
    - Set di disponibilità: **woodgrove-AS**

1. Fare clic su **aggiungere una configurazione IP di rete di destinazione**. Quindi, nella **macchina virtuale di destinazione** elenco, selezionare **woodgrove SVR01**. Nel **configurazione IP di rete** selezionare pool di indirizzi IP della macchina virtuale.

1. Fare clic su **aggiungere una configurazione IP di rete di destinazione**. Quindi, nella **macchina virtuale di destinazione** elenco, selezionare **woodgrove SVR02**e nel **configurazione IP di rete** selezionare pool di indirizzi IP della macchina virtuale.

1. Fare clic su **aggiungere una configurazione IP di rete di destinazione**. Quindi, nella **macchina virtuale di destinazione** elenco, selezionare **woodgrove SVR03**e nel **configurazione IP di rete** selezionare pool di indirizzi IP della macchina virtuale.

1. Nel **Aggiungi pool back-end** pannello, fare clic su **OK**.

1. Attendere che la configurazione di bilanciamento del carico è aggiornato prima di procedere con l'esercizio.

## <a name="create-a-health-probe-for-the-load-balancer"></a>Creare un probe di integrità per il bilanciamento del carico

Successivamente, aggiungere un probe di integrità per il protocollo HTTP sulla porta 80.

1. Nel **woodgrovelb** pannello, in **impostazioni**, fare clic su **i probe di integrità**e quindi fare clic su **Aggiungi**.

1. Nel **Aggiungi probe integrità** pannello, aggiungere le informazioni seguenti:
    - Name: **woodgrove-HP**
    - Protocollo: **HTTP**
    - Porta: **80**
    - Path: **/**
    - Intervallo: **15**
    - Soglia non integra: **2**

1. Nel **Aggiungi probe integrità** pannello, fare clic su **OK**.

1. Attendere che la configurazione di bilanciamento del carico è aggiornato prima di procedere con l'esercizio.

## <a name="create-a-load-balancer-rule"></a>Creare una regola di bilanciamento del carico

Infine, creare una regola con bilanciamento del carico per il protocollo HTTP sulla porta 80, che associa il pool back-end con il probe di integrità.

1. Nel **woodgrove-LB** pannello, in **impostazioni**, fare clic su **regole di bilanciamento del carico**e quindi fare clic su **Aggiungi**.

1. Nel **Aggiungi regola di bilanciamento del carico** pannello nella **nome** , digitare **woodgrove-HTTP-LBRule**. Verificare quindi che le informazioni seguenti sono stati immessi automaticamente:
    - Versione indirizzo IP: **IPv4**
    - Indirizzo IP Frontend: **LoadBalancerFrontEnd**
    - Protocollo: **TCP**
    - Porta: **80**
    - Porta back-end: **80**
    - Pool back-end: **woodgrove BEP**
    - Probe di integrità: **woodgrove-HP**
    - Persistenza della sessione: **None**
    - Timeout di inattività: **4 minuti**
    - L'indirizzo IP mobile: **disabilitato**

1. Nel **Aggiungi regola di bilanciamento del carico** pannello, fare clic su **OK**.

1. Attendere che la configurazione di bilanciamento del carico è aggiornato prima di procedere con l'esercizio.

## <a name="test-the-load-balancer"></a>Testare il servizio di bilanciamento del carico

1. Nel portale di Azure, nel menu a sinistra, fare clic su **tutte le risorse**, quindi nell'elenco di risorse, fare clic su **woodgrove-LB-ip**.

1. Nel **Overview** pannello, seleziona il **indirizzo IP**e quindi fare clic sul **copia** pulsante.

1. In una nuova scheda del browser, incollare l'indirizzo IP nella barra degli indirizzi del browser. Prendere nota del nome del server e mantenere aperta questa scheda.

1. Nel portale di Azure, nel menu a sinistra, fare clic su **tutte le risorse**. Quindi, nell'elenco di risorse, fare clic su server annotato in precedenza.

1. Nel **Overview** pannello, fare clic su **arrestare**e quindi fare clic su **Sì**.

1. Attendere fino a quando la macchina virtuale è stato arrestato e quindi passare alla scheda visualizzato al passaggio 3. Aggiornare la pagina.

1. Il servizio di bilanciamento del carico ora invia la richiesta HTTP a una delle altre macchine virtuali.

In questo esercizio è completata la distribuzione di macchine virtuali back-end e il bilanciamento del carico.
