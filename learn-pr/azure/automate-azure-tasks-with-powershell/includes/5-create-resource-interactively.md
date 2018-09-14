PowerShell consente di scrivere i comandi ed eseguirle immediatamente. Questa caratteristica è nota come **modalità interattiva**.

Ricordare che l'obiettivo complessivo dell'esempio relativo alla soluzione CRM (Customer Relationship Management) è di creare tre ambienti di test contenenti le macchine virtuali. Si useranno i gruppi di risorse per verificare che le macchine virtuali siano organizzate in ambienti distinti: uno per gli unit test, uno per i test di integrazione e uno per i test di accettazione. È sufficiente creare i gruppi di risorse una sola volta, quindi la modalità interattiva di PowerShell è una scelta ottimale.

Quando si digita un comando in PowerShell, corrisponde a un _cmdlet_ che quindi esegue l'azione richiesta. Dobbiamo esaminare alcuni comandi comuni è possibile usare e quindi esaminare il supporto tecnico di Azure per PowerShell di installazione.

## <a name="what-are-powershell-cmdlets"></a>Che cosa sono i cmdlet di PowerShell?
Un comando di PowerShell viene denominato **cmdlet** (pronunciato "command-let"). Un cmdlet è un comando che manipola una singola funzione. Il termine **cmdlet** è usato per indicare un "piccolo comando". Per convenzione, gli autori di cmdlet sono invitati a mantenere i cmdlet semplici e dedicati a un unico scopo.

Il prodotto PowerShell di base viene fornito con cmdlet utilizzabili per funzionalità quali sessioni e processi in background. È possibile aggiungere moduli all'installazione di PowerShell per ottenere cmdlet in grado di manipolare altre funzionalità. Esistono ad esempio moduli di terze parti per l'uso di connessioni FTP, l'amministrazione del sistema operativo, l'accesso al file system e così via.

I cmdlet seguono una convenzione di denominazione verbo-sostantivo, ad esempio **Get-Process**, **Format-Table** e **Start-Service**. Esiste inoltre una convenzione per la scelta del verbo: "get" per il recupero di dati, "set" per l'inserimento o l'aggiornamento di dati, "format" per la formattazione di dati, "out" per l'indirizzamento dell'output a una destinazione e così via.

Gli autori di cmdlet sono invitati a includere un file della Guida per ciascun cmdlet. Il **Get-Help** cmdlet consente di visualizzare il file della Guida per qualsiasi cmdlet. Ad esempio, è stato possibile ottenere assistenza `Get-ChildItem` cmdlet con l'istruzione seguente:

```powershell
Get-Help Get-ChildItem -detailed
```

## <a name="what-is-a-powershell-module"></a>Che cos'è un modulo di PowerShell?

I cmdlet vengono forniti nello _moduli_. Un modulo di PowerShell è una DLL che include il codice da processi di ogni cmdlet disponibili. , Caricare i cmdlet di PowerShell con il caricamento del modulo che sono racchiuse tra parentesi. È possibile ottenere un elenco di moduli caricati utilizzando il `Get-Module` comando:

```powershell
Get-Module
```

Verrà output è simile a:

```output
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Find-PackageProvider, Get-Package, Get-Pack...
Script     1.0.0.1    PowerShellGet                       {Find-Command, Find-DscResource, Find-Module, Find-RoleCap...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

## <a name="what-is-azurerm"></a>Che cos'è AzureRM?
**AzureRM** è il nome formale per il modulo di Azure PowerShell, che contiene i cmdlet per l'uso delle funzionalità di Azure (**RM** nel nome è l'acronimo di **Resource Manager**). Contiene centinaia di cmdlet che consentono di controllare praticamente ogni aspetto di ogni risorsa di Azure. È possibile lavorare con i gruppi di risorse, l'archiviazione, le macchine virtuali, Azure Active Directory, i contenitori, Machine Learning e così via. Questo modulo è un componente open source [disponibile in GitHub](https://github.com/Azure/azure-powershell).

> [!NOTE]
> Il modulo Azure PowerShell è un'installazione facoltativa: i cmdlet non sono disponibili finché non si importa il modulo.

### <a name="install-the-azurerm-module"></a>Installare il modulo AzureRM

Il modulo AzureRM è disponibile da un repository globale denominato PowerShell Gallery. È possibile installare il modulo nel computer locale tramite la `Install-Module` comando. È necessario una shell di PowerShell con privilegi elevata per installare i moduli da PowerShell Gallery. 

::: zone pivot="windows"

Per installare il modulo Azure PowerShell più recente, eseguire i comandi seguenti:

1. Aprire il menu **Start** e digitare **Windows PowerShell**.

1. Fare clic con il pulsante destro del mouse sull'icona di **Windows PowerShell** e scegliere **Esegui come amministratore**.

1. Nella finestra di dialogo **Controllo account utente** selezionare **Sì**.

1. Digitare il comando seguente e quindi premere INVIO:

    ```powershell
    Install-Module -Name AzureRM
    ```

Consente di installare il modulo per tutti gli utenti per impostazione predefinita (controllata dal parametro di ambito). 

Il comando si basa su NuGet per recuperare i componenti, a seconda della versione di NuGet è stato installato è possibile ottenere un prompt dei comandi per scaricare e installare la versione più recente di NuGet.

```output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files (x86)\PackageManagement\ProviderAssemblies' or
'C:\Users\<username>\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
 the NuGet provider now?
