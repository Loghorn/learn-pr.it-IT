Ora che la macchina virtuale è pronta e in esecuzione si vedrà come è possibile usarla. Di seguito verrà installato un server Web per visualizzare una semplice pagina Web che mostra il nome host della macchina virtuale.

Per configurare una macchina virtuale, sono disponibili diverse opzioni. È possibile connettersi direttamente e configurare in modo interattivo il sistema. Nei sistemi Windows, ad esempio, è possibile creare una sessione di Desktop remoto per connettersi all'interfaccia utente del computer Windows remoto, come se fosse a portata di mano. Nei sistemi Linux è possibile creare una connessione SSH per usare in modo sicuro il sistema Linux remoto dal terminale.

La configurazione manuale è un buon punto di partenza, ma è possibile automatizzare le distribuzioni man mano che si aggiungono sistemi. L'automazione prevede l'esecuzione di processi ripetibili, ad esempio programmi e script che gestiscono automaticamente le operazioni più ripetitive.

::: zone pivot="windows-cloud"

Qui si vedrà come configurare IIS in modalità remota dalla sessione di Cloud Shell usando una funzionalità delle macchine virtuali di Azure basata su Windows denominata estensione per script personalizzati.

::: zone-end

::: zone pivot="linux-cloud"

Qui si vedrà come configurare Nginx in modalità remota dalla sessione di Cloud Shell usando una funzionalità delle macchine virtuali di Azure basata su Linux denominata estensione per script personalizzati.

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a>Che cos'è IIS?

Internet Information Services, o IIS, è un server Web eseguito in Windows. È possibile usare IIS per rendere disponibile contenuto Web standard (HTML, CSS e JavaScript) o per eseguire ASP.NET o altri tipi di applicazioni Web. IIS è incluso in Windows Server, ma è necessario attivarlo per iniziare a servire pagine Web.

## <a name="whats-the-custom-script-extension"></a>Che cos'è l'estensione per script personalizzati?

L'estensione per script personalizzati è un modo semplice per scaricare ed eseguire script nelle macchine virtuali di Azure. È solo uno dei tanti modi in cui è possibile configurare il sistema dopo aver preparato e attivato la macchina virtuale.

È possibile archiviare gli script in Archiviazione di Azure o in una posizione pubblica, ad esempio GitHub. Gli script possono essere eseguiti manualmente o come parte di una distribuzione più automatizzata. In questo caso, si eseguirà un comando dell'interfaccia della riga di comando di Azure per scaricare uno script di PowerShell da GitHub ed eseguirlo nella macchina virtuale. Lo script configura IIS.

## <a name="configure-iis"></a>Configurare IIS

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

In questo caso si userà l'estensione per script personalizzati per configurare IIS in modalità remota nella macchina virtuale da Cloud Shell. Verrà anche configurato il firewall per consentire l'accesso di rete in ingresso sulla porta 80 (HTTP).

1. Da Cloud Shell, eseguire questo comando `az vm extension set` per scaricare ed eseguire uno script di PowerShell che installa IIS e configura una semplice home page.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    Il processo di configurazione di IIS, impostazione dei contenuti della home page e avvio del servizio richiede un paio di minuti per il completamento.

    Nel frattempo è possibile [esaminare lo script di PowerShell](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) in una scheda separata del browser. Lo script installa IIS e configura la home page per visualizzare un messaggio di benvenuto insieme al nome computer della macchina virtuale, "myVM".

1. Eseguire questo comando `az vm open-port` per aprire la porta 80 (HTTP) attraverso il firewall.

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Verificare la configurazione

Ora che IIS è configurato, verificare che sia in esecuzione.

1. Eseguire questo comando `az vm list-ip-addresses` per elencare gli indirizzi IP pubblici della macchina virtuale.

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > L'argomento `--query` rende leggermente complesso questo comando. Man mano che si esplora e si approfondisce Azure, diventerà più chiaro.

    Viene visualizzato l'indirizzo IP pubblico della macchina virtuale. Ecco un esempio.

    ```output
    104.211.9.245
    ```

1. In una nuova scheda del browser, passare all'indirizzo IP della macchina virtuale. Viene visualizzato il messaggio di benvenuto e il nome della macchina virtuale.

    ![](../media/4-iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a>Che cos'è Nginx?

Nginx (pronunciato "engine-x") è un server Web diffuso, gratuito e open source che può essere eseguito in Unix, Linux, macOS e Windows. In questo caso si userà Nginx per presentare una semplice pagina Web.

## <a name="whats-the-custom-script-extension"></a>Che cos'è l'estensione per script personalizzati?

L'estensione per script personalizzati è un modo semplice per scaricare ed eseguire script nelle macchine virtuali di Azure. È solo uno dei tanti modi in cui è possibile configurare il sistema dopo aver preparato e attivato la macchina virtuale.

È possibile archiviare gli script in Archiviazione di Azure o in una posizione pubblica, ad esempio GitHub. Gli script possono essere eseguiti manualmente o come parte di una distribuzione più automatizzata. In questo caso, si eseguirà un comando dell'interfaccia della riga di comando di Azure per scaricare uno script Bash da GitHub ed eseguirlo nella macchina virtuale. Lo script configura Nginx.

## <a name="configure-nginx"></a>Configurare Nginx

<!-- TODO: https://github.com/MicrosoftDocs/learn-pr/issues/1864 -->

In questo caso si userà l'estensione per script personalizzati per configurare Nginx in modalità remota nella macchina virtuale da Cloud Shell. Verrà anche configurato il firewall per consentire l'accesso di rete in ingresso sulla porta 80 (HTTP).

1. Da Cloud Shell, eseguire questo comando `az vm extension set` per scaricare ed eseguire uno script Bash che installa Nginx e configura una semplice home page.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --vm-name myVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    Il processo di configurazione di Nginx, impostazione dei contenuti della home page e avvio del servizio richiede un paio di minuti per il completamento.

    Nel frattempo è possibile [esaminare lo script Bash](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-nginx.sh?azure-portal=true) in una scheda separata del browser. Lo script installa Nginx e configura la home page per visualizzare un messaggio di benvenuto insieme al nome computer della macchina virtuale, "myVM".

1. Eseguire questo comando `az vm open-port` per aprire la porta 80 (HTTP) attraverso il firewall.

    ```azurecli
    az vm open-port \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Verificare la configurazione

Ora che Nginx è configurato, verificare che sia in esecuzione.

1. Eseguire questo comando `az vm list-ip-addresses` per elencare gli indirizzi IP pubblici della macchina virtuale.

    ```azurecli
    az vm list-ip-addresses \
      --name myVM \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > L'argomento `--query` rende leggermente complesso questo comando. Man mano che si esplora e si approfondisce Azure, diventerà più chiaro.

    Viene visualizzato l'indirizzo IP pubblico della macchina virtuale. Ecco un esempio.

    ```output
    137.135.110.210
    ```

1. In una nuova scheda del browser, passare all'indirizzo IP della macchina virtuale. Viene visualizzato il messaggio di benvenuto e il nome della macchina virtuale.

    ![](../media/4-nginx-browser.png)

::: zone-end

## <a name="summary"></a>Riepilogo

La macchina virtuale è in esecuzione e può ora servire pagine Web. E adesso?

Tenere presente che ogni percorso inizia dalla base e che quasi tutte le innovazioni eccezionali nate nel cloud, da aziende di ogni dimensione, sono partite da una configurazione simile a quella qui presentata. Man mano che evolvono, le idee inizieranno ad avere un impatto positivo sia sull'azienda che sugli utenti.