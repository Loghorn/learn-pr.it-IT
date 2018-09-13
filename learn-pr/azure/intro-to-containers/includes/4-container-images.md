Nell'ultima unità sono state usate immagini dei contenitori già pronte per eseguire alcune operazioni di Docker di base. In questa unità si creeranno immagini dei contenitori personalizzate, si eseguirà il push di queste immagini in un registro contenitori pubblico e si eseguiranno i contenitori da queste immagini.

Le immagini dei contenitori possono essere create manualmente o usando un cosiddetto Dockerfile per automatizzare il processo. Il metodo preferito consiste nell'usare un Dockerfile, ma questa unità illustrerà entrambi i metodi. L'obiettivo è conoscere il processo manuale per poter comprendere meglio cosa accade quando si usa un Dockerfile per l'automazione.

## <a name="manual-image-creation"></a>Creazione di un'immagine manuale

Quando si crea manualmente un'immagine del contenitore, vengono eseguite le azioni seguenti:

- Avviare un'istanza di un contenitore.
- Stabilire una sessione terminal con il contenitore.
- Modificare il contenitore installando software e apportando modifiche alla configurazione.
- Acquisizione del contenitore in una nuova immagine usando il comando `docker capture`.

In questo primo esempio si avvia un'istanza di un contenitore che esegue Python, si crea un'applicazione hello world e quindi si acquisisce il contenitore in una nuova immagine.

In primo luogo, eseguire un contenitore dall'immagine NGINX. Questo comando è leggermente diverso dai comandi eseguiti nell'unità precedente. Poiché si vuole stabilire una sessione terminal con il contenitore in esecuzione, vengono forniti gli argomenti `-t` e `-i`. L'unione di questi argomenti consente di indicare a Docker di allocare uno pseudoterminale che rimarrà in uno stato di esecuzione. In altre parole, gli argomenti `-t` e `-i` creano una sessione interattiva con il contenitore in esecuzione.

Si specifica quindi che viene usata l'immagine del contenitore `python` e il processo da eseguire nel contenitore è `bash`.

```bash
docker run --name python-demo -ti python bash
```

Dopo che il comando viene eseguito, la sessione terminal passerà allo pseudoterminale del contenitore. Questo passaggio può essere visualizzato dal prompt del terminale, che dovrebbe ora essere simile al seguente:

```bash
root@d8ccada9c61e:/#
```

A questo punto, si lavorerà all'interno del contenitore. Si noterà che lavorare all'interno di un contenitore è molto simile a lavorare all'interno di un sistema fisico o virtuale. È ad esempio possibile elencare, creare ed eliminare file, installare software e apportare modifiche alla configurazione. Per questo semplice esempio, viene creato uno script hello world basato su Python. A tale scopo, eseguire il comando seguente:

```bash
echo 'print("Hello World!")' > hello.py
```

Per testare lo script mentre si è ancora nel contenitore, eseguire il comando seguente:

```bash
python hello.py
```

L'output sarà il seguente:

```bash
Hello World!
```

Dopo aver verificato che lo script funziona come previsto, uscire dal contenitore digitando `exit`:

```bash
exit
```

Nel terminale del sistema locale usare il comando `docker ps` per elencare tutti i contenitori in esecuzione:

```bash
docker ps
```

Si noti che nessuno è in esecuzione. Quando è stato immesso `exit` nel contenitore in esecuzione, il processo Bash è stato completato e il contenitore è stato quindi arrestato. Si tratta del comportamento previsto ed è corretto.

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Usare di nuovo `docker ps` e includere l'argomento `-a`. Questo comando restituirà un elenco di tutti i contenitori, indipendentemente dal fatto che siano in esecuzione.

```bash
docker ps -a
```

Si noti che un contenitore con il nome *python-demo* ha lo stato *Exited*. Questo contenitore è l'istanza arrestata del contenitore da cui si è appena usciti.

```bash
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

Per creare una nuova immagine del contenitore da questo contenitore, usare il comando `docker commit`. L'esempio seguente indica a *docker commit* di creare un'immagine denominata *python-custom* dai contenitori *python-demo*.

```bash
docker commit python-demo python-custom
```

Dopo il completamento del comando, l'output dovrebbe essere simile al seguente:

```bash
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