```

Per impostazione predefinita, PowerShell Gallery non è configurata come archivio attendibile per PowerShellGet. La prima volta che si usa PSGallery verrà visualizzato il messaggio seguente:

```output
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
```

#### <a name="script-execution-failed"></a>Esecuzione di script non è riuscita
A seconda della configurazione di sicurezza, `Import-Module` potrebbe non riuscire con qualcosa di simile al seguente.

```output
import-module : File C:\Program Files (x86)\WindowsPowerShell\Modules\azurerm\6.8.1\AzureRM.psm1 cannot be loaded
because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ import-module azurerm
+ ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [Import-Module], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.PowerShell.Commands.ImportModuleCommand
```

In genere indica che i criteri di esecuzione sono "restricted", vale a dire che non è possibile eseguire i moduli scaricati da un'origine esterna, inclusa la raccolta di PowerShell. È possibile controllare se questo è il caso eseguendo il comando `Get-ExecutionPolicy`. Se viene restituito "Restricted", quindi eseguire le operazioni seguenti:

1. Aprire un prompt dei comandi di PowerShell con privilegi elevati.
1. Usare il `SetExecutionPolicy` cmdlet per modificare i criteri per "RemoteSigned":

```powershell
Set-ExecutionPolicy RemoteSigned
```

Questo richiederà l'autorizzazione:

```output
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

Quindi sarà possibile usare `Import-Module` per caricare i cmdlet.

::: end di zona

::: pivot zona = "linux, macos"

Utilizziamo gli stessi comandi per installare Azure PowerShell su Linux o macOS.

1. In un terminale, digitare il comando seguente per avviare PowerShell Core con privilegi elevati.

    ```bash
    sudo pwsh
    ```

1. Per installare Azure PowerShell, eseguire il comando seguente nel prompt di PowerShell Core.

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. Se viene richiesto se considerare attendibili i moduli di **PSGallery**, rispondere **Sì** oppure **Sì a tutti**.

::: end di zona

### <a name="update-a-module"></a>Aggiornare un modulo

Se viene visualizzato un avviso o un messaggio di errore che indica che è già installata una versione del modulo Azure PowerShell, è possibile aggiornare per il _più recente_ versione eseguendo il comando:

::: pivot zona = "windows"

```powershell
Update-Module -Name AzureRM
```

::: end di zona

::: pivot zona = "linux, macos"

```powershell
Update-Module -Name AzureRM.NetCore
```

::: end di zona

Come nel caso del comando `Install-Module`, rispondere **Sì** o **Sì a tutti** quando viene richiesto se considerare attendibile il modulo. È anche possibile usare il `Update-Module` comando per installare nuovamente un modulo, se si verificano problemi con esso.

## <a name="example-how-to-create-a-resource-group-with-azure-powershell"></a>Esempio: Come creare un gruppo di risorse con Azure PowerShell
Dopo aver caricato il modulo di Azure, è possibile iniziare con Azure. È possibile eseguire un'attività comune: creare un gruppo di risorse. Come è noto, i gruppi di risorse viene usato per amministrare le risorse correlate tra loro. Creazione di un nuovo gruppo di risorse è una delle prime attività che verranno eseguite quando si avvia una nuova soluzione di Azure.

Sono disponibili è necessario eseguire quattro passaggi:

1. Importare i cmdlet di Azure.

1. Connettersi alla sottoscrizione di Azure.

1. Creare il gruppo di risorse.

1. Verificare che la creazione sia stata eseguita (vedere di seguito).

La figura seguente mostra una panoramica di questi passaggi.

