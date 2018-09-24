Dopo aver configurato un account, è possibile accedere al **portale di Azure**. Il portale è un sito di amministrazione basata sul Web che consente di interagire con tutte le sottoscrizioni e le risorse create. Quasi tutte le operazioni eseguite con Azure possono essere completate tramite questa interfaccia Web.

## <a name="azure-portal-layout"></a>Layout del portale di Azure

Il portale di Azure è l'interfaccia utente grafica (GUI) principale per il controllo di Microsoft Azure. Il portale consente di eseguire la maggior parte delle azioni di gestione ed è in genere l'interfaccia migliore per l'esecuzione di singole attività o la visualizzazione dettagliata delle opzioni di configurazione.

![Portale di Azure](../media/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

La parte restante della visualizzazione del portale è riservata agli elementi specifici con cui si sta lavorando. La pagina predefinita (principale) è il _dashboard_. Verrà illustrato più avanti, ma offre una visualizzazione panoramica personalizzabile delle risorse. È possibile usarlo per passare alle risorse specifiche che si vogliono gestire o per cercare risorse con l'opzione **Tutte le risorse** nel pannello delle risorse. Per gestire una risorsa, come una macchina virtuale o un'app Web, si usa un _pannello_ che presenta informazioni specifiche sulla risorsa.

## <a name="what-is-a-blade"></a>Che cos'è un pannello?

Il portale di Azure usa un modello a pannelli per lo spostamento. Un _pannello_ è un elemento scorrevole contenente l'interfaccia utente per un singolo livello in una sequenza di spostamento. Ad esempio, ognuno degli elementi in questa sequenza è rappresentato da un pannello: **Macchine virtuali** > **Calcolo** > **Server Ubuntu**.

Ogni pannello contiene alcune informazioni e opzioni configurabili. Alcune di queste opzioni generano un altro pannello, che viene visualizzato a destra di quelli esistenti. Nel nuovo pannello le eventuali altre opzioni configurabili si espanderanno in un altro pannello e così via. In poco tempo si avranno quindi diversi pannelli aperti nello stesso momento. È possibile ingrandire i pannelli in modo da occupare l'intera schermata.

Poiché i nuovi pannelli vengono sempre aggiunti a destra del proprietario, è possibile usare la barra di scorrimento nella parte inferiore della finestra per tornare indietro e vedere come si è arrivati a questo punto della configurazione. In alternativa, è possibile chiudere i pannelli singolarmente facendo clic sul pulsante `X` nell'angolo in alto del pannello. Se sono presenti modifiche non salvate, Azure informa che, se si continua, queste modifiche andranno perse.

## <a name="configuring-settings-in-the-azure-portal"></a>Configurazione delle impostazioni nel portale di Azure

Il portale di Azure visualizza diverse opzioni di configurazione, principalmente nella barra di stato nella parte superiore destra della schermata.

### <a name="notifications"></a>Notifiche

Per visualizzare il riquadro **Notifiche** fare clic sull'icona a forma di campanello. Questo riquadro elenca le ultime azioni eseguite con il relativo stato.

### <a name="cloud-shell"></a>Cloud Shell

Selezionando l'icona di **Cloud Shell** (>_) viene creata una nuova sessione di Azure Cloud Shell. Azure Cloud Shell è una shell interattiva accessibile dal browser per la gestione delle risorse di Azure. Offre la flessibilità necessaria per scegliere l'esperienza shell più adatta al proprio modo di lavorare. Gli utenti Linux possono scegliere un'esperienza Bash, mentre gli utenti Windows possono scegliere PowerShell. Questo terminale basato su browser consente di controllare e amministrare tutte le risorse di Azure nella sottoscrizione corrente tramite un'interfaccia della riga di comando incorporata nel portale.

### <a name="settings"></a>Impostazioni

Per modificare le impostazioni del portale di Azure fare clic sull'icona a forma di **ingranaggio**. Le impostazioni includono:

- Tempo di disconnessione
- Temi a colori e a contrasto
- Avvisi popup (per un dispositivo mobile)
- Lingua e formato locale

![Impostazioni del portale](../media/5-settings-blade.png)

Dopo aver modificato le impostazioni, fare clic su **Applica** per accettare le modifiche.

### <a name="feedback-blade"></a>Pannello Commenti e suggerimenti

L'icona a forma di **faccina sorridente** apre il pannello **Inviare commenti e suggerimenti**. In quest'area è possibile inviare a Microsoft commenti e suggerimenti su Azure. Si noti che è possibile specificare se Microsoft può rispondere al feedback tramite posta elettronica.

### <a name="help-blade"></a>Pannello Guida e supporto

Fare clic sull'icona del **punto interrogativo** per visualizzare il pannello **Guida**. Qui è possibile scegliere tra diverse opzioni, tra cui:

- Novità
- Roadmap per Azure
- Avvia presentazione guidata
- Scelte rapide da tastiera
- Visualizza diagnostica
- Privacy e condizioni

### <a name="directory-and-subscription"></a>Directory e sottoscrizione

Fare clic sull'icona a forma di **libro e filtro** per visualizzare il pannello **Directory e sottoscrizione**.

Azure consente di avere più sottoscrizioni associate a una stessa directory. Nel pannello **Directory e sottoscrizione** è possibile passare da una sottoscrizione all'altra. In quest'area è possibile cambiare la sottoscrizione o passare a un'altra directory.

![Directory](../media/5-directory-blade.png)

### <a name="profile-settings"></a>Impostazioni del profilo

Se si fa clic sul proprio nome nell'angolo superiore destro, si apre un menu con alcune opzioni:

- Accedere con un altro account o disconnettersi completamente
- Visualizzare il profilo dell'account, in cui è possibile modificare la password
- Verificare le autorizzazioni
- Visualizza la fattura (fare clic sul pulsante "..." sul lato destro)
- Aggiornare le informazioni di contatto (fare clic sul pulsante "..." sul lato destro)

Se si seleziona "..." e quindi **Visualizza fattura**, viene visualizzata la pagina **Gestione dei costi e fatturazione - Fatture** che consente di analizzare i costi generati da Azure.

Azure è una piattaforma di grandi dimensioni, come testimonia l'interfaccia utente del portale di Azure. La struttura a pannelli consente di spostarsi avanti e indietro tra le varie attività di amministrazione con facilità. Nei prossimi esercizi faremo pratica con questa interfaccia utente.