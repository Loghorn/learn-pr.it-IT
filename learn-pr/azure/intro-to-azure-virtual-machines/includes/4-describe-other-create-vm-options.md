Il portale di Azure è il modo più semplice per creare per la prima volta risorse come le macchine virtuali. Tuttavia, non si tratta necessariamente del modo più rapido o efficiente per lavorare con Azure, specialmente se si devono creare più risorse contemporaneamente. In questo caso, si creeranno decine di macchine virtuali per gestire attività diverse. Senza dubbio, crearle manualmente nel portale di Azure non è la soluzione ideale;

verranno quindi illustrati altri modi per creare e amministrare risorse in Azure:

- Azure Resource Manager
- Azure PowerShell
- Interfaccia della riga di comando di Azure
- API REST di Azure
- SDK per client Azure
- Estensioni di macchina virtuale di Azure
- Servizi di Automazione di Azure

## <a name="azure-resource-manager"></a>Azure Resource Manager

Si supponga di voler creare una copia di una macchina virtuale con le stesse impostazioni. È possibile creare un'immagine di macchina virtuale, caricarla in Azure e basarsi su di essa per la nuova macchina, ma si tratta di un processo inefficiente e che richiede molto tempo. Azure offre la possibilità di creare un modello da cui creare una copia esatta di una macchina virtuale.

In genere, l'infrastruttura di Azure contiene molte risorse, molte delle quali correlate in qualche modo. Ad esempio, la macchina virtuale creata offre archiviazione, interfaccia di rete, server Web, database e la macchina stessa, tutte risorse create contemporaneamente per eseguire il sito WordPress. **Azure Resource Manager** permette di lavorare in modo più efficiente con queste risorse correlate, organizzandole in **gruppi di risorse** denominati che consentono di distribuirle, aggiornarle o eliminarle tutte contemporaneamente. Quando è stato creato il sito WordPress, il gruppo di risorse è stato identificato come parte della creazione della macchina virtuale e Resource Manager ha posizionato le risorse associate nel medesimo gruppo.

Resource Manager consente anche di creare _modelli_ utilizzabili per creare e distribuire configurazioni specifiche.

### <a name="what-are-resource-manager-templates"></a>Cosa sono i modelli di Azure Resource Manager?

I **modelli di Resource Manager** sono file JSON che definiscono le risorse che è necessario distribuire per la soluzione.

È possibile creare modelli di risorse dalla sezione **Impostazioni** per una determinata macchina virtuale selezionando l'opzione Script di automazione.

![Script di automazione per la macchina virtuale](../media/4-automation-script.png)

È possibile salvare il modello di risorse per usarlo in un secondo momento o distribuire subito una nuova macchina virtuale basata su di esso. Ad esempio, si potrebbe creare una macchina virtuale da un modello in un ambiente di test e riscontrare che non è adatta a sostituire quella locale. È possibile eliminare il gruppo di risorse, azione che eliminerà tutte le risorse, modificare il modello e riprovare. Se si vogliono solo apportare modifiche alle risorse distribuite esistenti, è possibile modificare il modello usato per crearla e distribuirla nuovamente. Resource Manager modificherà le risorse in modo da correlarle al nuovo modello.

Una volta ottenuto il modello desiderato, è possibile usarlo per ricreare più versioni dell'infrastruttura, ad esempio per la gestione temporanea e la produzione. È possibile impostare i parametri per i campi, ad esempio il nome della macchina virtuale, il nome di rete, il nome dell'account di archiviazione e così via, con la possibilità di caricare il modello più volte usando parametri diversi per personalizzare ciascun ambiente.

È possibile usare strumenti per gli script di automazione come l'interfaccia della riga di comando di Azure, Azure PowerShell o anche le API REST di Azure con il linguaggio di programmazione preferito per elaborare i modelli di risorse, cosa che rende questo strumento utile per rendere subito operativa l'infrastruttura.

## <a name="azure-powershell"></a>Azure PowerShell

La creazione di script di amministrazione rappresenta un potente strumento per ottimizzare il flusso di lavoro. È possibile automatizzare le attività quotidiane e ripetitive. Inoltre, dopo aver verificato uno script, questo verrà eseguito uniformemente, con una probabile riduzione degli errori. **Azure PowerShell** è ideale per attività occasionali interattive e/o per l'automazione di attività ripetute.