Eseguire ora `docker images` per visualizzare un elenco di immagini dei contenitori:

```bash
docker images
```

Si noterà ora l'immagine personalizzata di Python.

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

Eseguire un contenitore dalla nuova immagine. È anche necessario specificare quale comando o processo eseguire all'interno del contenitore. Con questo esempio, eseguire `python hello.py`.


```bash
docker run python-custom python hello.py
```

Il contenitore verrà avviato e restituirà il messaggio hello world. Il processo di Python viene quindi completato e il contenitore viene arrestato.

```bash
Hello World!
```

## <a name="automated-image-creation"></a>Creazione di un'immagine automatizzata

Nell'ultima sezione è stata creata manualmente un'immagine del contenitore. Questo metodo funziona perfettamente per la creazione occasionale o pratica di immagini, ma non è sostenibile in un ambiente di produzione. La creazione di un'immagine del contenitore può essere automatizzata usando un *Dockerfile* e il comando `docker build`. Il comando *docker build* esegue, in pratica, le istruzioni trovate nel *Dockerfile* e quindi acquisisce i risultati in una nuova immagine del contenitore.

Creare un file denominato *Dockerfile* e immettere il testo seguente:

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

L'istruzione *FROM* specifica l'immagine di base da usare durante le creazioni delle immagini. L'istruzione *RUN* esegue i comandi all'interno del contenitore. *RUN* può essere usata per installare software, apportare modifiche alla configurazione e pulire il contenitore prima dell'evento di acquisizione. L'istruzione *CMD* specifica il processo da eseguire quando viene avviato un contenitore.

Usare il comando `docker build` per creare una nuova immagine del contenitore con le istruzioni specificate nel Dockerfile.

```bash
docker build -t python-dockerfile .
```

L'output dovrebbe essere simile al seguente.

```bash
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

Usare il comando `docker images` per restituire un elenco di immagini dei contenitori.

```bash
docker images
```

Si noterà ora l'immagine personalizzata.

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

Usare il comando `docker run` per eseguire un contenitore dall'immagine personalizzata.

Si noti che non sono stati forniti argomenti al comando `docker run`. A differenza di quando si crea manualmente un'immagine del contenitore, un Dockerfile consente di includere un comando da eseguire all'avvio del contenitore. In questo caso, il comando specificato è `python hello.py`, che fa eseguire al contenitore lo script Python.

```bash
docker run python-dockerfile
```

Dopo aver eseguito il comando, verrà visualizzato l'output del contenitore.

```bash
Hello World!
```

## <a name="push-the-image-to-a-public-registry"></a>Eseguire il push dell'immagine in un registro pubblico

Docker Hub è un registro contenitori pubblico. Per eseguire il push delle immagini dei contenitori in Docker Hub, è necessario un account Docker. Se necessario, visitare il [sito di Docker Hub](https://hub.docker.com/) per eseguire la registrazione per un account.

Dopo aver creato un account Docker Hub, è necessario applicare un tag con il nome dell'account all'immagine del contenitore. A questo scopo, usare il comando `docker tag`.

Nell'esempio seguente all'immagine *python-dockerfile* viene applicato un tag con un nome dell'account Docker Hub. Sostituire `<account name>` con i nome dell'account Docker Hub.

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

Assicurarsi quindi di essere connessi a Docker Hub usando il comando `docker login`.

```bash
docker login
```

Eseguire infine il push dell'immagine *python-dockerfile* in Docker Hub con il comando `docker push`. Sostituire `<account name>` con i nome dell'account Docker Hub.

```bash
docker push <account name>/python-dockerfile
```

Mentre l'immagine del contenitore viene caricata in Docker Hub, verrà visualizzato un output simile al seguente:

```bash
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
```

L'immagine del contenitore è ora archiviata in Docker Hub ed è accessibile da qualsiasi computer connesso a Internet tramite `docker pull` o `docker run`.

## <a name="summary"></a>Riepilogo

In questa unità sono state create due immagini dei contenitori. La prima è stata creata manualmente e la seconda è stata automatizzata usando un Dockerfile.
