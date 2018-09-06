Prima di connettere il database all'app, è necessario controllare che sia possibile connettersi, aggiungere una tabella di base e lavorare con dati di esempio.

L'infrastruttura, gli aggiornamenti software e le patch per il database SQL di Azure sono gestiti da Microsoft. A parte questo, è possibile gestire il database SQL di Azure come qualsiasi altra installazione di SQL Server. È ad esempio possibile usare Visual Studio, SQL Server Management Studio o altri strumenti per gestire il database SQL di Azure.

È possibile scegliere come accedere al database e connetterlo all'app. Tuttavia, per acquisire familiarità con il database, verrà descritto come connettersi direttamente al database dal portale, creare una tabella ed eseguire alcune operazioni CRUD di base. Si apprenderà:

* Che cos'è Cloud Shell e come accedervi dal portale.
* Come accedere alle informazioni sul database dall'interfaccia della riga di comando di Azure, incluse le stringhe di connessione.
* Come connettersi al database tramite `sqlcmd`.
* Come inizializzare il database con una tabella di base e alcuni dati di esempio.

## <a name="what-is-azure-cloud-shell"></a>Che cos'è Azure Cloud Shell?

Azure Cloud Shell è un'esperienza shell basata su browser per gestire e sviluppare risorse di Azure. Cloud Shell può essere paragonato a una console interattiva che viene eseguita nel cloud.

In background, Cloud Shell viene eseguito su Linux. Tuttavia, a seconda del fatto che si preferisca un ambiente Linux o Windows, è possibile scegliere tra due esperienze: Bash e PowerShell.

Cloud Shell è accessibile da qualsiasi posizione. Oltre che dal portale, è possibile accedere a Cloud Shell anche da [shell.azure.com](https://shell.azure.com/), dall'app per dispositivi mobili di Azure o da Visual Studio Code.

Cloud Shell include strumenti ed editor di testo molto diffusi. Di seguito è riportata una breve descrizione di `az`, `jq` e `sqlcmd`, tre strumenti che verranno usati per l'attività corrente.

* `az` è anche noto come interfaccia della riga di comando di Azure. Si tratta dell'interfaccia della riga di comando per l'uso delle risorse di Azure. Verrà usata per ottenere informazioni sul database, compresa la stringa di connessione.
* `jq` è un parser JSON della riga di comando. È possibile inviare tramite pipe l'output dei comandi `az` a questo strumento per estrarre i campi importanti dall'output JSON.
* `sqlcmd` consente di eseguire istruzioni in SQL Server. Si userà `sqlcmd` per creare una sessione interattiva con il database SQL di Azure.

## <a name="get-information-about-your-azure-sql-database"></a>Ottenere informazioni sul database SQL di Azure

Prima di connettersi al database, è consigliabile verificare che esista e che sia online.

In questo caso, si aprirà Cloud Shell e si userà l'utilità `az` per elencare i database e visualizzare alcune informazioni sul database **Logistics**, tra cui la dimensione massima e lo stato.

1. Nel portale fare clic su **Cloud Shell** nella parte superiore. 
    ![Apertura di Cloud Shell](../media-draft/open-cloud-shell.png)
1. I comandi `az` da eseguire richiedono il nome del gruppo di risorse e il nome del server logico SQL di Azure. Per evitare di digitarli, eseguire questo comando `azure configure` per specificare i valori come valori predefiniti.
    Sostituire `contoso-logistics` con il nome del server logico SQL di Azure.
    ```azurecli
    az configure --defaults group=logistics-db-rg sql-server=contoso-logistics
    ```
1. Eseguire `az sql db list` per elencare tutti i database nel server logico SQL di Azure.
    ```azurecli
    az sql db list
    ```
    Si noterà un blocco di codice JSON di grandi dimensioni come output. 
1. Poiché si è interessati solo ai nomi dei database, eseguire il comando una seconda volta. Questa volta, inviare tramite pipe l'output a `jq` per visualizzare solo i campi del nome.
    ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    Verrà visualizzato quanto segue.
    ```console
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```
    **Logistics** è il database. Come in SQL Server, **master** include i metadati del server, come gli account di accesso e le impostazioni di configurazione di sistema.
1. Eseguire questo comando `az sql db show` per ottenere informazioni dettagliate sul database **Logistics**. 
    ```azurecli
    az sql db show --name Logistics
    ```
    Come nel caso precedente, si noterà un blocco di codice JSON di grandi dimensioni come output.
1. Eseguire il comando una seconda volta. Questa volta inviare tramite pipe l'output a `jq` per limitare l'output solo al nome, alla dimensione massima e allo stato del database **Logistics**.
    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```
    Si noterà che il database è online e può contenere circa 2 GB di dati.
    ```console
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a>Connettersi al database

Dopo aver ottenuto queste informazioni sul database, è possibile connettersi tramite `sqlcmd`, creare una tabella con informazioni sugli autisti ed eseguire alcune operazioni CRUD di base.

CRUD è l'acronimo di **create**, **read**, **update** e **delete**. Si tratta delle operazioni eseguite sui dati di una tabella e sono le quattro operazioni di base necessarie per l'app. In questa fase, è consigliabile verificare che sia possibile eseguire ognuna di esse.

1. Eseguire questo comando `az sql db show-connection-string` per ottenere la stringa di connessione per il database **Logistics** in un formato utilizzabile da `sqlcmd`.
    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```
    L'output è simile al seguente.
    ```console
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```
1. Eseguire l'istruzione `sqlcmd` ottenuta dal passaggio precedente per creare una sessione interattiva.
    Rimuovere le virgolette e sostituire `<username>` e `<password>` con il nome utente e la password specificati durante la creazione del database. Ecco un esempio.
    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```
    > [!TIP]
    > Inserire la password tra virgolette in modo che "&" e altri caratteri speciali non vengano interpretati come istruzioni di elaborazione.
1. Dalla sessione `sqlcmd` creare una tabella denominata `Drivers`.
    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255) );
    GO
    ```
    La tabella contiene quattro colonne: un identificatore univoco, il nome e il cognome dell'autista e la città di origine dell'autista.
    > [!NOTE]
    > Il linguaggio visualizzato qui è Transact-SQL, o T-SQL.
1. Verificare che la tabella `Drivers` esista.
    ```sql
    SELECT name FROM sys.tables;
    GO
    ```
    Verrà visualizzato quanto segue.
    ```console
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers
    
    (1 rows affected)
    ```
1. Eseguire questa istruzione T-SQL `INSERT` per aggiungere una riga di esempio per la tabella. Questa è l'operazione **create**.
    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Orton', 'Erick', 'Springfield');
    GO
    ```
    Questo indica che l'operazione è riuscita.
    ```console
    (1 rows affected)
    ```
