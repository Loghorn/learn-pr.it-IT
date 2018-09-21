<span data-ttu-id="f3547-101">Iniziamo con l'attività più ovvia, ovvero la creazione di una macchina virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="f3547-101">Let's start with the most obvious task: creating an Azure Virtual Machine.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a><span data-ttu-id="f3547-102">Account di accesso, sottoscrizioni e gruppi di risorse</span><span class="sxs-lookup"><span data-stu-id="f3547-102">Logins, subscriptions, and resource groups</span></span>

<span data-ttu-id="f3547-103">L'ambiente di lavoro è Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="f3547-103">You'll be working in the Azure Cloud Shell on the right.</span></span> <span data-ttu-id="f3547-104">Una volta attivato l'ambiente sandbox, viene stabilita una connessione ad Azure con una sottoscrizione gratuita gestita da Microsoft Learn.</span><span class="sxs-lookup"><span data-stu-id="f3547-104">Once you activate the sandbox, you'll be logged into Azure with a free subscription managed by Microsoft Learn.</span></span> <span data-ttu-id="f3547-105">Non è necessario accedere ad Azure in modo autonomo né selezionare una sottoscrizione, perché le operazioni vengono eseguite in modo automatico.</span><span class="sxs-lookup"><span data-stu-id="f3547-105">You don't have to log into Azure on your own, or select a subscription - this will be done for you.</span></span> <span data-ttu-id="f3547-106">In genere l'utente creerebbe anche un _gruppo di risorse_ in cui includere le nuove risorse.</span><span class="sxs-lookup"><span data-stu-id="f3547-106">In addition, normally you would create a _resource group_ to hold new resources.</span></span> <span data-ttu-id="f3547-107">In questo modulo il gruppo di risorse viene creato automaticamente nell'ambiente sandbox di Azure al fine di eseguire tutti i comandi.</span><span class="sxs-lookup"><span data-stu-id="f3547-107">In this module, the Azure Sandbox will create a resource group for you which will be used to execute all the commands.</span></span>

## <a name="create-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="f3547-108">Creare una macchina virtuale Linux con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="f3547-108">Create a Linux VM with the Azure CLI</span></span>

<span data-ttu-id="f3547-109">L'interfaccia della riga di comando di Azure include il comando `vm` per usare le macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="f3547-109">The Azure CLI includes the `vm` command to work with virtual machines in Azure.</span></span> <span data-ttu-id="f3547-110">È possibile fornire vari sottocomandi per eseguire attività specifiche.</span><span class="sxs-lookup"><span data-stu-id="f3547-110">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="f3547-111">Quelli più comuni includono:</span><span class="sxs-lookup"><span data-stu-id="f3547-111">The most common include:</span></span>

| <span data-ttu-id="f3547-112">Sottocomando</span><span class="sxs-lookup"><span data-stu-id="f3547-112">Sub-command</span></span> | <span data-ttu-id="f3547-113">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="f3547-113">Description</span></span> |
|-------------|-------------|
| `create`    | <span data-ttu-id="f3547-114">Creare una nuova macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="f3547-114">Create a new virtual machine</span></span> |
| `deallocate` | <span data-ttu-id="f3547-115">Deallocare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="f3547-115">Deallocate a virtual machine</span></span> |
| `delete` | <span data-ttu-id="f3547-116">Eliminare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="f3547-116">Delete a virtual machine</span></span> |
| `list` | <span data-ttu-id="f3547-117">Elencare le macchine virtuali create nella sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="f3547-117">List the created virtual machines in your subscription</span></span> |
| `open-port` | <span data-ttu-id="f3547-118">Aprire una porta di rete specifica per il traffico in ingresso</span><span class="sxs-lookup"><span data-stu-id="f3547-118">Open a specific network port for inbound traffic</span></span> |
| `restart` | <span data-ttu-id="f3547-119">Riavviare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="f3547-119">Restart a virtual machine</span></span> |
| `show` | <span data-ttu-id="f3547-120">Ottenere i dettagli di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="f3547-120">Get the details for a virtual machine</span></span> |
| `start` | <span data-ttu-id="f3547-121">Avviare una macchina virtuale arrestata</span><span class="sxs-lookup"><span data-stu-id="f3547-121">Start a stopped virtual machine</span></span> |
| `stop` | <span data-ttu-id="f3547-122">Arrestare una macchina virtuale in esecuzione</span><span class="sxs-lookup"><span data-stu-id="f3547-122">Stop a running virtual machine</span></span> |
| `update` | <span data-ttu-id="f3547-123">Aggiornare una proprietà di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="f3547-123">Update a property of a virtual machine</span></span> |

