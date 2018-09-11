<span data-ttu-id="bc782-101">L'interfaccia della riga di comando di Azure include il comando `vm` per lavorare con le macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="bc782-101">The Azure CLI includes the `vm` command to work with virtual machines in Azure.</span></span> <span data-ttu-id="bc782-102">È possibile fornire vari sottocomandi per eseguire attività specifiche.</span><span class="sxs-lookup"><span data-stu-id="bc782-102">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="bc782-103">Quelli più comuni includono:</span><span class="sxs-lookup"><span data-stu-id="bc782-103">The most common include:</span></span>

| <span data-ttu-id="bc782-104">Sottocomando</span><span class="sxs-lookup"><span data-stu-id="bc782-104">Sub-command</span></span> | <span data-ttu-id="bc782-105">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="bc782-105">Description</span></span> |
|-------------|-------------|
| `create`    | <span data-ttu-id="bc782-106">Creare una nuova macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-106">Create a new virtual machine</span></span> |
| `deallocate` | <span data-ttu-id="bc782-107">Deallocare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-107">Deallocate a virtual machine</span></span> |
| `delete` | <span data-ttu-id="bc782-108">Eliminare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-108">Delete a virtual machine</span></span> |
| `list` | <span data-ttu-id="bc782-109">Elencare le macchine virtuali create nella sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="bc782-109">List the created virtual machines in your subscription</span></span> |
| `open-port` | <span data-ttu-id="bc782-110">Aprire una porta di rete specifica per il traffico in ingresso</span><span class="sxs-lookup"><span data-stu-id="bc782-110">Open a specific network port for inbound traffic</span></span> |
| `restart` | <span data-ttu-id="bc782-111">Riavviare una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-111">Restart a virtual machine</span></span> |
| `show` | <span data-ttu-id="bc782-112">Ottenere i dettagli di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-112">Get the details for a virtual machine</span></span> |
| `start` | <span data-ttu-id="bc782-113">Avviare una macchina virtuale arrestata</span><span class="sxs-lookup"><span data-stu-id="bc782-113">Start a stopped virtual machine</span></span> |
| `stop` | <span data-ttu-id="bc782-114">Arrestare una macchina virtuale in esecuzione</span><span class="sxs-lookup"><span data-stu-id="bc782-114">Stop a running virtual machine</span></span> |
| `update` | <span data-ttu-id="bc782-115">Aggiornare una proprietà di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-115">Update a property of a virtual machine</span></span> |