> [!NOTE]
> PowerShell è una shell multipiattaforma che fornisce servizi come la finestra della shell e l'analisi dei comandi. Azure PowerShell è un pacchetto aggiuntivo facoltativo che aggiunge i comandi specifici di Azure (detti **cmdlet**). È possibile ottenere altre informazioni sull'installazione e l'utilizzo di Azure PowerShell in un modulo di formazione separato.

Ad esempio, è possibile usare il cmdlet `New-AzureRmVM` per creare una nuova macchina virtuale di Azure.

```powershell
New-AzureRmVm `
    -ResourceGroupName "TestResourceGroup" `
    -Name "test-wp1-eus-vm" `
    -Location "East US" `
    -VirtualNetworkName "test-wp1-eus-network" `
    -SubnetName "default" `
    -SecurityGroupName "test-wp1-eus-nsg" `
    -PublicIpAddressName "test-wp1-eus-pubip" `
    -OpenPorts 80,3389
```

Come illustrato di seguito, vengono forniti vari parametri per gestire tutte le varie impostazioni di configurazione della macchina virtuale disponibili. La maggior parte dei parametri ha valori accettabili, quindi è necessario specificare solo quelli richiesti. Altre informazioni sulla creazione e la gestione di macchine virtuali con Azure PowerShell sono disponibili nel modulo **Automatizzare le attività di Azure usando gli script con PowerShell**.

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Un'altra opzione per l'interazione di scripting e da riga di comando in Azure è l'**interfaccia della riga di comando di Azure**.

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure, ad esempio macchine virtuali e dischi, dalla riga di comando. È disponibile per macOS, Linux e Windows oppure nel browser, con Cloud Shell. Come Azure PowerShell, l'interfaccia della riga di comando di Azure è un potente strumento da usare per semplificare il flusso di lavoro di amministrazione. A differenza di Azure PowerShell, l'interfaccia della riga di comando di Azure non necessita di PowerShell per il funzionamento.

Ad esempio, è possibile creare una macchina virtuale di Azure con il comando `az vm create`.

```azurecli
az vm create \
    --resource-group TestResourceGroup \
    --name test-wp1-eus-vm \
    --image win2016datacenter \
    --admin-username jonc \
    --admin-password aReallyGoodPasswordHere
```

L'interfaccia della riga di comando di Azure è compatibile con altri linguaggi di scripting, ad esempio Ruby e Python. Entrambi i linguaggi vengono comunemente usati nei computer non basati su Windows in cui lo sviluppatore potrebbe non avere familiarità con PowerShell.

Altre informazioni sulla creazione e la gestione di macchine virtuali sono disponibili nel modulo **Gestire le macchine virtuali con l'interfaccia della riga di comando di Azure**.

## <a name="programmatic-apis"></a>Approccio a livello di codice (API)

In generale, Azure PowerShell e l'interfaccia della riga di comando di Azure sono modi validi se si hanno script semplici da eseguire e si preferisce usare strumenti da riga di comando. Quando entrano in gioco scenari più complessi, dove la creazione e la gestione delle macchine virtuali fanno parte di un'applicazione più ampia con logica più strutturata, è necessario un approccio diverso.

È possibile interagire con tutti i tipi di risorse in Azure a livello di codice.

### <a name="azure-rest-api"></a>API REST di Azure

L'API REST di Azure offre agli sviluppatori operazioni categorizzate per risorsa, oltre alla possibilità di creare e gestire le macchine virtuali. Le operazioni sono esposte come URI con i corrispondenti metodi HTTP (`GET`, `PUT`, `POST`, `DELETE` e `PATCH`) e una risposta corrispondente.

Le API di calcolo di Azure forniscono accesso a livello di codice alle macchine virtuali e alle relative risorse di supporto. Con questa API, sono disponibili operazioni per:

- Creare e gestire set di disponibilità
- Aggiungere e gestire estensioni macchina virtuale
- Creare e gestire dischi gestiti, snapshot e immagini
- Accedere alle immagini della piattaforma disponibili in Azure
- Recuperare le informazioni sull'utilizzo delle risorse
- Creare e gestire macchine virtuali
- Creare e gestire i set di scalabilità delle macchine virtuali

### <a name="azure-client-sdk"></a>SDK per client Azure

Anche se l'API REST non dipende da una piattaforma o da un linguaggio, spesso gli sviluppatori cercheranno un livello superiore di astrazione. L'SDK per client Azure incapsula l'API REST di Azure, facilitando l'interazione degli sviluppatori con Azure.

