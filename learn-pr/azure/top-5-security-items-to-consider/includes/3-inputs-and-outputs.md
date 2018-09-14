Il più diffuso punti deboli nella sicurezza delle applicazioni oggi consiste nell'elaborare in modo non corretto l'input ricevuti da origini esterne, in particolare _input dell'utente_. Tenere sempre analizzare con attenzione qualsiasi input per assicurarsi che è stato convalidato prima di utilizzarlo. Impossibile eseguire questa operazione può comportare la perdita di dati o l'esposizione, l'escalation dei privilegi, o persino esecuzione di codice dannoso nel computer di altri utenti.

La cosa disastro è che si tratta di un problema semplice da risolvere. Di seguito illustra le procedure trattare i dati; Quando viene ricevuta, quando viene visualizzato sullo schermo e quando questo viene archiviato per un uso successivo.

## <a name="why-do-we-need-to-validate-our-input"></a>Il motivo per cui è necessario convalidare l'input?

Si supponga che si compila un'interfaccia per consentire all'utente di creare un account nel sito Web. I dati di profilo includano un nome, indirizzo di posta elettronica e un nome alternativo che verrà visualizzato a chiunque visiti il sito. Cosa accade se un nuovo utente viene creato un profilo e immette un nome alternativo che include alcuni comandi SQL? Ad esempio: cosa accade se l'utente errati immette un valore, ad esempio:

```output
Eve'); DROP TABLE Users;--
```

Se questo valore è semplicemente alla cieca inserire in un database, potrebbe alterare l'istruzione SQL per eseguire i comandi che assolutamente non si desidera eseguire. Ciò viene considerato come un attacco "Attacchi SQL Injection" ed è uno dei _molti_ tipi di exploit che potenzialmente possono essere eseguite quando non viene gestita correttamente gli input. Quindi, cosa si può fare per risolvere questo problema? Questa unità insegnerà quando per convalidare l'input, come la codifica dell'output e come creare con parametri query (che risolve la vulnerabilità sfruttata precedente). Queste sono le tre tecniche principali di difesa contro input dannosi immesso in applicazioni.

## <a name="when-do-i-need-to-validate-input"></a>Quando è necessario convalidare l'input?

La risposta è _sempre_. È necessario convalidare **ogni** di input per l'applicazione. Questo comprende parametri nell'URL di input dell'utente, i dati dal database, i dati da un'API e tutto ciò che viene passati in chiaro che un utente potrebbe potenzialmente modificare. Usare sempre un approccio elenco elementi consentiti, che significa che accetti solo "valida nota" input, anziché una blacklist (in cui in particolare cercare input errato) perché non è possibile pensare a un elenco completo di input potenzialmente pericolosi.  Eseguire questo lavoro nel server, non il lato client (o oltre il lato client), per garantire che le difese non possono essere aggirate. Considerare **tutti** dati come non attendibili e si verrà proteggersi dalla maggior parte delle vulnerabilità di app web.

Se si usa ASP.NET, il framework offre [ottimo supporto per la convalida dell'input](https://docs.microsoft.com/aspnet/web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites) sul lato client e server.

Se si usa un altro framework web, esistono alcune tecniche eccezionale per eseguire la convalida dell'input disponibile nel [foglio informativo di convalida Input OWASP](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet).


## <a name="always-use-parameterized-queries"></a>Utilizzare sempre query con parametri

Database SQL vengono comunemente utilizzati per archiviare i dati, ad esempio informazioni sul profilo.  Non creare mai inline SQL o altri database query "in modo immediato" nel codice e inviarlo direttamente al database, si tratta di un file recipe per situazione di emergenza, come descritto in precedenza.

Ad esempio, **eseguire questa operazione** (noto come SQL inline):

```csharp
string userName = ... // receive input from the user BEWARE!
...
string query = "SELECT *  FROM  [dbo].[users] WHERE userName = '" + userName + "'";
```

Qui abbiamo concatenare stringhe di testo per creare la query, accettando l'input da parte dell'utente e la generazione di una query SQL dinamica per cercare l'utente. Anche in questo caso, se un utente malintenzionato realizzato è eravamo in questo modo, o semplicemente _provato_ diversi stili di input per vedere se si è verificato una vulnerabilità, è possibile finire con una grave emergenza. Usare invece le istruzioni SQL con parametri o stored procedure simile al seguente:

```sql
-- Lookup a user
CREATE PROCEDURE sp_findUser
(
@UserName varchar(50)
)

SELECT *  FROM  [dbo].[users] WHERE userName = @UserName
```

Con questo metodo è possibile richiamare la procedura dal codice in modo sicuro, passandogli il `userName` stringa senza doversi preoccupare, che vengono considerati parte dell'istruzione SQL.

## <a name="always-encode-your-output"></a>Codificare sempre l'output

L'output è presentare visivamente o all'interno di un documento deve sempre essere con codifica e caratteri di escape. Ciò può proteggere in caso di un elemento non è stato eseguito nella sessione di purificazione o accidentalmente il codice genera un elemento che può essere usato da utenti malintenzionati. Questo garantirà che tutto ciò che viene visualizzato come _output_ e inavvertitamente interpretato come un elemento che deve essere eseguito. Si tratta di un'altra tecnica attacco molto comune, detta "Cross-Site Scripting" (XSS).

Poiché si tratta, ad esempio un requisito comune, si tratta di un'altra area in cui ASP.NET si occuperà automaticamente. Per impostazione predefinita [tutto l'output è già stato codificato](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting?view=aspnetcore-2.1). Se si usa un altro framework web, è possibile verificare le opzioni per la codifica dell'output in siti Web con il [foglio informativo di OWASP XSS prevenzione](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet).

## <a name="summary"></a>Riepilogo

Santizing e convalidare l'input dell'utente è un requisito necessario per garantire l'input dell'utente sia valido e sicuro da usare e archiviare. Più moderno framework web offrono funzionalità incorporate che è possibile automatizzare alcune di queste operazioni. È possibile controllare la documentazione del framework preferito e vedere quali funzionalità offerte. Sebbene le applicazioni web sono il luogo più comune in cui ciò accade, tenere presente che altri tipi di applicazioni possono essere semplicemente come vulnerabile. Non pensare che sono sicuri solo perché la nuova applicazione è un'app desktop. È comunque necessario gestire correttamente l'input dell'utente per verificare che un utente non usi l'app per danneggiare i dati o danneggiare la reputazione dell'azienda.