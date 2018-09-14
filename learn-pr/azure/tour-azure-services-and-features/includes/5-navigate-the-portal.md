Ora che abbiamo un account, è possibile accedere al **portale di Azure**. Il portale è un sito di amministrazione basata su web che consente di interagire con tutte le sottoscrizioni e risorse che è stato creato. Quasi tutte le operazioni eseguite con Azure può avvenire tramite questa interfaccia web.

## <a name="azure-portal-layout"></a>Layout del portale di Azure

Il portale di Azure è l'interfaccia utente principale di con interfaccia grafica (GUI) per il controllo di Microsoft Azure. È possibile eseguire la maggior parte delle azioni di gestione nel portale, ed è in genere l'interfaccia ottimo per l'esecuzione delle singole attività o in cui si desidera esaminare le opzioni di configurazione in modo dettagliato.

![Portale di Azure](../media-draft/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media-draft/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

Il resto della vista del portale è per gli elementi specifici che si sta lavorando. La pagina (principale) predefinito è il _dashboard_. Parleremo un po' più avanti, ma rappresenta una personalizzabile volatili--visualizzazione delle risorse. È possibile usarlo per passare a risorse specifiche da gestire o eseguire la ricerca per le risorse con il **tutte le risorse** voce nel Pannello di risorse. Quando si gestiscono una risorsa, ad esempio una macchina virtuale o un'app web, verrà usata un' _pannello_ che presenta informazioni specifiche sulla risorsa.

## <a name="what-is-a-blade"></a>Che cos'è un pannello?

Il portale di Azure usa un modello a pannelli per lo spostamento. Un _pannello_ è un elemento scorrevole contenente l'interfaccia utente per un singolo livello in una sequenza di spostamento. Ad esempio, ognuno degli elementi in questa sequenza è rappresentato da un pannello: **Macchine virtuali** > **Calcolo** > **Server Ubuntu**.

Ogni pannello contiene alcune informazioni e opzioni configurabili. Alcune di queste opzioni generano un altro pannello, che viene visualizzato a destra di quelli esistenti. Nel nuovo pannello le eventuali altre opzioni configurabili si espanderanno in un altro pannello e così via. In poco tempo si avranno quindi diversi pannelli aperti nello stesso momento. È possibile ingrandire i pannelli in modo da occupare l'intera schermata.

Poiché nuovi pannelli vengono sempre aggiunti a destra del proprietario, è possibile utilizzare la barra di scorrimento nella parte inferiore della finestra per spostarsi indietro a vedere come si è arrivati a questo punto nella configurazione. In alternativa, è possibile chiudere i pannelli singolarmente facendo la `X` pulsante nell'angolo superiore del pannello. Se si presenti modifiche non salvate, Azure richiederà di consentono di sapere che le modifiche andranno perse se sceglie di continuare.

## <a name="configuring-settings-in-the-azure-portal"></a>Configurazione delle impostazioni nel portale di Azure

Il portale di Azure consente di visualizzare diverse opzioni di configurazione, principalmente nella barra di stato in alto a destra della schermata.

### <a name="notifications"></a>Notifiche

Per visualizzare il riquadro **Notifiche** fare clic sull'icona a forma di campanello. Questo riquadro elenca le ultime azioni completate con il relativo stato.

### <a name="cloud-shell"></a>Cloud Shell

Selezionando l'icona di **Cloud Shell** (>_) viene creata una nuova sessione di Azure Cloud Shell. Azure Cloud Shell è una shell interattiva accessibile dal browser per la gestione delle risorse di Azure. Offre la flessibilità necessaria per scegliere l'esperienza shell più adatta al proprio modo di lavorare. Gli utenti Linux possono scegliere un'esperienza Bash, mentre gli utenti Windows possono scegliere PowerShell. Questo terminal basati su browser consente di controllare e amministrare tutte le risorse nella sottoscrizione corrente tramite un'interfaccia della riga di comando integrata nel portale di Azure.

### <a name="settings"></a>Impostazioni

Per modificare le impostazioni del portale di Azure fare clic sull'icona a forma di **ingranaggio**. Le impostazioni includono:

- Tempo di disconnessione
- Temi di colore e a contrasto elevato
- Avvisi popup (per un dispositivo mobile)
- Linguaggio e il formato regionale

![Impostazioni del portale](../media-draft/5-settings-blade.png)

Dopo aver modificato le impostazioni, fare clic su **Applica** per accettare le modifiche.

### <a name="feedback-blade"></a>Pannello Commenti e suggerimenti

L'icona a forma di **faccina sorridente** apre il pannello **Inviare commenti e suggerimenti**. In quest'area è possibile inviare a Microsoft commenti e suggerimenti su Azure. Si noti che è possibile specificare se Microsoft può rispondere a commenti e suggerimenti tramite posta elettronica.

### <a name="help-blade"></a>Pannello Guida e supporto

Fare clic sull'icona del **punto interrogativo** per visualizzare il pannello **Guida e supporto**. Qui è possibile scegliere tra diverse opzioni, tra cui:

- Novità
- Roadmap per Azure
- Avvia presentazione guidata
- Scelte rapide da tastiera
- Visualizza diagnostica
- Privacy e condizioni

### <a name="directory-and-subscription"></a>Directory e sottoscrizione

Fare clic sull'icona a forma di **libro e filtro** per visualizzare il pannello **Directory e sottoscrizione**.

Azure consente di avere più sottoscrizioni associate a una stessa directory. Nel pannello **Directory e sottoscrizione** è possibile passare da una sottoscrizione all'altra. In quest'area è possibile cambiare la sottoscrizione o passare a un'altra directory.

![Directory](../media-draft/5-directory-blade.png)

### <a name="profile-settings"></a>Impostazioni del profilo

Se si fa clic sul nome nell'angolo superiore destro, è possibile modificare le impostazioni del profilo.
Le opzioni includono:

- Disconnetti da Azure
- Cambia password
- Modifica informazioni di contatto
- Visualizza autorizzazioni
- Invia un'idea al team di Azure
- Visualizzare la fattura
- Cambia directory (viene visualizzato il pannello **Directory e sottoscrizione** come nella sezione precedente)

Se si seleziona **Visualizza fattura**, viene visualizzata la pagina **Gestione dei costi e fatturazione - Fatture** che consente di analizzare i costi generati da Azure.

Azure è un prodotto di grandi dimensioni e l'interfaccia utente del portale di Azure (UI) rifletterà la sovrascrizione. L'approccio di pannello scorrevole consente di spostarsi avanti e indietro tra le varie attività di amministrazione con facilità. È possibile fare alcune prove con questa interfaccia utente in modo ottenere alcune procedure consigliate.