Gli SDK per client Azure sono disponibili per vari linguaggi e framework, inclusi quelli basati su .NET come C#, Java, Node.js, PHP, Python, Ruby e Go.

Ecco un frammento di codice C# di esempio per creare una macchina virtuale di Azure usando il pacchetto NuGet `Microsoft.Azure.Management.Fluent`:

```csharp
var azure = Azure
    .Configure()
    .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
    .Authenticate(credentials)
    .WithDefaultSubscription();
// ...
var vmName = "test-wp1-eus-vm";

azure.VirtualMachines.Define(vmName)
    .WithRegion(Region.USEast)
    .WithExistingResourceGroup("TestResourceGroup")
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("jonc")
    .WithAdminPassword("aReallyGoodPasswordHere")
    .WithComputerName(vmName)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

Di seguito è riportato lo stesso frammento in Java con **Azure Java SDK**:

```java
String vmName = "test-wp1-eus-vm";
// ...
VirtualMachine virtualMachine = azure.virtualMachines()
    .define(vmName)
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("TestResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("jonc")
    .withAdminPassword("aReallyGoodPasswordHere")
    .withComputerName(vmName)
    .withSize("Standard_DS1")
    .create();
```

## <a name="azure-vm-extensions"></a>Estensioni di macchina virtuale di Azure

Si supponga di voler configurare e installare software aggiuntivo sulla macchina virtuale dopo la distribuzione iniziale. Si vuole che l'attività usi una configurazione specifica, monitorata ed eseguita automaticamente.

**Le estensioni macchina virtuale di Azure** sono piccole applicazioni che consentono di configurare e automatizzare le attività nelle macchine virtuali di Azure dopo la distribuzione iniziale. Le **estensioni macchina virtuale di Azure** possono essere eseguite con l'interfaccia della riga di comando di Azure, PowerShell, i modelli di Azure Resource Manager e il portale di Azure.

Possono essere aggregate con una nuova distribuzione di macchina virtuale o eseguite su un sistema esistente.

## <a name="azure-automation-services"></a>Servizi di Automazione di Azure

Alcune delle sfide operative più rilevanti nella gestione dell'infrastruttura remota consistono nel risparmiare tempo, ridurre gli errori e aumentare l'efficienza. Se si hanno molti servizi di infrastruttura, è consigliabile usare servizi di livello superiore in Azure per lavorare con più facilità.

**Automazione di Azure** permette di integrare i servizi che consentono di automatizzare con facilità le attività di gestione più frequenti, complesse e soggette a errori. Questi servizi includono **automazione dei processi**, **gestione della configurazione** e **gestione degli aggiornamenti**.

- **Gestione dei processi**. Si supponga di avere una macchina virtuale monitorata per un evento di errore specifico. Si vuole intervenire e risolvere il problema non appena viene segnalato. L'automazione dei processi consente di configurare attività watcher in grado di rispondere agli eventi che possono verificarsi nel data center.

- **Gestione della configurazione**.  Potrebbe essere necessario tener traccia degli aggiornamenti software disponibili per il sistema operativo eseguito nella macchina virtuale, con aggiornamenti specifici da includere o escludere. La gestione della configurazione consente di tenere traccia di questi aggiornamenti e di intervenire in base alle esigenze. È possibile usare **System Center Configuration Manager** per gestire PC, server e dispositivi mobili aziendali. Il supporto può essere esteso alle macchine virtuali di Azure grazie a Configuration Manager.

- **Gestione aggiornamenti**. È possibile gestire gli aggiornamenti e le patch per le macchine virtuali. Con questo servizio, si può valutare in modo rapido lo stato degli aggiornamenti disponibili, pianificare l'installazione ed esaminare i risultati della distribuzione per verificare che gli aggiornamenti siano stati applicati correttamente. La gestione degli aggiornamenti include servizi che permettono di gestire i processi e la configurazione. È possibile abilitare Gestione aggiornamenti per una macchina virtuale direttamente nell'account di **Automazione di Azure**. È anche possibile abilitare Gestione aggiornamenti per una singola macchina virtuale dal riquadro della macchina virtuale nel portale.

Come si può notare, Azure offre un'ampia gamma di strumenti per creare e amministrare le risorse, quindi è possibile integrare le operazioni di gestione nel processo _più appropriato_. Ora verranno esaminati altri servizi di Azure per assicurare il funzionamento ottimale delle risorse di infrastruttura.
