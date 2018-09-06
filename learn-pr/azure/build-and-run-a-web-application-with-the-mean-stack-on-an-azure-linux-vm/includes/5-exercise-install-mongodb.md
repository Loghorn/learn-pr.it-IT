## <a name="exercise-install-mongodb"></a><span data-ttu-id="64ddf-101">Esercizio: Installare MongoDB</span><span class="sxs-lookup"><span data-stu-id="64ddf-101">Exercise: Install MongoDB</span></span>

<span data-ttu-id="64ddf-102">In questo esercizio si installerà MongoDB nella macchina virtuale Ubuntu Linux in modo che funga da archivio dati per la nuova applicazione Web di esempio.</span><span class="sxs-lookup"><span data-stu-id="64ddf-102">In this exercise, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="64ddf-103">Connettersi alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="64ddf-103">Connect to the VM</span></span>

<span data-ttu-id="64ddf-104">Per installare MongoDB, è necessario connettersi alla macchina virtuale mediante **ssh**.</span><span class="sxs-lookup"><span data-stu-id="64ddf-104">In order to install MongoDB, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="64ddf-105">Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della macchina virtuale precedenti.</span><span class="sxs-lookup"><span data-stu-id="64ddf-105">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-mongodb"></a><span data-ttu-id="64ddf-106">Installare MongoDB</span><span class="sxs-lookup"><span data-stu-id="64ddf-106">Install MongoDB</span></span>

> [!Important]
> <span data-ttu-id="64ddf-107">Ubuntu fornisce un pacchetto non ufficiale denominato **mongodb**, che non viene però gestito da MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="64ddf-107">Ubuntu provides an unofficial package called **mongodb**, but this package is not maintained by MongoDB Inc.</span></span>

1. <span data-ttu-id="64ddf-108">Importare la chiave di crittografia per il repository MongoDB.</span><span class="sxs-lookup"><span data-stu-id="64ddf-108">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="64ddf-109">Questo consente allo strumento di gestione pacchetti di verificare che i pacchetti mongodb da installare provengano da MongoDB Inc.</span><span class="sxs-lookup"><span data-stu-id="64ddf-109">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="64ddf-110">Il comando **sudo** determina l'esecuzione del comando specificato con privilegi amministrativi.</span><span class="sxs-lookup"><span data-stu-id="64ddf-110">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="64ddf-111">Registrare il repository Ubuntu di MongoDB in modo da consentire allo strumento di gestione pacchetti di individuare i pacchetti mongodb.</span><span class="sxs-lookup"><span data-stu-id="64ddf-111">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="64ddf-112">Questo comando varia a seconda delle versioni di Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="64ddf-112">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="64ddf-113">Per sapere qual è la versione di Ubuntu in uso, eseguire: `uname -v`.</span><span class="sxs-lookup"><span data-stu-id="64ddf-113">To find out which version of Ubuntu you are running please run: `uname -v`.</span></span>
    > <span data-ttu-id="64ddf-114">Verrà generato un output simile a `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span><span class="sxs-lookup"><span data-stu-id="64ddf-114">This will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="64ddf-115">Questo output indica che la versione di Ubuntu in esecuzione è la 16.04.1.</span><span class="sxs-lookup"><span data-stu-id="64ddf-115">This indicates that we are running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="64ddf-116">Vedere [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) (Installare MongoDB Community Edition su Ubuntu) per ottenere il comando esatto per la versione in uso.</span><span class="sxs-lookup"><span data-stu-id="64ddf-116">Please refer to [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version</span></span>

    <span data-ttu-id="64ddf-117">In Ubuntu 16.04 occorre eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="64ddf-117">On Ubuntu 16.04 we run this:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="64ddf-118">Ricaricare il database del pacchetto in modo da avere le informazioni sul pacchetto più recenti.</span><span class="sxs-lookup"><span data-stu-id="64ddf-118">Reload the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="64ddf-119">Installare il pacchetto MongoDB nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="64ddf-119">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="64ddf-120">Avviare il servizio MongoDB per potersi connettere a esso successivamente.</span><span class="sxs-lookup"><span data-stu-id="64ddf-120">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="64ddf-121">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="64ddf-121">Summary</span></span>

<span data-ttu-id="64ddf-122">MongoDB è ora installato nella macchina virtuale Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="64ddf-122">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="64ddf-123">Fungerà da archivio dati sottostante per le informazioni salvate e recuperate nell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="64ddf-123">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>
