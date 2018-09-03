I prodotti Azure presentano una struttura gerarchica a più livelli. Si supponga, ad esempio, di dover creare una macchina virtuale Linux. È possibile spostarsi tra questi livelli: **Home** **>** **Macchine virtuali** **>** **Calcolo** **>** **Ubuntu Server**. Il portale di Azure offre un'interfaccia utente ottimizzata per rendere più intuitivo questo tipo di sequenza di spostamento. In questo esercizio si esamineranno gli elementi principali dell'interfaccia utente che consentono di spostarsi tra i vari livelli. Sarà possibile spostarsi tra i menu e i sottomenu e usare i pannelli per trovare e configurare i servizi.

## <a name="azure-portal-layout"></a>Layout del portale di Azure

Il portale di Azure è la principale interfaccia utente grafica (GUI) per il controllo di Microsoft Azure. Nel portale è possibile eseguire la maggior parte delle operazioni di gestione e il portale offre in genere l'interfaccia migliore per l'esecuzione di singole attività o per la visualizzazione dettagliata delle opzioni di configurazione.

Nell'area sinistra del portale si trova il riquadro delle risorse in cui sono elencati i principali tipi di risorse. Si noti che Azure offre molti più tipi di risorse di quelli indicati.

## <a name="using-blades-in-azure-portal"></a>Uso dei pannelli nel portale di Azure

Il portale di Azure usa un modello a pannelli per lo spostamento. Un _pannello_ è un elemento scorrevole contenente l'interfaccia utente per un singolo livello in una sequenza di spostamento. Ad esempio, ognuno degli elementi in questa sequenza è rappresentata da un pannello: **Macchine virtuali** **>** **Calcolo** **>** **Ubuntu Server**.

Ogni pannello all'interno dell'interfaccia utente contiene in genere diverse opzioni configurabili. Alcune di queste opzioni generano un altro pannello, che viene visualizzato a destra di quelli esistenti. Nel nuovo pannello le eventuali altre opzioni configurabili si espanderanno in un altro pannello e così via. In poco tempo si avranno quindi diversi pannelli aperti nello stesso momento. È possibile ingrandire i pannelli in modo da occupare l'intera schermata.

Se si prova a chiudere un pannello senza salvare le modifiche di configurazione apportate, si riceverà un messaggio.

## <a name="configuring-settings-in-azure-portal"></a>Configurazione delle impostazioni nel portale di Azure

Il portale di Azure consente di visualizzare diverse opzioni di configurazione, principalmente nella barra di stato in alto a destra nella schermata.

### <a name="notifications"></a>Notifiche

Selezionando l'icona a forma di campanella viene visualizzato il riquadro Notifiche. Questo riquadro elenca le ultime azioni completate con il relativo stato.

![Pannello Notifiche](../images/2-notifications-blade.PNG)

### <a name="cloud-shell"></a>Cloud Shell

Selezionando l'icona di Cloud Shell (>_) è possibile creare una nuova sessione di Cloud Shell. In tale sessione viene chiesto di scegliere se usare Bash o PowerShell in Linux.

![Cloud Shell](../images/2-choose-shell.PNG)

### <a name="settings"></a>Impostazioni

Selezionando l'icona a forma di ingranaggio è possibile modificare le impostazioni del portale di Azure. Tali impostazioni includono:

* Tempo di disconnessione
* Combinazione colori
* Temi a contrasto elevato
* Avvisi popup (per un dispositivo mobile)
* Doppio clic per modificare il tema
* Lingua
* Formato regionale

![Impostazioni del portale](../images/2-settings-blade.PNG)

Dopo aver modificato le impostazioni, fare clic su **Applica** per accettare le modifiche.

### <a name="feedback-blade"></a>Pannello Commenti e suggerimenti

L'icona a forma di sorriso consente di aprire il pannello **Inviare commenti e suggerimenti**. In quest'area è possibile inviare a Microsoft commenti e suggerimenti su Azure. Si noti che è possibile specificare se Microsoft può rispondere a commenti e suggerimenti tramite posta elettronica.

![Commenti e suggerimenti](../images/2-feedback-blade.PNG)

### <a name="help-blade"></a>Pannello Guida e supporto

Fare clic sul punto interrogativo per visualizzare il pannello **Guida e supporto**. In quest'area è possibile scegliere tra diversi argomenti, ad esempio:

* Novità
* Roadmap per Azure
* Avvia presentazione guidata
* Scelte rapide da tastiera
* Visualizza diagnostica
* Privacy e condizioni

### <a name="directory-and-subscription"></a>Directory e sottoscrizione

Fare clic sull'icona a forma di libro e filtro per visualizzare il pannello **Directory e sottoscrizione**.

Azure consente di avere più sottoscrizioni associate a una stessa directory. Nel pannello Directory e sottoscrizione è possibile passare da una sottoscrizione all'altra. In questo caso, è possibile cambiare sottoscrizione o passare a un'altra directory.

![Directory](../images/2-directory-blade-1.PNG)

### <a name="profile-settings"></a>Impostazioni del profilo

Se si fa clic sul nome nell'angolo superiore destro, è possibile modificare le impostazioni del profilo.
Le opzioni includono:

* Disconnetti da Azure
* Cambia password
* Modifica informazioni di contatto
* Visualizza autorizzazioni
* Invia un'idea al team di Azure
* Visualizza fattura
* Cambia directory (viene visualizzato il pannello Directory e sottoscrizione come nella sezione precedente)

![Impostazioni del profilo](../images/2-portal-menu.png)

Se si seleziona **Visualizza fattura**, viene visualizzata la pagina **Gestione dei costi e fatturazione - Fatture**, che consente di analizzare i costi generati da Azure.

![Pagina Fatture](../images/2-portal-billing.PNG)

## <a name="summary"></a>Riepilogo

Azure è una piattaforma di grandi dimensioni e l'interfaccia utente del portale riflette questa caratteristica. Per esplorare le numerose funzionalità disponibili, il portale offre un'interfaccia a pannelli indicante i vari livelli della gerarchia. I pannelli consentono di esaminare una specifica attività indicando chiaramente il percorso seguito per raggiungere tale punto.