In questo esercizio si userà il portale di Azure per creare un macchina virtuale set di scalabilità con le regole per la scalabilità automatica.

## <a name="create-a-virtual-machine-scale-set"></a>Creare un set di scalabilità di macchine virtuali

1. Nel portale di Azure fare clic su **Crea una risorsa**.

1. Nella casella di ricerca, digitare **set di scalabilità** e premere INVIO. Nel **risultati** pannello, fare clic su **set di scalabilità di macchine virtuali**e nel **set di scalabilità di macchine virtuali** pannello, fare clic su **crea**.

1. Nel **creare set di scalabilità di macchine virtuali** blade, immettere i valori seguenti (sostituendo `<your initials>`, `<date>`, e `<time>` con i dati rilevanti) e quindi fare clic su **Create**.

    |Impostazione|Valore|
    |---|---|
    |Nome del set di scalabilità di macchine virtuali|WebServerSS|
    |Gruppo di risorse|Usa esistente - <rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>|
    |Username|LocalAdmin|
    |La password e Conferma password|Adm1nPa$$word|
    |Numero di istanze|2|
    |Dimensioni istanza|D2s_v3|
    |Scegliere le opzioni di bilanciamento del carico|Bilanciamento del carico|
    |Nome dell'indirizzo IP pubblico|WebServerPubIP|
    |Etichetta del nome di dominio|`<your initials><date><time>` (ad esempio ja0904181202)|

1. Attendere che il set di scalabilità per essere creata.

1. Passare il **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>** gruppo di risorse. Nel **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>** pannello, fare clic sul **WebServerSS** oggetto e nel **WebServerSS** pannello, fare clic su **Istanze**. Tenere presente le due istanze di macchina virtuale in esecuzione all'interno del set di scalabilità.

## <a name="create-and-apply-autoscale-rules"></a>Creare e applicare regole di scalabilità automatica

1. Nel **WebServerSS** pannello, fare clic su **Scaling**. Nel **WebServerSS - Scaling** pannello, fare clic su **Abilita scalabilità automatica**.

1. Nel **WebServerSS - Scaling** pannello, fare clic su **aggiungere una regola**.

1. Nel **regole di scalabilità** blade, immettere le informazioni seguenti per creare una regola per scalare orizzontalmente un'altre due macchine virtuali quando l'utilizzo medio della CPU è superiore al 75% su un periodo di cinque minuti e quindi fare clic su **Add**.

    |Impostazione|Valore|
    |---|---|
    |Aggregazione temporale|Media|
    |Nome metrica|CPU percentuale|
    |Statistica intervallo di tempo|Media|
    |Operatore|Maggiore di|
    |Soglia|75|
    |Durata (in minuti)|5|
    |Operazione|Aumenta numero di|
    |Numero di istanze|2|
    |Disattiva regole dopo (minuti)|5|

1. Nel **WebServerSS - Scaling** pannello, fare clic su **aggiungere una regola**.

1. Nel **regole di scalabilità** blade, immettere le informazioni seguenti per creare una regola per la scalabilità in un server alla volta quando utilizzo medio della CPU è inferiore al 30% per un periodo di cinque minuti e quindi fare clic su **Add**.

    |Impostazione|Valore|
    |---|---|
    |Aggregazione temporale|Media|
    |Nome metrica|CPU percentuale|
    |Statistica intervallo di tempo|Media|
    |Operatore|Minore di|
    |Soglia|30|
    |Durata (in minuti)|5|
    |Operazione|Riduci numero di|
    |Numero di istanze|1|
    |Disattiva regole dopo (minuti)|5|

1. Nel **WebServerSS - ridimensionamento** pannello nella **nome dell'impostazione di scalabilità automatica** , digitare **WebAutoscaleSetting**. Accanto a **limiti per le istanze**, impostare i valori seguenti e quindi fare clic su **salvare**.

    |Impostazione|Valore|
    |---|---|
    |Minima|2|
    |Massima|8|
    |Predefinito|2|

