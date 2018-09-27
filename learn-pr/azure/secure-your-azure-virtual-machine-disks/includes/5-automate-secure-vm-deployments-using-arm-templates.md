<span data-ttu-id="11ead-101">Si supponga che l'azienda voglia distribuire numerosi server come parte della transizione al cloud.</span><span class="sxs-lookup"><span data-stu-id="11ead-101">Suppose your company is deploying several servers as part of their cloud transition.</span></span> <span data-ttu-id="11ead-102">I dischi delle macchine virtuali devono essere crittografati durante la distribuzione, per evitare periodi di vulnerabilità dei dischi stessi.</span><span class="sxs-lookup"><span data-stu-id="11ead-102">VM disks must be encrypted during the deployment, so there's no time when the disks are vulnerable.</span></span> <span data-ttu-id="11ead-103">Si vuole automatizzare questo processo ed è necessario modificare i modelli di Azure Resource Manager (ARM) per abilitare automaticamente la crittografia.</span><span class="sxs-lookup"><span data-stu-id="11ead-103">You want to automate this process, and have to modify the Azure Resource Manager templates to automatically enable encryption.</span></span>

<span data-ttu-id="11ead-104">In questo caso si illustrerà come usare un modello di Azure Resource Manager per abilitare automaticamente la crittografia per le nuove macchine virtuali Windows.</span><span class="sxs-lookup"><span data-stu-id="11ead-104">Here, we'll look at how to use an Azure Resource Manager template to automatically enable encryption for new Windows VMs.</span></span>

## <a name="what-are-azure-resource-manager-templates"></a><span data-ttu-id="11ead-105">Cosa sono i modelli di Azure Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="11ead-105">What are Azure Resource Manager templates?</span></span>

<span data-ttu-id="11ead-106">I modelli di Azure Resource Manager sono file JSON usati per definire un set di risorse da distribuire in Azure.</span><span class="sxs-lookup"><span data-stu-id="11ead-106">Resource Manager templates are JSON files used to define a set of resources to deploy to Azure.</span></span> <span data-ttu-id="11ead-107">È possibile scriverli da zero e per alcune risorse di Azure, incluse le macchine virtuali, è possibile generarli tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="11ead-107">You can write them from scratch, and for some Azure resources, including VMs, you can use the Azure portal to generate them.</span></span> <span data-ttu-id="11ead-108">Sarà necessario immettere le informazioni necessarie per una distribuzione di macchina virtuale manuale, ma anziché distribuire la macchina virtuale in Azure, si salverà il modello.</span><span class="sxs-lookup"><span data-stu-id="11ead-108">You'll need to complete the required information for a manual VM deployment, but instead of deploying the VM to Azure, you save the template.</span></span> <span data-ttu-id="11ead-109">È quindi possibile _riusare_ il modello per creare la configurazione di macchina virtuale specifica.</span><span class="sxs-lookup"><span data-stu-id="11ead-109">You can then _reuse_ the template to create that specific VM configuration.</span></span>

<span data-ttu-id="11ead-110">Sono disponibili [modelli di esempio in docs](https://azure.microsoft.com/resources/templates) per automatizzare tutti i tipi di attività amministrative.</span><span class="sxs-lookup"><span data-stu-id="11ead-110">There are [example templates available in docs](https://azure.microsoft.com/resources/templates) to automate all sorts of administrative tasks.</span></span> <span data-ttu-id="11ead-111">Avremmo infatti potuto usare uno di questi modelli per crittografare la macchina virtuale che abbiamo appena creato manualmente.</span><span class="sxs-lookup"><span data-stu-id="11ead-111">In fact, we could have used one of these templates to encrypt our VM that we just did manually!</span></span>

![Screenshot che mostra i modelli di Azure](../media/5-browse-templates.png)

## <a name="using-github-templates"></a><span data-ttu-id="11ead-113">Uso dei modelli di GitHub</span><span class="sxs-lookup"><span data-stu-id="11ead-113">Using GitHub templates</span></span>

<span data-ttu-id="11ead-114">L'origine del modello viene archiviata in GitHub.</span><span class="sxs-lookup"><span data-stu-id="11ead-114">The actual template source is stored in GitHub.</span></span> <span data-ttu-id="11ead-115">È possibile passare a un modello in GitHub e distribuirlo direttamente in Azure dalla stessa pagina.</span><span class="sxs-lookup"><span data-stu-id="11ead-115">You can browse to a template in GitHub and deploy right to Azure from the page.</span></span>

![Screenshot che mostra il modello GitHub con il pulsante Distribuisci in Azure evidenziato](../media/5-deploy-from-github.png)

<span data-ttu-id="11ead-117">Quando viene distribuito il modello, Azure visualizza l'elenco dei campi di input obbligatori.</span><span class="sxs-lookup"><span data-stu-id="11ead-117">When the template is deployed, Azure will display a list of required input fields.</span></span>

![Screenshot del modello nel portale di Azure](../media/5-fill-in-template.png)

<span data-ttu-id="11ead-119">È quindi possibile eseguire il modello per creare, modificare o rimuovere le risorse.</span><span class="sxs-lookup"><span data-stu-id="11ead-119">You can then execute the template to create, modify, or remove resources.</span></span>

### <a name="running-templates-in-the-azure-portal"></a><span data-ttu-id="11ead-120">Esecuzione di modelli nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="11ead-120">Running templates in the Azure portal</span></span>

<span data-ttu-id="11ead-121">Se si conosce già il modello che si desidera utilizzare oppure se ci si sono modelli salvati nell'account di Azure, è possibile usare la risorsa **Crea una risorsa** > **Distribuzione modello** per individuare ed eseguire i modelli definiti nel portale.</span><span class="sxs-lookup"><span data-stu-id="11ead-121">If you already know the template you want to use, or you have saved templates in your Azure account, you can use the **Create a resource** > **Template Deployment** resource to locate and run defined templates in the portal.</span></span> <span data-ttu-id="11ead-122">È possibile eseguire ricerche nei modelli in base al nome, modificare un modello per cambiare i parametri o il comportamento ed eseguire il modello direttamente dall'interfaccia utente grafica.</span><span class="sxs-lookup"><span data-stu-id="11ead-122">You can search through templates by name, edit a template to change the parameters or behavior, and execute the template right from the GUI.</span></span>

### <a name="running-templates-from-the-command-line"></a><span data-ttu-id="11ead-123">Esecuzione di modelli dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="11ead-123">Running templates from the command line</span></span>

<span data-ttu-id="11ead-124">Quando a un modello viene assegnato un URL, il modello può essere eseguito con Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="11ead-124">Given a URL to a template, you can execute it with Azure PowerShell.</span></span> <span data-ttu-id="11ead-125">Ad esempio, si potrebbe eseguire il modello di crittografia dischi con il comando PowerShell seguente:</span><span class="sxs-lookup"><span data-stu-id="11ead-125">For example, we could run the disk encryption template with the following PowerShell command:</span></span>

```powershell
New-AzureRmResourceGroupDeployment `
    -Name encrypt-disk `
    -ResourceGroupName <resource-group-name> `
    -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

<span data-ttu-id="11ead-126">In alternativa, se i preferisce l'interfaccia della riga di comando di Azure, con il comando `group deployment create`.</span><span class="sxs-lookup"><span data-stu-id="11ead-126">Or, if you prefer the Azure CLI, with the `group deployment create` command.</span></span>

```azurecli
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> \ 
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

