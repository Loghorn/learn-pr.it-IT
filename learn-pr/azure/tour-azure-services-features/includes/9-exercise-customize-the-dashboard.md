In questo esercizio verrà creato e configurato un dashboard personalizzato.

## <a name="create-a-new-dashboard"></a>Creare un nuovo dashboard

1. Nel portale di Azure fare clic sul pulsante **Nuovo dashboard**.

2. Nella casella denominata **My Dashboard** (Dashboard) modificare il nome in **Customer Dashboard**  (Dashboard cliente).

## <a name="add-and-configure-the-clock-tile"></a>Aggiungere e configurare il riquadro dell'orologio

1. Nella raccolta riquadri trascinare l'orologio nell'area di lavoro. Posizionarlo nella parte superiore destra dello spazio disponibile.

2. Nel pannello **Modifica orologio** modificare la località in **Pacifico (USA e Canada)**.

3. In **Formato ora** fare clic su **24 ore**.

4. Fare clic su **Fatto**.

5. Ripetere i quattro passaggi precedenti, ad eccezione della selezione di **Fuso orientale (USA e Canada)**. Dovrebbero essere ora presenti due orologi, uno che indica l'ora della costa occidentale, l'altro quella della costa orientale.

## <a name="resize-a-tile"></a>Ridimensionare un riquadro

1. Nel riquadro **Raccolta riquadri** fare clic su **Tutte le risorse** e rilasciare il riquadro in alto a sinistra nella nuova area di lavoro del dashboard.

2. Fare clic sul riquadro, fare clic con il pulsante destro del mouse sui puntini di sospensione e quindi selezionare **6x6**.

3. Fare clic sull'angolo grigio nella parte inferiore destra del riquadro e ridimensionare il riquadro su 3,5 in senso verticale e su 6 in senso orizzontale. Quando si rilascia l'angolo, questo viene ridimensionato su 4x6.

4. In Raccolta riquadri fare clic sul riquadro **Gruppi di risorse** e trascinarlo nell'area di lavoro. Inserirlo sotto il riquadro **Tutte le risorse**.

5. In Raccolta riquadri fare clic sul riquadro **Integrità del servizio** e trascinarlo nell'area di lavoro. Inserirlo a destra del riquadro **Tutte le risorse**.

6. Continuare ad aggiungere i riquadri seguenti, ridisporli per adattarli:

    * Guida + supporto
    * Attività rapide
    * Marketplace
    * Novità

7. Dopo aver aggiunto questi riquadri, fare clic su **Fatto**. Viene visualizzato il dashboard **Dashboard cliente**.

## <a name="clone-a-dashboard"></a>Clonare un dashboard

Ora si vuole creare un dashboard molto simile per altri clienti.

1. Fare clic sul pulsante **Clona**.

2. Rinominare il dashboard da **Clone of Customer Dashboard** (Clone del dashboard cliente) in **Azure AD Admin Dashboard** (Dashboard di amministrazione di Azure AD).

3. Nel riquadro **Gruppi di risorse** fare clic sull'icona della pattumiera per eliminare questo riquadro.

