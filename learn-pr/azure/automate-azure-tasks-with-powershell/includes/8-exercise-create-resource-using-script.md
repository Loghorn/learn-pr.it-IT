<span data-ttu-id="47363-101">In questo esercizio, si continuerà con l'esempio di un'azienda che sviluppa strumenti di amministrazione per Linux.</span><span class="sxs-lookup"><span data-stu-id="47363-101">In this exercise, you will continue with the example of a company that makes Linux admin tools.</span></span> <span data-ttu-id="47363-102">È importante ricordare che si prevede di usare macchine virtuali Linux per consentire ai potenziali clienti di testare il software.</span><span class="sxs-lookup"><span data-stu-id="47363-102">Recall that you plan to use Linux VMs to let potential customers test your software.</span></span> <span data-ttu-id="47363-103">È stato predisposto un gruppo di risorse e a questo punto è possibile creare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="47363-103">You have a resource group ready and now it is time to create the VMs.</span></span>

<span data-ttu-id="47363-104">L'azienda presso cui si lavora prevede di organizzare uno stand nel corso una grande fiera dedicata a Linux.</span><span class="sxs-lookup"><span data-stu-id="47363-104">Your company has paid for a booth at a big Linux trade show.</span></span> <span data-ttu-id="47363-105">È stata pianificata un'area demo con tre terminali, ognuno connesso a una macchina virtuale Linux distinta.</span><span class="sxs-lookup"><span data-stu-id="47363-105">You plan a demo area containing three terminals each connected to a separate Linux VM.</span></span> <span data-ttu-id="47363-106">Alla fine di ogni giornata, si vuole eliminare e ricreare le macchine virtuali, in modo che ogni mattina siano nuove.</span><span class="sxs-lookup"><span data-stu-id="47363-106">At the end of each day, you want to delete the VMs and recreate them, so they start fresh every morning.</span></span> <span data-ttu-id="47363-107">Ricreare manualmente le macchine virtuali dopo una giornata di lavoro potrebbe dare luogo a errori.</span><span class="sxs-lookup"><span data-stu-id="47363-107">Creating the VMs manually after work when you are tired would be error prone.</span></span> <span data-ttu-id="47363-108">Si vuole scrivere uno script di PowerShell per automatizzare il processo di creazione delle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="47363-108">You want to write a PowerShell script to automate the VM creation process.</span></span>

## <a name="write-a-script-that-creates-virtual-machines"></a><span data-ttu-id="47363-109">Scrivere uno script per la creazione delle macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="47363-109">Write a script that creates Virtual Machines</span></span>

<span data-ttu-id="47363-110">Per scrivere lo script, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="47363-110">Follow these steps to write the script:</span></span>

1. <span data-ttu-id="47363-111">Creare un nuovo file di testo denominato **ConferenceDailyReset.ps1**.</span><span class="sxs-lookup"><span data-stu-id="47363-111">Create a new text file named **ConferenceDailyReset.ps1**.</span></span>

1. <span data-ttu-id="47363-112">Acquisire il parametro in una variabile:</span><span class="sxs-lookup"><span data-stu-id="47363-112">Capture the parameter in a variable:</span></span>

    ```powershell
    param([string]$resourceGroup)
    ```

1. <span data-ttu-id="47363-113">Eseguire l'autenticazione in Azure con le proprie credenziali:</span><span class="sxs-lookup"><span data-stu-id="47363-113">Authenticate with Azure using your credentials:</span></span>

    ```powershell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="47363-114">Richiedere il nome utente e la password per l'account di amministratore della macchina virtuale e acquisire il risultato in una variabile:</span><span class="sxs-lookup"><span data-stu-id="47363-114">Prompt for a username and password for the VM's admin account and capture the result in a variable:</span></span>

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. <span data-ttu-id="47363-115">Creare un ciclo da eseguire tre volte:</span><span class="sxs-lookup"><span data-stu-id="47363-115">Create a loop that executes three times:</span></span>

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. <span data-ttu-id="47363-116">Nel corpo del ciclo, creare un nome per ogni macchina virtuale e archiviarlo in una variabile:</span><span class="sxs-lookup"><span data-stu-id="47363-116">In the loop body, create a name for each VM and store it in a variable:</span></span>

    ```powershell
    $vmName = "ConferenceDemo" + $i
    ```

1. <span data-ttu-id="47363-117">Creare quindi una macchina virtuale usando la variabile `$vmName`:</span><span class="sxs-lookup"><span data-stu-id="47363-117">Next, create a VM using the `$vmName` variable:</span></span>

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
   ```

1. <span data-ttu-id="47363-118">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="47363-118">Save the file.</span></span>

<span data-ttu-id="47363-119">Lo script completato avrà l'aspetto seguente:</span><span class="sxs-lookup"><span data-stu-id="47363-119">The completed script should look like this:</span></span>

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

Connect-AzureRmAccount

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i

    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a><span data-ttu-id="47363-120">Eseguire lo script</span><span class="sxs-lookup"><span data-stu-id="47363-120">Execute the script</span></span>

<span data-ttu-id="47363-121">Avviare PowerShell e passare alla directory in cui è stato salvato il file di script.</span><span class="sxs-lookup"><span data-stu-id="47363-121">Launch PowerShell and change to the directory where you saved the script file.</span></span> <span data-ttu-id="47363-122">Per eseguire lo script, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="47363-122">To run the script, execute the following command:</span></span>

```powershell
.\ConferenceDailyReset.ps1 TrialsResourceGroup
```

<span data-ttu-id="47363-123">Per il completamento dello script possono essere necessari alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="47363-123">The script may take a few minutes to complete.</span></span> <span data-ttu-id="47363-124">Al termine, verificare che sia stato eseguito correttamente:</span><span class="sxs-lookup"><span data-stu-id="47363-124">When it is finished, verify that it ran successfully:</span></span>

1. <span data-ttu-id="47363-125">Da un browser accedere al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="47363-125">In a browser, sign into the Azure portal.</span></span>

1. <span data-ttu-id="47363-126">Nel riquadro di spostamento a sinistra fare clic su **Gruppi di risorse**.</span><span class="sxs-lookup"><span data-stu-id="47363-126">In the navigation on the left, click **Resource Groups**.</span></span>

1. <span data-ttu-id="47363-127">Nell'elenco dei gruppi di risorse fare clic su **TrialsResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="47363-127">In the list of resource groups, click **TrialsResourceGroup**.</span></span> <span data-ttu-id="47363-128">Nell'elenco delle risorse dovrebbero essere visibili le macchine virtuali appena create e le risorse associate.</span><span class="sxs-lookup"><span data-stu-id="47363-128">In the list of resources, you should see the newly created VMs and their associated resources.</span></span>

## <a name="summary"></a><span data-ttu-id="47363-129">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="47363-129">Summary</span></span>
<span data-ttu-id="47363-130">È stato scritto uno script per automatizzare la creazione di tre macchine virtuali nel gruppo di risorse indicato da un parametro dello script.</span><span class="sxs-lookup"><span data-stu-id="47363-130">You wrote a script that automated the creation of three VMs in the resource group indicated by a script parameter.</span></span> <span data-ttu-id="47363-131">Lo script è breve e semplice ma consente di automatizzare un processo il cui completamento manuale tramite il portale richiederebbe molto tempo.</span><span class="sxs-lookup"><span data-stu-id="47363-131">The script is short and simple but automates a process that would take a long time to complete manually with the portal.</span></span>