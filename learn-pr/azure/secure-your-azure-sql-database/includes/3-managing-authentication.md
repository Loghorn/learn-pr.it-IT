Si supponga che si esegue un sito Web e un utente malintenzionato ha ottenuto l'accesso al database. L'autore dell'attacco ha ottenuto l'accesso a un account dbo nell'attacco e possono eseguire qualsiasi operazione nel database. Un account dbo puoi accedere e modificare informazioni come metadati ed eseguire tutti i livelli di accesso.

Se solo l'autore dell'attacco ha ottenuto l'accesso a un account utente normale, quindi è stato possibile solo accedere tabelle, viste, stored procedure e altri oggetti definiti per l'account utente. Ad esempio, se un sito Web di accesso a un database è stata compromessa a causa di un attacco SQL injection, l'autore dell'attacco potrebbe essere limitata in cosa potevano.

Nel caso in cui si verifica una violazione della sicurezza, è utile per ridurre l'impatto della violazione. Esaminiamo come limitare l'accesso al database di SQL Azure a livello di utente.

## <a name="reduce-the-attack-surface-of-the-database"></a>Ridurre la superficie di attacco del database

Per ridurre l'impatto di eventuali violazioni di sicurezza, è limitare la superficie di attacco. Se ci si connette al database usando un account dbo o l'amministratore e un utente malintenzionato ottiene accesso al database, l'autore dell'attacco potrà accedere per eseguire tutte le operazioni nel database. L'accesso è stato possibile includere una query di metadati del database, che determina quali dati sono disponibili e/o sensibili e sfruttare queste informazioni.

Evitare rischi di sicurezza creando utenti del database con autorizzazioni limitate. Si userà il termine con privilegi minimi in questo caso, gli utenti devono solo abbiano l'accesso per le tabelle, viste, stored procedure e altre entità necessarie per svolgere il proprio lavoro.

## <a name="create-a-database-user"></a>Creare un utente database

Per creare un utente con privilegi ridotti, si sarà creare un utente del database e quindi associare tale utente al database. È possibile creare un utente di SQL Server e concedere le autorizzazioni utente nel database.

> [!Note]
> Sono disponibili tipi tredici (13) degli utenti in SQL Server. Se è necessario creare un altro tipo di utente del database, usare il collegamento appropriato per scoprire la sintassi corretta. Visualizzare [creare un utente del database](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-database-user?view=sql-server-2017).

Procedure consigliate preferito consiste nel creare l'utente a livello di database. Questo passaggio si evita le autorizzazioni utente da allontanamento i limiti del database che si sta tentando di proteggere.

1. In primo luogo, selezionare il database.
2. Creare quindi l'utente usando la query seguente:

   ```sql
   CREATE USER MyWebAppUser WITH PASSWORD = '<YourSuperStrongPassword>';
   ```

   La query precedente Crea utente *MyWebUser*. Per impostazione predefinita l'utente non sarà possibile accedere a tutte le tabelle, viste o stored procedure.

3. A questo punto, creare delle autorizzazioni appropriate per l'utente aggiungendoli ai ruoli, ad esempio db_datareader e db_datawriter.

   Il ruolo db_datareader consentirà all'utente l'accesso a tutte le tabelle e viste utente all'interno del database. Allo stesso modo, il ruolo db_datawriter verrà consente all'utente di accedere leggere e scrivere, aggiornare ed eliminare righe nel database.

   ```sql
   ALTER ROLE db_datareader ADD MEMBER MyWebAppUser;
   ALTER ROLE db_datawriter ADD MEMBER MyWebAppUser;
   ```

È possibile negare un accesso utente agli altri elementi all'interno del database utilizzando l'operatore di NEGAZIONE. Qui si sta negando MyWebAppUser all'utente la possibilità di selezionare dati dalla tabella Customers.

```sql
DENY SELECT ON Customers TO MyWebAppUser;
```

È possibile usare anche l'autorizzazione GRANT per concedere in modo esplicito le autorizzazioni a un utente o ruolo.

```sql
GRANT SELECT ON Customers TO MyWebAppUser;
```

Si continuerà a migliorare le operazioni sul database per ottenere l'utente per il livello di accesso necessario. Invece di un utente, è possibile creare un ruolo con autorizzazioni minime necessite e quindi aggiungere l'utente al ruolo.

Se sono presenti più utenti che hanno le stesse autorizzazioni, è possibile creare tali utenti come parte del ruolo. L'utente avrà accesso solo per le operazioni necessarie, dopo che tutte le autorizzazioni di accesso sono stato concesse o negate.
Se un utente malintenzionato riesce ad accedere al database tramite l'utente appena creato, essi verranno solo vedere ed eseguire gli stessi dati e le operazioni che l'utente può. Blocco dell'accesso utente notevolmente riduce la superficie di attacco per il database.
