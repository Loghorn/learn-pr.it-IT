<span data-ttu-id="1f24c-101">Si supponga di lavorare presso una società che sviluppa una suite di strumenti di amministrazione per Linux.</span><span class="sxs-lookup"><span data-stu-id="1f24c-101">Suppose you work at a company that makes a suite of Linux admin tools.</span></span> <span data-ttu-id="1f24c-102">La propria mansione è aiutare i potenziali clienti a provare il software prima dell'acquisto.</span><span class="sxs-lookup"><span data-stu-id="1f24c-102">Your job is to help potential customers try your software before they buy it.</span></span> <span data-ttu-id="1f24c-103">Poiché il software apporta modifiche al sistema operativo a livello di radice, si è deciso di creare una macchina virtuale Linux per ogni cliente con una versione di valutazione.</span><span class="sxs-lookup"><span data-stu-id="1f24c-103">Because the software makes root-level changes to the OS, you have decided to create a Linux VM for each trial customer.</span></span> <span data-ttu-id="1f24c-104">Le macchine virtuali vengono create in base alle esigenze ed eliminate al termine della sottoscrizione di valutazione.</span><span class="sxs-lookup"><span data-stu-id="1f24c-104">You create the VMs as needed and delete them at the end of the trial subscription.</span></span> <span data-ttu-id="1f24c-105">In questo modo, ogni cliente inizia con una versione pulita del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="1f24c-105">This way, each customer starts with a clean version of the OS.</span></span> 

<span data-ttu-id="1f24c-106">Per mantenere queste macchine virtuali separate dalle macchine virtuali usate nell'azienda per i test interni, verrà creato un gruppo di risorse dedicato per ospitarle.</span><span class="sxs-lookup"><span data-stu-id="1f24c-106">To keep these VMs separate from the VMs your company uses for internal testing, you will create a dedicated resource group to house them.</span></span> <span data-ttu-id="1f24c-107">Poiché è necessario un solo gruppo di risorse, l'uso di Azure PowerShell in modalità interattiva rappresenta una scelta ragionevole per questa attività.</span><span class="sxs-lookup"><span data-stu-id="1f24c-107">You only need one resource group so using Azure PowerShell in interactive mode is a reasonable choice for this task.</span></span>

## <a name="steps-to-create-a-resource-group"></a><span data-ttu-id="1f24c-108">Procedura per creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="1f24c-108">Steps to create a resource group</span></span>

1. <span data-ttu-id="1f24c-109">Avviare PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f24c-109">Launch PowerShell.</span></span>

1. <span data-ttu-id="1f24c-110">Importare il modulo nella sessione corrente, in modo da avere accesso ai cmdlet di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f24c-110">Import the module into the current session so you have access to the Azure cmdlets.</span></span>

   ```powershell
   Import-Module AzureRM
   ```

1. <span data-ttu-id="1f24c-111">Collegarsi ad Azure usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="1f24c-111">Connect to Azure using the command shown below.</span></span> <span data-ttu-id="1f24c-112">Dopo aver immesso il comando, eseguire l'autenticazione fornendo le credenziali di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f24c-112">After entering the command, authenticate by providing your Azure credentials.</span></span>

   ```powershell
   Connect-AzureRmAccount
   ```

1. <span data-ttu-id="1f24c-113">Creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="1f24c-113">Create a resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name "TrialsResourceGroup" -Location "East US"
    ```

1. <span data-ttu-id="1f24c-114">Verificare che il gruppo di risorse sia stato creato correttamente.</span><span class="sxs-lookup"><span data-stu-id="1f24c-114">Verify the resource group was created successfully.</span></span>

    ```powershell
    Get-AzureRmResource | Format-Table
    ```
<span data-ttu-id="1f24c-115">Un altro modo per verificare se il gruppo di risorse è stato creato correttamente è usare il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f24c-115">Another way to check whether the resource group was created successfully is to use the Azure Portal.</span></span> <span data-ttu-id="1f24c-116">A tale scopo, accedere al portale e passare alla sezione **Gruppi di risorse** (vedere di seguito).</span><span class="sxs-lookup"><span data-stu-id="1f24c-116">To do this, login to the Portal and navigate to the **Resource Groups** section (see below).</span></span> <span data-ttu-id="1f24c-117">Il nuovo gruppo di risorse dovrebbe essere visualizzato nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="1f24c-117">The new resource group should be displayed in the list.</span></span>

<span data-ttu-id="1f24c-118">Lo screenshot seguente mostra la posizione della categoria Gruppi di risorse nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f24c-118">The following screenshot shows the location of the Resource groups category in the Azure Portal.</span></span>

![Screenshot del pannello Preferiti del portale di Azure con la categoria Gruppo di risorse evidenziata.](../media/6-listing-resource-groups.png)

## <a name="summary"></a><span data-ttu-id="1f24c-120">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1f24c-120">Summary</span></span>
<span data-ttu-id="1f24c-121">In questo esercizio viene illustrato un modello comune per una sessione di PowerShell interattiva.</span><span class="sxs-lookup"><span data-stu-id="1f24c-121">This exercise shows a common pattern for an interactive PowerShell session.</span></span> <span data-ttu-id="1f24c-122">Sono stati usati un cmdlet standard per importare il modulo AzureRM, quindi i cmdlet di Azure PowerShell per eseguire un'attività specifica.</span><span class="sxs-lookup"><span data-stu-id="1f24c-122">You used a standard cmdlet to import the AzureRM module and then the Azure PowerShell cmdlets to perform a specific task.</span></span> <span data-ttu-id="1f24c-123">A questo punto, è disponibile un gruppo di risorse nella sottoscrizione e si è pronti per creare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="1f24c-123">You now have a resource group in your subscription and are ready to create VMs.</span></span>