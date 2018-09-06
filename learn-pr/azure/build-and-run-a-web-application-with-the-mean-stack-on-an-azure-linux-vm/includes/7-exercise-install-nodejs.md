<span data-ttu-id="dd0a0-101">In questo esercizio si installerà Node.js, che rappresenta la lettera **N** nell'acronimo MEAN, in una macchina virtuale Ubuntu Linux ospitata in Azure.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-101">In this exercise, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="dd0a0-102">Node.js fungerà da runtime per la gestione del traffico HTTP e l'hosting dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="dd0a0-103">Connettersi alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="dd0a0-103">Connect to the VM</span></span>

<span data-ttu-id="dd0a0-104">Per installare Node.js, è necessario connettersi alla macchina virtuale mediante **ssh**.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-104">In order to install Node.js, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="dd0a0-105">Se non ci si è ancora connessi alla macchina virtuale, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-105">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="dd0a0-106">Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della macchina virtuale precedenti.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-106">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a><span data-ttu-id="dd0a0-107">Installare Node.js</span><span class="sxs-lookup"><span data-stu-id="dd0a0-107">Install Node.js</span></span>

> [!Important]
> <span data-ttu-id="dd0a0-108">Ubuntu fornisce un pacchetto non ufficiale denominato **Node.js-legacy**, che non viene però gestito da Node.js e non è aggiornato.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-108">Ubuntu provides an unofficial package called **Node.js-legacy**, but this package is not maintained by Node.js and is outdated.</span></span>

1. <span data-ttu-id="dd0a0-109">Registrare il repository di pacchetti Node.js per consentire ad **apt-get** di trovare il pacchetto corretto da installare nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-109">Register the Node.js package repository so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="dd0a0-110">Installare il pacchetto Node.js nel sistema Linux.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-110">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="dd0a0-111">Verificare che l'installazione di Node.js sia riuscita eseguendo il semplice comando Node.js seguente:</span><span class="sxs-lookup"><span data-stu-id="dd0a0-111">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="dd0a0-112">L'output dovrebbe essere simile a `v8.11.4`, con la versione corrispondente alla versione di Node.js più recente al momento dell'installazione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-112">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="dd0a0-113">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="dd0a0-113">Summary</span></span>

<span data-ttu-id="dd0a0-114">Con Node.js installato nella macchina virtuale, è possibile iniziare a compilare un'applicazione Web della cui esecuzione sarà responsabile.</span><span class="sxs-lookup"><span data-stu-id="dd0a0-114">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>
