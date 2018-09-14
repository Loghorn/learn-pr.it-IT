La creazione di script di amministrazione rappresenta un potente strumento per ottimizzare il flusso di lavoro. È possibile automatizzare le attività comuni e ripetitive. Inoltre, dopo aver verificato uno script, questo verrà eseguito uniformemente, con una probabile riduzione degli errori.

Si supponga di lavorare presso un'azienda che usa macchine virtuali di Azure per testare software CRM (Customer Relationship Management). Le macchine virtuali sono costituite da immagini che includono un front-end Web, un servizio Web che implementa la logica di business e un database SQL.

Sono state eseguite più sessioni di test in una singola macchina virtuale, ma si è notato che le modifiche apportate al database e ai file di configurazione possono causare risultati incoerenti. In un caso, un bug ha creato un record per una chiamata telefonica senza alcun cliente corrispondente nel database. A causa del record orfano, i successivi test di integrazione hanno avuto esito negativo, anche dopo che è stato corretto il bug. Si prevede di risolvere il problema tramite una nuova distribuzione della macchina virtuale per ogni ciclo di test. Si vuole automatizzare la configurazione della creazione della macchina virtuale, perché verrà eseguita diverse volte alla settimana. 

Di seguito verrà illustrato come gestire le risorse di Azure tramite Azure PowerShell. Azure PowerShell sarà usato in modalità interattiva per le attività occasionali e verranno scritti script per automatizzare le attività ripetitive. 

## <a name="learning-objectives"></a>Obiettivi di apprendimento
In questo modulo verrà descritto come:

- Decidere se Azure PowerShell è lo strumento appropriato per le attività di amministrazione di Azure
- Installare Azure PowerShell su Linux, macOS e/o Windows
- Connettersi a una sottoscrizione di Azure tramite Azure PowerShell
- Creare risorse di Azure con Azure PowerShell

## <a name="prerequisites"></a>Prerequisiti

- Esperienza con un'interfaccia della riga di comando, ad esempio PowerShell o Bash
- Conoscenza dei concetti di base di Azure, ad esempio i gruppi di risorse e le macchine virtuali
- Esperienza di amministrazione delle risorse di Azure tramite il portale di Azure
