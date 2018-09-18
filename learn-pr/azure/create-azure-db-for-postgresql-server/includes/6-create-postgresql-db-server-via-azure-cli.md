Si decide di creare un Database di Azure per PostgreSQL per archiviare i percorsi acquisiti dai dispositivi di fitness dei runner. In base ai volumi di dati cronologici acquisiti si sa che i requisiti di archiviazione del server devono essere impostati su 20 GB. Per supportare i requisiti di elaborazione è necessario il supporto di una risorsa di calcolo Gen 5 con 1 vCore. Si sa anche di aver bisogno di un periodo di conservazione di 15 giorni per i backup dei dati.

> [!TIP]
> Tutti gli esercizi di Microsoft Learn sono gratuiti, ma quando si inizia a esplorare per proprio conto, è necessario procurarsi una sottoscrizione di Azure. Se non si dispone di una sottoscrizione, creare un account [gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) in pochi minuti.

Iniziamo.

Accedere al [portale di Azure](https://portal.azure.com?azure-portal=true).

Sarà necessario avviare una sessione di Azure Cloud Shell. Selezionare l'icona Cloud Shell nella parte superiore della schermata per avviare la sessione.

![Pulsante Cloud Shell](../media-draft/cloud-shell-button.png)

Se non si ha già un account di archiviazione da usare con Cloud Shell, è necessario crearne uno al primo accesso. L'interfaccia del portale guiderà l'utente attraverso il processo di creazione di un account di archiviazione.

Questa esercitazione usa `bash` come ambiente della riga di comando.

1. Selezionare la sottoscrizione da usare per creare il server.

    Se si hanno più sottoscrizioni, verificare che sia attiva la sottoscrizione appropriata con il comando seguente, sostituendo gli zero con l'identificatore della sottoscrizione.

    ``` bash
    az account set --subscription "00000000-0000-0000-0000-000000000000"
    ```

    È possibile elencare tutte le sottoscrizioni con il comando `az account list --output table`. Nell'elenco scegliere l'identificatore della sottoscrizione da usare.

1. Se non è ancora stato fatto in un'unità precedente, creare un gruppo di risorse. Eseguire il comando seguente.

    ```bash
    az group create --name <resource_group_name> --location <location>
    ```

    > [!Note]
    > È possibile recuperare un elenco di tutte le località con il comando `az account list-locations`. Selezionare il valore `displayName` o `name` e usarlo per il parametro `<location>`.

1. A questo punto è possibile eseguire il comando `az postgres server create`.

    Tenere presente che le dimensioni di archiviazione del server devono essere impostate su 20 GB e che si deve scegliere il supporto di calcolo Gen 5 con 1 vCore e un periodo di conservazione di 15 giorni per i backup dei dati.

    È necessario specificare alcuni parametri:

    - `--resource-group <resource_group_name>`
    - `--name <new_server_name>`
    - `--location <location>`
    - `--admin-user <admin_user_name>`
    - `--admin-password <server_admin_password>`
    - `--sku-name <sku>`
    - `--storage-size <size>`
    - `--backup-retention <days>`
    - `--version <version_number>`

    Provare a scrivere il comando e a completare i parametri senza guardare la soluzione riportata di seguito. Sostituire i valori in `<>` con i propri valori.

    > [!NOTE]
    > È possibile recuperare un elenco di tutte le località con il comando `az account list-locations`. Selezionare il valore `displayName` o `name` e usarlo per il parametro `<location>`.

    ```bash
    az postgres server create --resource-group <resource_group_name> --name <unique_server_name>  --location "UK West" --admin-user <server_admin_login_id> --admin-password <server_admin_password> --sku-name B_Gen5_1 --storage-size 20480 --backup-retention 15 --version 10
    ```

Quando viene seguito, il sistema impiegherà alcuni minuti per elaborare le informazioni. Se il server è stato creato, viene restituita una stringa JSON (Java Script Object Notation) che descrive il server. Se il server non è stato creato, viene visualizzato un messaggio di errore. Usare le informazioni sull'errore per esaminare e correggere i parametri del comando.

La creazione del server PostgreSQL tramite l'interfaccia della riga di comando di Azure è stata completata. Nell'unità di successiva verrà illustrato come configurare le impostazioni di sicurezza del server.
