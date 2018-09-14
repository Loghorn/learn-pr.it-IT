Si supponga che si riceve un avviso dall'amministratore della sicurezza della società che è stata rilevata una potenziale violazione della sicurezza della rete. Per proteggere i server di database, si decide di aggiungere il controllo e monitoraggio.

In questa unità, esamineremo come il controllo è configurato su un database e come usare questi controlli.

## <a name="configure-auditing"></a>Configurare il controllo

Si sarà Abilita il controllo archiviare le operazioni che si verificano nel database per l'analisi successive o avere strumenti automatizzati analizzarli. Il controllo viene utilizzato anche per la gestione della conformità o informazioni sulle modalità di utilizzo del database. Il controllo è necessario anche se si vuole usare il rilevamento delle minacce in Azure nel database SQL di Azure.

Per poter archiviare i controlli del database, sarà necessario un account di archiviazione di Azure per archiviare la cronologia di controllo.

Esaminiamo la procedura da seguire per configurare il controllo del sistema.

1. Selezionare il Server SQL di Azure nel portale.

2. Passare all'elemento di controllo nelle opzioni di configurazione a sinistra. Si troverà nella categoria di sicurezza.

3. Il controllo è stato disattivata per impostazione predefinita. Per abilitarla sul server di database, toccare il pulsante su ON.

4. Dopo aver selezionato il pulsante su ON, selezionare il pulsante Configura per definire l'account di archiviazione. È possibile selezionare un account di archiviazione esistente o creare un nuovo account di archiviazione per archiviare i controlli. L'account di archiviazione deve essere configurato per usare la stessa area del server.

5. Fare clic sul pulsante "Salva" nella barra degli strumenti per salvare le modifiche.

Queste azioni configurare i controlli a livello di server di database. È anche possibile configurare il controllo che viene eseguita a livello di database.

Ora si configurerà Advanced Threat Protection.

## <a name="configure-advanced-threat-protection"></a>Configurare la protezione avanzata dalle minacce

Il sistema di Advanced Threat Protection consente di analizzare i log di controllo per individuare potenziali problemi e le minacce nel database.

Esaminiamo i passaggi per la configurazione di Advanced Threat Protection nel sistema.

1. Selezionare il Server SQL di Azure nel portale.

2. Passare all'elemento di Advanced Threat Protection nelle opzioni di configurazione a sinistra. Si potrà risultare nella categoria di sicurezza.

3. Fare clic sul pulsante 'Abilita Advanced Threat Protection nel server'.

4. Modificare il commutatore di Advanced Threat Protection su ON.

5. Selezionare le impostazioni di visualizzazione Advanced Threat Protection server per visualizzare le opzioni per il sistema di database.

6. Successivamente, definire in cui la notifica verrà recapitate messaggi di posta elettronica come un elenco di punti e virgola separati indirizzi di posta elettronica. Selezionare il servizio di posta elettronica e ai coamministratori per inviare i rischi per gli amministratori del servizio.

7. Verrà ora specificare la sottoscrizione e account di archiviazione che verrà analizzato per minacce nel sistema. La sottoscrizione e l'archiviazione di Azure deve essere l'account configurato per il controllo. È anche necessario impostare il numero di giorni per cui conservare la cronologia di controllo. Impostazione del valore su zero indica che il controllo verrà archiviato per sempre.

Selezionare quindi la chiave di accesso di archiviazione per connettersi a controlli. Dopo aver configurato le opzioni, fare clic su OK.

Infine, impostare i tipi di rilevamento delle minacce. L'opzione preferita è tutto.

Tutti rappresenta i valori seguenti:

- Report SQL injection in cui si sono verificati gli attacchi SQL injection.
- Rapporti sulle vulnerabilità di attacchi injection SQL in cui la possibilità di un attacco SQL injection è probabilmente; e
- Accesso client anomalo vengono esaminati gli account di accesso irregolare e potrebbe essere motivo di preoccupazione, ad esempio un potenziale che accede autore dell'attacco.

Scegliere il **salvare** pulsante per applicare le modifiche.

Si riceveranno notifiche tramite posta elettronica vengono rilevate vulnerabilità. Messaggio di posta elettronica descrive cosa è accaduto e le azioni da eseguire.

