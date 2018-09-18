Il punto debole più comune nelle applicazioni moderne, a livello di sicurezza, è la non corretta elaborazione dell'input ricevuto da origini esterne, in particolare l'_input dell'utente_. È importante esaminare sempre attentamente qualsiasi input per assicurarsi che sia stato convalidato prima di usarlo. In caso contrario, le conseguenze potrebbero essere l'esposizione o perdita di dati, l'escalation di privilegi o persino l'esecuzione di malware nei computer di altri utenti.

L'aspetto tragico è che questo problema è in realtà facile da risolvere. In questa unità verrà illustrato come trattare i dati, quando vengono ricevuti, quando vengono visualizzati sullo schermo e quando vengono archiviati per uso futuro.

## <a name="why-do-we-need-to-validate-our-input"></a>Perché è necessario convalidare l'input?

Immaginiamo di voler compilare un'interfaccia che consenta a un utente di creare un account sul sito Web. I dati del profilo includono un nome, un indirizzo di posta elettronica e un nome alternativo che saranno visibili a chiunque visiti il sito. Supponiamo che un nuovo utente crei un profilo e immetta un nome alternativo che include alcuni comandi SQL. Un utente malintenzionato potrebbe ad esempio immettere qualcosa di simile a questo:

```output
Eve'); DROP TABLE Users;--
```

Se si inserisse ciecamente questo valore in un database, potrebbe alterare l'istruzione SQL in modo da eseguire comandi potenzialmente dannosi. Questo comportamento è detto "attacco SQL injection" ed è uno dei _numerosi_ tipi di exploit possibili quando non si gestiscono correttamente gli input. Come si può risolvere questo problema? In questa unità verrà illustrato come purificare l'input, codificare l'output e creare query con parametri (risolvendo l'exploit descritto sopra). Queste sono le principali tecniche di difesa dagli input dannosi che vengono immessi nelle applicazioni.

## <a name="when-do-i-need-to-validate-input"></a>Quando è necessario convalidare l'input?

La risposta è _sempre_. Occorre convalidare **ogni** input per l'applicazione. Sono inclusi i parametri nell'URL, l'input dell'utente, i dati del database, i dati di un'API e qualsiasi elemento passato in formato non crittografato che potrebbe essere modificato da un utente. Adottare sempre un approccio in base a cui accettare solo input notoriamente valido, invece di cercare specificamente l'input non valido, in quanto è impossibile avere un elenco completo di input potenzialmente pericolosi.  Fare tutto questo sul lato server, non sul lato client (oppure su entrambi i lati), per assicurarsi che le difese non vengano aggirate. Considerare **TUTTI** i dati come non attendibili per proteggersi dalle più comuni vulnerabilità per le app Web.

Se si usa ASP.NET, il framework offre un [ottimo supporto per la convalida dell'input](https://docs.microsoft.com/aspnet/web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites) sia sul lato client che sul lato server.

Se si usa un altro framework Web, sul sito [OWASP](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet) sono descritte alcune ottime tecniche di convalida dell'input.


## <a name="always-use-parameterized-queries"></a>Usare sempre query con parametri

I database SQL vengono generalmente usati per archiviare dati. Si potrebbe ad esempio archiviare le informazioni sul profilo in SQL Server.  Evitare sempre di creare SQL inline o altre query di database "sul momento" nel codice e di inviarlo direttamente al database, in quanto è una strada sicura verso il disastro, come abbiamo visto prima.

Ad esempio, **non fare questo** (noto come SQL inline):

```csharp
string userName = ... // receive input from the user BEWARE!
...
string query = "SELECT *  FROM  [dbo].[users] WHERE userName = '" + userName + "'";
```

In questo esempio vengono concatenate più stringhe di testo per creare la query, recuperando l'input dall'utente e generando una query SQL dinamica per cercare l'utente. Anche in questo caso, se un utente malintenzionato capisse quale azione si sta eseguendo o anche solo _provasse_ diversi stili di input per rilevare eventuali vulnerabilità, le conseguenze potrebbero essere estremamente gravi. È invece preferibile usare istruzioni SQL con parametri o, meglio ancora, stored procedure:

```sql
-- Lookup a user
CREATE PROCEDURE sp_findUser
(
@UserName varchar(50)
)

SELECT *  FROM  [dbo].[users] WHERE userName = @UserName
```

Si può quindi richiamare la stored procedure dal codice in tutta sicurezza, passandole la stringa `userName` senza preoccuparsi che sia considerata parte dell'istruzione SQL.

## <a name="always-encode-your-output"></a>Codificare sempre l'output

Qualsiasi output presentato visivamente o in file deve sempre essere codificato e preceduto da un carattere di escape. In questo modo ci si protegge in caso di omissioni nel processo di purificazione o qualora il codice generi accidentalmente un elemento che può essere usato per fare danni. Ci si assicura così che ogni elemento venga visualizzato come _output_ e non venga inavvertitamente interpretato come qualcosa che deve essere eseguito. Questa è un'altra tecnica di attacco molto comune nota come XSS (Cross-Site Scripting), ossia un attacco tramite script da altri siti.

Essendo un requisito molto comune, questa è un'altra area in cui ASP.NET può agire automaticamente. Per impostazione predefinita, [tutto l'output è già codificato](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting?view=aspnetcore-2.1). Se si usa un altro framework Web, è possibile verificare le opzioni impostate per la codifica dell'output nei siti Web usando le informazioni di riferimento sulla prevenzione degli attacchi XSS sul sito [OWASP](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet).

## <a name="summary"></a>Riepilogo

La purificazione e la convalida dell'input sono un requisito necessario per assicurarsi che l'input sia valido e possa essere usato e archiviato in tutta sicurezza. I framework Web più moderni offrono funzionalità incorporate che consentono di automatizzare parte di questo lavoro. Consultare la documentazione del framework preferito per sapere quali funzionalità offre. Tenere anche presente che, benché questo discorso valga soprattutto per le applicazioni Web, anche altri tipi di applicazioni possono avere problemi simili, quindi non basta scrivere app desktop sicure perché sia sicuro tutto l'ambiente. È comunque necessario gestire correttamente l'input dell'utente per assicurare che nessuno possa usare l'app per danneggiare i dati o, peggio ancora, la reputazione dell'azienda.


## <a name="knowledge-check"></a>Verifica delle conoscenze

Quali delle origini dati seguenti devono essere purificate?
* Dati di un'API di terze parti
* Dati del parametro URL
* Dati raccolti dall'utente tramite un campo di input
* Tutte le risposte precedenti (risposta corretta)

Quali dei dati seguenti devono essere codificati prima dell'output?
* Dati salvati nel database
* Dati da visualizzare sullo schermo (risposta corretta)
* Dati inviati a un'API di terze parti
* Dati nei parametri URL

Le query con parametri (stored procedure in SQL) sono sempre una scelta più sicura per comunicare con il database perché:
* Sono più organizzate dei comandi di database inline e quindi generano meno confusione negli utenti.
* La stored procedure presenta una chiara struttura dello script, assicurando una maggiore visibilità.
* I pirati informatici non sono capaci di programmare in SQL.
* Le query con parametri sostituiscono le variabili prima di eseguire le query, impedendo così agli utenti malintenzionati di inserire codice dannoso al posto di una variabile. (risposta corretta)
