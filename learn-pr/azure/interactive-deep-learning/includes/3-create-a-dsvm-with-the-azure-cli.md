<span data-ttu-id="75ce1-101">Gli sviluppatori software di un'azienda hanno l'opportunità di arricchire le loro competenze ed entrare a far parte del team di intelligenza artificiale in-house.</span><span class="sxs-lookup"><span data-stu-id="75ce1-101">As a software developer at your company, you've the opportunity to grow your skills and become part of the in-house AI team.</span></span> <span data-ttu-id="75ce1-102">Pur impegnandosi a raggiungere questo nuovo ruolo, devono comunque svolgere il loro lavoro quotidiano.</span><span class="sxs-lookup"><span data-stu-id="75ce1-102">While you ramp-up on this exciting new role, you still have your day job to do.</span></span> <span data-ttu-id="75ce1-103">Il senior engineer di intelligenza artificiale del team ha segnalato alcuni utili notebook Jupyter con lab basati su PyTorch per il training di un modello di classificazione delle immagini.</span><span class="sxs-lookup"><span data-stu-id="75ce1-103">The senior AI engineer on your team has told you about some useful Jupyter notebooks that have PyTorch-based labs to train an image classification model.</span></span> <span data-ttu-id="75ce1-104">Interessante, ma non è il caso di installare un set di framework nella piattaforma di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="75ce1-104">Exciting stuff, but you don't want to install a set of frameworks onto your dev rig.</span></span> <span data-ttu-id="75ce1-105">Al contrario, creare una macchina virtuale basata sull'immagine della Data Science Virtual Machine (DSVM).</span><span class="sxs-lookup"><span data-stu-id="75ce1-105">Instead, you decide to create a virtual machine based on the Data Science Virtual Machine (DSVM) image.</span></span> 

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="75ce1-106">Che cos'è l'interfaccia della riga di comando di Azure?</span><span class="sxs-lookup"><span data-stu-id="75ce1-106">What is the Azure CLI</span></span>

<span data-ttu-id="75ce1-107">L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="75ce1-107">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="75ce1-108">È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="75ce1-108">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span> <span data-ttu-id="75ce1-109">L'uso di questo strumento è descritto in dettaglio nel modulo **Controllare i servizi di Azure con l'interfaccia della riga di comando**.</span><span class="sxs-lookup"><span data-stu-id="75ce1-109">We have complete coverage of using this tool in the **Control Azure services with the CLI** module.</span></span>

## <a name="managing-deployments"></a><span data-ttu-id="75ce1-110">Gestione delle distribuzioni</span><span class="sxs-lookup"><span data-stu-id="75ce1-110">Managing deployments</span></span>

<span data-ttu-id="75ce1-111">L'interfaccia della riga di comando di Azure include il comando `az group deployment` per gestire le distribuzioni di Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="75ce1-111">The Azure CLI includes the `az group deployment` command to manage Azure Resource Manager deployments.</span></span> <span data-ttu-id="75ce1-112">È possibile fornire vari sottocomandi per eseguire attività specifiche.</span><span class="sxs-lookup"><span data-stu-id="75ce1-112">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="75ce1-113">Quelli più comuni includono:</span><span class="sxs-lookup"><span data-stu-id="75ce1-113">The most common include:</span></span>

| <span data-ttu-id="75ce1-114">Sottocomando</span><span class="sxs-lookup"><span data-stu-id="75ce1-114">Subcommand</span></span> | <span data-ttu-id="75ce1-115">Descrizione</span><span class="sxs-lookup"><span data-stu-id="75ce1-115">Description</span></span> |
|-------------|-------------|
| `create` | <span data-ttu-id="75ce1-116">Avvia una distribuzione.</span><span class="sxs-lookup"><span data-stu-id="75ce1-116">Start a deployment.</span></span> |
| `list` | <span data-ttu-id="75ce1-117">Ottiene tutte le distribuzioni di un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="75ce1-117">Get all the deployments for a resource group.</span></span> |
| `export` | <span data-ttu-id="75ce1-118">Esporta il modello usato per una distribuzione.</span><span class="sxs-lookup"><span data-stu-id="75ce1-118">Export the template used for a deployment.</span></span> |

<span data-ttu-id="75ce1-119">Per un elenco completo dei comandi di distribuzione disponibili, vedere il [riferimento per il comando di distribuzione az](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)</span><span class="sxs-lookup"><span data-stu-id="75ce1-119">For a complete list of available deployment commands, see the [az group deployment command reference](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)</span></span>

