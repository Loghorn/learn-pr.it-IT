<span data-ttu-id="a9a66-101">In questa unità si installerà MongoDB nella macchina virtuale Ubuntu Linux in modo che funga da archivio dati per la nuova applicazione Web di esempio.</span><span class="sxs-lookup"><span data-stu-id="a9a66-101">In this unit, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="a9a66-102">Installare MongoDB</span><span class="sxs-lookup"><span data-stu-id="a9a66-102">Install MongoDB</span></span>

1. <span data-ttu-id="a9a66-103">Da Cloud Shell, connettersi con SSH alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="a9a66-103">From Cloud Shell, SSH into your VM.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="a9a66-104">Importare la chiave di crittografia per il repository MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a9a66-104">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="a9a66-105">Questo consente allo strumento di gestione pacchetti di verificare che i pacchetti mongodb da installare provengano da MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="a9a66-105">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="a9a66-106">Il comando **sudo** determina l'esecuzione del comando specificato con privilegi amministrativi.</span><span class="sxs-lookup"><span data-stu-id="a9a66-106">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="a9a66-107">Registrare il repository Ubuntu di MongoDB in modo da consentire allo strumento di gestione pacchetti di individuare i pacchetti mongodb.</span><span class="sxs-lookup"><span data-stu-id="a9a66-107">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a9a66-108">Questo comando varia a seconda delle versioni di Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a9a66-108">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="a9a66-109">Per sapere qual è la versione di Ubuntu in uso, eseguire: `uname -v`.</span><span class="sxs-lookup"><span data-stu-id="a9a66-109">To find out which version of Ubuntu you're using, run: `uname -v`.</span></span>
    > <span data-ttu-id="a9a66-110">Questo comando genererà un output simile a `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span><span class="sxs-lookup"><span data-stu-id="a9a66-110">This command will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="a9a66-111">Questo output indica che la versione di Ubuntu in esecuzione è la 16.04.1.</span><span class="sxs-lookup"><span data-stu-id="a9a66-111">This output indicates that we're running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="a9a66-112">Vedere [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) (Installare MongoDB Community Edition su Ubuntu) per ottenere il comando esatto per la versione in uso.</span><span class="sxs-lookup"><span data-stu-id="a9a66-112">Refer to the [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version.</span></span>

    <span data-ttu-id="a9a66-113">In Ubuntu 16.04 occorre eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="a9a66-113">On Ubuntu 16.04, we run this command:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="a9a66-114">Aggiornare il database del pacchetto in modo da avere le informazioni sul pacchetto più recenti.</span><span class="sxs-lookup"><span data-stu-id="a9a66-114">Update the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="a9a66-115">Installare il pacchetto MongoDB nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="a9a66-115">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="a9a66-116">Avviare il servizio MongoDB per potersi connettere a esso successivamente.</span><span class="sxs-lookup"><span data-stu-id="a9a66-116">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="a9a66-117">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a9a66-117">Summary</span></span>

<span data-ttu-id="a9a66-118">MongoDB è ora installato nella macchina virtuale Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="a9a66-118">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="a9a66-119">Fungerà da archivio dati sottostante per le informazioni salvate e recuperate nell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="a9a66-119">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>