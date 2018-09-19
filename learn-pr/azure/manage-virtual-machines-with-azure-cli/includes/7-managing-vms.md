<span data-ttu-id="32cab-101">Avvio e arresto sono tra le attività principali da eseguire con le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="32cab-101">One of the main tasks you'll want to do while running virtual machines is to start and stop them.</span></span>

## <a name="stopping-a-vm"></a><span data-ttu-id="32cab-102">Arresto di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="32cab-102">Stopping a VM</span></span>

<span data-ttu-id="32cab-103">È possibile arrestare una macchina virtuale in esecuzione con il comando `vm stop`.</span><span class="sxs-lookup"><span data-stu-id="32cab-103">We can stop a running VM with the `vm stop` command.</span></span> <span data-ttu-id="32cab-104">È necessario passare il nome e il gruppo di risorse oppure l'ID univoco della macchina virtuale:</span><span class="sxs-lookup"><span data-stu-id="32cab-104">You must pass the name and resource group, or the unique ID for the VM:</span></span>

```azurecli
az vm stop -n SampleVM -g <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="32cab-105">Per verificare che la macchina sia stata arrestata, è possibile eseguire il ping dell'indirizzo IP pubblico, usare `ssh` oppure il comando `vm get-instance-view`.</span><span class="sxs-lookup"><span data-stu-id="32cab-105">We can verify it has stopped by attempting to ping the public IP address, using `ssh`, or through the `vm get-instance-view` command.</span></span> <span data-ttu-id="32cab-106">Quest'ultimo approccio restituisce sostanzialmente gli stessi dati del comando `vm show` ma include informazioni sull'istanza.</span><span class="sxs-lookup"><span data-stu-id="32cab-106">This final approach returns the same basic data as `vm show` but includes details about the instance itself.</span></span> <span data-ttu-id="32cab-107">Per visualizzare lo stato di esecuzione corrente della macchina virtuale inserire il comando seguente in Azure Cloud Shell:</span><span class="sxs-lookup"><span data-stu-id="32cab-107">Try typing the following command into Azure Cloud Shell to see the current running state of your VM:</span></span>

```azurecli
az vm get-instance-view -n SampleVM -g <rgn>[Sandbox resource group name]</rgn> --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

<span data-ttu-id="32cab-108">Il comando dovrebbe restituire come risultato `VM stopped`.</span><span class="sxs-lookup"><span data-stu-id="32cab-108">This command should return `VM stopped` as the result.</span></span>

## <a name="starting-a-vm"></a><span data-ttu-id="32cab-109">Avvio di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="32cab-109">Starting a VM</span></span>

<span data-ttu-id="32cab-110">È possibile eseguire l'operazione inversa con il comando `vm start`.</span><span class="sxs-lookup"><span data-stu-id="32cab-110">We can do the reverse through the `vm start` command.</span></span>

```azurecli
az vm start -n SampleVM -g <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="32cab-111">Questo comando avvia una macchina virtuale arrestata.</span><span class="sxs-lookup"><span data-stu-id="32cab-111">This command will start a stopped VM.</span></span> <span data-ttu-id="32cab-112">Per verificare l'esecuzione, inviare la query `vm get-instance-view`, che dovrebbe restituire `VM running`.</span><span class="sxs-lookup"><span data-stu-id="32cab-112">We can verify it through the `vm get-instance-view` query, which should now return `VM running`.</span></span>

## <a name="restarting-a-vm"></a><span data-ttu-id="32cab-113">Riavvio di una macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="32cab-113">Restarting a VM</span></span>

<span data-ttu-id="32cab-114">Infine, se sono state apportate modifiche che richiedono un riavvio, è possibile riavviare una macchina virtuale usando il comando `vm restart`.</span><span class="sxs-lookup"><span data-stu-id="32cab-114">Finally, we can restart a VM if we have made changes that require a reboot using the `vm restart` command.</span></span> <span data-ttu-id="32cab-115">È possibile aggiungere il flag `--no-wait` per tornare immediatamente all'interfaccia della riga di comando di Azure senza attendere il riavvio della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="32cab-115">You can add the `--no-wait` flag if you want the Azure CLI to return immediately without waiting for the VM to reboot.</span></span>

