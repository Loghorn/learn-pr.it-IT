In questo esercizio si userà il portale di Azure per creare un set di scalabilità di macchine virtuali con regole per la scalabilità automatica.

## <a name="create-a-virtual-machine-scale-set"></a>Creare un set di scalabilità di macchine virtuali

1. Nel [portale di Azure](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) fare clic su **Crea una risorsa**.

1. Nella casella di ricerca digitare **set di scalabilità** e premere <kbd>INVIO</kbd>. Nel pannello **Risultati** fare clic su **Set di scalabilità di macchine virtuali**, quindi nel pannello **Set di scalabilità di macchine virtuali** fare clic su **Crea**.

1. Nel pannello **Crea set di scalabilità di macchine virtuali** immettere i valori seguenti.
    - Usare _WebServerSS_ per **Nome**.
    - Lasciare _Windows Server 2016 Datacenter_ per **Immagine del disco del sistema operativo**.
    - Usare _Concierge Subscription_ (Sottoscrizione Concierge) per il campo **Sottoscrizione**.
    - Selezionare **<rgn>[nome del gruppo di risorse sandbox]</rgn>** per **Gruppo di risorse**.
    - Usare una posizione inclusa nell'elenco seguente: [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

    - Immettere un **Nome utente** e una **Password** validi. Prendere nota di questi dati per usarli in seguito.
    - Impostare **Numero di istanze** su _2_.
    - Impostare **Dimensioni istanza** su _D2s_v3_.
    - Lasciare **Scalabilità automatica** come _Disabilitata_.
    - Selezionare **Bilanciamento del carico**.
    - Immettere _WebServerPubIP_ in **Nome indirizzo IP pubblico**.
    - Usare le proprie iniziali, la data e l'ora per **Etichetta del nome di dominio** per ottenere un valore univoco.
    - In **Rete virtuale**, selezionare **Crea nuovo**. Assegnare il nome **SSVNet** e fare clic su **Crea** per creare la rete virtuale.

1. Fare clic su **Crea** per distribuire il set di scalabilità.

1. Attendere che il set di scalabilità venga creato.

## <a name="create-and-apply-autoscale-rules"></a>Creare e applicare regole di scalabilità automatica

1. Nella barra laterale sinistra fare clic su **Gruppi di risorse**.

1. Selezionare il gruppo di risorse **<rgn>[nome gruppo di risorse sandbox]</rgn>**.

1. Nel pannello **<rgn>[nome gruppo di risorse sandbox]</rgn>** fare clic sull'oggetto **WebServerSS** per aprire il set di scalabilità.

1. Nel pannello **WebServerSS** in **Impostazioni** fare clic su **Istanze**. Si noti la presenza delle due istanze di macchina virtuale in esecuzione nel set di scalabilità.

1. Nel pannello **WebServerSS** fare clic su **Scalabilità**.

1. Nel pannello **WebServerSS - Scalabilità** fare clic su **Abilita scalabilità automatica**.

1. Nella schermata di configurazione impostare il nome su **WebAutoscaleSetting**.

1. Nella condizione predefinita trovare **Limiti per le istanze** e impostare i valori seguenti.

    |Impostazione|Valore|
    |---|---|
    |Minimo|2|
    |Massimo|8|
    |Predefinito|2|

1. Fare clic su **Aggiungi regola**.

1. Nel pannello **Regola scalabilità** immettere le informazioni seguenti per creare una regola che aggiunge altre due macchine virtuali quando l'uso medio della CPU supera il 75% in un periodo di cinque minuti, quindi fare clic su **Aggiungi**.

    |Impostazione|Valore|
    |---|---|
    |Aggregazione temporale|Media|
    |Nome metrica|Percentuale CPU|
    |Statistica intervallo di tempo|Media|
    |Operatore|Maggiore di|
    |Soglia|75|
    |Durata (in minuti)|5|
    |Operazione|Aumenta numero di|
    |Numero di istanze|2|
    |Disattiva regole dopo (minuti)|5|

1. Aggiungere un'altra regola. Questa volta immettere le informazioni seguenti per creare una regola che elimina un server alla volta quando l'uso medio della CPU scende sotto il 30% in un periodo di cinque minuti, quindi fare clic su **Aggiungi**.

    |Impostazione|Valore|
    |---|---|
    |Aggregazione temporale|Media|
    |Nome metrica|Percentuale CPU|
    |Statistica intervallo di tempo|Media|
    |Operatore|Minore di|
    |Soglia|30|
    |Durata (in minuti)|5|
    |Operazione|Riduci numero di|
    |Numero di istanze|1|
    |Disattiva regole dopo (minuti)|5|

