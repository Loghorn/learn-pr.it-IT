In questa unità si installerà MongoDB nella macchina virtuale Ubuntu Linux in modo che funga da archivio dati per la nuova applicazione Web di esempio.

## <a name="install-mongodb"></a>Installare MongoDB

1. Da Cloud Shell, connettersi con SSH alla macchina virtuale.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. Importare la chiave di crittografia per il repository MongoDB. Questo consente allo strumento di gestione pacchetti di verificare che i pacchetti mongodb da installare provengano da MongoDB Inc.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    Il comando **sudo** determina l'esecuzione del comando specificato con privilegi amministrativi.

1. Registrare il repository Ubuntu di MongoDB in modo da consentire allo strumento di gestione pacchetti di individuare i pacchetti mongodb.

    > [!NOTE]
    > Questo comando varia a seconda delle versioni di Ubuntu. Per sapere qual è la versione di Ubuntu in uso, eseguire: `uname -v`.
    > Questo comando genererà un output simile a `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.
    >
    > Questo output indica che la versione di Ubuntu in esecuzione è la 16.04.1.
    > Vedere [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) (Installare MongoDB Community Edition su Ubuntu) per ottenere il comando esatto per la versione in uso.

    In Ubuntu 16.04 occorre eseguire questo comando:

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. Aggiornare il database del pacchetto in modo da avere le informazioni sul pacchetto più recenti.

    ```bash
    sudo apt-get update
    ```

1. Installare il pacchetto MongoDB nella macchina virtuale.

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. Avviare il servizio MongoDB per potersi connettere a esso successivamente.

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a>Riepilogo

MongoDB è ora installato nella macchina virtuale Ubuntu Linux. Fungerà da archivio dati sottostante per le informazioni salvate e recuperate nell'applicazione Web.