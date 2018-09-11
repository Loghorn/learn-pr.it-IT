Azure PowerShell consente di scrivere i comandi ed eseguirli immediatamente. Questa caratteristica è nota come **modalità interattiva**.

Ricordare che l'obiettivo complessivo dell'esempio relativo alla soluzione CRM (Customer Relationship Management) è di creare tre ambienti di test contenenti le macchine virtuali. Si useranno i gruppi di risorse per verificare che le macchine virtuali siano organizzate in ambienti distinti: uno per gli unit test, uno per i test di integrazione e uno per i test di accettazione. È sufficiente creare i gruppi di risorse una sola volta, quindi la modalità interattiva di PowerShell è una scelta ottimale.

In questa sezione sono illustrati alcuni esempi di uso di PowerShell in modalità interattiva per accedere alla sottoscrizione di Azure e creare i gruppi di risorse.

## <a name="what-are-powershell-cmdlets"></a>Che cosa sono i cmdlet di PowerShell?
Un comando di PowerShell viene denominato **cmdlet** (pronunciato "command-let"). Un cmdlet è un comando che manipola una singola funzione. Il termine **cmdlet** è usato per indicare un "piccolo comando". Per convenzione, gli autori di cmdlet sono invitati a mantenere i cmdlet semplici e dedicati a un unico scopo.

Il prodotto PowerShell di base viene fornito con cmdlet utilizzabili per funzionalità quali sessioni e processi in background. È possibile aggiungere moduli all'installazione di PowerShell per ottenere cmdlet in grado di manipolare altre funzionalità. Esistono ad esempio moduli di terze parti per l'uso di connessioni FTP, l'amministrazione del sistema operativo, l'accesso al file system e così via.

I cmdlet seguono una convenzione di denominazione verbo-sostantivo, ad esempio **Get-Process**, **Format-Table** e **Start-Service**. Esiste inoltre una convenzione per la scelta del verbo: "get" per il recupero di dati, "set" per l'inserimento o l'aggiornamento di dati, "format" per la formattazione di dati, "out" per l'indirizzamento dell'output a una destinazione e così via.

Gli autori di cmdlet sono invitati a includere un file della Guida per ciascun cmdlet. Il cmdlet **Get-Help** consente di visualizzare il file della Guida per qualsiasi cmdlet:

```powershell
Get-Help <cmdlet-name> -detailed
```

## <a name="what-is-azurerm"></a>Che cos'è AzureRM?
**AzureRM** è il nome formale per il modulo di Azure PowerShell, che contiene i cmdlet per l'uso delle funzionalità di Azure (**RM** nel nome è l'acronimo di **Resource Manager**). Contiene centinaia di cmdlet che consentono di controllare praticamente ogni aspetto di ogni risorsa di Azure. È possibile lavorare con i gruppi di risorse, l'archiviazione, le macchine virtuali, Azure Active Directory, i contenitori, Machine Learning e così via.

## <a name="how-to-create-a-resource-group"></a>Come creare un gruppo di risorse
A questo punto, verrà creato un gruppo di risorse usando un'installazione locale di Azure PowerShell. 

Sono necessari quattro passaggi: 

1. Importare i cmdlet di Azure.

1. Connettersi alla sottoscrizione di Azure.

1. Creare il gruppo di risorse.

1. Verificare che la creazione sia stata eseguita (vedere di seguito).

La figura seguente mostra una panoramica di questi passaggi.

![Illustrazione che mostra i passaggi per creare un gruppo di risorse.](../media/5-create-resource-overview.png)

Ogni passaggio corrisponde a un diverso cmdlet.

### <a name="import"></a>Importa
All'avvio, per impostazione predefinita PowerShell carica solo i cmdlet di base. Questo significa che i cmdlet che necessari per lavorare con Azure non verranno caricati. Il modo più affidabile per caricare i cmdlet necessari consiste nell'importarli manualmente all'inizio della sessione PowerShell.

È possibile usare il cmdlet **Import-Module** per caricare i moduli. Questo cmdlet dispone di numerosi parametri per gestire un'ampia gamma di situazioni. Ad esempio, può caricare più moduli, una versione specifica di un modulo, parte di un modulo e così via. Per caricare interamente un modulo, la sintassi è semplicemente:

```powershell
Import-Module <module-name>
```

> [!TIP]
> Se si lavora con Azure PowerShell di frequente, vi sono due modi in cui è possibile automatizzare il processo di caricamento dei moduli. È possibile aggiungere una voce al proprio profilo PowerShell per importare il modulo di Azure all'avvio oppure usare le versioni più recenti di PowerShell, che carica automaticamente il modulo contenente quando si usa un cmdlet.

### <a name="connect"></a>Connettere
Dato che si sta lavorando con un'installazione locale di Azure PowerShell, sarà necessario eseguire l'autenticazione prima di poter eseguire i comandi di Azure. Il cmdlet **Connect-AzureRmAccount** richiede le credenziali di Azure e quindi esegue la connessione alla sottoscrizione di Azure. Il cmdlet dispone di numerosi parametri facoltativi, ma se tutto quello che occorre è un prompt interattivo, non sono necessari parametri:

```powershell
Connect-AzureRmAccount
```

### <a name="create"></a>Creare
Il cmdlet **New-AzureRmResourceGroup** consente di creare un gruppo di risorse. È necessario specificare un nome e una posizione. Il nome deve essere univoco all'interno della sottoscrizione. La posizione determina dove vengono archiviati i metadati per il gruppo di risorse (questo aspetto può essere importante per motivi di conformità). Si usano stringhe come "Stati Uniti occidentali", "Europa settentrionale" o "India occidentale" per specificare la posizione. Come la maggior parte dei cmdlet di Azure, **New-AzureRmResourceGroup** presenta numerosi parametri facoltativi, ma la sintassi di base è la seguente:

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

### <a name="verify"></a>Verificare
**Get-AzureRmResource** elenca le risorse di Azure. Questo comando è utile in questo contesto per verificare se è stata completata la creazione del gruppo di risorse.

```powershell
Get-AzureRmResource
```

Per ottenere una visione più concisa, è possibile inviare l'output da **Get-AzureRmResource** al cmdlet **Format-Table** usando una barra verticale "|".

```powershell
Get-AzureRmResource | Format-Table
```

## <a name="summary"></a>Riepilogo
La modalità interattiva di PowerShell è appropriata per le attività occasionali. In questo esempio, si userà lo stesso gruppo di risorse per tutta la durata del progetto, pertanto è ragionevole crearlo in modalità interattiva. La modalità interattiva è spesso più veloce e semplice per questa attività che scrivere uno script ed eseguirlo una sola volta.