<span data-ttu-id="4c016-101">In questa unità si continuerà con l'esempio di un'azienda che sviluppa strumenti di amministrazione per Linux.</span><span class="sxs-lookup"><span data-stu-id="4c016-101">In this unit, you will continue with the example of a company that makes Linux admin tools.</span></span> <span data-ttu-id="4c016-102">È importante ricordare che si prevede di usare macchine virtuali Linux per consentire ai potenziali clienti di testare il software.</span><span class="sxs-lookup"><span data-stu-id="4c016-102">Recall that you plan to use Linux VMs to let potential customers test your software.</span></span> <span data-ttu-id="4c016-103">È stato predisposto un gruppo di risorse e a questo punto è possibile creare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="4c016-103">You have a resource group ready and now it is time to create the VMs.</span></span>

<span data-ttu-id="4c016-104">L'azienda presso cui si lavora prevede di organizzare uno stand nel corso una grande fiera dedicata a Linux.</span><span class="sxs-lookup"><span data-stu-id="4c016-104">Your company has paid for a booth at a big Linux trade show.</span></span> <span data-ttu-id="4c016-105">È stata pianificata un'area demo con tre terminali, ognuno connesso a una macchina virtuale Linux distinta.</span><span class="sxs-lookup"><span data-stu-id="4c016-105">You plan a demo area containing three terminals each connected to a separate Linux VM.</span></span> <span data-ttu-id="4c016-106">Alla fine di ogni giornata, si vuole eliminare e ricreare le macchine virtuali, in modo che ogni mattina siano nuove.</span><span class="sxs-lookup"><span data-stu-id="4c016-106">At the end of each day, you want to delete the VMs and recreate them, so they start fresh every morning.</span></span> <span data-ttu-id="4c016-107">Ricreare manualmente le macchine virtuali dopo una giornata di lavoro potrebbe dare luogo a errori.</span><span class="sxs-lookup"><span data-stu-id="4c016-107">Creating the VMs manually after work when you are tired would be error prone.</span></span> <span data-ttu-id="4c016-108">Si vuole scrivere uno script di PowerShell per automatizzare il processo di creazione delle macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="4c016-108">You want to write a PowerShell script to automate the VM creation process.</span></span>

## <a name="write-a-script-that-creates-virtual-machines"></a><span data-ttu-id="4c016-109">Scrivere uno script per la creazione delle macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="4c016-109">Write a script that creates Virtual Machines</span></span>

<span data-ttu-id="4c016-110">Seguire questi passaggi in Cloud Shell a destra per scrivere lo script:</span><span class="sxs-lookup"><span data-stu-id="4c016-110">Follow these steps in the Cloud Shell on the right to write the script:</span></span>

1. <span data-ttu-id="4c016-111">Passare alla cartella principale in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="4c016-111">Switch to your home folder in the Cloud Shell.</span></span>

    ```powershell
    cd $HOME\clouddrive
    ```

1. <span data-ttu-id="4c016-112">Creare un nuovo file di testo denominato **ConferenceDailyReset.ps1**.</span><span class="sxs-lookup"><span data-stu-id="4c016-112">Create a new text file named **ConferenceDailyReset.ps1**.</span></span>

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. <span data-ttu-id="4c016-113">Aprire l'editor integrato e selezionare il file **ConferenceDailyReset.ps1**.</span><span class="sxs-lookup"><span data-stu-id="4c016-113">Open the integrated editor and select the **ConferenceDailyReset.ps1** file.</span></span>

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > <span data-ttu-id="4c016-114">Il servizio Cloud Shell integrato supporta anche vim, nano ed emacs, se si preferisce usare uno di questi editor.</span><span class="sxs-lookup"><span data-stu-id="4c016-114">The integrated Cloud Shell also supports vim, nano, and emacs if you'd prefer to use one of those editors.</span></span>

