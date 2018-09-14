I dashboard rappresentano uno strumento flessibile per gestire diversi aspetti dei servizi di Azure tramite il portale. Facilitano il monitoraggio dello stato dei servizi. Poiché sono condivisibili, consentono di assicurarsi che tutti i membri del team visualizzino gli stessi dati e conoscano lo stato dei componenti critici. Verrà creato un nuovo dashboard e vi verranno aggiunti alcuni riquadri.

## <a name="create-a-new-dashboard"></a>Creare un nuovo dashboard

1. Nel portale di Azure fare clic sul pulsante **Nuovo dashboard**.

1. Nella casella denominata **My Dashboard** (Dashboard) modificare il nome in **Customer Dashboard**  (Dashboard cliente).

## <a name="add-and-configure-the-clock-tile"></a>Aggiungere e configurare il riquadro dell'orologio

1. Nella raccolta riquadri trascinare l'orologio nell'area di lavoro. Posizionarlo nella parte superiore destra dello spazio disponibile.

1. Nel pannello **Modifica orologio** modificare la località in **Pacifico (USA e Canada)**.

1. In **Formato ora** fare clic su **24 ore**.

1. Fare clic su **Fatto**.

1. Ripetere i quattro passaggi precedenti, ad eccezione della selezione di **Fuso orientale (USA e Canada)**. Dovrebbero essere ora presenti due orologi, uno che indica l'ora della costa occidentale, l'altro quella della costa orientale.

## <a name="resize-a-tile"></a>Ridimensionare un riquadro

1. Nel riquadro **Raccolta riquadri** fare clic su **Tutte le risorse** e rilasciare il riquadro in alto a sinistra nella nuova area di lavoro del dashboard.

1. Fare clic sul riquadro, fare clic con il pulsante destro del mouse sui puntini di sospensione e quindi scegliere **6x6**.

1. Fare clic sull'angolo grigio nella parte inferiore destra del riquadro e ridimensionare il riquadro su 3,5 in senso verticale e su 6 in senso orizzontale. Si noti che al termine del ridimensionamento, l'impostazione sarà 4x6.

1. In Raccolta riquadri fare clic sul riquadro **Gruppi di risorse** e trascinarlo nell'area di lavoro. Inserirlo sotto il riquadro **Tutte le risorse**.

1. In Raccolta riquadri fare clic sul riquadro **Integrità del servizio** e trascinarlo nell'area di lavoro. Inserirlo a destra del riquadro **Tutte le risorse**.

1. Continuare ad aggiungere i riquadri seguenti, ridisporli per adattarli:

    - Guida + supporto
    - Attività rapide
    - Marketplace
    - Novità

1. Dopo aver aggiunto questi riquadri, fare clic su **Fatto**. Viene visualizzato il dashboard **Dashboard cliente**.

## <a name="clone-a-dashboard"></a>Clonare un dashboard

Ora si vuole creare un dashboard molto simile per altri clienti.

1. Fare clic sul pulsante **Clona**.

1. Rinominare il dashboard da **Clone of Customer Dashboard** (Clone del dashboard cliente) in **Azure AD Admin Dashboard** (Dashboard di amministrazione di Azure AD).

1. Nel riquadro **Gruppi di risorse** fare clic sull'icona della pattumiera per eliminare questo riquadro.

