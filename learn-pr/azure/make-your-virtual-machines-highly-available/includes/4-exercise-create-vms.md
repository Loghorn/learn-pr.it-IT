<span data-ttu-id="82b5f-101">Il bilanciamento del carico prende il traffico Internet in ingresso e lo instrada. A questo punto sono necessarie alcune VM di Azure verso cui instradare il traffico.</span><span class="sxs-lookup"><span data-stu-id="82b5f-101">We have a load balancer to take inbound Internet traffic and route it, now we need some Azure VMs to route traffic to.</span></span> <span data-ttu-id="82b5f-102">In questo esempio verrà usato Linux, ma la stessa identica tecnica funziona con qualsiasi immagine di macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="82b5f-102">We'll use Linux for this example, but the exact same technique would work with any virtual machine image.</span></span>

## <a name="create-a-set-of-vms"></a><span data-ttu-id="82b5f-103">Creare un set di macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="82b5f-103">Create a set of VMs</span></span>

<span data-ttu-id="82b5f-104">Verranno create tre macchine virtuali che verranno raggruppate in un set di disponibilità con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="82b5f-104">We're going to create three VMs and group them into an availability set with the Azure CLI.</span></span> <span data-ttu-id="82b5f-105">Si sarebbe potuto usare il portale per questa attività, ma poiché stiamo creando più risorse, è molto più semplice eseguire questa operazione con uno strumento di scripting.</span><span class="sxs-lookup"><span data-stu-id="82b5f-105">We could have used the portal for this task - but because we're creating multiple resources, it's much easier to do this through a scripting tool.</span></span>

<span data-ttu-id="82b5f-106">Se si preferisce _davvero_ usare il portale di Azure, un approccio alternativo potrebbe consistere nell'usare un _modello_.</span><span class="sxs-lookup"><span data-stu-id="82b5f-106">An alternative approach if you _really_ prefer to use the Azure portal would be to use a _template_.</span></span> <span data-ttu-id="82b5f-107">Per distribuire le risorse sono disponibili istruzioni JSON.</span><span class="sxs-lookup"><span data-stu-id="82b5f-107">These are JSON instructions which can be used to deploy resources.</span></span> <span data-ttu-id="82b5f-108">È possibile definire un modello per la macchina virtuale e quindi distribuire il modello più volte nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="82b5f-108">We could define a template for the virtual machine and then deploy the template multiple times in the Azure portal.</span></span>

### <a name="create-some-azure-cli-defaults"></a><span data-ttu-id="82b5f-109">Creare impostazioni predefinite dell'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="82b5f-109">Create some Azure CLI defaults</span></span>

1. <span data-ttu-id="82b5f-110">Iniziare configurando alcune impostazioni predefinite nell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="82b5f-110">Start by setting some defaults in the Azure CLI.</span></span> <span data-ttu-id="82b5f-111">Ogni comando usato richiede un _gruppo di risorse_ ed eventualmente un _percorso_.</span><span class="sxs-lookup"><span data-stu-id="82b5f-111">Every command we use requires a _resource group_ and an optional _location_.</span></span> <span data-ttu-id="82b5f-112">Invece di digitare tutto ogni volta, è possibile impostare questi elementi come parametri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="82b5f-112">Rather than typing that every time, we can set it as a default parameter.</span></span> <span data-ttu-id="82b5f-113">Usare il gruppo di risorse creato in precedenza nel sandbox di Azure e lo stesso percorso selezionato per Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="82b5f-113">Use the pre-created Azure sandbox resource group, and the same location you selected for the Azure Load Balancer.</span></span> <span data-ttu-id="82b5f-114">Come promemoria, di seguito sono elencati i percorsi disponibili:</span><span class="sxs-lookup"><span data-stu-id="82b5f-114">As a reminder, here are the available locations:</span></span>

    [!include[](../../../includes/azure-sandbox-regions-note.md)]

