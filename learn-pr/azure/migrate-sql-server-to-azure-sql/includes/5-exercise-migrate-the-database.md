Dopo aver valutato il database e corretto gli eventuali errori segnalati, si è pronti per eseguire la migrazione del database. In questa unità si eseguirà la migrazione di un database SQL al database SQL di Azure.

## <a name="setup"></a>Configurazione

Se non è già stato fatto, scaricare i dati di esempio.

1. Aprire un browser Internet e passare all'indirizzo https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017.

1. In **Download OLTP** fare clic su **AdventureWorks2008R2.bak**.

1. In Management Studio ripristinare l'istanza predefinita di **AdventureWorks 2008R2**.

## <a name="create-an-azure-sql-database"></a>Creare un database SQL di Azure

1. Accedere all'account del portale di Azure.

1. Fare clic su **Crea una risorsa** nell'angolo superiore sinistro del portale.

1. Fare clic su **Database** > **Database SQL**.

1. Compilare i seguenti campi nel modulo Database SQL:

    |Campo|Descrizione|
    |-----|---|
    |Nome database|Immettere un nome per il database. Può essere il nome del database esistente di cui si sta eseguendo la migrazione o un nuovo nome di propria scelta|
    |Gruppo di risorse|Selezionare un gruppo di risorse o immettere un nome per il nuovo gruppo di risorse|
    |Selezionare l'origine|Selezionare *Database vuoto*|

1. In **Server** selezionare un server esistente oppure immettere un **nome server** univoco a livello globale, un **nome di accesso amministratore server**, una **password** (che è necessario confermare) e una **posizione**.

    > [!NOTE]
    > Il nome del server, il nome di accesso e la password verranno usati per connettersi al database.

1. Fare clic su **Seleziona**.

1. In **Piano tariffario** è possibile selezionare un database con prestazioni superiori, ma per questa esercitazione selezionare il valore predefinito.

1. Fare clic su **Crea**. Attendere il completamento del provisioning del database prima di continuare l'esercitazione.

    Lo screenshot seguente mostra una potenziale configurazione per il nuovo database SQL.

    ![Screenshot del pannello di un nuovo database SQL nel portale di Azure con una configurazione di esempio.](../media-draft/5-create-azure-sql-db.png)

## <a name="create-a-new-migration-project"></a>Creare un nuovo progetto di migrazione

1. Avviare **Data Migration Assistant**.

1. Scegliere l'icona **Nuovo** e specificare le opzioni seguenti:

    - **Tipo di progetto**: selezionare l'opzione *Migrazione*.
    - **Nome progetto**: immettere un nome facile da ricordare per il progetto.
    - **Tipo del server di origine**: selezionare *SQL Server*.
    - **Tipo del server di destinazione**: selezionare *Database SQL di Azure*.
    - **Ambito migrazione**: selezionare *Schema e dati*.

1. Fare clic su **Crea**.

## <a name="specify-the-source-server-and-database"></a>Specificare il server e il database di origine

1. In **Connect to source server** (Connessione al server di origine), nel campo **Nome server**, immettere il nome dell'istanza di SQL Server di origine.

1. Selezionare il **tipo di autenticazione** supportato dall'istanza di SQL Server di origine.
    > [!TIP]
    > È consigliabile selezionare la casella di controllo **Crittografa connessione** in Proprietà connessione per crittografare la connessione.

1. Selezionare **Connetti**.

1. Selezionare un singolo database di origine per eseguire la migrazione al database SQL di Azure e fare clic su **Avanti**.

## <a name="specify-the-target-server-and-database"></a>Specificare il server e il database di destinazione

1. In **Connect to target server** (Connessione al server di destinazione), nel campo **Nome server**, immettere il nome dell'istanza del database SQL di Azure. Aggiungere **database.windows.net** all'istanza del database.

1. Selezionare il tipo di autenticazione supportato dall'istanza del database SQL di Azure di destinazione. Per questa esercitazione, selezionare **Autenticazione di SQL Server**.
    > [!TIP]
    > È consigliabile selezionare la casella di controllo **Crittografa connessione** in Proprietà connessione per crittografare la connessione.

1. Immettere il **nome utente** e la **password** definiti al momento della creazione del database SQL di Azure.

1. Fare clic su **Connetti**.

1. Selezionare un singolo database di destinazione in cui eseguire la migrazione.
    > [NOTA] Se si prevede di eseguire la migrazione di utenti di Windows, nella casella di testo Nome di dominio dell'utente esterno di destinazione assicurarsi che il nome di dominio dell'utente esterno di destinazione sia specificato correttamente.

1. Fare clic su **Avanti**.

## <a name="select-schema-objects"></a>Selezionare gli oggetti dello schema

1. Selezionare gli oggetti dello schema dal database di origine di cui si vuole eseguire la migrazione al database SQL di Azure.

    > [!NOTE]
    > Vengono presentati alcuni degli oggetti che non possono essere convertiti così come sono, offrendo l'opportunità di eseguire la correzione automatica. Facendo clic su questi oggetti nel riquadro sinistro, vengono visualizzate le correzioni suggerite nel riquadro destro. Esaminare le correzioni e scegliere se applicare o ignorare tutte le modifiche, per ogni singolo oggetto. Si noti che il fatto di applicare o ignorare tutte le modifiche per un oggetto non influisce sulle modifiche degli altri oggetti di database. Le istruzioni che non possono essere convertite o corrette automaticamente vengono riprodotte nel database di destinazione e impostate come commenti.

    ![Screenshot di Data Migration Assistant che mostra l'elenco dei problemi di migrazione relativi ai dati SQL Server AdventureWorks con il problema "Stored procedure: dbo.uspSearchCandidateResumes" selezionato e i dettagli sul problema visualizzati.](../media-draft/5-suggested-fix.png)

1. Fare clic su **Genera script SQL**.

## <a name="deploy-schema"></a>Distribuire lo schema

1. Fare clic su **Distribuisci schema**.

1. Esaminare i risultati della distribuzione dello schema.

## <a name="migrate-data"></a>Eseguire la migrazione dei dati

1. Fare clic su **Esegui la migrazione dei dati** per avviare il processo di migrazione dei dati.

1. Selezionare le tabelle con i dati di cui eseguire la migrazione.

1. Fare clic su **Esegui la migrazione dei dati**.

La schermata finale mostra lo stato complessivo.

## <a name="summary"></a>Riepilogo

In questa unità è stato creato un database SQL di Azure vuoto ed è stata eseguita la migrazione di un database di SQL Server locale in questo nuovo database.