1. Le regole saranno simili alle seguenti:

    ![Screenshot del set di regole di scalabilità automatica applicate al set di scalabilità della macchina virtuale](../media/5-scale-rules.png)

1. Fare clic su **Salva**.

## <a name="generate-load-to-demonstrate-autoscaling"></a>Generare il carico per dimostrare la scalabilità automatica

A questo punto si userà lo strumento SysInternals **CPUStress.exe** per generare il carico sulle macchine virtuali nel set di scalabilità ed eseguire una dimostrazione della scalabilità orizzontale automatica.

Si eseguirà l'operazione nella macchina virtuale. Non è necessario avere Windows per questa operazione, ma _è necessario_ avere un client Desktop remoto. Microsoft dispone di client per macOS e Windows e anche diverse distribuzioni di Linux includono un client.

1. Per determinare l'indirizzo IP pubblico a cui connettersi, passare al gruppo di risorse **<rgn>[nome gruppo di risorse sandbox]</rgn>**. Nel pannello **<rgn>[nome gruppo di risorse sandbox]</rgn>** fare clic sull'oggetto **WebServerSSlb**.

1. Nel pannello **WebServerSSlb** fare clic su **Configurazione IP front-end** e prendere nota dell'indirizzo IP pubblico visualizzato. Chiudere il pannello **LoadBalancerFrontEnd**.

1. Per determinare il numero di porta a cui connettersi, nel pannello **WebServerSSlb** fare clic su **Regole NAT in ingresso**. Prendere nota del numero di porta personalizzato nella colonna Servizio di ogni macchina virtuale.

1. Nel computer locale avviare **Connessione Desktop remoto**. Nella casella **Computer** digitare l'indirizzo IP pubblico del servizio di bilanciamento del carico, seguito dai due punti e dal numero di porta dell'istanza 0 (ad esempio 40.67.191.221:50000), quindi fare clic su **Connetti**.

1. Nella finestra di dialogo **Sicurezza di Windows** visualizzata fare clic su **Altre scelte** e quindi fare clic su **Usa un account diverso**. Nelle caselle **Nome utente** e **Password** immettere le credenziali specificate in precedenza durante la creazione del set di scalabilità.

1. Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **Sì**.

1. Nel desktop remoto della macchina virtuale aprire Internet Explorer, quindi nella finestra di dialogo **Installa Internet Explorer** fare clic su **OK**.

1. In **Internet Explorer** digitare **http://download.sysinternals.com/files/CPUSTRES.zip** nella barra degli indirizzi e premere <kbd>INVIO</kbd>. Nella finestra di dialogo **Internet Explorer** fare clic su **Aggiungi**. Nella finestra di dialogo **Siti attendibili** fare clic su **Aggiungi** e quindi su **Chiudi**.

1. Se la richiesta di conferma del download non viene visualizzata automaticamente, potrebbe essere necessario immettere di nuovo l'URL e premere <kbd>INVIO</kbd>. Nella finestra di dialogo **Internet Explorer** fare clic su **Salva**. Al termine del download, fare clic su **Apri cartella**.

1. Nella cartella **Download** fare doppio clic su **CPUSTRES** e quindi di nuovo doppio clic sull'applicazione **CPUSTRES**. Nella finestra di dialogo **Cartelle compresse** fare clic su **Esegui**.

1. Nella finestra **CPU Stress** (Sollecitazione CPU), in **Thread 1**, selezionare **Maximum** (Massimo) nell'elenco a discesa **Activity** (Attività). In **Thread 2** fare clic sulla casella di controllo **Active** (Attivo) quindi nell'elenco a discesa **Activity** (Attività) selezionare **Maximum** (Massimo). Queste impostazioni vengono applicate immediatamente.

1. Fare clic sul pulsante Start di Windows e quindi su **Gestione attività**. In Gestione attività fare clic su **Più dettagli** e verificare che il valore per la CPU sia superiore al 75%.

1. **Ripetere** l'installazione e l'avvio di **CPUSTRES** nell'altra macchina virtuale del set di scalabilità e quindi attendere cinque minuti.

1. Nel portale di Azure selezionare il gruppo di risorse **<rgn>[nome gruppo di risorse sandbox]</rgn>**. Nel pannello **<rgn>[nome gruppo di risorse sandbox]</rgn>** fare clic sull'oggetto **WebServerSS**. Nel pannello **WebServerSS** fare clic su **Istanze**. Osservare il numero di istanze di macchina virtuale visualizzate all'interno del set di scalabilità e lo stato corrispondente.

In questo esercizio è stato creato un set di scalabilità di macchine virtuali con due macchine virtuali. Sono quindi state create regole per aumentare e ridurre automaticamente le dimensioni del set di scalabilità e tali regole sono state aggiunte a un profilo di scalabilità automatica.