## <a name="generate-load-to-demonstrate-autoscaling"></a>Generare carico per illustrare la scalabilità automatica

A questo punto si utilizzerà lo strumento CPUStress.exe per generare il carico nelle macchine virtuali nel set di scalabilità e dimostrare la scalabilità automatica orizzontale.

1. Per determinare l'indirizzo IP pubblico a cui connettersi, selezionare la **ExerciseRG** gruppo di risorse. Nel **ExerciseRG** pannello, fare clic sui **WebServerSSlb** oggetto.

1. Nel **WebServerSSlb** pannello, fare clic su **configurazione Frontend IP** e prendere nota dell'indirizzo IP pubblico visualizzato. Chiudi il **LoadBalancerFrontEnd** pannello.

1. Per determinare il numero di porta a cui connettersi, scegliere il **WebServerSSlb** pannello, fare clic su **regole NAT in ingresso**. Annotare il numero di porta personalizzato nella colonna servizio per ogni macchina virtuale.

1. Nel computer locale, avviare il **connessione Desktop remoto**. Nel **Computer** casella, digitare l'indirizzo IP pubblico del bilanciamento del carico, seguita da due punti e quindi il numero di porta al valore, ad esempio 0 (ad esempio, 40.67.191.221:50000) e quindi fare clic su **Connect**.

1. Nel **sicurezza di Windows** della finestra di dialogo fare clic su **altre scelte**e quindi fare clic su **Usa un account diverso**. Nel **nome utente** , digitare **LocalAdmin**. Nel **Password** , digitare **word $$ Adm1nPa**, quindi fare clic su **OK**.

1. Nel **connessione Desktop remoto** finestra di dialogo, fare clic su **Yes**.

1. In desktop remoto della macchina virtuale, aprire Internet Explorer e il **configurare Internet Explorer** finestra di dialogo, fare clic su **OK**.

1. Nelle **Internet Explorer**, nella barra degli indirizzi, digitare **http://download.sysinternals.com/files/CPUSTRES.zip** e premere INVIO. Nel **Internet Explorer** finestra di dialogo, fare clic su **Add**. Nel **dei siti attendibili** finestra di dialogo, fare clic su **Add** e quindi fare clic su **Chiudi**.

1. Se la richiesta di download non viene visualizzata automaticamente, si potrebbe essere necessario immettere di nuovo l'URL e premere INVIO. Nel **Internet Explorer** finestra di dialogo, fare clic su **salvare**. Una volta completato il download, fare clic su **Apri cartella**.

1. Nel **Downloads** cartella, fare doppio clic su **CPUSTRES**e quindi fare doppio clic sul **CPUSTRES** dell'applicazione. Nel **compressa di cartelle compresse** finestra di dialogo, fare clic su **eseguire**.

1. Nel **CPU Stress** finestra, sotto **Thread 1**, nel **attività** elenco a discesa, selezionare **massimo**. Sotto **Thread 2**, fare clic sui **Active** casella di controllo e nel **attività** elenco a discesa, selezionare **massimo**.

1. Fare clic sul pulsante Start, quindi fare clic su **Task Manager**. In Gestione attività, fare clic su **altri dettagli**e verificare che la CPU è superiore al 75%.

1. Ripetere l'installazione e avvio del **CPUSTRES** in altra macchina virtuale nella scala dei set e quindi attendere cinque minuti.

1. Nel portale di Azure, selezionare la **ExerciseRG** gruppo di risorse. Nel **ExerciseRG** pannello, fare clic sui **WebServerSS** oggetto. Nel **WebServerSS** pannello, fare clic su **istanze**. Si noti il numero di istanze di macchina virtuale viene visualizzato all'interno del set di scalabilità e lo stato.

Qui viene creato un set con due macchine virtuali di scalabilità di macchine virtuali. Quindi create le regole per scalare automaticamente e la scalabilità in set di scalabilità, e si aggiungono le regole a un profilo di scalabilità automatica.