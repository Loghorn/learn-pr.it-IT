Si supponga di aver scelto Azure PowerShell come soluzione di automazione. Gli amministratori preferiscono eseguire gli script in locale anziché in Azure Cloud Shell. Il team usa computer che eseguono Linux, macOS e Windows. È necessario fare in modo che Azure PowerShell sia disponibile e funzionante in tutti i dispositivi. 

## <a name="what-must-be-installed"></a>Che cosa è necessario installare?
Le effettive istruzioni di installazione verranno descritte nell'unità successiva. Ora verranno esaminati i due componenti che costituiscono Azure PowerShell.

- **Il prodotto PowerShell di base**. È disponibile in due varianti: PowerShell in Windows e PowerShell Core in macOS e Linux.
- **Il modulo di Azure PowerShell**. Questo modulo aggiuntivo deve essere installato per aggiungere i comandi specifici di Azure a PowerShell.

> [!TIP]
> PowerShell è incluso in Windows, ma potrebbe essere disponibile un aggiornamento. In Linux e macOS è necessario installare PowerShell Core.

Dopo aver installato il prodotto di base, aggiungere il modulo di Azure PowerShell all'installazione.

## <a name="how-to-install-powershell-core"></a>Come installare PowerShell Core
In Linux e macOS, si usa uno strumento di gestione pacchetti per installare PowerShell Core. Lo strumento di gestione pacchetti consigliato è diverso in base al sistema operativo e alla distribuzione.

> [!NOTE]
> PowerShell Core è disponibile nel repository Microsoft, quindi è prima necessario aggiungere tale repository allo strumento di gestione pacchetti.

### <a name="linux"></a>Linux
In Linux, lo strumento di gestione pacchetti cambia a seconda della distribuzione di Linux scelta.

| Distribuzione  | Strumento di gestione pacchetti |
|------------------|-----------------|
| Ubuntu, Debian   | `apt-get`       |
| Red Hat, CentOS  | `yum`           |
| OpenSUSE         | `zypper`        |
| Fedora           | `dnf`           |

### <a name="mac"></a>Mac
In macOS, si userà `Homebrew` per installare PowerShell Core.

## <a name="summary"></a>Riepilogo
In Windows, PowerShell è incorporato, ma è necessario installare il modulo di Azure PowerShell. In Linux e macOS, è necessario installare sia PowerShell Core che il modulo di Azure PowerShell. Nella sezione successiva verrà descritta la procedura di installazione dettagliata per alcune piattaforme comuni.