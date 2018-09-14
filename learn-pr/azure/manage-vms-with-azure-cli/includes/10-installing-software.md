<span data-ttu-id="55862-101">Resta solo da provare a installare un server Web nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="55862-101">The last thing we want to try on our VM is to install a web server.</span></span> <span data-ttu-id="55862-102">Uno dei pacchetti più semplici da installare è `nginx`.</span><span class="sxs-lookup"><span data-stu-id="55862-102">One of the easiest packages to install is `nginx`.</span></span>

1. <span data-ttu-id="55862-103">Individuare l'indirizzo IP pubblico della macchina virtuale di Linux.</span><span class="sxs-lookup"><span data-stu-id="55862-103">Locate the public IP address of your Linux virtual machine.</span></span> <span data-ttu-id="55862-104">È possibile usare il comando `vm list-ip-addresses` per cercarlo.</span><span class="sxs-lookup"><span data-stu-id="55862-104">Remember you can use the `vm list-ip-addresses` command to look it up.</span></span>

1. <span data-ttu-id="55862-105">Aprire quindi una connessione `ssh` alla macchina come fatto in precedenza durante il test.</span><span class="sxs-lookup"><span data-stu-id="55862-105">Next, open an `ssh` connection to the machine like you did when we tested it.</span></span> <span data-ttu-id="55862-106">Tenere presente che è necessario specificare il nome amministratore ("**aldis**").</span><span class="sxs-lookup"><span data-stu-id="55862-106">Remember you will need to pass in the admin name ("**aldis**").</span></span>

1. <span data-ttu-id="55862-107">Nella shell presentata, eseguire il comando seguente per installare il server Web `nginx`.</span><span class="sxs-lookup"><span data-stu-id="55862-107">In the presented shell, execute the following command to install the `nginx` web server.</span></span>

```azurecli
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. <span data-ttu-id="55862-108">In Azure Cloud Shell usare `curl` per leggere la pagina predefinita del server Web di Linux.</span><span class="sxs-lookup"><span data-stu-id="55862-108">In Azure Cloud Shell, use `curl` to read the default page from your Linux web server.</span></span> <span data-ttu-id="55862-109">In alternativa, è possibile aprire una nuova scheda del browser e navigare all'indirizzo IP pubblico.</span><span class="sxs-lookup"><span data-stu-id="55862-109">Alternatively, you can open a new browser tab and browse to the public IP address.</span></span>

```azurecli
curl 168.61.54.62
```

<span data-ttu-id="55862-110">Non verrà completata perché la macchina virtuale Linux non espone la porta 80 (`http`) attraverso il firewall incorporato.</span><span class="sxs-lookup"><span data-stu-id="55862-110">It will fail because the Linux virtual machine doesn't expose port 80 (`http`) through the built-in firewall.</span></span> <span data-ttu-id="55862-111">Per fortuna, l'interfaccia della riga comando di Azure dispone di un comando apposito: `vm open-port`.</span><span class="sxs-lookup"><span data-stu-id="55862-111">Luckily, the Azure CLI has a command for that: `vm open-port`.</span></span> 

1. <span data-ttu-id="55862-112">Per aprire la porta 80 in Cloud Shell, digitare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="55862-112">Type the following into Cloud Shell to open port 80:</span></span>

```
az vm open-port --port 80 --resource-group ExerciseResources --name SampleVM
```

<span data-ttu-id="55862-113">Infine, provare `curl` nuovamente.</span><span class="sxs-lookup"><span data-stu-id="55862-113">Finally, try `curl` again.</span></span> <span data-ttu-id="55862-114">Questa volta dovrebbe restituire i dati.</span><span class="sxs-lookup"><span data-stu-id="55862-114">This time it should return data.</span></span> <span data-ttu-id="55862-115">È possibile vedere la pagina anche in un browser.</span><span class="sxs-lookup"><span data-stu-id="55862-115">You can see the page in a browser as well.</span></span>
