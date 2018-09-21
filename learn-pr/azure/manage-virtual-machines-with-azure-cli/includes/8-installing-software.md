<span data-ttu-id="6d012-101">Resta solo da provare a installare un server Web nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="6d012-101">The last thing we want to try on our VM is to install a web server.</span></span> <span data-ttu-id="6d012-102">Uno dei pacchetti più semplici da installare è `nginx`.</span><span class="sxs-lookup"><span data-stu-id="6d012-102">One of the easiest packages to install is `nginx`.</span></span>

## <a name="install-nginx-web-server"></a><span data-ttu-id="6d012-103">Installare il server Web NGINX</span><span class="sxs-lookup"><span data-stu-id="6d012-103">Install NGINX web server</span></span>

1. <span data-ttu-id="6d012-104">Individuare l'indirizzo IP pubblico della macchina virtuale di Linux.</span><span class="sxs-lookup"><span data-stu-id="6d012-104">Locate the public IP address of your Linux virtual machine.</span></span> <span data-ttu-id="6d012-105">È possibile usare il comando `vm list-ip-addresses` per cercarlo.</span><span class="sxs-lookup"><span data-stu-id="6d012-105">Remember you can use the `vm list-ip-addresses` command to look it up.</span></span>

1. <span data-ttu-id="6d012-106">Aprire quindi una connessione `ssh` alla macchina come fatto in precedenza durante il test.</span><span class="sxs-lookup"><span data-stu-id="6d012-106">Next, open an `ssh` connection to the machine like you did when we tested it.</span></span> <span data-ttu-id="6d012-107">Tenere presente che è necessario specificare il nome amministratore ("**aldis**").</span><span class="sxs-lookup"><span data-stu-id="6d012-107">Remember you will need to pass in the admin name ("**aldis**").</span></span>

1. <span data-ttu-id="6d012-108">Nella shell presentata, eseguire il comando seguente per installare il server Web `nginx`.</span><span class="sxs-lookup"><span data-stu-id="6d012-108">In the presented shell, execute the following command to install the `nginx` web server.</span></span>

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. <span data-ttu-id="6d012-109">Chiudere la Secure Shell.</span><span class="sxs-lookup"><span data-stu-id="6d012-109">Exit the Secure Shell.</span></span>

```bash
exit
```

## <a name="retrieve-our-default-page"></a><span data-ttu-id="6d012-110">Recuperare la pagina predefinita</span><span class="sxs-lookup"><span data-stu-id="6d012-110">Retrieve our default page</span></span>

1. <span data-ttu-id="6d012-111">In Azure Cloud Shell usare `curl` per leggere la pagina predefinita del server Web di Linux.</span><span class="sxs-lookup"><span data-stu-id="6d012-111">In Azure Cloud Shell, use `curl` to read the default page from your Linux web server.</span></span> <span data-ttu-id="6d012-112">In alternativa, è possibile aprire una nuova scheda del browser e navigare all'indirizzo IP pubblico.</span><span class="sxs-lookup"><span data-stu-id="6d012-112">Alternatively, you can open a new browser tab and browse to the public IP address.</span></span>

```azurecli
curl 40.83.165.85
```

<span data-ttu-id="6d012-113">Non verrà completata perché la macchina virtuale Linux non espone la porta 80 (`http`) attraverso il firewall incorporato.</span><span class="sxs-lookup"><span data-stu-id="6d012-113">It will fail because the Linux virtual machine doesn't expose port 80 (`http`) through the built-in firewall.</span></span> <span data-ttu-id="6d012-114">Per fortuna, l'interfaccia della riga comando di Azure dispone di un comando apposito: `vm open-port`.</span><span class="sxs-lookup"><span data-stu-id="6d012-114">Luckily, the Azure CLI has a command for that: `vm open-port`.</span></span> 

1. <span data-ttu-id="6d012-115">Per aprire la porta 80 in Cloud Shell, digitare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="6d012-115">Type the following into Cloud Shell to open port 80:</span></span>

```azurecli
az vm open-port --port 80 --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM
```

<span data-ttu-id="6d012-116">L'aggiunta della regola della rete e l'apertura della porta nel firewall richiederanno alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="6d012-116">It will take a moment to add the network rule and open the port through the firewall.</span></span> <span data-ttu-id="6d012-117">Provare `curl` di nuovo.</span><span class="sxs-lookup"><span data-stu-id="6d012-117">Try `curl` again.</span></span> <span data-ttu-id="6d012-118">Questa volta dovrebbe restituire i dati.</span><span class="sxs-lookup"><span data-stu-id="6d012-118">This time it should return data.</span></span> <span data-ttu-id="6d012-119">È possibile vedere la pagina anche in un browser.</span><span class="sxs-lookup"><span data-stu-id="6d012-119">You can see the page in a browser as well.</span></span>

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
