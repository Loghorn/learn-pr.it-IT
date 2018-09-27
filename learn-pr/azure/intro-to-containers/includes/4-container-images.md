Fino a questo punto sono state scaricate ed eseguite immagini Docker create da altri utenti. Ora si creeranno direttamente le immagini contenitore. Si apprenderà come:

* Creare un'immagine mediante un processo manuale.
* Creare la stessa immagine usando un processo più automatizzato, detto Dockerfile.
* Pubblicare l'immagine in un hub Docker, un registro contenitori pubblico.

## <a name="what-is-a-dockerfile"></a>Che cos'è un Dockerfile?

Un Dockerfile è un file di testo che descrive tutti gli elementi necessari per l'esecuzione del contenitore.

Un'istruzione nel Dockerfile può definire:

* L'immagine padre sulla quale si basa l'immagine.
* Un pacchetto software da installare.
* Un file di configurazione o un altro file che l'app deve eseguire.
* Impostazioni di rete, ad esempio una porta che si vuole rendere disponibile.
* Il comando da eseguire all'avvio del contenitore.

È possibile creare un'immagine del contenitore usando un processo manuale o usare un Dockerfile per automatizzare il processo. La creazione manuale di un'immagine Docker è un buon metodo per comprendere il processo. L'uso di un Dockerfile rende il processo più veloce e semplice da ripetere.

## <a name="what-is-docker-hub"></a>Che cos'è l'hub Docker?

L'hub Docker è una posizione in cui archiviare e scaricare le immagini Docker. È possibile scaricare le immagini Docker create da Docker e dalla community Docker, ad esempio l'immagine Nginx usata in precedenza. È anche possibile condividere le proprie immagini con la community Docker o solo con il proprio team.

## <a name="create-an-image-manually"></a>Creare un'immagine manualmente

Ora si creerà un'immagine Docker che esegue un'applicazione Python di base.