<span data-ttu-id="75ce1-120">Si userà `az group deployment create` per eseguire il provisioning della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="75ce1-120">We'll use `az group deployment create` to provision our virtual machine.</span></span>

## <a name="create-a-json-deployment-parameters-file"></a><span data-ttu-id="75ce1-121">Creare un file di parametri di distribuzione JSON</span><span class="sxs-lookup"><span data-stu-id="75ce1-121">Create a JSON deployment parameters file</span></span>

<span data-ttu-id="75ce1-122">Si creerà una VM con un modello di Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="75ce1-122">We're going to create our VM using an Azure Resource Manager template.</span></span> <span data-ttu-id="75ce1-123">Il modello definisce l'immagine di DSVM di Linux di cui si intende eseguire il provisioning.</span><span class="sxs-lookup"><span data-stu-id="75ce1-123">The template defines the Linux DSVM image we want to provision.</span></span> <span data-ttu-id="75ce1-124">È necessario fornire alcuni parametri al modello, ad esempio le dimensioni della VM che si intende usare, il nome utente amministratore e la password e il nome host del computer.</span><span class="sxs-lookup"><span data-stu-id="75ce1-124">We need to supply some parameters to the template, such as the VM size we want to use, the admin username and password, and the host name of our machine.</span></span> 

1. <span data-ttu-id="75ce1-125">Eseguire il comando seguente in Azure Cloud Shell a destra di questa unità:</span><span class="sxs-lookup"><span data-stu-id="75ce1-125">Execute the following command in Azure Cloud Shell to the right of this unit:</span></span>

    ```bash
    code parameter_file.json
    ```
    <span data-ttu-id="75ce1-126"><!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> Questo comando apre un file vuoto denominato `parameter_file.json` nell'editor integrato.</span><span class="sxs-lookup"><span data-stu-id="75ce1-126"><!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> This command opens and empty file called `parameter_file.json` in the built-in editor.</span></span> 

1. <span data-ttu-id="75ce1-127">Incollare il frammento JSON seguente nel file vuoto nell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="75ce1-127">Paste the following JSON snippet into the empty file into the code editor.</span></span>

    ```json
    { 
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "adminUsername": { "value" : "<USERNAME>"},
         "adminPassword": { "value" : "<PASSWORD>"},
         "vmName": { "value" : "<HOSTNAME>"},
         "vmSize": { "value" : "Standard_DS2_v2"},
      }
    }
    ```

1. <span data-ttu-id="75ce1-128">Aggiornare i parametri seguenti nel frammento JSON incollato nell'editor:</span><span class="sxs-lookup"><span data-stu-id="75ce1-128">Update the following parameters in the JSON you pasted in the editor:</span></span>

    |<span data-ttu-id="75ce1-129">Parametro</span><span class="sxs-lookup"><span data-stu-id="75ce1-129">Parameter</span></span>  |<span data-ttu-id="75ce1-130">Valore corrente</span><span class="sxs-lookup"><span data-stu-id="75ce1-130">Current value</span></span>  |<span data-ttu-id="75ce1-131">Valore personalizzato</span><span class="sxs-lookup"><span data-stu-id="75ce1-131">Your value</span></span>  |
    |---------|---------|---------|
    |<span data-ttu-id="75ce1-132">adminUsername</span><span class="sxs-lookup"><span data-stu-id="75ce1-132">adminUsername</span></span>     |  `<USERNAME>`       |    <span data-ttu-id="75ce1-133">Scegliere un nome per l'utente amministratore di questo nuovo computer, ad esempio *azuser*.</span><span class="sxs-lookup"><span data-stu-id="75ce1-133">Choose a name for the admin user of this new machine, such as, *azuser*.</span></span>     |
    |<span data-ttu-id="75ce1-134">adminPassword</span><span class="sxs-lookup"><span data-stu-id="75ce1-134">adminPassword</span></span>     |  `<PASSWORD>`       |   <span data-ttu-id="75ce1-135">Scegliere una password per questo account utente amministratore.</span><span class="sxs-lookup"><span data-stu-id="75ce1-135">Choose a password for this admin user account.</span></span> <span data-ttu-id="75ce1-136">Per altre informazioni sui requisiti delle password, vedere [Domande frequenti sulle macchine virtuali Linux](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)</span><span class="sxs-lookup"><span data-stu-id="75ce1-136">To learn more about password requirements, see [Frequently asked question about Linux Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)</span></span>     |
    |<span data-ttu-id="75ce1-137">vmName</span><span class="sxs-lookup"><span data-stu-id="75ce1-137">vmName</span></span>     |   `<HOSTNAME>`      |  <span data-ttu-id="75ce1-138">Scegliere un nome per la nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="75ce1-138">Choose a name for the new virtual machine.</span></span> <span data-ttu-id="75ce1-139">Il nome deve iniziare con una lettera e contenere solo lettere minuscole e numeri.</span><span class="sxs-lookup"><span data-stu-id="75ce1-139">Your name must begin with a letter and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="75ce1-140">Provare a scegliere un nome univoco, ad esempio uno che includa le iniziali del nome e l'anno di nascita.</span><span class="sxs-lookup"><span data-stu-id="75ce1-140">Try to choose a unique name, such as one that includes your initials and your birth year.</span></span> |
    |<span data-ttu-id="75ce1-141">vmSize</span><span class="sxs-lookup"><span data-stu-id="75ce1-141">vmSize</span></span>     |  <span data-ttu-id="75ce1-142">Standard_DS2_v2</span><span class="sxs-lookup"><span data-stu-id="75ce1-142">Standard_DS2_v2</span></span>       |  <span data-ttu-id="75ce1-143">Queste dimensioni di macchina virtuale sono idonee per questo esercizio, ma è possibile modificarle.</span><span class="sxs-lookup"><span data-stu-id="75ce1-143">This VM size will work fine for this exercise, but you are free to change it.</span></span> <span data-ttu-id="75ce1-144">Un elenco delle dimensioni di VM disponibili è reperibile in [Dimensioni delle macchine virtuali Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)</span><span class="sxs-lookup"><span data-stu-id="75ce1-144">A list of available vm sizes can be found here [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)</span></span>       |