1. Da Raccolta riquadri aggiungere i seguenti riquadri:

    - Identità dell'organizzazione
    - Utenti e gruppi
    - User Activity Summary (Riepilogo attività dell'utente)
    - Welcome to the Azure AD Admin Center (Benvenuti al centro di amministrazione di Azure AD)

1. Riposizionare i riquadri in base alle esigenze e fare clic su **Fatto**.

## <a name="share-a-dashboard"></a>Condividere un dashboard

Ora si vuole rendere disponibile questo dashboard per altri utenti. Per eseguire questa operazione, seguire la procedura riportata:

1. Assicurarsi che il dashboard Azure Active Directory (Azure AD) Admin (Dashboard di amministrazione di Azure AD) sia selezionato e fare clic su **Condividi**.

1. Nel pannello **Condivisione e controllo dell'accesso** assicurarsi che **Esegue la pubblicazione nel gruppo di risorse "dashboard"** sia selezionato.

1. Impostare la **Località** appropriata per l'area geografica. In genere, questo valore viene impostato automaticamente sul centro dati più vicino.

1. Fare clic su **Pubblica** e chiudere il pannello **Condivisione e controllo di accesso**.

1. Fare clic su **Azure AD Admin Dashboard** (Dashboard di amministrazione di Azure AD) e selezionare **Customer Dashboard** (Dashboard clienti).

    In **Tutte le risorse** è presente una risorsa del dashboard condiviso e in **Gruppi di risorse** è presente anche un gruppo di risorse dei dashboard.

1. Ripetere i passaggi da 1 a 3 per condividere il Customer Dashboard (Dashboard cliente).

## <a name="edit-a-dashboardjson-file"></a>Modificare il file con estensione json di un dashboard

Per illustrare il modo in cui è possibile scaricare e modificare un file dashboard, attenersi alla procedura seguente:

1. Fare clic su **Scarica**.

1. Aprire Esplora risorse e passare alla cartella Download.

1. Trovare il file *Customer Dashboard.json* e fare doppio clic su di esso.

1. Nell'editor di file cercare il testo *ClockPart*.

1. Nella prima occorrenza di ClockPart modificare il valore **rowSpan** precedente su 1.

1. Anche nella seconda occorrenza di ClockPart modificare il valore **rowSpan** precedente su 1.

1. Nella seconda occorrenza di Clockpart modificare il valore Y da 2 a 1.

1. Salvare il file *Customer Dashboard.json* e chiudere l'editor di codice.

1. Nel dashboard di Azure fare clic su **Carica**.

1. Nella finestra di dialogo **Apri** individuare la cartella Download e fare doppio clic su *Customer Dashboard.json*.

    Gli orologi si sono ridimensionati in una riga in alto e l'orologio inferiore è stato spostato in alto di una riga.

## <a name="select-a-shared-dashboard"></a>Selezionare un dashboard condiviso

Si è valutato che gli orologi di dimensioni inferiori non sono adatti e si vuole ripristinare la versione precedente condivisa del Customer Dashboard (Dashboard cliente). È possibile farlo modificando il file e caricandolo nuovamente o accedendo alla versione condivisa. Per eseguire questa operazione, eseguire la procedura seguente:

1. Fare clic sulla freccia in giù accanto a **Customer Dashboard** (Dashboard cliente).

1. Fare clic su **Esplora tutti i dashboard**.

1. Nel pannello **Tutti i dashboard** in **TIPO** selezionare **Dashboard condivisi**.

1. Fare clic su **Customer Dashboard** (Dashboard cliente).

1. Chiudere il pannello **Tutti i dashboard**.

    Gli orologi sono ritornati alle dimensioni originali.

## <a name="switch-to-full-screen"></a>Passare alla modalità schermo intero

1. Fare clic sulla freccia in giù accanto a **Customer Dashboard** (Dashboard cliente). 

    È presente un altro Customer Dashboard (Dashboard cliente), senza il simbolo condiviso accanto a esso. Fare clic sulla versione del Customer Dashboard (Dashboard cliente) e le dimensioni degli orologi si ridimensionano nuovamente.

1. Tornare al Customer Dashboard (Dashboard cliente) condiviso.

1. Fare clic sul pulsante **Schermo intero**. 

    I menu e le barre del browser non vengono più visualizzati.

1. Fare clic su **Chiudi la visualizzazione schermo intero** per tornare allo schermo normale.

## <a name="unshare-a-dashboard"></a>Annullare la condivisione del dashboard

Se si vuole impedire che un dashboard condiviso sia disponibile per la selezione, è possibile _annullarne la condivisione_. Per annullare la condivisione di un dashboard, eseguire la procedura seguente:

1. Fare clic sul pulsante **Annulla la condivisione**. Viene visualizzato il pannello **Condivisione + Controllo dell'accesso**.

1. Fare clic sul pulsante **Annulla pubblicazione**.

1. Nel messaggio di conferma fare clic su **OK** .

1. Fare clic sulla freccia in giù accanto a **Customer Dashboard** (Dashboard cliente).

1. Fare clic su **Esplora tutti i dashboard**.

1. Nel pannello **Tutti i dashboard** in **TIPO** selezionare **Dashboard condivisi**.

    Il **Customer Dashboard** (Dashboard cliente) non viene più visualizzato nell'elenco dei dashboard disponibili.

1. Chiudere il pannello **Tutti i dashboard**.

## <a name="delete-a-dashboard"></a>Eliminare un dashboard

1. Assicurarsi che il dashboard **Azure AD Admin** (Dashboard di amministrazione di Azure AD) sia selezionato.

1. Fare clic sul pulsante **Elimina**.

1. Nella finestra di messaggio **Conferma** selezionare la casella di controllo per confermare che questo dashboard non sia più visibile e fare clic su **OK**.

## <a name="reset-a-dashboard"></a>Reimpostare un dashboard

1. Assicurarsi che **Customer Dashboard** (Dashboard cliente) sia selezionato.

1. Fare clic su **Modifica**.

1. Fare clic con il pulsante destro del mouse sull'area di lavoro e fare clic su **Ripristina valori predefiniti**.

1. Nella finestra di messaggio **Reset dashboard to default state** (Ripristina lo stato predefinito del dashboard) fare clic su **Sì**.

    Sono stati ripristinati i riquadri predefiniti del Customer Dashboard (Dashboard cliente).

1. Fare clic su **Fatto**.

1. Fare clic sul nome in alto a destra nel portale.

1. Fare clic su **Esci**.

1. Chiudere il browser.

## <a name="summary"></a>Riepilogo

In questo esercizio sono stati creati e modificati dashboard, sono stati condivisi, alterati come file con estensione **json**, è stata annullata la relativa condivisione e infine è stato ripristinato lo stato predefinito. A questo punto è facile comprendere quali potenti strumenti siano i dashboard e come è possibile usarli per creare interfacce efficaci per i diversi ruoli all'interno dell'organizzazione.
