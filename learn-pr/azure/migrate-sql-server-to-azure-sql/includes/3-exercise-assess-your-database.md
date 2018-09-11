In questa unità si valuterà un database esistente mediante Data Migration Assistant e si rivedranno le funzionalità usate nell'istanza di SQL Server locale che non sono attualmente supportate dal database SQL di Azure.

## <a name="setup"></a>Configurazione

1. [Installare **Data Migration Assistant**](https://www.microsoft.com/download/details.aspx?id=53595), se non lo si è già fatto.

1. Sarà necessaria un'istanza di SQL Server in esecuzione. Assicurarsi pertanto di avere i dettagli sulla connessione a portata di mano.

<!-- 1. [**** likely replace with an LOD VM *****] TODO: -->

1. Aprire un browser Internet e passare all'indirizzo https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.

1. In **Download OLTP** fare clic su **AdventureWorks2008R2.bak** e salvarlo nel computer locale.

1. In Management Studio ripristinare l'istanza predefinita di *AdventureWorks 2008R2*.

## <a name="create-an-assessment"></a>Creare una valutazione

1. Avviare **Microsoft Data Migration Assistant**.

1. Nel riquadro di spostamento sinistro dell'app fare clic su __+__ per creare un nuovo progetto Data Migration Assistant.

1. Specificare le opzioni seguenti:

    - **Tipo di progetto**: selezionare *Valutazione*
    - **Nome progetto**: immettere un nome per il progetto, ad esempio "Bicycle DB Assessment"
    - **Tipo del server di origine**: selezionare *SQL Server*
    - **Tipo del server di destinazione**: selezionare *Database SQL di Azure*

1. Fare clic su **Crea**.
    ![Screenshot che mostra la configurazione descritta in Data Migration Assistant per i dati SQL Server AdventureWorks.](../media-draft/3-create-assessment.png)

1. Selezionare il tipo di report di valutazione. Selezionare entrambe le opzioni:
    - Verifica la compatibilità del database
    - Verifica parità delle funzionalità

1. Fare clic su **Avanti**.

## <a name="add-databases-to-assess"></a>Aggiungere i database da valutare

1. Fare clic su **Aggiungi origini** per aprire il menu della connessione.

1. Eseguire le operazioni seguenti:

    - Immettere il nome dell'istanza di SQL Server esistente
    - Selezionare il tipo **Autenticazione**
    - Specificare le proprietà di connessione per il server

1. Fare clic su **Connetti**.

1. In **Aggiungi origini** selezionare i database da valutare. Per questo esercizio selezionare **AdventureWorks2008R2**.

1. Fare clic su **Aggiungi**.
    > [!NOTE]
    > Per aggiungere database da più istanze di SQL Server, usare il pulsante **Aggiungi origini**. Per rimuovere più database, tenere premuto MAIUSC+CTRL per selezionare i database da rimuovere, quindi fare clic su **Rimuovi origini**.

1. Fare clic su **Avvia valutazione**.

## <a name="view-results"></a>Visualizzare i risultati

Se sono presenti più database, i risultati relativi a ogni database vengono visualizzati appena disponibili. Non è necessario attendere che venga completata la valutazione di tutti i database.

1. Una volta completata la valutazione di **AdventureWorks**, fare clic su **Problemi di compatibilità** e su **Funzionalità consigliate** per visualizzare i risultati.

    - La categoria di parità delle funzionalità di SQL Server elenca le funzionalità che potrebbero non essere completamente supportate e la procedura per risolvere questi problemi. I problemi di parità delle funzionalità non interrompono una migrazione.
    - La categoria dei problemi di compatibilità elenca le funzionalità che potrebbero bloccare una migrazione e la procedura per risolvere questi problemi.

## <a name="summary"></a>Riepilogo

In questa unità è stato valutato un database di SQL Server installato in locale per verificare la presenza di eventuali funzionalità che non sarebbero disponibili dopo la migrazione del database al database SQL di Azure.