Con Azure Container Registry Build (ACR Build) è possibile creare immagini del contenitore nel cloud, senza bisogno di strumenti Docker locali. ACR Build consente inoltre l'integrazione dei processi DevOps con compilazione automatizzata al commit del codice sorgente.

In questa unità si automatizza la creazione di un'immagine del contenitore usando ACR Build.

## <a name="create-a-container-image-with-build"></a>Creare un'immagine del contenitore con ACR Build

Quando si usa ACR Build, per fornire le istruzioni di compilazione viene utilizzato un Dockerfile standard. Con ACR Build si può usare qualsiasi Dockerfile attualmente in uso nell'ambiente, incluse le compilazioni in più fasi.

Creare un file denominato `Dockerfile` e aprirlo con qualsiasi editor di testo.

```bash
code Dockerfile
```

Copiare il contenuto seguente, salvare il file e uscire dall'editor di testo. Questo Dockerfile aggiunge un'applicazione Node.js all'immagine `node:9-alpine` e configura il contenitore per fungere da server per l'applicazione sulla porta 80.

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

Eseguire il comando seguente per compilare l'immagine del contenitore dal Dockerfile.

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

Durante l'esecuzione di questa operazione, si vedrà che l'immagine viene compilata e ne viene eseguito il push nel registro contenitori.

## <a name="verify-the-image"></a>Verificare l'immagine

Al termine dell'operazione di compilazione, eseguire il comando seguente per verificare che l'immagine sia stata creata e archiviata nel registro.

```azurecli
az acr repository list --name <acrName> --output table
```

L'output dovrebbe essere simile al seguente.

```console
Result
-------------
helloacrbuild
```

L'immagine `helloacrbuild` è ora pronta per l'utilizzo.

## <a name="summary"></a>Riepilogo

In questa unità è stato usato ACR Build per automatizzare la creazione di un'immagine del contenitore. L'immagine è stata quindi archiviata nel Registro contenitori di Azure. Nella prossima unità si distribuirà un'istanza della nuova immagine del contenitore in Istanze di contenitore di Azure.