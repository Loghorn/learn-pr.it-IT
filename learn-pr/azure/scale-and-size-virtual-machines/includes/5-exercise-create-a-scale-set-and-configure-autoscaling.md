In questo esercizio si userà il portale di Azure per creare un set di scalabilità di macchine virtuali con regole per la scalabilità automatica.

## <a name="create-a-virtual-machine-scale-set"></a>Creare un set di scalabilità di macchine virtuali

1. Nel portale di Azure fare clic su **Crea una risorsa**.

1. Nella casella di ricerca digitare **set di scalabilità** e premere INVIO. Nel pannello **Risultati** fare clic su **Set di scalabilità di macchine virtuali**, quindi nel pannello **Set di scalabilità di macchine virtuali** fare clic su **Crea**.

1. Nel pannello **Crea set di scalabilità di macchine virtuali** immettere i valori seguenti (sostituendo `<your initials>`, `<date>` e `<time>` con i dati pertinenti), quindi fare clic su **Crea**.

    |Impostazione|Valore|
    |---|---|
    |Nome del set di scalabilità di macchine virtuali|WebServerSS|
    |Gruppo di risorse|Usa esistente - ExerciseRG|
    |Nome utente|LocalAdmin|
    |Password e Conferma password|Adm1nPa$$word|
    |Numero di istanze|2|
    |Dimensioni istanza|D2s_v3|
    |Scegliere le opzioni di bilanciamento del carico|Bilanciamento del carico|
    |Nome indirizzo IP pubblico|WebServerPubIP|
    |Etichetta del nome di dominio|`<your initials><date><time>` (ad esempio ja0904181202)|

1. Attendere che il set di scalabilità venga creato.

1. Passare al gruppo di risorse **ExerciseRG**. Nel pannello **ExerciseRG** fare clic sull'oggetto**WebServerSS**, quindi nel pannello **WebServerSS** fare clic su **Istanze**. Notare le due istanze di macchina virtuale in esecuzione nel set di scalabilità.

## <a name="create-and-apply-autoscale-rules"></a>Creare e applicare regole di scalabilità automatica

1. Nel pannello **WebServerSS** fare clic su **Scalabilità**. Nel pannello **WebServerSS - Scalabilità** fare clic su **Abilita scalabilità automatica**.

1. Nel pannello **WebServerSS - Scalabilità** fare clic su **Aggiungi una regola**.

1. Nel pannello Regola scalabilità immettere le informazioni seguenti per creare una regola che aggiunga altre due macchine virtuali quando l'utilizzo medio della CPU supera il 75% in un periodo di cinque minuti, quindi fare clic su **Aggiungi**.

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

1. Nel pannello **WebServerSS - Scalabilità** fare clic su **Aggiungi una regola**.

1. Nel pannello Regola scalabilità immettere le informazioni seguenti per creare una regola che elimini un server alla volta quando l'utilizzo medio della CPU scende sotto il 30% in un periodo di cinque minuti, quindi fare clic su **Aggiungi**.

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

1. Nel pannello **WebServerSS - Scalabilità** digitare **WebAutoscaleSetting** nella casella **Nome impostazione di scalabilità automatica**. Accanto a **Limiti per le istanze** impostare i valori seguenti e fare clic su **Salva**.

    |Impostazione|Valore|
    |---|---|
    |Minimo|2|
    |Massimo|8|
    |Predefinito|2|

## <a name="generate-load-to-demonstrate-autoscaling"></a>Generare il carico per dimostrare la scalabilità automatica

A questo punto si userà lo strumento CPUStress.exe per generare il carico sulle macchine virtuali nel set di scalabilità ed eseguire una dimostrazione della scalabilità orizzontale automatica.

1. Per determinare l'indirizzo IP pubblico a cui connettersi, passare al gruppo di risorse **ExerciseRG**. Nel pannello **ExerciseRG** fare clic sull'oggetto **WebServerSSlb**.

1. Nel pannello **WebServerSSlb** fare clic su Configurazione IP front-end e prendere nota dell'indirizzo IP pubblico visualizzato. Chiudere il pannello **LoadBalancerFrontEnd**.

1. Per determinare il numero di porta a cui connettersi, nel pannello **WebServerSSlb** fare clic su **Regole NAT in ingresso**. Prendere nota del numero di porta personalizzato nella colonna Servizio di ogni macchina virtuale.

1. Nel computer locale avviare **Connessione Desktop remoto**. Nella casella **Computer** digitare l'indirizzo IP pubblico del servizio di bilanciamento del carico, seguito da un segno di due punti e dal valore del numero di porta dell'istanza 0 (ad esempio 40.67.191.221:50000), quindi fare clic su Connetti.

1. Nella finestra di dialogo **Sicurezza di Windows** fare clic su **Altre opzioni** e quindi su **Usa un altro account**. Nella casella **Nome utente** digitare **LocalAdmin**, nella casella **Password** digitare **Adm1nPa$$word** e quindi fare clic su **OK**.

1. Nella finestra di dialogo **Connessione Desktop remoto** fare clic su **Sì**.

1. Nel desktop remoto della macchina virtuale aprire Internet Explorer, quindi nella finestra di dialogo **Installa Internet Explorer** fare clic su **OK**.

1. In **Internet Explorer** digitare **http://download.sysinternals.com/files/CPUSTRES.zip** nella barra degli indirizzi e premere INVIO. Nella finestra di dialogo **Internet Explorer** fare clic su **Aggiungi**. Nella finestra di dialogo **Siti attendibili** fare clic su **Aggiungi** e quindi su **Chiudi**.

1. Se la richiesta di conferma del download non viene visualizzata automaticamente, potrebbe essere necessario immettere di nuovo l'URL e premere INVIO. Nella finestra di dialogo **Internet Explorer** fare clic su **Salva**. Al termine del download, fare clic su **Apri cartella**.

1. Nella cartella **Download** fare doppio clic su **CPUSTRES** e quindi di nuovo doppio clic sull'applicazione **CPUSTRES**. Nella finestra di dialogo **Cartelle compresse** fare clic su **Esegui**.

1. Nella finestra **CPU Stress**, in **Thread 1**, selezionare **Maximum** nell'elenco a discesa **Activity**. In **Thread 2** fare clic sulla casella di controllo **Active**, quindi nell'elenco a discesa **Activity** selezionare **Maximum**.

1. Fare clic sul pulsante Start e quindi su **Gestione attività**. In Gestione attività fare clic su **Più dettagli** e verificare che il valore per la CPU sia superiore al 75%.

1. Ripetere l'installazione e l'avvio di **CPUSTRES** nell'altra macchina virtuale del set di scalabilità e quindi attendere cinque minuti.

1. Nel portale di Azure passare al gruppo di risorse **ExerciseRG**, quindi nel pannello **ExerciseRG** fare clic sull'oggetto **WebServerSS** e nel pannello **WebServerSS** fare clic su **Istanze**. Osservare il numero di istanze di macchina virtuale all'interno del set di scalabilità e lo stato.

In questo esercizio è stato creato un set di scalabilità di macchine virtuali con due macchine virtuali. Sono quindi state create regole per aumentare e ridurre automaticamente le dimensioni del set di scalabilità e tali regole sono state aggiunte a un profilo di scalabilità automatica.