> [!NOTE]
> <span data-ttu-id="f3547-124">Per un elenco completo dei comandi, è possibile controllare la [documentazione di riferimento dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="f3547-124">For a complete list of commands, you can check the [Azure CLI reference documentation](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>

<span data-ttu-id="f3547-125">Si inizia dal primo: `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="f3547-125">Let's start with the first one: `az vm create`.</span></span> <span data-ttu-id="f3547-126">Questo comando viene usato per creare una macchina virtuale in un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="f3547-126">This command is used to create a virtual machine in a resource group.</span></span> <span data-ttu-id="f3547-127">Esistono vari parametri che è possibile passare per configurare tutti gli aspetti della nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-127">There are several parameters you can pass to configure all the aspects of the new VM.</span></span> <span data-ttu-id="f3547-128">I tre parametri da fornire sono:</span><span class="sxs-lookup"><span data-stu-id="f3547-128">The three parameters that must be supplied are:</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="f3547-129">Parametro</span><span class="sxs-lookup"><span data-stu-id="f3547-129">Parameter</span></span> | <span data-ttu-id="f3547-130">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f3547-130">Description</span></span> |
> |-----------|-------------|
> | `resource-group` | <span data-ttu-id="f3547-131">Il gruppo di risorse in cui si trova la macchina virtuale. Usare **<rgn>[gruppo di risorse sandbox]</rgn>**.</span><span class="sxs-lookup"><span data-stu-id="f3547-131">The resource group that will own the virtual machine, use **<rgn>[Sandbox Resource Group]</rgn>**.</span></span> |
> | `name` | <span data-ttu-id="f3547-132">Il nome della macchina virtuale deve essere univoco all'interno del gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="f3547-132">The name of the virtual machine - must be unique within the resource group.</span></span> |
> | `image` | <span data-ttu-id="f3547-133">L'immagine del sistema operativo da usare per creare la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-133">The operating system image to use to create the VM.</span></span> |
> | `location` | <span data-ttu-id="f3547-134">L'area in cui inserire la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-134">The region to place the VM in.</span></span> <span data-ttu-id="f3547-135">In genere tale area è vicina all'utente della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-135">Typically this would be close to the consumer of the VM.</span></span> <span data-ttu-id="f3547-136">In questo esercizio scegliere una località nelle vicinanze nell'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="f3547-136">In this exercise, choose a location nearby from the following list.</span></span> |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="f3547-137">È anche utile aggiungere il flag `--verbose` per visualizzare lo stato di avanzamento della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-137">In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.</span></span> 

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="f3547-138">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="f3547-138">Create a Linux virtual machine</span></span>

<span data-ttu-id="f3547-139">Verrà ora illustrato come creare una nuova macchina virtuale Linux.</span><span class="sxs-lookup"><span data-stu-id="f3547-139">Let's create a new Linux virtual machine.</span></span> <span data-ttu-id="f3547-140">Eseguire questo comando in Azure Cloud Shell per creare una macchina Linux Debian nell'area "West US".</span><span class="sxs-lookup"><span data-stu-id="f3547-140">Execute the following command in Azure Cloud Shell to create a Debian Linux machine in the "West US" location.</span></span> <span data-ttu-id="f3547-141">Modificare la località se quella scelta non è nelle vicinanze.</span><span class="sxs-lookup"><span data-stu-id="f3547-141">Change the location if that one isn't nearby.</span></span>

```azurecli
az vm create --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


<span data-ttu-id="f3547-142">Questo comando crea una nuova macchina virtuale Linux **Debian** chiamata `SampleVM`.</span><span class="sxs-lookup"><span data-stu-id="f3547-142">This command will create a new **Debian** Linux virtual machine with the name `SampleVM`.</span></span> <span data-ttu-id="f3547-143">Si noti che lo strumento dell'interfaccia della riga di comando di Azure rimane in attesa durante la creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-143">Notice that the Azure CLI tool waits while the VM is being created.</span></span> <span data-ttu-id="f3547-144">È possibile aggiungere l'opzione `--no-wait` per indicare allo strumento dell'interfaccia della riga di comando di Azure di rispondere immediatamente in modo che Azure prosegua con la creazione della macchina virtuale in background.</span><span class="sxs-lookup"><span data-stu-id="f3547-144">You can add the `--no-wait` option to tell the Azure CLI tool to return immediately and have Azure continue creating the VM in the background.</span></span> <span data-ttu-id="f3547-145">Questo comportamento è utile se si esegue il comando in uno script.</span><span class="sxs-lookup"><span data-stu-id="f3547-145">This is useful if you're executing the command in a script.</span></span> <span data-ttu-id="f3547-146">Nello script usare in seguito il comando `azure vm wait --name [vm-name]` per attendere il completamento della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-146">Later in the script, use the `azure vm wait --name [vm-name]` command to wait for the VM to finish being created.</span></span>

<span data-ttu-id="f3547-147">Se si esaminano le risposte dettagliate, si vedrà anche come viene usato il nome `SampleVM` per denominare diverse dipendenze per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-147">If you look at the verbose responses, you will also see that the `SampleVM` name is used to name various dependencies for the VM.</span></span>

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

<span data-ttu-id="f3547-148">È possibile eseguire l'override di questi nomi di risorse generati automaticamente usando parametri facoltativi per `vm create`, ad esempio `--vnet-name` e `--public-ip-address-dns-name`.</span><span class="sxs-lookup"><span data-stu-id="f3547-148">You can override these auto-generated resource names using optional parameters to `vm create`, such as `--vnet-name` and `--public-ip-address-dns-name`.</span></span>

<span data-ttu-id="f3547-149">Il nome dell'account amministratore viene specificato tramite il flag `admin-username` in modo che sia **"aldis"**.</span><span class="sxs-lookup"><span data-stu-id="f3547-149">We are specifying the administrator account name through the `admin-username` flag to be **"aldis"**.</span></span> <span data-ttu-id="f3547-150">Se lo si omette, il comando `vm create` userà il _nome dell'utente corrente_.</span><span class="sxs-lookup"><span data-stu-id="f3547-150">If you omit this, the `vm create` command will use your _current user name_.</span></span> <span data-ttu-id="f3547-151">Dato che le regole per i nomi degli account sono diverse per ogni sistema operativo, è consigliabile specificare un nome.</span><span class="sxs-lookup"><span data-stu-id="f3547-151">Since the rules for account names are different for each OS, it's safer to specify a specific name.</span></span> 

> [!NOTE]
> <span data-ttu-id="f3547-152">Nomi comuni, ad esempio "root" e "admin", non sono consentiti per la maggior parte delle immagini.</span><span class="sxs-lookup"><span data-stu-id="f3547-152">Common names such as "root" and "admin" are not allowed for most images.</span></span>

<span data-ttu-id="f3547-153">Si usa anche il flag `generate-ssh-keys`.</span><span class="sxs-lookup"><span data-stu-id="f3547-153">We are also using the `generate-ssh-keys` flag.</span></span> <span data-ttu-id="f3547-154">Questo parametro viene usato per le distribuzioni di Linux e crea una coppia di chiavi di sicurezza che permettono di usare lo strumento `ssh` per accedere da remoto alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-154">This parameter is used for Linux distributions and creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely.</span></span> <span data-ttu-id="f3547-155">I due file vengono inseriti nella cartella `.ssh` nella macchina e nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f3547-155">The two files are placed into the `.ssh` folder on your machine and in the VM.</span></span> <span data-ttu-id="f3547-156">Se si dispone già di una chiave SSH denominata `id_rsa` nella cartella di destinazione, questa verrà usata e non ne verrà generata una nuova.</span><span class="sxs-lookup"><span data-stu-id="f3547-156">If you already have an SSH key named `id_rsa` in the target folder, then it will be used rather than having a new key generated.</span></span>

<span data-ttu-id="f3547-157">Dopo aver creato la macchina virtuale, si otterrà una risposta JSON che include lo stato corrente della macchina virtuale e i relativi indirizzi IP pubblici e privati assegnati da Azure:</span><span class="sxs-lookup"><span data-stu-id="f3547-157">Once it finishes creating the VM, you will get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```
