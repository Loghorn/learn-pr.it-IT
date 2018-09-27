Il bilanciamento del carico prende il traffico Internet in ingresso e lo instrada. A questo punto sono necessarie alcune VM di Azure verso cui instradare il traffico. In questo esempio verrà usato Linux, ma la stessa identica tecnica funziona con qualsiasi immagine di macchina virtuale.

## <a name="create-a-set-of-vms"></a>Creare un set di macchine virtuali

Verranno create tre macchine virtuali che verranno raggruppate in un set di disponibilità con l'interfaccia della riga di comando di Azure. Si sarebbe potuto usare il portale per questa attività, ma poiché stiamo creando più risorse, è molto più semplice eseguire questa operazione con uno strumento di scripting.

Se si preferisce _davvero_ usare il portale di Azure, un approccio alternativo potrebbe consistere nell'usare un _modello_. Per distribuire le risorse sono disponibili istruzioni JSON. È possibile definire un modello per la macchina virtuale e quindi distribuire il modello più volte nel portale di Azure.

### <a name="create-some-azure-cli-defaults"></a>Creare impostazioni predefinite dell'interfaccia della riga di comando di Azure

1. Iniziare configurando alcune impostazioni predefinite nell'interfaccia della riga di comando di Azure. Ogni comando usato richiede un _gruppo di risorse_ ed eventualmente un _percorso_. Invece di digitare tutto ogni volta, è possibile impostare questi elementi come parametri predefiniti. Usare il gruppo di risorse creato in precedenza nel sandbox di Azure e lo stesso percorso selezionato per Azure Load Balancer. Come promemoria, di seguito sono elencati i percorsi disponibili:

    [!include[](../../../includes/azure-sandbox-regions-note.md)]

1. Digitare il comando seguente in Cloud Shell a destra, sostituendo il segnaposto `<location>` con il percorso usato per Azure Load Balancer.

    ```azurecli
    az configure --defaults group=<rgn>[sandbox Resource Group</rgn> location=<location>
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

### <a name="create-an-availability-set"></a>Creare un set di disponibilità

Creare prima di tutto un _set di disponibilità_.

Il set di disponibilità serve per raggruppare le macchine virtuali. Usare il comando `vm availability-set` per crearne uno. Per il nome usare **woodgrove-avail-set**.

```azurecli
az vm availability-set create --name woodgrove-avail-set
```

### <a name="create-a-configuration-script"></a>Creare uno script di configurazione

Usare la stessa configurazione di base per ogni VM.

    - Ubuntu Linux
    - Server Web nginx
    - Node.js
    - Sito Web Basic con una pagina singola per il test

La scelta del sistema operativo è basata sull'immagine che viene selezionata ma gli altri elementi di configurazione richiedono alcuni script. Creare un file locale in Cloud Shell per installare un server Web e configurarlo con un sito Basic.

1. Aprire l'editor di Cloud Shell digitando `code` nel terminale.

    ```bash
    code
    ```

    Si tratta di un editor predefinito, che è possibile usare per creare e modificare i file direttamente nell'ambiente di Cloud Shell. I file vengono salvati in Azure in un account di archiviazione creato quando è stato avviato l'ambiente di Cloud Shell.

1. Copiare lo script seguente e incollarlo nella finestra dell'editor.

    ```script
    #cloud-config
    package_upgrade: true
    packages:
      - nginx
      - nodejs
      - npm
    write_files:
      - owner: www-data:www-data
      - path: /etc/nginx/sites-available/default
        content: |
          server {
            listen 80;
            location / {
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection keep-alive;
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
          }
      - owner: azureuser:azureuser
      - path: /home/azureuser/myapp/index.js
        content: |
          var express = require('express')
          var app = express()
          var os = require('os');
          app.get('/', function (req, res) {
            res.send('Hello World from host ' + os.hostname() + '!')
          })
          app.listen(3000, function () {
            console.log('Hello world app listening on port 3000!')
          })
    runcmd:
      - service nginx restart
      - cd "/home/azureuser/myapp"
      - npm init
      - npm install express -y
      - nodejs index.js
    ```

1. Salvare il file e denominarlo **cloud-init.txt**. È possibile usare il menu a comparsa "..." nell'angolo in alto a destra dell'editor o usare <kbd>CTRL+S</kbd> in Windows e Linux e <kbd>Cmd+S</kbd> in macOS.

1. Uscire dall'editor: anche in questo caso è possibile usare il menu a comparsa "..." o il tasto di scelta rapida (<kbd>CTRL+Q</kbd>).

1. Il file deve trovarsi nel file system. Provare a usare il comando `ls` per elencare il contenuto della cartella. È anche possibile usare `cat` per il file per verificare il contenuto.

### <a name="create-the-virtual-machines"></a>Creare le macchine virtuali

Successivamente, creare tre macchine virtuali Ubuntu Linux con l'interfaccia della riga di comando di Azure. Non verranno illustrate tutte le opzioni qui, ma per altre informazioni sulla gestione delle VM con l'interfaccia della riga di comando, vedere il modulo **Gestire macchine virtuali con l'interfaccia della riga di comando di Azure**.

È anche necessario creare un'interfaccia di rete virtuale per ogni VM. È possibile crearle insieme usando un ciclo.

1. Usare il comando `vm-create` per creare tre macchine virtuali in un ciclo. È possibile usare il codice seguente per:
    - Creare una NIC denominata _woodgrove_NicX_ dove X è [1,2,3].
    - Creare una VM denominata _woodgrove-VMX_ dove X è [1,2,3].
    - Associare ogni NIC alla rete virtuale creata
    - Associare ogni VM alla NIC e al set di disponibilità.

    ```azurecli
    for i in `seq 1 3`; do

        az network nic create --name woodgrove-Nic$i \
            --vnet-name woodgrove-VNET \
            --subnet backendSubnet

        az vm create --name woodgrove-VM$i \
            --availability-set woodgrove-avail-set \
            --nics woodgrove-Nic$i \
            --image UbuntuLTS \
            --admin-username azureuser \
            --generate-ssh-keys \
            --custom-data cloud-init.txt \
            --no-wait
    done
    ```
    > [!TIP]
    > Il comando imposta il nome dell'amministratore su "azureuser", ma è possibile modificare questa impostazione in qualsiasi modo si voglia. È anche possibile specificare una password se si preferisce la combinazione nome utente/password piuttosto che le chiavi SSH.

1. L'esecuzione dell'operazione richiederà un po' di tempo e anche quando il ciclo è terminato, le VM non saranno disponibili. È possibile passare al portale di Azure e usare la schermata **Tutte le risorse** per visualizzare le risorse create.

1. È anche possibile usare il comando `vm list` per visualizzare quante macchine virtuali sono state distribuite.

    ```azurecli
    az vm list -o table
    ```

Successivamente verrà illustrato come configurare il bilanciamento del carico.