1. Eseguire questo elenco di istruzioni T-SQL `SELECT` per ottenere la colonna `DriverID` da tutte le righe nella tabella. Questa è l'operazione **read**.
    ```sql
    SELECT DriverID FROM Drivers;
    GO
    ```
    Viene visualizzato un solo risultato, il valore `DriverID` per la riga che è stata creata nel passaggio precedente.
    ```console
    DriverID
    -----------
            123
    
    (1 rows affected)
    ```
1. Eseguire questa istruzione T-SQL `UPDATE` per modificare la città di origine da "Springfield" in "Springfield, OR" per l'autista con un valore `DriverID` pari a 123. Questa è l'operazione **update**.
    ```sql
    UPDATE Drivers SET OriginCity='Springfield, OR' WHERE DriverID=123;
    GO
    ```
    Verrà visualizzato quanto segue.
    ```console
    (1 rows affected)
    ```
1. Eseguire questa istruzione T-SQL `DELETE` per eliminare il record. Questa è l'operazione **delete**.
    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```
    
    ```console
    (1 rows affected)
    ```
1. Eseguire questa istruzione T-SQL `SELECT` per verificare che la tabella `Drivers` sia vuota.
    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```
    È possibile notare che la tabella non contiene righe.
    ```console
    -----------
              0
    
    (1 rows affected)
    ```

## <a name="summary"></a>Riepilogo

Dopo aver acquisito familiarità con l'uso del database SQL di Azure da Cloud Shell, è possibile ottenere la stringa di connessione per il proprio strumento di gestione SQL preferito, che si tratti di SQL Server Management Studio, Visual Studio o un altro strumento.

Cloud Shell rende molto semplice accedere e usare le risorse di Azure. Poiché Cloud Shell è basato su browser, è possibile accedervi da Windows, macOS o Linux: essenzialmente da qualsiasi sistema dotato di un Web browser.

È stata acquisita esperienza pratica su come eseguire i comandi dell'interfaccia della riga di comando di Azure per ottenere informazioni sul database di SQL Azure. Sono inoltre state applicate le competenze relative a T-SQL.

Per concludere, vediamo come eliminare il database.