> [!NOTE]
> <span data-ttu-id="bc782-116">Per un elenco completo dei comandi, è possibile controllare la [documentazione di riferimento dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="bc782-116">For a complete list of commands, you can check the [Azure CLI reference documentation](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>

<span data-ttu-id="bc782-117">Si inizia dal primo: `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="bc782-117">Let's start with the first one: `az vm create`.</span></span> <span data-ttu-id="bc782-118">Questo comando viene usato per creare una macchina virtuale in un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="bc782-118">This command is used to create a virtual machine in a resource group.</span></span> <span data-ttu-id="bc782-119">Esistono vari parametri che è possibile passare per configurare tutti gli aspetti della nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-119">There are several parameters you can pass to configure all the aspects of the new VM.</span></span> <span data-ttu-id="bc782-120">I tre parametri da fornire sono:</span><span class="sxs-lookup"><span data-stu-id="bc782-120">The three parameters that must be supplied are:</span></span>

| <span data-ttu-id="bc782-121">Parametro</span><span class="sxs-lookup"><span data-stu-id="bc782-121">Parameter</span></span> | <span data-ttu-id="bc782-122">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="bc782-122">Description</span></span> |
|-----------|-------------|
| `resource-group` | <span data-ttu-id="bc782-123">Il gruppo di risorse che conterrà la macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-123">The resource group that will own the virtual machine</span></span> |
| `name` | <span data-ttu-id="bc782-124">Il nome della macchina virtuale deve essere univoco all'interno del gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="bc782-124">The name of the virtual machine - must be unique within the resource group</span></span> |
| `image` | <span data-ttu-id="bc782-125">L'immagine del sistema operativo da usare per creare la macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="bc782-125">The operating system image to use to create the VM</span></span> |

<span data-ttu-id="bc782-126">È anche utile aggiungere il flag `--verbose` per visualizzare lo stato di avanzamento della creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-126">In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.</span></span> 

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="bc782-127">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="bc782-127">Create a Linux virtual machine</span></span>

<span data-ttu-id="bc782-128">Verrà ora illustrato come creare una nuova macchina virtuale Linux.</span><span class="sxs-lookup"><span data-stu-id="bc782-128">Let's create a new Linux virtual machine.</span></span> <span data-ttu-id="bc782-129">Eseguire il comando seguente in Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="bc782-129">Execute the following command in Azure Cloud Shell:</span></span>

```azurecli
az vm create --resource-group ExerciseResources --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --verbose 
```

<span data-ttu-id="bc782-130">Questo comando crea una nuova macchina virtuale Linux **Debian** chiamata `SampleVM`.</span><span class="sxs-lookup"><span data-stu-id="bc782-130">This command will create a new **Debian** Linux virtual machine with the name `SampleVM`.</span></span> <span data-ttu-id="bc782-131">Si noti che lo strumento dell'interfaccia della riga di comando di Azure è bloccato durante la creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-131">Notice that the Azure CLI tool is blocked while the VM is being created.</span></span> <span data-ttu-id="bc782-132">Se si preferisce non attendere, è possibile usare l'opzione `--no-wait` per indicare allo strumento dell'interfaccia della riga di comando di Azure di restituire subito un risultato, ad esempio se si esegue il comando in uno script.</span><span class="sxs-lookup"><span data-stu-id="bc782-132">If you would prefer not to wait, you can use the `--no-wait` option to tell the Azure CLI tool to return immediately, for example if you're executing the command in a script.</span></span> <span data-ttu-id="bc782-133">In seguito nello script, usare il comando `azure vm wait --name [vm-name]` per attendere la creazione della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-133">Later in the script, use the `azure vm wait --name [vm-name]` command to wait for the VM to finish being created.</span></span>

<span data-ttu-id="bc782-134">Se si esaminano le risposte dettagliate, si vedrà anche come viene usato il nome `SampleVM` per denominare diverse dipendenze per la macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-134">If you look at the verbose responses, you will also see that the `SampleVM` name is used to name various dependencies for the VM.</span></span>

```
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

<span data-ttu-id="bc782-135">È possibile eseguire l'override di questi nomi di risorse generati automaticamente usando parametri facoltativi per `vm create`, ad esempio `--vnet-name` e `--public-ip-address-dns-name`.</span><span class="sxs-lookup"><span data-stu-id="bc782-135">You can override these auto-generated resource names using optional parameters to `vm create`, such as `--vnet-name` and `--public-ip-address-dns-name`.</span></span>

<span data-ttu-id="bc782-136">Si noti che viene specificato il nome dell'account amministratore "aldis" tramite il flag `admin-username`.</span><span class="sxs-lookup"><span data-stu-id="bc782-136">Notice that we are specifying the admin account name through the `admin-username` flag to be "aldis".</span></span> <span data-ttu-id="bc782-137">Se lo si omette, il comando `vm create` userà il _nome dell'utente corrente_.</span><span class="sxs-lookup"><span data-stu-id="bc782-137">If you omit this, the `vm create` command will use your _current user name_.</span></span> <span data-ttu-id="bc782-138">Dato che le regole per i nomi degli account sono diverse per ogni sistema operativo, è consigliabile specificare un nome.</span><span class="sxs-lookup"><span data-stu-id="bc782-138">Since the rules for account names are different for each OS, it's safer to specify a specific name.</span></span> <span data-ttu-id="bc782-139">Nomi comuni, ad esempio "root" e "admin", non sono consentiti per la maggior parte delle immagini.</span><span class="sxs-lookup"><span data-stu-id="bc782-139">Common names such as "root" and "admin" are not allowed for most images.</span></span>

<span data-ttu-id="bc782-140">Si usa anche il flag `generate-ssh-keys`.</span><span class="sxs-lookup"><span data-stu-id="bc782-140">We are also using the `generate-ssh-keys` flag.</span></span> <span data-ttu-id="bc782-141">Questo parametro viene usato per le distribuzioni di Linux e crea una coppia di chiavi di sicurezza che permettono di usare lo strumento `ssh` per accedere da remoto alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-141">This parameter is used for Linux distributions and creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely.</span></span> <span data-ttu-id="bc782-142">I due file vengono inseriti nella cartella `.ssh` nella macchina e nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="bc782-142">The two files are placed into the `.ssh` folder on your machine and in the VM.</span></span> <span data-ttu-id="bc782-143">Se si dispone già di una chiave SSH denominata `id_rsa` nella cartella di destinazione, questa verrà usata e non ne verrà generata una nuova.</span><span class="sxs-lookup"><span data-stu-id="bc782-143">If you already have an SSH key named `id_rsa` in the target folder, then it will be used rather than having a new key generated.</span></span>

<span data-ttu-id="bc782-144">Dopo aver creato la macchina virtuale, si otterrà una risposta JSON che include lo stato corrente della macchina virtuale e i relativi indirizzi IP pubblici e privati assegnati da Azure:</span><span class="sxs-lookup"><span data-stu-id="bc782-144">Once it finishes creating the VM, you will get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1A-D9-74",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "168.61.54.62",
  "resourceGroup": "ExerciseResources",
  "zones": ""
}
```

> [!NOTE]
> <span data-ttu-id="bc782-145">Si noti che la macchina virtuale è stata creata nella località **eastus**.</span><span class="sxs-lookup"><span data-stu-id="bc782-145">Notice that the VM was created in the **eastus** location.</span></span> <span data-ttu-id="bc782-146">Per impostazione predefinita, la macchina virtuale viene creata nella località identificata dall'area di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="bc782-146">By default, the virtual machine is created in the location identified by the owning region.</span></span> <span data-ttu-id="bc782-147">Tuttavia, in alcuni casi potrebbe essere necessario associare la macchina virtuale a un'area esistente, anche se si trova in un'altra parte del mondo.</span><span class="sxs-lookup"><span data-stu-id="bc782-147">However, sometimes you might want to associate the VM with an existing region, but actually have it spin up somewhere else in the world.</span></span> <span data-ttu-id="bc782-148">A tale scopo, è possibile specificare il parametro `--location` dell'opzione come parte del comando `az vm create`.</span><span class="sxs-lookup"><span data-stu-id="bc782-148">You can do this by specifying the option `--location` parameter as part of the `az vm create` command.</span></span>