1. <span data-ttu-id="75ce1-145">Salvare le modifiche in `parameter_file.json` e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="75ce1-145">Save your changes in `parameter_file.json` and close the text editor.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="75ce1-146">Ricordare i valori scelti per adminUsername, adminPassword e vmName.</span><span class="sxs-lookup"><span data-stu-id="75ce1-146">Remember the values you chose for adminUsername, adminPassword and vmName.</span></span> <span data-ttu-id="75ce1-147">Verranno usati di nuovo in questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="75ce1-147">We'll use them again in this exercise.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="75ce1-148">Creare un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="75ce1-148">Create a resource group</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="75ce1-149">In genere si crea un gruppo di risorse in un'area a propria scelta.</span><span class="sxs-lookup"><span data-stu-id="75ce1-149">Normally you'd create a resource group in a region of your choice.</span></span> <span data-ttu-id="75ce1-150">Tuttavia, la sessione sandbox in cui ci si trova fornisce un gruppo di risorse disponibili per l'uso.</span><span class="sxs-lookup"><span data-stu-id="75ce1-150">However, the sandbox session you are currently in supplies a resource group for you to use.</span></span> <span data-ttu-id="75ce1-151">Il gruppo di risorse per questa sessione è **<rgn>[nome gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="75ce1-151">Your resource group for this session is **<rgn>[Sandbox resource group name]</rgn>**.</span></span>

## <a name="deploy-the-dsvm-to-your-resource-group"></a><span data-ttu-id="75ce1-152">Distribuire la DSVM nel gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="75ce1-152">Deploy the DSVM to your resource group</span></span>

<span data-ttu-id="75ce1-153">Si è scelto un gruppo di risorse e sono stati definiti parametri per il modello di Resource Manager della DSVM in un file denominato `parameter_file.json`.</span><span class="sxs-lookup"><span data-stu-id="75ce1-153">We now have a resource group and have defined parameters for the DSVM Resource Manager template in a file called `parameter_file.json`.</span></span> <span data-ttu-id="75ce1-154">Si eseguirà quindi `az group deployment create` per eseguire il provisioning della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="75ce1-154">We'll run the `az group deployment create` next to provision our virtual machine.</span></span>

1. <span data-ttu-id="75ce1-155">Eseguire il comando seguente in Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="75ce1-155">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    az group deployment create \
    --resource-group  <rgn>[Sandbox resource group name]</rgn> \
    --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json \
    --parameters parameter_file.json
    ```

    <span data-ttu-id="75ce1-156">Il comando usa il modello di Resource Manager e i parametri per creare la macchina virtuale nel gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="75ce1-156">The command uses the Resource Manager template and our parameters to create the virtual machine in our resource group.</span></span> 

2. <span data-ttu-id="75ce1-157">La distribuzione di una macchina virtuale richiede alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="75ce1-157">Deploying a virtual machine takes a few minutes to complete.</span></span> <span data-ttu-id="75ce1-158">La console visualizza ` - Running ..` e poco altro fino al completamento dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="75ce1-158">The console displays ` - Running ..` and not much else until the operation completes.</span></span> <span data-ttu-id="75ce1-159">Al termine dell'operazione verrà visualizzata una risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="75ce1-159">When the operation finishes, a JSON response is output to the screen.</span></span> <span data-ttu-id="75ce1-160">Scorrere fino alla fine del file JSON e verificare che il campo **"provisioningState"** abbia il valore *Succeeded*.</span><span class="sxs-lookup"><span data-stu-id="75ce1-160">Scroll to the bottom of the JSON and check that the field **"provisioningState"** has the value *Succeeded*.</span></span>

    > [!TIP]
    > <span data-ttu-id="75ce1-161">Se si riceve un errore indicante che il record DNS è già usato da un altro indirizzo IP pubblico, provare a modificare **vmName** nel file `parameter_file.json` in un altro nome univoco.</span><span class="sxs-lookup"><span data-stu-id="75ce1-161">If you get an error stating that the DNS record is already used by another public IP, try changing **vmName** in `parameter_file.json` to another name that's unique.</span></span>

3. <span data-ttu-id="75ce1-162">Eseguire il comando seguente per ottenere informazioni sulla VM, sostituendo `<HOSTNAME>` con il nome host definito per la VM.</span><span class="sxs-lookup"><span data-stu-id="75ce1-162">Execute the following command to get information about the VM, replacing `<HOSTNAME>` with the host name you defined for your VM.</span></span>

    ```azurecli
    az vm get-instance-view \
    --name <HOSTNAME> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query instanceView.statuses[1] \
    --output table
    ```

    <span data-ttu-id="75ce1-163">Questo comando visualizza lo stato della VM.</span><span class="sxs-lookup"><span data-stu-id="75ce1-163">This command displays the status of the VM.</span></span> <span data-ttu-id="75ce1-164">Dovrebbe indicare *Esecuzione della macchina virtuale in corso*.</span><span class="sxs-lookup"><span data-stu-id="75ce1-164">It should say *VM running*.</span></span>

<span data-ttu-id="75ce1-165">Congratulazioni.</span><span class="sxs-lookup"><span data-stu-id="75ce1-165">Congratulations!</span></span> <span data-ttu-id="75ce1-166">È stata creata e distribuita una macchina virtuale Linux basata sull'immagine della DSVM.</span><span class="sxs-lookup"><span data-stu-id="75ce1-166">You've created and deployed a Linux VM based on the DSVM image.</span></span>

## <a name="open-the-vm-to-ssh-traffic-on-port-22"></a><span data-ttu-id="75ce1-167">Aprire la VM indirizzare tramite SSH il traffico sulla porta 22</span><span class="sxs-lookup"><span data-stu-id="75ce1-167">Open the VM to ssh traffic on port 22</span></span>

<span data-ttu-id="75ce1-168">Per impostazione predefinita, la macchina virtuale non ha alcuna porta aperta.</span><span class="sxs-lookup"><span data-stu-id="75ce1-168">By default, our VM doesn't have any ports open.</span></span> <span data-ttu-id="75ce1-169">L'obiettivo è di connettersi in remoto, avviare un server Jupyter Notebook ed eseguire altri comandi locali nel computer.</span><span class="sxs-lookup"><span data-stu-id="75ce1-169">Our goal is to connect remotely, start a Jupyter Notebook server and run other local commands on the machine.</span></span> <span data-ttu-id="75ce1-170">Per accedere in remoto alla macchina virtuale usando il protocollo Secure Shell (SSH), è necessario aprire una porta.</span><span class="sxs-lookup"><span data-stu-id="75ce1-170">To remote into the VM using the Secure Shell (SSH) protocol, we need to open a port.</span></span> <span data-ttu-id="75ce1-171">La porta predefinita per SSH è la porta 22.</span><span class="sxs-lookup"><span data-stu-id="75ce1-171">Port 22 is the default port for ssh.</span></span>  

1. <span data-ttu-id="75ce1-172">Eseguire il comando seguente in Azure Cloud Shell, sostituendo `<HOSTNAME>` con il nome assegnato alla DSVM durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="75ce1-172">Execute the following command in Azure Cloud Shell, replacing `<HOSTNAME>` wit the name you gave your DSV virtual machine during setup.</span></span> 

    ```azurecli
    az vm open-port \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 22 \
    --priority 900
    ```

<span data-ttu-id="75ce1-173">Il comando può richiedere fino a un minuto.</span><span class="sxs-lookup"><span data-stu-id="75ce1-173">This command can take up to a minute to complete.</span></span> <span data-ttu-id="75ce1-174">Al termine, il comando restituisce una risposta JSON per la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="75ce1-174">When the command finishes, it returns a JSON response to the command line.</span></span> <span data-ttu-id="75ce1-175">Verificare che il campo **"provisioningState"** abbia il valore *Succeeded*.</span><span class="sxs-lookup"><span data-stu-id="75ce1-175">Check that the field **"provisioningState"** has the value *Succeeded*.</span></span> <span data-ttu-id="75ce1-176">A breve verrà testato il funzionamento di SSH. Per il momento, verrà aperta un'altra porta.</span><span class="sxs-lookup"><span data-stu-id="75ce1-176">We'll test that ssh works shortly, but first let's open one more port.</span></span>

## <a name="open-the-vm-to-access-the-jupyter-notebook-server-remotely"></a><span data-ttu-id="75ce1-177">Aprire la VM per accedere al server Jupyter Notebook in remoto</span><span class="sxs-lookup"><span data-stu-id="75ce1-177">Open the VM to access the Jupyter Notebook server remotely</span></span>

<span data-ttu-id="75ce1-178">Come accennato in precedenza, l'immagine della DSVM include software, strumenti ed esempi preinstallati di supporto per i progetti di data science, apprendimento automatico e apprendimento profondo.</span><span class="sxs-lookup"><span data-stu-id="75ce1-178">As mentioned previously, the DSVM image comes pre-installed with software, tools, and samples to help you with your data science, machine learning, and deep learning projects.</span></span> <span data-ttu-id="75ce1-179">Jupyter è installato nell'immagine, insieme a notebook di esempio.</span><span class="sxs-lookup"><span data-stu-id="75ce1-179">Jupyter is installed in the image, along with sample notebooks.</span></span> <span data-ttu-id="75ce1-180">Avviare un server Jupyter Notebook nella VM e quindi connettersi in remoto al server dal computer locale.</span><span class="sxs-lookup"><span data-stu-id="75ce1-180">We want to start a Jupyter notebook server on the VM and then remotely connect to the notebook server from our local machine.</span></span> <span data-ttu-id="75ce1-181">Per impostazione predefinita, il server notebook è in esecuzione sulla porta 8888.</span><span class="sxs-lookup"><span data-stu-id="75ce1-181">By default, the notebook server runs on port 8888.</span></span> <span data-ttu-id="75ce1-182">Per accedere al server in remoto, è necessario aprire la porta nella VM.</span><span class="sxs-lookup"><span data-stu-id="75ce1-182">To access the server remotely, we need to open that port on our VM.</span></span> 

1. <span data-ttu-id="75ce1-183">Eseguire il comando seguente in Azure Cloud Shell, sostituendo `<HOSTNAME>` con il nome assegnato alla DSVM durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="75ce1-183">Execute the following command in Azure Cloud Shell, replacing `<HOSTNAME>` with the name you gave your DSVM virtual machine during setup.</span></span>

    ```azurecli
    az vm open-port \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 8888 \
    --priority 901
    ```

<span data-ttu-id="75ce1-184">Anche questa volta, il comando può richiedere fino a un minuto.</span><span class="sxs-lookup"><span data-stu-id="75ce1-184">Again, this command can take up to a minute to complete.</span></span> <span data-ttu-id="75ce1-185">Al termine, il comando restituisce una risposta JSON per la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="75ce1-185">When the command finishes, it returns a JSON response to the command line.</span></span> <span data-ttu-id="75ce1-186">Verificare che il campo **"provisioningState"** abbia il valore *Succeeded*.</span><span class="sxs-lookup"><span data-stu-id="75ce1-186">Check that the field **"provisioningState"** has the value *Succeeded*.</span></span>  

## <a name="connect-to-the-vm-with-secure-shell-ssh"></a><span data-ttu-id="75ce1-187">Connettersi alla macchina virtuale con Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="75ce1-187">Connect to the VM with Secure Shell (ssh)</span></span>

1. <span data-ttu-id="75ce1-188">Eseguire il comando seguente in Azure Cloud Shell per individuare l'indirizzo IP pubblico della VM.</span><span class="sxs-lookup"><span data-stu-id="75ce1-188">Execute the following command in Azure Cloud Shell to find the public IP address of the VM.</span></span> <span data-ttu-id="75ce1-189">Sostituire `<HOSTNAME>` con il nome assegnato alla DSVM durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="75ce1-189">Replace `<HOSTNAME>` with the name you gave your DSVM virtual machine during setup.</span></span>

    ```azurecli
    az vm list-ip-addresses \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --output table
    ```

1. <span data-ttu-id="75ce1-190">Eseguire il comando seguente in Cloud Shell per accedere alla VM.</span><span class="sxs-lookup"><span data-stu-id="75ce1-190">Execute the following command in the Cloud Shell to sign into the VM.</span></span> <span data-ttu-id="75ce1-191">Sostituire `<USERNAME>` con il nome utente scelto all'inizio di questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="75ce1-191">Replace `<USERNAME>` with the username you chose at the start of this exercise.</span></span> <span data-ttu-id="75ce1-192">Sostituire `<IP>` con il valore della colonna **PublicIPAddresses** del passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="75ce1-192">Replace `<IP>`  with the value from the **PublicIPAddresses** column of the previous step.</span></span>

    <span data-ttu-id="75ce1-193">Ad esempio, se il nome utente scelto è *azuser* e PublicIPAddresses ha un valore di 33.165.103.23, il comando sarà:</span><span class="sxs-lookup"><span data-stu-id="75ce1-193">For example, if the username you chose was *azuser* and the PublicIPAddresses had a value of 33.165.103.23, then this command would read:</span></span>
    
    `ssh azuser@33.165.103.23`
    
    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 

1. <span data-ttu-id="75ce1-194">Quando richiesto, immettere la password per l'utente amministratore scelto all'inizio di questo esercizio.</span><span class="sxs-lookup"><span data-stu-id="75ce1-194">When prompted, enter the password for the admin user you chose at the start of this exercise.</span></span> <span data-ttu-id="75ce1-195">Una volta eseguito l'accesso, il prompt dovrebbe cambiare nel formato `username@hostname`, ad esempio `azuser@js1982`.</span><span class="sxs-lookup"><span data-stu-id="75ce1-195">When you've signed in successfully, your prompt should change to the format `username@hostname`, for example, `azuser@js1982`.</span></span>

<span data-ttu-id="75ce1-196">Il passaggio successivo è quello per avviare il server Jupyter Notebook nella VM e aprire un notebook in remoto.</span><span class="sxs-lookup"><span data-stu-id="75ce1-196">The next step is to start the Jupyter notebook server on our VM and open a notebook remotely.</span></span>

## <a name="start-the-jupyter-notebook-server-on-the-vm"></a><span data-ttu-id="75ce1-197">Avviare il server Jupyter Notebook nella VM</span><span class="sxs-lookup"><span data-stu-id="75ce1-197">Start the Jupyter notebook server on the VM</span></span>

<span data-ttu-id="75ce1-198">È disponibile un set di notebook nella cartella `~/notebooks` della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="75ce1-198">There's a set of notebooks in the `~/notebooks` folder of your VM.</span></span> <span data-ttu-id="75ce1-199">Presupponendo che si è ancora connessi tramite una sessione SSH, avviare il server notebook e aprire uno di questi notebook per verificare che tutto funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="75ce1-199">Assuming you are still logged in through an SSH session,  start the notebook server and open one of these notebooks to make sure everything is working.</span></span>


1. <span data-ttu-id="75ce1-200">Al prompt dei comandi della VM eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="75ce1-200">Run the following command at the command prompt of your VM:</span></span>

    ```bash
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
    ```

> [!CAUTION]
> <span data-ttu-id="75ce1-201">L'accesso al server notebook in questo esercizio avviene tramite `http://`.</span><span class="sxs-lookup"><span data-stu-id="75ce1-201">Access to the notebook server in this exercise happens over `http://`.</span></span> <span data-ttu-id="75ce1-202">Per eseguire un server notebook in pubblico, è consigliabile proteggerlo.</span><span class="sxs-lookup"><span data-stu-id="75ce1-202">If you want to run a notebook server in public, you should secure it.</span></span> <span data-ttu-id="75ce1-203">Per altre informazioni sulla protezione di un server notebook, vedere la documentazione ufficiale di Jupyter online.</span><span class="sxs-lookup"><span data-stu-id="75ce1-203">For more information about securing a notebook server, see the official Jupyter documentation online.</span></span> 

<span data-ttu-id="75ce1-204">Nel comando precedente è stato avviato il server Jupyter Notebook con il comando `jupyter notebook`.</span><span class="sxs-lookup"><span data-stu-id="75ce1-204">In the preceding command, we start the Jupyter Notebook server with the `jupyter notebook` command.</span></span> <span data-ttu-id="75ce1-205">Sono disponibili tre importanti argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="75ce1-205">We supply three important command-line arguments.</span></span> <span data-ttu-id="75ce1-206">Tenere presente che è stato eseguito l'accesso al computer in remoto tramite una console.</span><span class="sxs-lookup"><span data-stu-id="75ce1-206">Remember, we're logged into this machine remotely through a console.</span></span> <span data-ttu-id="75ce1-207">I notebook vengono gestiti in un browser.</span><span class="sxs-lookup"><span data-stu-id="75ce1-207">Notebooks are served in a browser.</span></span> 

 - <span data-ttu-id="75ce1-208">`--ip=0.0.0.0` Per impostazione predefinita, un server notebook viene eseguito in locale all'indirizzo 127.0.0.1:8888 ed è accessibile solo da localhost.</span><span class="sxs-lookup"><span data-stu-id="75ce1-208">`--ip=0.0.0.0` By default, a notebook server runs locally at 127.0.0.1:8888 and is accessible only from localhost.</span></span> <span data-ttu-id="75ce1-209">È possibile accedere al server notebook dal browser in locale usando http://127.0.0.1:8888.</span><span class="sxs-lookup"><span data-stu-id="75ce1-209">You can access the notebook server from the browser locally using http://127.0.0.1:8888.</span></span> <span data-ttu-id="75ce1-210">Impostare l'indirizzo IP su 0.0.0.0 per indica al server di ascoltare il traffico su tutti gli indirizzi IP.</span><span class="sxs-lookup"><span data-stu-id="75ce1-210">Setting the IP Address to 0.0.0.0 tells the server to listen for traffic on all IPs.</span></span> <span data-ttu-id="75ce1-211">Se il server notebook è in ascolto su 0.0.0.0, sarà raggiungibile attraverso l'indirizzo IP del computer host.</span><span class="sxs-lookup"><span data-stu-id="75ce1-211">If the notebook server listens on 0.0.0.0, it will be reachable through the IP address of the host machine.</span></span>  
 - <span data-ttu-id="75ce1-212">`--no-browser`  Poiché si intende connettersi al notebook da un altro computer tramite internet, configurare il server notebook per impedire l'apertura del browser (comportamento predefinito).</span><span class="sxs-lookup"><span data-stu-id="75ce1-212">`--no-browser`  Because we want to connect to the notebook from another computer over the internet, we configure the notebook server to not open the browser, which is the default behavior.</span></span> 
 - <span data-ttu-id="75ce1-213">`--allow-root`  In questo esercizio è disponibile solo un account amministratore nella VM, pertanto si vuole essere in grado di eseguire i notebook come radice.</span><span class="sxs-lookup"><span data-stu-id="75ce1-213">`--allow-root`  In this exercise, we only have an admin account on the VM, so  we want to be able to run notebooks as root.</span></span>

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a><span data-ttu-id="75ce1-214">Connettersi al server Jupyter Notebook da un browser remoto</span><span class="sxs-lookup"><span data-stu-id="75ce1-214">Connect to the Jupyter Notebook server from a remote browser</span></span>

<span data-ttu-id="75ce1-215">Quando il comando riportato sopra viene eseguito nella macchina virtuale, il server notebook si avvia e la console mostra un URL da usare in un browser.</span><span class="sxs-lookup"><span data-stu-id="75ce1-215">When the above command runs on the VM, the notebook server starts and the console displays a URL for you to use in a browser.</span></span> 

![Screenshot del messaggio visualizzato nella console da un server notebook in esecuzione, contenente l'URL per accedere al server dal computer host](../media/notebook-url.png)

1. <span data-ttu-id="75ce1-217">Copiare l'URL visualizzato dal server notebook nella barra degli indirizzi del browser preferito.</span><span class="sxs-lookup"><span data-stu-id="75ce1-217">Copy the URL the notebook server displays into the address bar of your favorite browser.</span></span> <span data-ttu-id="75ce1-218">È anche possibile fare clic sull'URL per l'apertura nel browser predefinito.</span><span class="sxs-lookup"><span data-stu-id="75ce1-218">You can also click on the URL to open in your default browser.</span></span> 

    <span data-ttu-id="75ce1-219">Si riceverà il messaggio "Impossibile raggiungere il sito" perché l'URL assegnato è la connessione al server notebook dal computer host.</span><span class="sxs-lookup"><span data-stu-id="75ce1-219">You will receive a "Site can't be reached" message because the URL you were given is the connection to the notebook server from the host machine.</span></span>

1. <span data-ttu-id="75ce1-220">Per accedere in remoto al server, sostituire il nome host nell'URL con l'indirizzo IP della macchina virtuale salvata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="75ce1-220">To access the server remotely,  replace the hostname in the URL with the IP address of the VM you saved earlier.</span></span> 

    <span data-ttu-id="75ce1-221">Di seguito è riportato un URL di un server notebook di esempio.</span><span class="sxs-lookup"><span data-stu-id="75ce1-221">Here's a sample notebook server URL.</span></span>

    <span data-ttu-id="75ce1-222">"http://**ab-dsvm-4**:8888/?token={some token}"</span><span class="sxs-lookup"><span data-stu-id="75ce1-222">"http://**ab-dsvm-4**:8888/?token={some token}"</span></span>

    <span data-ttu-id="75ce1-223">In questo caso si sostituirà **ab-dsvm-4** con l'indirizzo IP del computer.</span><span class="sxs-lookup"><span data-stu-id="75ce1-223">In this case, we would replace **ab-dsvm-4** with IP address of the machine.</span></span> <span data-ttu-id="75ce1-224">Se l'indirizzo IP è `52.175.199.43`, l'URL diventa:</span><span class="sxs-lookup"><span data-stu-id="75ce1-224">If our IP address is `52.175.199.43`, then the URL becomes:</span></span>

    <span data-ttu-id="75ce1-225">"http://**52.175.199.43**:8888/?token={some token}"</span><span class="sxs-lookup"><span data-stu-id="75ce1-225">"http://**52.175.199.43**:8888/?token={some token}"</span></span>

    <span data-ttu-id="75ce1-226">Assicurarsi che l'indirizzo della porta `:8888` venga mantenuto nell'URL.</span><span class="sxs-lookup"><span data-stu-id="75ce1-226">Make sure `:8888`, the port address, is kept in the URL.</span></span>

    > [!TIP]
    > <span data-ttu-id="75ce1-227">Se non si intende usare l'indirizzo IP, è possibile usare anche il nome completo del server nel formato `<HOST NAME>.<REGION>.cloudapp.azure.com`</span><span class="sxs-lookup"><span data-stu-id="75ce1-227">If you don't want to use the IP address, you can also use the fully qualified name of your server which is in the form `<HOST NAME>.<REGION>.cloudapp.azure.com`</span></span>

    <span data-ttu-id="75ce1-228">Lo screenshot seguente mostra l'aspetto del dashboard Jupyter nel browser.</span><span class="sxs-lookup"><span data-stu-id="75ce1-228">The following  screenshot shows what the Jupyter dashboard looks like in your browser.</span></span>

    ![<span data-ttu-id="75ce1-229">Screenshot che mostra il dashboard dei Jupyter Notebook.</span><span class="sxs-lookup"><span data-stu-id="75ce1-229">Screenshot showing Jupyter Notebooks dashboard.</span></span> ](../media/jupyter-in-browser.png)

1. <span data-ttu-id="75ce1-230">Passare a **notebooks/IntroToJupyterPython.ipynb** e selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="75ce1-230">Navigate to **notebooks/IntroToJupyterPython.ipynb** and select it.</span></span> <span data-ttu-id="75ce1-231">Testare questo notebook per verificare che tutto funzioni come previsto.</span><span class="sxs-lookup"><span data-stu-id="75ce1-231">Try out this notebook to verify everythin  works as expected.</span></span>

    <span data-ttu-id="75ce1-232">Congratulazioni.</span><span class="sxs-lookup"><span data-stu-id="75ce1-232">Congratulations!</span></span> <span data-ttu-id="75ce1-233">La macchina virtuale basata su DSVM è ora in esecuzione e può lavorare in remoto con Jupyter.</span><span class="sxs-lookup"><span data-stu-id="75ce1-233">You now have a running DSVM-based virtual machine running and can work remotely with Jupyter.</span></span> <span data-ttu-id="75ce1-234">In questo esercizio è stato eseguito il software installato nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="75ce1-234">In this exercise, we're running the software that was installed on the VM.</span></span> <span data-ttu-id="75ce1-235">Nel prossimo esercizio il software verrà isolato in un contenitore nella macchina virtuale in modo da potersi esercitare in tutta sicurezza.</span><span class="sxs-lookup"><span data-stu-id="75ce1-235">In the next exercise, we'll isolate the software in a container on the VM so we can experiment with confidence.</span></span>

4. <span data-ttu-id="75ce1-236">Una volta terminato l'uso del notebook, sarà possibile arrestare il server Jupyter con `Control-C` in Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="75ce1-236">When you have finished with the notebook, you can stop the Jupyter server with `Control-C` in the Cloud Shell.</span></span>