1. <span data-ttu-id="82b5f-115">Digitare il comando seguente in Cloud Shell a destra, sostituendo il segnaposto `<location>` con il percorso usato per Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="82b5f-115">Type this command into the Cloud Shell on the right, replacing the `<location>` placeholder with the location you used for your Azure Load Balancer.</span></span>

    ```azurecli
    az configure --defaults group=<rgn>[sandbox Resource Group</rgn> location=<location>
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

### <a name="create-an-availability-set"></a><span data-ttu-id="82b5f-116">Creare un set di disponibilità</span><span class="sxs-lookup"><span data-stu-id="82b5f-116">Create an availability set</span></span>

<span data-ttu-id="82b5f-117">Creare prima di tutto un _set di disponibilità_.</span><span class="sxs-lookup"><span data-stu-id="82b5f-117">Let's start by creating an _availability set_.</span></span>

<span data-ttu-id="82b5f-118">Il set di disponibilità serve per raggruppare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="82b5f-118">We will need an availability set to group our virtual machines into.</span></span> <span data-ttu-id="82b5f-119">Usare il comando `vm availability-set` per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="82b5f-119">We can use the `vm availability-set` command to create one.</span></span> <span data-ttu-id="82b5f-120">Per il nome usare **woodgrove-avail-set**.</span><span class="sxs-lookup"><span data-stu-id="82b5f-120">It just needs a name, we'll use **woodgrove-avail-set**.</span></span>

```azurecli
az vm availability-set create --name woodgrove-avail-set
```

### <a name="create-a-configuration-script"></a><span data-ttu-id="82b5f-121">Creare uno script di configurazione</span><span class="sxs-lookup"><span data-stu-id="82b5f-121">Create a configuration script</span></span>

<span data-ttu-id="82b5f-122">Usare la stessa configurazione di base per ogni VM.</span><span class="sxs-lookup"><span data-stu-id="82b5f-122">We want to use the same basic configuration for each of the VMs.</span></span>

    - <span data-ttu-id="82b5f-123">Ubuntu Linux</span><span class="sxs-lookup"><span data-stu-id="82b5f-123">Ubuntu Linux</span></span>
    - <span data-ttu-id="82b5f-124">Server Web nginx</span><span class="sxs-lookup"><span data-stu-id="82b5f-124">nginx web server</span></span>
    - <span data-ttu-id="82b5f-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="82b5f-125">Node.js</span></span>
    - <span data-ttu-id="82b5f-126">Sito Web Basic con una pagina singola per il test</span><span class="sxs-lookup"><span data-stu-id="82b5f-126">Basic website with a single page for testing</span></span>

<span data-ttu-id="82b5f-127">La scelta del sistema operativo è basata sull'immagine che viene selezionata ma gli altri elementi di configurazione richiedono alcuni script.</span><span class="sxs-lookup"><span data-stu-id="82b5f-127">The choice of OS is based on the image we select, but the other configuration elements require some scripting.</span></span> <span data-ttu-id="82b5f-128">Creare un file locale in Cloud Shell per installare un server Web e configurarlo con un sito Basic.</span><span class="sxs-lookup"><span data-stu-id="82b5f-128">Let's create a local file in the Cloud Shell to install a web server and configure it with a basic site.</span></span>

1. <span data-ttu-id="82b5f-129">Aprire l'editor di Cloud Shell digitando `code` nel terminale.</span><span class="sxs-lookup"><span data-stu-id="82b5f-129">Open the Cloud Shell Editor by typing `code` in the terminal.</span></span>

    ```bash
    code
    ```

    <span data-ttu-id="82b5f-130">Si tratta di un editor predefinito, che è possibile usare per creare e modificare i file direttamente nell'ambiente di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="82b5f-130">This is a built-in editor you can use to create and edit files right in the Cloud Shell environment.</span></span> <span data-ttu-id="82b5f-131">I file vengono salvati in Azure in un account di archiviazione creato quando è stato avviato l'ambiente di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="82b5f-131">The files are saved in Azure in a Storage account created when you launched the Cloud Shell environment.</span></span>

1. <span data-ttu-id="82b5f-132">Copiare lo script seguente e incollarlo nella finestra dell'editor.</span><span class="sxs-lookup"><span data-stu-id="82b5f-132">Copy the following script and paste it into the editor window.</span></span>

    ```script
    #cloud-config
    package_upgrade: true
    packages:
      - nginx
      - nodejs
      - npm
    write_files:
      - owner: www-data:www-data
      - path: /etc/nginx/sites-available/default
        content: |
          server {
            listen 80;
            location / {
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection keep-alive;
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
          }
      - owner: azureuser:azureuser
      - path: /home/azureuser/myapp/index.js
        content: |
          var express = require('express')
          var app = express()
          var os = require('os');
          app.get('/', function (req, res) {
            res.send('Hello World from host ' + os.hostname() + '!')
          })
          app.listen(3000, function () {
            console.log('Hello world app listening on port 3000!')
          })
    runcmd:
      - service nginx restart
      - cd "/home/azureuser/myapp"
      - npm init
      - npm install express -y
      - nodejs index.js
    ```

1. <span data-ttu-id="82b5f-133">Salvare il file e denominarlo **cloud-init.txt**.</span><span class="sxs-lookup"><span data-stu-id="82b5f-133">Save the file - give it the name **cloud-init.txt**.</span></span> <span data-ttu-id="82b5f-134">È possibile usare il menu a comparsa "..." nell'angolo in alto a destra dell'editor o usare <kbd>CTRL+S</kbd> in Windows e Linux e <kbd>Cmd+S</kbd> in macOS.</span><span class="sxs-lookup"><span data-stu-id="82b5f-134">You can use the "..." context menu on the top/right corner of the editor, or use <kbd>Ctrl+S</kbd> in Windows and Linux, and <kbd>Cmd+S</kbd> on macOS.</span></span>

1. <span data-ttu-id="82b5f-135">Uscire dall'editor: anche in questo caso è possibile usare il menu a comparsa "..." o il tasto di scelta rapida (<kbd>CTRL+Q</kbd>).</span><span class="sxs-lookup"><span data-stu-id="82b5f-135">Exit the editor - again you can use the "..." context menu, or an accelerator key (<kbd>Ctrl+Q</kbd>).</span></span>

1. <span data-ttu-id="82b5f-136">Il file deve trovarsi nel file system.</span><span class="sxs-lookup"><span data-stu-id="82b5f-136">The file should be on the file system.</span></span> <span data-ttu-id="82b5f-137">Provare a usare il comando `ls` per elencare il contenuto della cartella.</span><span class="sxs-lookup"><span data-stu-id="82b5f-137">Try using the `ls` command to list the contents of the folder.</span></span> <span data-ttu-id="82b5f-138">È anche possibile usare `cat` per il file per verificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="82b5f-138">You can also `cat` the file to verify the contents.</span></span>

### <a name="create-the-virtual-machines"></a><span data-ttu-id="82b5f-139">Creare le macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="82b5f-139">Create the virtual machines</span></span>

<span data-ttu-id="82b5f-140">Successivamente, creare tre macchine virtuali Ubuntu Linux con l'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="82b5f-140">Next, let's create three Ubuntu Linux virtual machines with the Azure CLI.</span></span> <span data-ttu-id="82b5f-141">Non verranno illustrate tutte le opzioni qui, ma per altre informazioni sulla gestione delle VM con l'interfaccia della riga di comando, vedere il modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure**.</span><span class="sxs-lookup"><span data-stu-id="82b5f-141">We won't cover the options here, but if you are interested in learning more about VM management with the CLI, check out the **Manage virtual machines with the Azure CLI** module.</span></span>

<span data-ttu-id="82b5f-142">È anche necessario creare un'interfaccia di rete virtuale per ogni VM.</span><span class="sxs-lookup"><span data-stu-id="82b5f-142">We also need to create a virtual network interface for each VM.</span></span> <span data-ttu-id="82b5f-143">È possibile crearle insieme usando un ciclo.</span><span class="sxs-lookup"><span data-stu-id="82b5f-143">We can use a loop and create them together.</span></span>

1. <span data-ttu-id="82b5f-144">Usare il comando `vm-create` per creare tre macchine virtuali in un ciclo.</span><span class="sxs-lookup"><span data-stu-id="82b5f-144">Use the `vm-create` command to create three virtual machines in a loop.</span></span> <span data-ttu-id="82b5f-145">È possibile usare il codice seguente per:</span><span class="sxs-lookup"><span data-stu-id="82b5f-145">You can use the following code to:</span></span>
    - <span data-ttu-id="82b5f-146">Creare una NIC denominata _woodgrove_NicX_ dove X è [1,2,3].</span><span class="sxs-lookup"><span data-stu-id="82b5f-146">Create a NIC named _woodgrove_NicX_ where X is [1,2,3].</span></span>
    - <span data-ttu-id="82b5f-147">Creare una VM denominata _woodgrove-VMX_ dove X è [1,2,3].</span><span class="sxs-lookup"><span data-stu-id="82b5f-147">Create a VM named _woodgrove-VMX_ where X is [1,2,3].</span></span>
    - <span data-ttu-id="82b5f-148">Associare ogni NIC alla rete virtuale creata</span><span class="sxs-lookup"><span data-stu-id="82b5f-148">Associate each NIC to the created VNET</span></span>
    - <span data-ttu-id="82b5f-149">Associare ogni VM alla NIC e al set di disponibilità.</span><span class="sxs-lookup"><span data-stu-id="82b5f-149">Associate each VM to the NIC and availability set.</span></span>

    ```azurecli
    for i in `seq 1 3`; do

        az network nic create --name woodgrove-Nic$i \
            --vnet-name woodgrove-VNET \
            --subnet backendSubnet

        az vm create --name woodgrove-VM$i \
            --availability-set woodgrove-avail-set \
            --nics woodgrove-Nic$i \
            --image UbuntuLTS \
            --admin-username azureuser \
            --generate-ssh-keys \
            --custom-data cloud-init.txt \
            --no-wait
    done
    ```
    > [!TIP]
    > <span data-ttu-id="82b5f-150">Il comando imposta il nome dell'amministratore su "azureuser", ma è possibile modificare questa impostazione in qualsiasi modo si voglia.</span><span class="sxs-lookup"><span data-stu-id="82b5f-150">The command is setting the administrator name to "azureuser" here, you can change that to anything you like.</span></span> <span data-ttu-id="82b5f-151">È anche possibile specificare una password se si preferisce la combinazione nome utente/password piuttosto che le chiavi SSH.</span><span class="sxs-lookup"><span data-stu-id="82b5f-151">You can also supply a password if you prefer username/password over SSH keys.</span></span>

1. <span data-ttu-id="82b5f-152">L'esecuzione dell'operazione richiederà un po' di tempo e anche quando il ciclo è terminato, le VM non saranno disponibili.</span><span class="sxs-lookup"><span data-stu-id="82b5f-152">This will take a while to run - and even when the loop is finished, the VMs won't quite be available.</span></span> <span data-ttu-id="82b5f-153">È possibile passare al portale di Azure e usare la schermata **Tutte le risorse** per visualizzare le risorse create.</span><span class="sxs-lookup"><span data-stu-id="82b5f-153">You can switch over to the Azure portal and use the **All Resources** display to see the created resources.</span></span>

1. <span data-ttu-id="82b5f-154">È anche possibile usare il comando `vm list` per visualizzare quante macchine virtuali sono state distribuite.</span><span class="sxs-lookup"><span data-stu-id="82b5f-154">You can also use the `vm list` command to see how many VMs have been deployed.</span></span>

    ```azurecli
    az vm list -o table
    ```

<span data-ttu-id="82b5f-155">Successivamente verrà illustrato come configurare il bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="82b5f-155">Next we'll talk about how to configure the load balancer.</span></span>