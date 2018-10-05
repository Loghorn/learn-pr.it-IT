Si supponga che l'azienda faccia uso di immagini del contenitore per gestire i carichi di lavoro di calcolo. Per compilare le immagini del contenitore vengono usati gli strumenti Docker locali.

È ora possibile usare Azure Container Registry Tasks per creare tali contenitori. Container Registry Tasks consente anche l'integrazione del processo DevOps con compilazione automatica al commit del codice sorgente.

È ora possibile automatizzare la creazione di un'immagine del contenitore usando Azure Container Registry Tasks.

## <a name="create-a-container-image-with-azure-container-registry-tasks"></a>Creare un'immagine del contenitore con Azure Container Registry Tasks

Per fornire le istruzioni di compilazione viene usato un Dockerfile standard. Azure Container Registry Tasks consente di riutilizzare qualsiasi Dockerfile attualmente disponibile nell'ambiente, incluse le compilazioni in più fasi.

Per l'esempio verrà usato un nuovo Dockerfile.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

Il primo passaggio consiste nel creare un nuovo file denominato `Dockerfile`. È possibile usare qualsiasi editor di testo per modificare il file. Per questo esempio si userà l'editor di Cloud Shell. Per aprire l'editor, immettere il comando seguente nella finestra di Cloud Shell.

```bash
code
```

Copiare i contenuti seguenti nel nuovo Dockerfile. Assicurarsi di salvare il file.

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

Per il salvataggio usare la combinazione di tasti <kbd>Ctrl+S</kbd>. Quando viene richiesto, assegnare al file il nome `Dockerfile`.

Questa configurazione aggiunge un'applicazione Node.js all'immagine `node:9-alpine`. Configura quindi il contenitore in modo che gestisca l'applicazione sulla porta 80 tramite l'istruzione *EXPOSE*.

Eseguire ora il comando dell'interfaccia della riga di comando di Azure `az acr build` per compilare l'immagine del contenitore da Dockerfile.

```azurecli
az acr build --registry <acrName> --image helloacrtasks:v1 .
```

Verrà mostrata la creazione dell'immagine e ne verrà eseguito il push nel registro contenitori durante l'esecuzione del comando.

## <a name="verify-the-image"></a>Verificare l'immagine

Eseguire il comando seguente per verificare che l'immagine sia stata creata e archiviata nel registro.

```azurecli
az acr repository list --name <acrName> --output table
```

L'output dovrebbe essere simile al seguente:

```console
Result
-------------
helloacrtasks
```

L'immagine `helloacrtasks` è ora pronta per l'uso.
