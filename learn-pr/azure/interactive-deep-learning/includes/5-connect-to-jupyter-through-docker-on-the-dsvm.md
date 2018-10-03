Una volta configurata e avviata la Data Science Virtual Machine, si deciderà di eseguire il training del primo modello di apprendimento profondo. Si intende eseguire gli esperimenti in modo isolato da tutti gli altri elementi sul server. A tale scopo, verranno eseguiti in un contenitore Docker.

## <a name="connect-to-the-vm-with-ssh"></a>Connettersi alla macchina virtuale con SSH

Accertarsi di essere ancora connessi alla macchina virtuale tramite SSH. In caso contrario, è sufficiente eseguire nuovamente questo comando per collegarsi in remoto alla macchina virtuale.

1. Eseguire il comando seguente in Azure Cloud Shell per accedere alla VM.

    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 
    
    Sostituire `<USERNAME>` con il nome dell'account amministratore definito durante la creazione della VM. Sostituire `<IP>` con l'indirizzo IP della VM salvato nell'esercizio precedente.  

1. Quando richiesto, immettere la password per l'account amministratore per completare il processo di accesso.

## <a name="run-a-jupyter-notebook-server-in-a-docker-container-in-the-vm"></a>Eseguire un server Jupyter Notebook in un contenitore Docker nella VM

> [!NOTE]
> Poiché è presente solo un account amministratore o radice nella VM, è necessario eseguire tutti i comandi di Docker come radice usando `sudo`

1. Per vedere quali contenitori Docker sono presenti nella VM, eseguire questo comando al prompt dei comandi.

    ```azurecli 
    sudo docker ps
    ```

1. Eseguire questo comando al prompt dei comandi per creare un nuovo contenitore per i nostri esperimenti.

    ```azurecli 
    sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
    'conda install jupyter matplotlib -y &&\
    curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
    ``` 

    Questo comando rimarrà in esecuzione a lungo. Nel frattempo, ecco una descrizione della sua funzione. 
    - `docker run` esegue un comando in un nuovo contenitore. L'immagine Docker usata è pytorch/pytorch. In prima istanza crea un livello contenitore scrivibile sull'immagine specificata e ne esegue l'avvio con il comando specificato.
    - `--rm` rimuoverà il contenitore all'uscita. Per mantenere il contenitore, eliminare questo argomento. 
    - `--entrypoint 'bin/sh'` sovrascrive il punto di ingresso predefinito dell'immagine con la shell Bash
    - `-c` definisce il comando da eseguire all'avvio del contenitore. In questo caso, esegue tre comandi:
        - Installa Jupyter e matplotlib
        - Copia un notebook (cifar10_tutorial.ipynb) da pytorch.org in un file nel contenitore denominato `first_pytorch_classifier.ipynb`
        - Avvia il server notebook nel contenitore come nell'esercizio precedente.  Nessun browser viene avviato. Consentire l'accesso al notebook dalla radice e restare in ascolto su tutte le porte. 
    
    Il server notebook è in ascolto su tutte le porte per il contenitore. In che modo viene ricevuto il traffico dall'esterno del contenitore? L'argomento `-p 8888:8888` associa la porta `8888` del contenitore alla porta TCP 8888 sul computer host. Pertanto, il traffico in ingresso nella VM sulla porta 8888 verrà rilevato dal contenitore. 

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a>Connettersi al server Jupyter Notebook da un browser remoto 

Quando Jupyter Notebook è in esecuzione nel contenitore, verrà mostrato un messaggio simile al seguente. 

> *Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}* (Copiare e incollare l'URL nel browser quando si esegue la connessione per la prima volta per accedere con un token: http://(5b8783e7911d o 127.0.0.1):8888/?token={token})

1. Sostituire la parte **http://(5b8783e7911d or 127.0.0.1)** dell'URL con il nome di dominio completo (FQDN, Fully Qualified Domain Name) o con l'indirizzo IP della VM e passare all'indirizzo in una nuova scheda del browser.

    ![Screenshot che illustra il dashboard di un Jupyter Notebook. ](../media/notebook-in-docker.png)

    > [!TIP]
    > È possibile ottenere l'FQDN e l'indirizzo IP della VM con il comando seguente:
    > 
    > `az vm show -d --name <HOSTNAME> --resource-group <rgn>[sandbox resource group name]</rgn> --output table`
    >
    > Ricordarsi di sostituire `<HOSTNAME>` con il nome assegnato alla VM. 
    
    Questa volta viene visualizzato un unico notebook. Questo perché ci si trova in un contenitore ed è stato copiato solo questo notebook. Nel prossimo esercizio sarà possibile esercitarsi con questo notebook. 
    
    > [!TIP]
    > Non arrestare ancora il server notebook. Nel prossimo esercizio si userà il notebook `first_pytorch_classifier.ipynb`.