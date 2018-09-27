Il punto debole più comune nelle applicazioni moderne, a livello di sicurezza, è la non corretta elaborazione dell'input ricevuto da origini esterne, in particolare l'_input dell'utente_. È importante esaminare sempre attentamente qualsiasi input per assicurarsi che sia stato convalidato prima essere usato. In caso contrario, le conseguenze potrebbero essere l'esposizione o la perdita di dati, l'escalation di privilegi o persino l'esecuzione di codice dannoso nei computer di altri utenti.

L'aspetto tragico è che questo problema è in realtà facile da risolvere. In questa unità verrà illustrato come trattare i dati, quando vengono ricevuti, quando vengono visualizzati sullo schermo e quando vengono archiviati per uso futuro.

## <a name="why-do-we-need-to-validate-our-input"></a>Perché è necessario convalidare l'input?

Immaginiamo di voler compilare un'interfaccia che consenta a un utente di creare un account sul sito Web. I dati del profilo includono un nome, un indirizzo di posta elettronica e un nome alternativo che saranno visibili a chiunque visiti il sito. Supponiamo che un nuovo utente crei un profilo e immetta un nome alternativo che include alcuni comandi SQL. Un utente malintenzionato potrebbe ad esempio immettere qualcosa di simile a questo:

```sql
Eve'); DROP TABLE Users;--
```

Se si inserisse ciecamente questo valore in un database, potrebbe alterare l'istruzione SQL in modo da eseguire comandi potenzialmente dannosi. Questo comportamento è detto "attacco SQL injection" ed è uno dei _numerosi_ tipi di exploit possibili quando non si gestiscono correttamente gli input. Come si può risolvere questo problema? In questa unità verrà illustrato quando convalidare l'input, come codificare l'output e creare query con parametri, risolvendo l'exploit descritto sopra. Queste sono le tre principali tecniche di difesa dagli input dannosi che vengono immessi nelle applicazioni.

## <a name="when-do-i-need-to-validate-input"></a>Quando è necessario convalidare l'input?

La risposta è _sempre_. Occorre convalidare **ogni** input per l'applicazione. Sono inclusi i parametri nell'URL, l'input dell'utente, i dati del database, i dati di un'API e qualsiasi elemento passato in formato non crittografato che potrebbe essere modificato da un utente. Adottare sempre un approccio in base a cui accettare solo input notoriamente valido, invece di cercare specificamente l'input non valido, in quanto è impossibile avere un elenco completo di input potenzialmente pericolosi.  Fare tutto questo sul lato server, non sul lato client (oppure su entrambi i lati), per assicurarsi che le difese non vengano aggirate. Considerare **TUTTI** i dati come non attendibili per proteggersi dalle più comuni vulnerabilità per le app Web.

Se si usa ASP.NET, il framework offre un [ottimo supporto per la convalida dell'input](https://docs.microsoft.com/aspnet/web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites) sia sul lato client che sul lato server.

Se si usa un altro framework Web, sul sito [OWASP](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet) sono descritte alcune ottime tecniche di convalida dell'input.


## <a name="always-use-parameterized-queries"></a>Usare sempre query con parametri

I database SQL vengono comunemente usati per archiviare i dati, ad esempio le informazioni sul profilo.  Evitare sempre di creare SQL inline o altre query di database "all'istante" nel codice e di inviarlo direttamente al database, in quanto è una strada sicura verso il disastro, come abbiamo visto prima.

Ad esempio, **non fare questo** (noto come SQL inline):

```csharp
string userName = ... // receive input from the user BEWARE!
...
string query = "SELECT *  FROM  [dbo].[users] WHERE userName = '" + userName + "'";
```

In questo esempio vengono concatenate più stringhe di testo per creare la query, recuperando l'input dall'utente e generando una query SQL dinamica per cercare l'utente. Anche in questo caso, se un utente malintenzionato capisse quale azione si sta eseguendo o anche solo _provasse_ diversi stili di input per rilevare eventuali vulnerabilità, le conseguenze potrebbero essere estremamente gravi. Usare invece istruzioni SQL con parametri o stored procedure, come illustrato di seguito:

```sql
-- Lookup a user
CREATE PROCEDURE sp_findUser
(
@UserName varchar(50)
)

SELECT *  FROM  [dbo].[users] WHERE userName = @UserName
```

Con questo metodo, è possibile quindi richiamare la stored procedure dal codice in tutta sicurezza, passandole la stringa `userName` senza preoccuparsi che sia considerata parte dell'istruzione SQL.

## <a name="always-encode-your-output"></a>Codificare sempre l'output

Qualsiasi output presentato visivamente o all'interno di un documento deve sempre essere codificato e preceduto da un carattere di escape. In questo modo ci si protegge in caso di omissioni nel processo di purificazione o qualora il codice generi accidentalmente un elemento che può essere usato in modo dannoso. Ci si assicura così che ogni elemento venga visualizzato come _output_ e non venga inavvertitamente interpretato come qualcosa che deve essere eseguito. Questa è un'altra tecnica di attacco molto comune nota come XSS (Cross-Site Scripting), ossia un attacco tramite script da altri siti.

Essendo un requisito comune, questa è un'altra area in cui ASP.NET può agire automaticamente. Per impostazione predefinita, tutto l'output è già codificato. Se si usa un altro framework Web, è possibile verificare le opzioni impostate per la codifica dell'output nei siti Web usando le [informazioni di riferimento sulla prevenzione degli attacchi XSS sul sito OWASP](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet).

## <a name="summary"></a>Riepilogo

La purificazione e la convalida dell'input sono requisiti necessari per assicurarsi che l'input sia valido e possa essere usato e archiviato in tutta sicurezza. Framework Web più moderni offrono funzionalità predefinite che consentono di automatizzare parte di queste operazioni. È possibile controllare la documentazione del framework preferito per scoprire le funzionalità offerte. Sebbene le applicazioni Web siano più a rischio, tenere presente che altri tipi di applicazioni possono essere altrettanto vulnerabili. Non pensare di essere al sicuro solo perché la nuova applicazione è un'app desktop. È comunque necessario gestire correttamente l'input dell'utente per assicurare che nessuno possa usare l'app per danneggiare i dati o la reputazione dell'azienda.