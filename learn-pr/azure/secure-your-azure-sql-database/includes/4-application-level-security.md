Si supponga che un utente malintenzionato sta provando ad accedere al database. Le applicazioni che si connettono al database sono aree vulnerabili agli attacchi. Tali applicazioni potrebbero non essere connesso al database usando metodi sicuri.

I database necessari per la protezione, ma come si accede al database possono svolgono un ruolo importante nella protezione dei dati. Violazioni del database ha esito positivo sono in genere il risultato di attacchi SQL injection. Attacchi SQL injection sono il risultato delle applicazioni che non usano procedure Preferiti per l'accesso a un database.

Esaminiamo le tecniche per proteggere il database al livello dell'applicazione.

## <a name="sql-injection-attacks"></a>Attacchi SQL injection

Il [foundation OWASP](https://owasp.org) è un'organizzazione no profit che è progettata per compilare gli standard per le applicazioni siano attendibili. Pubblica regolarmente un elenco le prime 10 vulnerabilità della protezione.

La vulnerabilità più comune in base a OWASP è attacchi intrusivi nel codice, che in genere assumono la forma di attacchi SQL injection. In un attacco SQL injection, le informazioni passate a un'istruzione SQL viene modificate. Queste query modificate restituiscono le informazioni riservate, o eseguono operazioni dannose per il database.

Esaminiamo il codice seguente di ASP.NET Core usando ADO.NET per recuperare le interazioni con un cliente specifico.

```csharp
public List<CustomerInteraction> GetCustomerInteractions(string customerId)
{
    var result = new List<CustomerInteraction>();

    using (var conn = new SqlConnection(ConnectionString))
    {
        var sql = "select Id, CustomerId, InteractionDate, Details " +
            "from CustomerInteractions where CustomerId = '" + customerId + "'";

        using (var command = conn.CreateCommand())
        {
            command.CommandText = sql;
            conn.Open();

            using (var reader = command.ExecuteReader())
            {
                if (reader.HasRows)
                {
                    while (reader.Read())
                        result.Add(GetInteractionsFromReader(reader));
                }
            }
        }
        return result.OrderByDescending(i => i.InteractionDate).ToList();
    }
}
```

La query stessa è si prepara per un attacco SQL, come il parametro customerId non viene ripulito prima di approfondire la query e pertanto può essere modificato. Il sito Web che chiama la query è passando il parametro customerId nella stringa di query URL come indicato di seguito:

.../Home/ViewInteractions?customerId=8c69a607-3c09-45ac-9beb-c59ca2de2385

Il problema con questa strategia è che il parametro customerId può essere modificato. Ad esempio, è possibile modificare il parametro customerId per inserire informazioni aggiuntive nella query e selezionare i dati da un'altra tabella.

```sql
select Id, CustomerId, InteractionDate, Details from CustomerInteractions where CustomerId = '8c69a607-3c09-45ac-9beb-c59ca2de2385'
```

Ora può essere modificato per aggiungere informazioni aggiuntive tramite un'istruzione UNION per aggiungere informazioni aggiuntive, aggiungere il codice seguente alla query:

```sql
union select Id, Id as CustomerId, getdate() as InteractionDate, CreditCardNumber + '/' + STR(CreditCardExpiryMonth, 2) + '/' + STR(CreditCardExpiryYear, 4) + ' cvv ' + STR(CreditCardCVV, 3) as Details from Customers --
```

La query SQL è codificato con URL in modo che il valore viene accettato come una parte di URL web valido. L'URL viene passata al sito Web e la query SQL aggiuntiva eseguite sul database.

```sql
%27+union+select+Id%2C+Id+as+CustomerId%2C+getdate%28%29+as+InteractionDate%2C+CreditCardNumber+%2B+%27%2F%27+%2B+STR%28CreditCardExpiryMonth%2C+2%29+%2B+%27%2F%27+%2B+STR%28CreditCardExpiryYear%2C+4%29+%2B+%27+cvv+%27+%2B+STR%28CreditCardCVV%2C+3%29+as+Details+from+Customers+--
```

Questo esempio viene illustrato come non bonificando le informazioni del sito può causare problemi di protezione dati.

![Screenshot di un indirizzo web browser a barre che mostra un esempio di un tentativo di attacco SQL injection tramite un'app web.](../media-draft/4-view-web-page-after-sql-injection.png)

> [!Note]
> Per altre informazioni sugli attacchi intrusivi nel codice, visitare il [OWASP Foundation](https://www.owasp.org/).

## <a name="avoiding-sql-injection-attacks"></a>Come evitare attacchi SQL injection

Per evitare attacchi SQL injection, è necessario assicurarsi sempre che qualsiasi input immesso dall'utente viene ripulito. Input dell'utente usati come parametri per le query non deve essere costruita mediante la concatenazione di stringhe, ma passati come parametri di query effettivo.

Nell'esempio seguente, è possibile visualizzare come CustomerId ora passato come un parametro usando il simbolo @. I parametri vengono definiti in modo esplicito e i valori passati nella query.

```csharp
using (var command = conn.CreateCommand())
{
    var sql = "select Id, CustomerId, InteractionDate, Details " +
        "from CustomerInteractions where CustomerId = @CustomerId order by InteractionDate";

    command.CommandText = sql;
    var prmCustomerId = command.Parameters.Add("@CustomerId", SqlDbType.UniqueIdentifier);
    prmCustomerId.Value = Guid.Parse(customerId);

    conn.Open();

    using (var reader = command.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
                result.Add(GetInteractionsFromReader(reader));
        }
    }
}
```

Le linee di base di codice sono:

```csharp
    var prmCustomerId = command.Parameters.Add("@CustomerId", SqlDbType.UniqueIdentifier);
    prmCustomerId.Value = Guid.Parse(customerId);
```

Usando input di dati puro e le query con parametri, si riduce le probabilità di attacchi intrusivi nel codice SQL nel database.

Questo esempio usato ASP.NET Core per illustrare i concetti. Tuttavia, tenere presente che tutti i sistemi di programmazione che supportano l'accesso a SQL Server dispongono di meccanismi per passare i parametri nei valori per le query.

## <a name="dynamic-data-masking"></a>Maschera dati dinamica

È possibile osservare che alcune informazioni nel database sono particolarmente sensibili; ad esempio informazioni della carta di credito. In un sistema nel mondo reale, si potrebbero non archiviare mai le informazioni della carta di credito non crittografate. Sfortunatamente, le informazioni della carta di credito non sono crittografate e sarà necessario trovare un altro modo per nascondere i dati.

Si supponga che si sta creando un sito Web di acquisti e durante il processo di ordine il sito dispone di visualizzare i dettagli della carta di credito. Si desidera visualizzare i primi 12 cifre impedite all'utente, simile alla seguente xxxx-xxxx-xxxx-1234.

Questa tecnica è definita la maschera dati dinamica. Maschera dati dinamica consentono di aggiungere le maschere in base alle colonne all'interno del database.

1. Tramite il portale, selezionare il database si desidera applicare le maschere per e quindi selezionare il **Dynamic Data Masking** opzione il **sicurezza** impostazione.

    Schermata di regole di maschera mostra un elenco di maschera dati dinamica esistente e le raccomandazioni per le colonne che devono avere una maschera dati dinamica applicata.

    ![Screenshot del portale di Azure che mostra un elenco delle maschere consigliate per le varie colonne di un database di esempio del database.](../media-draft/4-view-recommended-masked-columns.png)

1. Per aggiungere una maschera a una colonna, scegliere il **Aggiungi maschera** pulsante per aggiungere la maschera consigliata per la colonna.

    ![Schermata di Auzre portale che Mostra colonne con mascherate applicate consigliate e funzioni di maschera usate.](../media-draft/4-recommended-masks-applied.png)

1. Ogni nuova maschera verrà aggiunti all'elenco di regole di maschera. Scegliere il **salvare** pulsante per applicare le maschere.

Quando si eseguono query su colonne, gli amministratori di database verranno ancora visualizzati i valori originali, ma non sono amministratori verranno visualizzati i valori con mascherati.

È possibile consentire ad altri utenti di visualizzare le versioni non mascherate aggiungendoli agli utenti SQL esclusi dalla maschera di elenco.

È possibile visualizzare qui i dati mascherati aspetto quando eseguire una query mediante un utente non amministratore.

![Screenshot di una query sul database che mostra il messaggio di posta elettronica, il numero di telefono, SocialSecurityNumber e CreditCardNumber risultato colonne nascoste durante cui verrebbe visualizzati da un utente non amministratore.](../media-draft/4-sql-query-showing-masks.png)

Le maschere sugli schermi sono quelli aggiunti in base alle raccomandazioni, ma è possibile aggiungere una maschera manualmente troppo. Selezionare il + Aggiungi maschera e quindi pop-over modo sarà possibile selezionare lo schema, tabella e colonna da utilizzare. È quindi possibile definire il mascheramento che viene usato. Sono disponibili le maschere di standard che possono essere utilizzate, ad esempio:

- Valore predefinito, ovvero il valore predefinito per tale tipo di dati viene invece visualizzato.
- Valore carta di credito, che mostra solo le ultime quattro cifre del numero, la conversione di tutti gli altri numeri in caratteri minuscoli x;
- Messaggio di posta elettronica, che nasconde il dominio nome e tutto tranne il primo carattere del nome dell'account di posta elettronica;
- Numero, che specifica un numero casuale compreso tra un intervallo di valori. Nel mese di scadenza della carta di credito e anno, ad esempio, è possibile selezionare i mesi casuale compreso tra 1 e 12 e impostare l'intervallo di anni da 2018 a 3000; o
- Stringa personalizzata. In questo modo è possibile impostare il numero di caratteri esposto dall'inizio dei dati, il numero di caratteri esposte dalla fine dei dati e i caratteri ripetuta per il resto dei dati.
