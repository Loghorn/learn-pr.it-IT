<span data-ttu-id="fc502-101">Prima di iniziare, è consigliabile ripassare la sintassi dello strumento dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="fc502-101">Before we start, let's review the syntax for the Azure CLI tool.</span></span> <span data-ttu-id="fc502-102">Se è stato seguito il modulo **Controllare i servizi di Azure con l'interfaccia della riga di comando di Azure**, si sa che i comandi dell'interfaccia hanno la forma di:</span><span class="sxs-lookup"><span data-stu-id="fc502-102">If you've taken the **Control Azure services with the Azure CLI** module, then you know that Azure CLI commands take the form of:</span></span>

```azurecli
az [command] [subcommand] [--parameter --parameter]
```

<span data-ttu-id="fc502-103">`[command]` identifica l'area specifica di Azure che si desidera controllare.</span><span class="sxs-lookup"><span data-stu-id="fc502-103">The `[command]` identifies the specific area of Azure you want to control.</span></span> <span data-ttu-id="fc502-104">Ad esempio, è possibile gestire le informazioni della sottoscrizione con il comando `account` oppure i database SQL con il comando `sql`.</span><span class="sxs-lookup"><span data-stu-id="fc502-104">For example, you can manage subscription information with the `account` command, or SQL databases with the `sql` command.</span></span> <span data-ttu-id="fc502-105">`[subcommand]` e `[--parameters]` dipendono quindi dal comando con cui si sta lavorando.</span><span class="sxs-lookup"><span data-stu-id="fc502-105">The `[subcommand]` and `[--parameters]` are then dependent upon the command you're working with.</span></span> 

<span data-ttu-id="fc502-106">È possibile visualizzare un elenco di comandi, sottocomandi e parametri digitando un comando parziale.</span><span class="sxs-lookup"><span data-stu-id="fc502-106">You can view a list of commands, subcommands, and parameters by typing in a partial command.</span></span> <span data-ttu-id="fc502-107">Ad esempio, se si digita `az` dalla riga di comando verrà visualizzata la schermata di aiuto principale; digitando `az vm` si otterranno invece tutti i sottocomandi per le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="fc502-107">For example, typing `az` at the command line will give you the top-level help screen, and typing `az vm` will give you all the subcommands for virtual machines.</span></span> <span data-ttu-id="fc502-108">Questo approccio può essere un ottimo modo per esplorare lo strumento dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="fc502-108">This approach can be a great way to explore the Azure CLI tool.</span></span>

> [!NOTE]
> <span data-ttu-id="fc502-109">Si userà Azure Cloud Shell ospitata nel browser per lavorare con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="fc502-109">We will be using browser-hosted Azure Cloud Shell to work with the Azure CLI.</span></span> <span data-ttu-id="fc502-110">Se si preferisce lavorare dalla macchina locale, è possibile eseguire tutti i comandi menzionati anche dalla riga di comando [installando l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="fc502-110">If you prefer to work from your local machine, all of the commands we cover can also be executed from the command line by [installing the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="fc502-111">Accedere ad Azure</span><span class="sxs-lookup"><span data-stu-id="fc502-111">Log in to Azure</span></span>

<span data-ttu-id="fc502-112">La prima cosa da fare quando si lavora con l'interfaccia della riga di comando di Azure è accedere al proprio account di Azure.</span><span class="sxs-lookup"><span data-stu-id="fc502-112">The first thing you'll do when working with the Azure CLI is to log in to your Azure account.</span></span> <span data-ttu-id="fc502-113">Questa operazione viene eseguita con il comando `login`.</span><span class="sxs-lookup"><span data-stu-id="fc502-113">This is done with the `login` command.</span></span> <span data-ttu-id="fc502-114">Se si usa Cloud Shell, sarà presente un pulsante per accedere ad Azure.</span><span class="sxs-lookup"><span data-stu-id="fc502-114">If you're using Cloud Shell, there will be a button to sign in to Azure.</span></span>

```azurecli
az login
```

<span data-ttu-id="fc502-115">Questo comando avvia una finestra del browser e consente di selezionare l'account Microsoft da usare.</span><span class="sxs-lookup"><span data-stu-id="fc502-115">This command will launch a browser window and allow you to select the Microsoft account you want to use.</span></span>

## <a name="working-with-subscriptions"></a><span data-ttu-id="fc502-116">Uso di più sottoscrizioni</span><span class="sxs-lookup"><span data-stu-id="fc502-116">Working with subscriptions</span></span>

<span data-ttu-id="fc502-117">In questo modulo si lavorerà in una sottoscrizione temporanea creata come ambiente di prova. Solitamente, invece, l'utente userà una sottoscrizione presente nel proprio account.</span><span class="sxs-lookup"><span data-stu-id="fc502-117">In this module, we will work in a temporary subscription created as a playground, but you will normally use a subscription from your own account.</span></span> <span data-ttu-id="fc502-118">Se si dispone di più sottoscrizioni, è possibile ottenere un relativo elenco formattato in modo chiaro usando l'istruzione `az account list --output table`.</span><span class="sxs-lookup"><span data-stu-id="fc502-118">If you have more than one subscription, you can get a clearly formatted list of subscriptions using the `az account list --output table` statement.</span></span>

```
Name                                  CloudName    SubscriptionId                        State    IsDefault
------------------------------------  -----------  ------------------------------------  -------  -----------
Contoso Legacy Resources              AzureCloud   abc13b0c-d2c4-64b2-9ac5-2f4cb819b752  Enabled  True
Visual Studio Enterprise              AzureCloud   233aebce-23c2-4572-c056-c029449e93ed  Enabled  False
```

<span data-ttu-id="fc502-119">Si noti che il comando identifica anche la sottoscrizione _predefinita_ in cui verranno applicati tutti i comandi.</span><span class="sxs-lookup"><span data-stu-id="fc502-119">Notice that the command also identifies the _default_ subscription where all your commands will apply.</span></span> <span data-ttu-id="fc502-120">Se si preferisce lavorare in una sottoscrizione diversa, è possibile usare il comando `az account set --subscription "[name]"`.</span><span class="sxs-lookup"><span data-stu-id="fc502-120">If you would prefer to work in a different subscription, you can use the `az account set --subscription "[name]"` command.</span></span> <span data-ttu-id="fc502-121">Ad esempio, è possibile impostare la sottoscrizione corrente come `Visual Studio Enterprise` dall'elenco precedente tramite il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fc502-121">For example, we could set our current subscription to be `Visual Studio Enterprise` from the above list through the following command:</span></span>

```azurecli
az account set --subscription "Visual Studio Enterprise"
```