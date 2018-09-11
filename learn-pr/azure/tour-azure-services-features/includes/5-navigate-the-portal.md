I prodotti Azure presentano una struttura gerarchica a più livelli. Si supponga, ad esempio, di dover creare una macchina virtuale Linux. È possibile spostarsi tra questi livelli: **Home page** > **Macchine virtuali** > **Calcolo** > **Server Ubuntu**. Il portale di Azure offre un'interfaccia utente ottimizzata per rendere più intuitivo questo tipo di sequenza di spostamento. In questo esercizio si esamineranno gli elementi principali dell'interfaccia utente che consentono di spostarsi tra i vari livelli. Sarà possibile spostarsi tra i menu e i sottomenu e usare i pannelli per trovare e configurare i servizi.

## <a name="azure-portal-layout"></a>Layout del portale di Azure

Il portale di Azure è la principale interfaccia utente grafica (GUI) per il controllo di Microsoft Azure. Il portale consente di eseguire la maggior parte delle azioni di gestione ed è in genere l'interfaccia migliore per l'esecuzione di singole attività o la visualizzazione dettagliata delle opzioni di configurazione.

Nell'area sinistra del portale si trova il riquadro delle risorse in cui sono elencati i principali tipi di risorse. Si noti che Azure offre molti più tipi di risorse di quelli indicati.

## <a name="using-blades-in-the-azure-portal"></a>Uso dei pannelli nel portale di Azure

Il portale di Azure usa un modello a pannelli per lo spostamento. Un _pannello_ è un elemento scorrevole contenente l'interfaccia utente per un singolo livello in una sequenza di spostamento. Ad esempio, ognuno degli elementi in questa sequenza è rappresentato da un pannello: **Macchine virtuali** > **Calcolo** > **Server Ubuntu**.

Ogni pannello all'interno dell'interfaccia utente contiene in genere diverse opzioni configurabili. Alcune di queste opzioni generano un altro pannello, che viene visualizzato a destra di quelli esistenti. Nel nuovo pannello le eventuali altre opzioni configurabili si espanderanno in un altro pannello e così via. In poco tempo si avranno quindi diversi pannelli aperti nello stesso momento. È possibile ingrandire i pannelli in modo da occupare l'intera schermata.

Se si prova a chiudere un pannello senza salvare le modifiche di configurazione apportate, verrà visualizzato un messaggio.

## <a name="configuring-settings-in-the-azure-portal"></a>Configurazione delle impostazioni nel portale di Azure

Il portale di Azure visualizza diverse opzioni di configurazione, principalmente nella barra di stato nella parte superiore destra della schermata.

### <a name="notifications"></a>Notifiche

Per visualizzare il riquadro **Notifiche** fare clic sull'icona a forma di campanello. Questo riquadro elenca le ultime azioni completate con il relativo stato.

![Pannello Notifiche](../media-draft/2-notifications-blade.PNG)

### <a name="cloud-shell"></a>Cloud Shell

Selezionando l'icona di **Cloud Shell** (>_) viene creata una nuova sessione di Azure Cloud Shell. Nella sessione viene chiesto di scegliere se usare Bash o PowerShell in Linux.

![Cloud Shell](../media-draft/2-choose-shell.PNG)

### <a name="settings"></a>Impostazioni

Per modificare le impostazioni del portale di Azure fare clic sull'icona a forma di **ingranaggio**. Le impostazioni includono:

* Tempo di disconnessione
* Combinazione colori
* Temi a contrasto elevato
* Avvisi popup (per un dispositivo mobile)
* Doppio clic per modificare il tema
* Lingua
* Formato regionale

![Impostazioni del portale](../media-draft/2-settings-blade.PNG)

Dopo aver modificato le impostazioni, fare clic su **Applica** per accettare le modifiche.

### <a name="feedback-blade"></a>Pannello Commenti e suggerimenti

L'icona a forma di **faccina sorridente** apre il pannello **Inviare commenti e suggerimenti**. In quest'area è possibile inviare a Microsoft commenti e suggerimenti su Azure. Si noti che è possibile specificare se Microsoft può rispondere a commenti e suggerimenti tramite posta elettronica.

![Commenti e suggerimenti](../media-draft/2-feedback-blade.PNG)

### <a name="help-blade"></a>Pannello Guida e supporto

Fare clic sull'icona del **punto interrogativo** per visualizzare il pannello **Guida e supporto**. In quest'area è possibile scegliere tra diversi argomenti, tra cui:

* Novità
* Roadmap per Azure
* Avvia presentazione guidata
* Scelte rapide da tastiera
* Visualizza diagnostica
* Privacy e condizioni

### <a name="directory-and-subscription"></a>Directory e sottoscrizione

Fare clic sull'icona a forma di **libro e filtro** per visualizzare il pannello **Directory e sottoscrizione**.

Azure consente di avere più sottoscrizioni associate a una stessa directory. Nel pannello **Directory e sottoscrizione** è possibile passare da una sottoscrizione all'altra. In quest'area è possibile cambiare la sottoscrizione o passare a un'altra directory.

![Directory](../media-draft/2-directory-blade-1.PNG)

### <a name="profile-settings"></a>Impostazioni del profilo

Se si fa clic sul nome nell'angolo superiore destro, è possibile modificare le impostazioni del profilo.
Le opzioni includono:

* Disconnetti da Azure
* Cambia password
* Modifica informazioni di contatto
* Visualizza autorizzazioni
* Invia un'idea al team di Azure
* Visualizzare la fattura
* Cambia directory (viene visualizzato il pannello **Directory e sottoscrizione** come nella sezione precedente)

![Impostazioni del profilo](../media-draft/2-portal-menu.png)

Se si seleziona **Visualizza fattura**, viene visualizzata la pagina **Gestione dei costi e fatturazione - Fatture** che consente di analizzare i costi generati da Azure.

![Pagina Fatturazione](../media-draft/2-portal-billing.PNG)

## <a name="summary"></a>Riepilogo

Azure è una piattaforma di grandi dimensioni e l'interfaccia utente del portale di Azure riflette questa caratteristica. Per esplorare le numerose funzionalità disponibili, il portale offre un'interfaccia a pannelli indicante i vari livelli della gerarchia. I pannelli consentono di esaminare una specifica attività indicando chiaramente il percorso seguito per raggiungere tale punto.