## <a name="enable-advanced-threat-protection"></a>Abilita Advanced Threat Protection

Dopo aver configurato il server per Advanced Threat Protection, si abilita l'opzione su ogni singolo database. Passare a singoli database e abilitare Advanced Threat Protection selezionando 'Abilitare Advanced Threat Protection nel server'.

È possibile attivare le analisi ricorrenti periodiche che analizzeranno il sistema ogni sette giorni per cercare i punti deboli.

Quando si seleziona l'opzione di analisi ricorrenti periodiche, verrà eseguita immediatamente dopo il salvataggio delle impostazioni di un'analisi.

Fare clic sul pulsante **Salva** per salvare le modifiche apportate.

Si riceverà una notifica tramite posta elettronica di notifica delle eventuali problemi di sicurezza. Assicurarsi di risolvere immediatamente il rischio. Si potrebbe ricevere una notifica per una serie di motivi:

![Un avviso di notifica di esempio da Advanced Threat Protection](../media-draft/5-email-with-warning.png)

Selezionando l'opzione di Advanced Threat Protection durante l'esecuzione di Advanced Threat Protection, vedrai un elenco di problemi presentati. Questo elenco potrebbe includere individuazione dati e i problemi di classificazione, ad esempio i dati sensibili, un elenco delle vulnerabilità nel sistema e le potenziali minacce.

![Individuazione dati e classificazione](../media-draft/5-data-discovery-and-classification.png)

Il pannello di individuazione dati e classificazione Mostra colonne all'interno delle tabelle che devono essere protetti. Alcune colonne potrebbero contenere informazioni sensibili o possa essere considerato classificati in diversi paesi o aree geografiche.

Fare clic sul pannello di individuazione dati e classificazione.

Verrà visualizzato un messaggio se tutte le colonne necessitano garantire la protezione configurata. Questo messaggio verrà costituiti *"Abbiamo scoperto 10 colonne con raccomandazioni di classificazione"*. È possibile fare clic sul testo per visualizzare le raccomandazioni.

Selezionare le colonne che si desidera classificare facendo clic sul segno di spunta accanto alla colonna oppure selezionare la casella di controllo a sinistra dell'intestazione dello schema. Selezionare le opzioni di raccomandazioni Accept selezionato per applicare le raccomandazioni di classificazione.

Si modifica le colonne e quindi definire il tipo di informazioni e l'etichetta di riservatezza per il database. Fare clic sul pulsante Salva per salvare le modifiche.

Nessuna raccomandazione attiva dovrebbero essere elencata quando è stato gestito correttamente le raccomandazioni.

![Dashboard di valutazione della vulnerabilità](../media-draft/5-vulnerability-assessment-dashboard.png)

La valutazione della vulnerabilità sono elencati i problemi di configurazione nel database e i rischi associati. Ad esempio, nell'immagine precedente, è possibile visualizzare che il firewall a livello di server deve essere configurato.

Fare clic sul pannello di valutazione della vulnerabilità per esaminare un elenco completo delle vulnerabilità. A questo punto, sarà fare clic su ogni singola vulnerabilità.

Nella pagina verranno visualizzati i dettagli, ad esempio il livello di rischio, il database a cui si applica a, una descrizione della vulnerabilità e la correzione consigliata per risolvere il problema. Verrà applicata la correzione per risolvere il problema o problemi. Assicurarsi di soddisfare tutte le vulnerabilità.

![Rilevamento delle minacce](../media-draft/5-threat-detection-dashboard.png)

L'ultimo grafico visualizza un elenco di rilevamento delle minacce. In questo elenco, ad esempio, si noterà un numero di potenziali attacchi SQL injection.

Ad esempio le vulnerabilità, fare clic sul pannello di rilevamento delle minacce per passare all'elenco di voci per visualizzare gli elementi della condizione di minaccia. Quindi risolvere il problema seguendo le indicazioni.  Per i problemi, ad esempio gli avvisi di attacchi injection SQL, sarà possibile esaminare la query e procedere a ritroso per cui è in esecuzione tale query nel codice. Una volta trovato, è possibile riscrivere il codice in modo che non sarà più possibile il problema.
