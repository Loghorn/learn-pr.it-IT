<span data-ttu-id="009c7-101">In questa unità si installerà Node.js, che rappresenta la lettera **N** nell'acronimo MEAN, in una macchina virtuale Ubuntu Linux ospitata in Azure.</span><span class="sxs-lookup"><span data-stu-id="009c7-101">In this unit, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="009c7-102">Node.js fungerà da runtime per la gestione del traffico HTTP e l'hosting dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="009c7-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="install-nodejs"></a><span data-ttu-id="009c7-103">Installare Node.js</span><span class="sxs-lookup"><span data-stu-id="009c7-103">Install Node.js</span></span>

1. <span data-ttu-id="009c7-104">**Se non si è ancora eseguito sulla macchina virtuale nell'esercizio precedente**, connettersi tramite SSH.</span><span class="sxs-lookup"><span data-stu-id="009c7-104">**If you aren't still SSHed into your VM from the previous exercise**, SSH in.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="009c7-105">Registrare il repository di pacchetti Node.js per consentire ad **apt-get** di trovare il pacchetto corretto da installare nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="009c7-105">Register the Node.js package repository so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="009c7-106">Installare il pacchetto Node.js nel sistema Linux.</span><span class="sxs-lookup"><span data-stu-id="009c7-106">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="009c7-107">Verificare che l'installazione di Node.js sia riuscita eseguendo il semplice comando Node.js seguente.</span><span class="sxs-lookup"><span data-stu-id="009c7-107">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="009c7-108">L'output dovrebbe essere simile a `v8.11.4`, con la versione corrispondente alla versione di Node.js più recente al momento dell'installazione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="009c7-108">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version that's available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="009c7-109">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="009c7-109">Summary</span></span>

<span data-ttu-id="009c7-110">Con Node.js installato nella macchina virtuale, è possibile iniziare a compilare un'applicazione Web della cui esecuzione sarà responsabile.</span><span class="sxs-lookup"><span data-stu-id="009c7-110">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>