1. Per iniziare, attivare un'istanza di contenitore dall'immagine [python](https://hub.docker.com/_/python/?azure-portal=true).

    ```bash
    docker run --name python-demo -it python bash
    ```

    L'estrazione dell'immagine dall'hub Docker richiederà qualche istante. Nel frattempo è possibile analizzare ciò che esegue il comando.

    * L'argomento `--name` specifica il nome del contenitore, "python-demo".
    * Gli argomenti `-i` e `-t` vengono combinati in un solo argomento, `-it`. Questi argomenti creano una sessione interattiva con il contenitore in esecuzione.
      * `-i` crea una sessione interattiva con il contenitore.
      * `-t` crea un pseudo terminale che resta in stato di esecuzione.
    * `python` specifica l'immagine di base. Docker scarica l'immagine dall'hub Docker. Questa immagine è preconfigurata con un ambiente per la programmazione Python.
    * `bash` specifica il programma da eseguire.

    Questa configurazione può essere paragonata a un ambiente interattivo per la scrittura di app Python. Quando l'app è operativa è possibile acquisire un'immagine (o istantanea) dell'ambiente. Man mano che si migliora l'app è possibile creare immagini aggiuntive per uso personale o per il team.

    La sessione del terminale passa al pseudo terminale del contenitore. Il prompt dei comandi è simile al seguente:

    ```docker
    root@d8ccada9c61e:/#
    ```

1. Dalla sessione di Docker eseguire questa istruzione per creare una directory denominata `./app` e passare a tale directory.

    ```bash
    mkdir ./app && cd ./app
    ```

    Sebbene questa operazione non sia obbligatoria, la directory `./app` offre una posizione in cui aggiungere il codice Python.

1. Eseguire questa istruzione per creare un programma Python di base.

    ```bash
    echo 'print("Hello World!")' > hello.py
    ```

    Questo programma visualizza "Hello World!" nel terminale.

    > [!TIP]
    > In pratica è possibile _montare_ una directory della workstation in una directory nel contenitore. In questo modo è possibile scrivere codice nella workstation usando l'editor preferito. Quando si salva il file, anche questo file viene visualizzato nel contenitore.

1. Eseguire il codice seguente per provare il programma Python.

    ```bash
    python hello.py
    ```

    Verrà visualizzato quanto segue.

    ```output
    Hello World!
    ```

1. Uscire dalla sessione interattiva.

    ```bash
    exit
    ```

1. Nella VM Linux eseguire `docker ps` per elencare tutti i contenitori in esecuzione:

    ```bash
    docker ps
    ```

    Si noti che nessun elemento è in esecuzione. 

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

    Questo si verifica perché con l'uscita dal contenitore termina la sessione di Bash. Quando l'applicazione o il servizio eseguito dal contenitore viene chiuso, Docker interrompe l'esecuzione del contenitore.

1. Eseguire `docker ps` una seconda volta e includere l'argomento `-a`. Questo comando restituisce tutti i contenitori, in esecuzione o arrestati.

    ```bash
    docker ps -a
    ```

    Viene visualizzata una schermata simile alla seguente:

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
    ```

   Il contenitore **python-demo** visualizza lo stato **Chiuso**.

1. Eseguire `docker commit` per creare un'immagine dal contenitore.

   ```bash
   docker commit python-demo python-custom
   ```

   In questo caso, Docker crea un'immagine con nome **python-custom** dal contenitore con nome **python-demo**. Il nome dell'immagine può essere qualsiasi nome.

1. Eseguire `docker images` per elencare le immagini.

    ```bash
    docker images
    ```

    Viene visualizzata l'immagine **python-custom**.

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-custom       latest              1f231e7127a1        6 seconds ago       922MB
    python              latest              638817465c7d        24 hours ago        922MB
    ```

1. Usare `docker run` per eseguire il test dell'immagine.

    ```bash
    docker run python-custom python app/hello.py
    ```

    Tenere presente che `docker run` accetta come argomento finale il comando da eseguire. In questo caso il comando è `python app/hello.py`.

    Verrà visualizzato quanto segue.

    ```output
    Hello World!
    ```

    Dato che questo contenitore non è eseguito in modo interattivo, viene eseguito il programma Python e il contenitore si arresta.

## <a name="use-a-dockerfile-to-create-an-image-automatically"></a>Usare un Dockerfile per creare automaticamente un'immagine

Ora si userà un processo più automatico per creare la stessa immagine Docker che esegue il programma Python.

La creazione manuale di un'immagine è un ottimo modo per provare ed esplorare le nuove funzionalità. Tuttavia si supponga di voler condividere il processo con il team o di volerlo rendere ripetibile. Si supponga ad esempio di voler creare ogni sera una nuova immagine che esegue la versione più recente del software che il team sta compilando.

In questo caso il Dockerfile è la miglior soluzione. Un Dockerfile può essere considerato come un modo per descrivere istruzioni che in alternativa andrebbero eseguite manualmente.

1. Dalla macchina virtuale di Linux eseguire questo comando per creare un Dockerfile che esegue il programma Python.

    ```bash
    cat << EOF > Dockerfile
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    EOF
    ```

    Questo esempio è un modo semplice per creare il file. Nella pratica, per la creazione si usa l'editor di codice preferito.

    Stampare il Dockerfile nella console.

    ```bash
    cat Dockerfile
    ```

    Verrà visualizzato quanto segue.

    ```output
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    ```

    Ora si procederà ad analizzare in dettaglio il funzionamento del Dockerfile.

    * `FROM` specifica l'immagine padre sulla quale si basa l'immagine corrente. In questo caso l'immagine padre è **python**.
    * `WORKDIR` specifica la directory di lavoro per qualsiasi istruzione `RUN` e `CMD` visualizzata più avanti nel Dockerfile. Docker crea automaticamente la directory, in questo caso `./app`.
    * `RUN` specifica un comando da eseguire nel contenitore. È possibile usare `RUN` per installare software, apportare modifiche alla configurazione e pulire il contenitore prima che venga acquisito. In questo caso si crea un programma Python di base per `./app/hello.py`.
    * `CMD` specifica il processo da eseguire all'avvio del contenitore. In questo caso viene eseguito `python hello.py` dalla directory `/.app`.

    Si sarà notato che la procedura è quasi identica a quella eseguita per creare l'immagine manualmente. La conversione della procedura in codice semplifica la condivisione e la ripetizione del processo.

1. Eseguire `docker build` per creare l'immagine usando le istruzioni specificate nel Dockerfile.

    ```bash
    docker build -t python-dockerfile .
    ```

    In genere Docker crea l'immagine in pochi secondi.

    Viene visualizzato un output simile al seguente.

    ```output
    Sending build context to Docker daemon  2.048kB
    Step 1/4 : FROM python
     ---> 638817465c7d
    Step 2/4 : WORKDIR ./app
     ---> Running in 990d17e86466
    Removing intermediate container 990d17e86466
     ---> 59a074a092cc
    Step 3/4 : RUN echo 'print("Hello World!")' > hello.py
     ---> Running in aed707c53bc5
    Removing intermediate container aed707c53bc5
     ---> d7f55a9d0e85
    Step 4/4 : CMD python hello.py
     ---> Running in e87ec55a8d36
    Removing intermediate container e87ec55a8d36
     ---> 98c39b91770f
    Successfully built 98c39b91770f
    Successfully tagged python-dockerfile:latest
    ```

1. Usare `docker images` per elencare le immagini.

    ```bash
    docker images
    ```

    Viene visualizzata l'immagine **python-dockerfile** appena compilata.

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-dockerfile   latest              8ed5f85c5e5b        24 seconds ago      923MB
    python-custom       latest              6789cbe2cceb        4 minutes ago       923MB
    python              latest              a9d071760c82        2 weeks ago         923MB
    ```

1. Usare `docker run` per eseguire il test del contenitore.

    ```bash
    docker run python-dockerfile
    ```

    Verrà visualizzato quanto segue.

    ```output
    Hello World!
    ```

    A differenza di quanto avviene nel test dell'immagine creata manualmente, in questo caso non si specifica il comando da eseguire, `python app/hello.py`. Il comando viene specificato dal Dockerfile, pertanto è incorporato nell'immagine.

## <a name="publish-your-image-to-docker-hub"></a>Pubblicare l'immagine nell'hub Docker

Per pubblicare un'immagine nell'hub Docker è necessario disporre di un account. Ora si imposterà l'account dell'hub Docker e si pubblicherà l'immagine **python-dockerfile** nell'hub Docker.

1. [Creare il proprio account hub Docker](https://hub.docker.com?azure-portal=true).

1. Esportare il nome dell'account hub Docker come variabile di ambiente. Sostituire **account-name** con il nome dell'account.

    ```bash
    export docker_account=account-name
    ```

    La variabile di ambiente renderà più facile l'esecuzione dei comandi seguenti.

1. Eseguire `docker tag` per contrassegnare l'immagine con il nome del proprio account hub Docker.

    ```bash
    docker tag python-dockerfile $docker_account/python-dockerfile
    ```

1. Eseguire `docker login` per accedere all'hub Docker.

    ```bash
    docker login
    ```

    Quando richiesto, immettere nome utente e password.

1. Eseguire `docker push` per eseguire il push dell'immagine **python-dockerfile** nell'hub Docker (caricare l'immagine).

    ```bash
    docker push $docker_account/python-dockerfile
    ```

    L'output sarà simile al seguente:

    ```output
    The push refers to repository [docker.io/account/python-dockerfile]
    f39073ca4d5a: Pushed
    9dfcec2738a9: Pushed
    ffab8273c674: Mounted from account/python
    e6f6cbe5e14e: Mounted from account/python
    3eb255f8a6ac: Mounted from account/python
    b860b6c48eec: Mounted from account/python
    1fa8778eb779: Mounted from account/python
    fa0c3f992cbd: Mounted from account/python
    ce6466f43b11: Mounted from account/python
    719d45669b35: Mounted from account/python
    3b10514a95be: Mounted from account/python
    latest: digest: sha256:b816c32382d06ac74530d56f2a7b83000c86174218f430c6bb5039fdcfba38aa size: 2631
    ```

    L'immagine Docker è ora archiviata nell'hub Docker. È possibile usare `docker pull` o `docker run` da un altro computer per scaricare o eseguire l'immagine dall'hub Docker. Ecco due esempi:

    1. Questo esempio estrae l'immagine più recente dall'hub Docker.

        ```bash
        docker pull my_docker_account/python-dockerfile
        ```

    1. Questo esempio esegue il contenitore.

        ```bash
        docker run my_docker_account/python-dockerfile
        ```

1. Eseguire il test del contenitore.

    Accedere al proprio [account hub Docker](https://hub.docker.com?azure-portal=true). Nella scheda **Repository** viene visualizzata l'immagine Docker.

    ![Immagine Docker nell'hub Docker](../media/4-docker-hub-python-dockerfile.png)
