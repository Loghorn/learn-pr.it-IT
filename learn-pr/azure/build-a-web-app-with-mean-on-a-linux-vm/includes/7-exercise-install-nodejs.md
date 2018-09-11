In questa unità si installerà Node.js, che rappresenta la lettera **N** nell'acronimo MEAN, in una macchina virtuale Ubuntu Linux ospitata in Azure. Node.js fungerà da runtime per la gestione del traffico HTTP e l'hosting dell'applicazione Web.

## <a name="connect-to-the-vm"></a>Connettersi alla macchina virtuale

Per installare Node.js, è necessario connettersi alla macchina virtuale mediante **ssh**. Se non ci si è ancora connessi alla macchina virtuale, eseguire il comando seguente. Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della macchina virtuale precedenti.

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a>Installare Node.js

> [!Important]
> Ubuntu fornisce un pacchetto non ufficiale denominato **Node.js-legacy**. Questo pacchetto non è gestito da Node.js e non è aggiornato.

1. Registrare il repository di pacchetti Node.js per consentire ad **apt-get** di trovare il pacchetto corretto da installare nella macchina virtuale.

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. Installare il pacchetto Node.js nel sistema Linux.

    ```bash
    sudo apt-get install -y Node.js
    ```

1. Verificare che l'installazione di Node.js sia riuscita eseguendo il semplice comando Node.js seguente.

    ```bash
    node -v
    ```

    L'output dovrebbe essere simile a `v8.11.4`, con la versione corrispondente alla versione di Node.js più recente al momento dell'installazione del pacchetto.

## <a name="summary"></a>Riepilogo

Con Node.js installato nella macchina virtuale, è possibile iniziare a compilare un'applicazione Web della cui esecuzione sarà responsabile.