1. <span data-ttu-id="4c016-115">Per iniziare, acquisire il parametro di input in una variabile.</span><span class="sxs-lookup"><span data-stu-id="4c016-115">Start by capturing the input parameter in a variable.</span></span> <span data-ttu-id="4c016-116">Aggiungere la riga seguente allo script:</span><span class="sxs-lookup"><span data-stu-id="4c016-116">Add the following line to your script:</span></span>

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > <span data-ttu-id="4c016-117">In genere, è necessario eseguire l'autenticazione con Azure usando le credenziali con `Connect-AzureRmAccount` e questa operazione può essere eseguita nello script.</span><span class="sxs-lookup"><span data-stu-id="4c016-117">Normally, you'd have to authenticate with Azure using your credentials using `Connect-AzureRmAccount` and this could be done in the script.</span></span> <span data-ttu-id="4c016-118">Tuttavia, nell'ambiente Cloud Shell l'utente è già autenticato quindi l'operazione non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="4c016-118">However, in the Cloud Shell environment you will already be authenticated so this is unnecessary.</span></span>

1. <span data-ttu-id="4c016-119">Richiedere il nome utente e la password per l'account di amministratore della macchina virtuale e acquisire il risultato in una variabile:</span><span class="sxs-lookup"><span data-stu-id="4c016-119">Prompt for a username and password for the VM's admin account and capture the result in a variable:</span></span>

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. <span data-ttu-id="4c016-120">Creare un ciclo da eseguire tre volte:</span><span class="sxs-lookup"><span data-stu-id="4c016-120">Create a loop that executes three times:</span></span>

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. <span data-ttu-id="4c016-121">Nel corpo del ciclo creare un nome per ogni macchina virtuale, archiviarlo in una variabile e inviarlo alla console:</span><span class="sxs-lookup"><span data-stu-id="4c016-121">In the loop body, create a name for each VM and store it in a variable and output it to the console:</span></span>

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. <span data-ttu-id="4c016-122">Creare quindi una macchina virtuale usando la variabile `$vmName`:</span><span class="sxs-lookup"><span data-stu-id="4c016-122">Next, create a VM using the `$vmName` variable:</span></span>

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. <span data-ttu-id="4c016-123">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="4c016-123">Save the file.</span></span> <span data-ttu-id="4c016-124">È possibile usare il menu "..." nell'angolo superiore destro dell'editor.</span><span class="sxs-lookup"><span data-stu-id="4c016-124">You can use the "..." menu at the top right corner of the editor.</span></span> <span data-ttu-id="4c016-125">Sono disponibili anche i tasti di scelta rapida comuni per salvare.</span><span class="sxs-lookup"><span data-stu-id="4c016-125">There are also common accelerator keys for Save.</span></span>

<span data-ttu-id="4c016-126">Lo script completato avrà l'aspetto seguente:</span><span class="sxs-lookup"><span data-stu-id="4c016-126">The completed script should look like this:</span></span>

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a><span data-ttu-id="4c016-127">Eseguire lo script</span><span class="sxs-lookup"><span data-stu-id="4c016-127">Execute the script</span></span>

<span data-ttu-id="4c016-128">Chiudere l'editor usando il menu di scelta rapida "...".</span><span class="sxs-lookup"><span data-stu-id="4c016-128">Close the editor through the "..." context menu.</span></span>

1. <span data-ttu-id="4c016-129">Eseguire lo script.</span><span class="sxs-lookup"><span data-stu-id="4c016-129">Execute the script.</span></span>

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[sandbox resource group name]</rgn>
    ```
    
<span data-ttu-id="4c016-130">Il completamento dello script può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="4c016-130">The script will take several minutes to complete.</span></span> <span data-ttu-id="4c016-131">Al termine, verificare se è stato eseguito correttamente esaminando le risorse che ora sono disponibili nel gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="4c016-131">When it is finished, verify that it ran successfully by looking at the resources you now have in your resource group:</span></span>

```powershell
Get-AzureRmResource -ResourceType Microsoft.Compute/virtualMachines
```

<span data-ttu-id="4c016-132">Dovrebbero essere visualizzate tre macchine virtuali, ognuna con un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="4c016-132">You should see three VMs, each with a unique name.</span></span>

<span data-ttu-id="4c016-133">È stato scritto uno script per automatizzare la creazione di tre macchine virtuali nel gruppo di risorse indicato da un parametro dello script.</span><span class="sxs-lookup"><span data-stu-id="4c016-133">You wrote a script that automated the creation of three VMs in the resource group indicated by a script parameter.</span></span> <span data-ttu-id="4c016-134">Lo script è breve e semplice ma consente di automatizzare un processo il cui completamento manuale tramite il portale richiederebbe molto tempo.</span><span class="sxs-lookup"><span data-stu-id="4c016-134">The script is short and simple but automates a process that would take a long time to complete manually with the portal.</span></span>