![Illustrazione che mostra i passaggi per creare un gruppo di risorse.](../media/5-create-resource-overview.png)

Ogni passaggio corrisponde a un diverso cmdlet.

### <a name="import-the-azure-cmdlets"></a>Importare i cmdlet di Azure
All'avvio, per impostazione predefinita PowerShell carica solo i cmdlet di base. Questo significa che i cmdlet che necessari per lavorare con Azure non verranno caricati. Il modo più affidabile per caricare i cmdlet necessari consiste nell'importarli manualmente all'inizio della sessione PowerShell.

È possibile usare il cmdlet **Import-Module** per caricare i moduli. Questo cmdlet dispone di numerosi parametri per gestire un'ampia gamma di situazioni. Ad esempio, può caricare più moduli, una versione specifica di un modulo, parte di un modulo e così via.

Ad esempio, è possibile caricare tutti i cmdlet di AzureRM con il comando seguente **in una sessione di PowerShell con privilegi elevata**:

::: pivot zona = "windows"

```powershell
Import-Module AzureRM
```

::: end di zona

::: pivot zona = "linux, macos"

```powershell
Import-Module AzureRM.NetCore
```

::: end di zona

> [!TIP]
> Se si lavora con Azure PowerShell di frequente, vi sono due modi in cui è possibile automatizzare il processo di caricamento dei moduli. È possibile aggiungere una voce al proprio profilo PowerShell per importare il modulo di Azure all'avvio oppure usare le versioni più recenti di PowerShell, che carica automaticamente il modulo contenente quando si usa un cmdlet.

### <a name="connect"></a>Connettere
Quando si lavora con un'installazione locale di Azure PowerShell, è necessario eseguire l'autenticazione prima di poter eseguire i comandi di Azure. Il cmdlet **Connect-AzureRmAccount** richiede le credenziali di Azure e quindi esegue la connessione alla sottoscrizione di Azure. Il cmdlet dispone di numerosi parametri facoltativi, ma se tutto quello che occorre è un prompt interattivo, non sono necessari parametri:

```powershell
Connect-AzureRmAccount
```

È necessario ripetere questi passaggi per ogni nuova sessione di PowerShell che si avvia perché questo modulo non fa parte del set di base.


### <a name="working-with-subscriptions"></a>Uso di più sottoscrizioni
Se si ha familiarità con Azure, è probabilmente sufficiente una singola sottoscrizione. Tuttavia, se si usa Azure già da tempo, probabilmente si saranno create più sottoscrizioni di Azure. È possibile configurare Azure PowerShell per eseguire comandi su una determinata sottoscrizione.

È possibile solo in una sottoscrizione alla volta. Usare il `Get-AzureRmContext` cmdlet per determinare quale sottoscrizione è attiva. Se non è quella corretta, è possibile modificarlo.

1. Ottenere un elenco di tutti i nomi di sottoscrizione nell'account con il `Get-AzureRmSubscription` comando. 

2. Modificare la sottoscrizione passando il nome di quello in cui selezionare.

```powershell
Select-AzureRmSubscription -Subscription "Visual Studio Enterprise"
```

### <a name="get-a-list-of-all-resource-groups"></a>Ottenere un elenco di tutti i gruppi di risorse

È possibile recuperare un elenco di tutti i gruppi di risorse nella sottoscrizione attiva:

```powershell
Get-AzureRmResourceGroup
```

Per ottenere una visione più concisa, è possibile inviare l'output di `Get-AzureRmResourceGroup` per il `Format-Table` cmdlet utilizzando una barra verticale ' |'.

```powershell
Get-AzureRmResourceGroup | Format-Table
```

Verrà output è simile a:

```output
ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
-----------------                  --------       ----------------- ---- --------- ----------
cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
ExerciseResources                  eastus         Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
```

### <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Come saprete, quando si creano risorse in Azure, verrà sempre inserirli in un gruppo di risorse per scopi di gestione. Un gruppo di risorse è spesso una delle prime cose che verrà creato quando si avvia una nuova applicazione.

È possibile creare gruppi di risorse con il `New-AzureRmResourceGroup` cmdlet. È necessario specificare un nome e una posizione. Il nome deve essere univoco all'interno della sottoscrizione. La posizione determina dove vengono archiviati i metadati per il gruppo di risorse (questo aspetto può essere importante per motivi di conformità). Si usano stringhe come "Stati Uniti occidentali", "Europa settentrionale" o "India occidentale" per specificare la posizione. Come con la maggior parte dei cmdlet di Azure, `New-AzureRmResourceGroup` presenta numerosi parametri facoltativi, ma la sintassi di base è:

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