4. Da Raccolta riquadri aggiungere i seguenti riquadri:

    * Identità dell'organizzazione
    * Utenti e gruppi
    * User Activity Summary (Riepilogo attività dell'utente)
    * Welcome to the Azure AD Admin Center (Benvenuti al centro di amministrazione di Azure AD)

5. Riposizionare i riquadri in base alle esigenze e fare clic su **Fatto**.

## <a name="share-a-dashboard"></a>Condividere un dashboard

Ora si vuole rendere disponibile questo dashboard per altri utenti. Per eseguire questa operazione, seguire la procedura riportata:

1. Assicurarsi che il dashboard Azure Active Directory (Azure AD) Admin (Dashboard di amministrazione di Azure AD) sia selezionato e fare clic su **Condividi**.

2. Nel pannello **Condivisione e controllo dell'accesso** assicurarsi che **Esegue la pubblicazione nel gruppo di risorse "dashboard"** sia selezionato.

3. Impostare la **Località** appropriata per l'area geografica. In genere, questo valore viene impostato automaticamente sul centro dati più vicino.

4. Fare clic su **Pubblica** e chiudere il pannello **Condivisione e controllo di accesso**.

5. Fare clic su **Azure AD Admin Dashboard** (Dashboard di amministrazione di Azure AD) e selezionare **Customer Dashboard** (Dashboard clienti).

    In **Tutte le risorse** è presente una risorsa del dashboard condiviso e in **Gruppi di risorse** è presente anche un gruppo di risorse dei dashboard.

6. Ripetere i passaggi da 1 a 3 per condividere il Customer Dashboard (Dashboard cliente).

## <a name="edit-a-dashboardjson-file"></a>Modificare il file con estensione json di un dashboard

Per illustrare il modo in cui è possibile scaricare e modificare un file dashboard, attenersi alla procedura seguente:

1. Fare clic su **Scarica**.

2. Aprire Esplora risorse e passare alla cartella Download.

3. Trovare il file *Customer Dashboard.json* e fare doppio clic su di esso.

4. Nell'editor di file cercare il testo *ClockPart*.

5. Nella prima occorrenza di ClockPart modificare il valore **rowSpan** precedente su 1.

6. Anche nella seconda occorrenza di ClockPart modificare il valore **rowSpan** precedente su 1.

7. Nella seconda occorrenza di Clockpart modificare il valore Y da 2 a 1.

8. Salvare il file *Customer Dashboard.json* e chiudere l'editor di codice.

9. Nel dashboard di Azure fare clic su **Carica**.

10. Nella finestra di dialogo **Apri** individuare la cartella Download e fare doppio clic su *Customer Dashboard.json*.

    Gli orologi si sono ridimensionati in una riga in alto e l'orologio inferiore è stato spostato in alto di una riga.

## <a name="select-a-shared-dashboard"></a>Selezionare un dashboard condiviso

Si è valutato che gli orologi di dimensioni inferiori non sono adatti e si vuole ripristinare la versione precedente condivisa del Customer Dashboard (Dashboard cliente). È possibile farlo modificando il file e caricandolo nuovamente o accedendo alla versione condivisa. Per eseguire questa operazione, eseguire la procedura seguente:

1. Fare clic sulla freccia in giù accanto a **Customer Dashboard** (Dashboard cliente).

2. Fare clic su **Esplora tutti i dashboard**.

3. Nel pannello **Tutti i dashboard** in **TIPO** selezionare **Dashboard condivisi**.

4. Fare clic su **Customer Dashboard** (Dashboard cliente).

5. Chiudere il pannello **Tutti i dashboard**.

    Gli orologi sono ritornati alle dimensioni originali.

## <a name="switch-to-full-screen"></a>Passare alla modalità schermo intero

1. Fare clic sulla freccia in giù accanto a **Customer Dashboard** (Dashboard cliente). 

    È presente un altro Customer Dashboard (Dashboard cliente), senza il simbolo condiviso accanto a esso. Fare clic sulla versione del Customer Dashboard (Dashboard cliente) e le dimensioni degli orologi si ridimensionano nuovamente.

2. Tornare al Customer Dashboard (Dashboard cliente) condiviso.

3. Fare clic sul pulsante **Schermo intero**. 

    I menu e le barre del browser non vengono più visualizzati.

4. Fare clic su **Chiudi la visualizzazione schermo intero** per tornare allo schermo normale.

## <a name="unshare-a-dashboard"></a>Annullare la condivisione del dashboard

Se si vuole impedire che un determinato dashboard condiviso sia disponibile per la selezione, è possibile annullare la condivisione. Per annullare la condivisione di un dashboard, eseguire la procedura seguenti:

1. Fare clic sul pulsante **Annulla la condivisione**. Viene visualizzato il pannello **Condivisione + Controllo dell'accesso**.

2. Fare clic sul pulsante **Annulla pubblicazione**.

3. Nel messaggio di conferma fare clic su **OK** .

4. Fare clic sulla freccia in giù accanto a **Customer Dashboard** (Dashboard cliente).

5. Fare clic su **Esplora tutti i dashboard**.

6. Nel pannello **Tutti i dashboard** in **TIPO** selezionare **Dashboard condivisi**.

    Il **Customer Dashboard** (Dashboard cliente) non viene più visualizzato nell'elenco dei dashboard disponibili.

7. Chiudere il pannello **Tutti i dashboard**.

## <a name="delete-a-dashboard"></a>Eliminare un dashboard

1. Assicurarsi che il dashboard **Azure AD Admin** (Dashboard di amministrazione di Azure AD) sia selezionato.

2. Fare clic sul pulsante **Elimina**.

3. Nella finestra di messaggio **Conferma** selezionare la casella di controllo per confermare che questo dashboard non sia più visibile e fare clic su **OK**.

## <a name="reset-a-dashboard"></a>Reimpostare un dashboard

1. Assicurarsi che **Customer Dashboard** (Dashboard cliente) sia selezionato.

2. Fare clic su **Modifica**.

3. Fare clic con il pulsante destro del mouse sull'area di lavoro e fare clic su **Ripristina valori predefiniti**.

4. Nella finestra di messaggio **Reset dashboard to default state** (Ripristina lo stato predefinito del dashboard) fare clic su **Sì**.

    Sono stati ripristinati i riquadri predefiniti del Customer Dashboard (Dashboard cliente).

5. Fare clic su **Fatto**.

6. Fare clic sul nome in alto a destra nel portale.

7. Fare clic su **Esci**.

8. Chiudere il browser.

## <a name="summary"></a>Riepilogo

In questo esercizio sono stati creati e modificati dashboard, sono stati condivisi, alterati come file con estensione **json**, è stata annullata la relativa condivisione e infine è stato ripristinato lo stato predefinito. A questo punto è facile comprendere quali potenti strumenti siano i dashboard e come è possibile usarli per creare interfacce efficaci per i diversi ruoli all'interno dell'organizzazione.