> [!NOTE]
> Tenere presente che è possibile lavorare in Sandbox di Azure che crea il gruppo di risorse per l'utente. Il comando precedente verrà utilizzato qualora si lavora nella propria sottoscrizione.

### <a name="verify-the-resources"></a>Verificare le risorse
Il `Get-AzureRmResource` Elenca le risorse di Azure. Questo comando è utile in questo contesto per verificare se è stata completata la creazione del gruppo di risorse.

```powershell
Get-AzureRmResource
```

Ad esempio la `Get-AzureRmResourceGroup` comando, è possibile ottenere una visualizzazione più concisa tramite il `Format-Table` cmdlet. In questo caso si userà una versione abbreviata `ft`:

```powershell
Get-AzureRmResource | ft
```

È anche possibile filtrare per gruppi di risorse specifico per elencare solo le risorse associate al gruppo:

```powershell
Get-AzureRmResource -ResourceGroup ExerciseResources
```

### <a name="creating-an-azure-virtual-machine"></a>Creazione di una macchina virtuale di Azure

Un'altra attività comune che è stato possibile eseguire con PowerShell consiste nel creare le macchine virtuali.

Azure PowerShell fornisce il `New-AzureRmVm` cmdlet per creare una macchina virtuale. Il cmdlet dispone di diversi parametri che consentono di gestire le numerose impostazioni di configurazione della macchina virtuale. La maggior parte dei parametri ha valori predefiniti accettabili, pertanto è necessario specificare solo cinque elementi:

- **ResourceGroupName**: il gruppo di risorse in cui verrà inserita la nuova macchina virtuale.
- **Name**: il nome della macchina virtuale in Azure.
- **Location**: la posizione geografica in cui verrà eseguito il provisioning della macchina virtuale.
- **Credential**: un oggetto contenente il nome utente e la password per l'account di amministratore della macchina virtuale. Si userà il `Get-Credential` cmdlet. Questo cmdlet verrà richiesto un nome utente e password e comprimerlo in un oggetto credenziale.
- **Immagine**: l'immagine del sistema operativo da usare per la macchina virtuale. Si tratta spesso di una distribuzione Linux o Windows Server.

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

È possibile fornire questi parametri direttamente al cmdlet come illustrato in precedenza. In alternativa, gli altri cmdlet possono essere usati per configurare la macchina virtuale, ad esempio `Set-AzureRmVMOperatingSystem`, `Set-AzureRmVMSourceImage`, `Add-AzureRmVMNetworkInterface`, e `Set-AzureRmVMOSDisk`.

Di seguito è riportato un esempio che le stringhe di `Get-Credential` cmdlet con il `-Credential` parametro:

```powershell
New-AzureRmVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

Il `AzureRmVM` suffisso è specifico di comandi basati su macchina virtuale in PowerShell. Esistono diversi altri che è possibile usare:

| Comando | Descrizione |
|---------|-------------|
| `Remove-AzureRmVM` | Elimina una macchina virtuale di Azure. |
| `Start-AzureRmVM` | Avvia una VM arrestata. |
| `Stop-AzureRmVM` | Arrestare una macchina virtuale in esecuzione. |
| `Restart-AzureRmVM` | Riavviare una macchina virtuale. |
| `Update-AzureRmVM` | Aggiorna la configurazione per una macchina virtuale. |

#### <a name="example-getting-the-information-for-a-vm"></a>Esempio: recupero delle informazioni per una macchina virtuale

È possibile elencare le macchine virtuali nella sottoscrizione con il `Get-AzureRmVM -Status` comando. Ciò può anche specificare una macchina virtuale con il `-Name` proprietà. Qui abbiamo assegnarlo a una variabile di PowerShell:

```powershell
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName ExerciseResources
```

L'aspetto interessante è si tratta di un' _oggetto_ è possibile interagire con. Ad esempio, si può prendere questo oggetto, apportare modifiche e quindi push delle modifiche in Azure con il `Update-AzureRmVM` comando:

```powershell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzureRmVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

## <a name="summary"></a>Riepilogo
La modalità interattiva di PowerShell è appropriata per le attività occasionali. In questo esempio, si userà probabilmente stesso gruppo di risorse per la durata del progetto, ovvero creato in modo interattivo, è ragionevole. La modalità interattiva è spesso più veloce e semplice per questa attività che scrivere uno script ed eseguirlo una sola volta.