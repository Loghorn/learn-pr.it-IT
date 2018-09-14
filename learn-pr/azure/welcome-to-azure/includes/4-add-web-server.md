Ora che la macchina virtuale sia in esecuzione, è possibile eseguire un'operazione con esso. Di seguito verrà installato un server web e fornire una pagina web di base che visualizza il nome host della macchina virtuale.

Per configurare una macchina virtuale, sono disponibili diverse opzioni. È possibile connettersi direttamente e configurare in modo interattivo il sistema. Nei sistemi Windows, ad esempio, è possibile creare una sessione di Desktop remoto per connettersi all'interfaccia utente del computer Windows remoti come se sono seduti. Nei sistemi Linux, è possibile creare una connessione SSH a funzionare in modo sicuro con il sistema di Linux remoto dal terminale.

Configurazione manuale è un buon punto di partenza, ma quando si aggiungono i sistemi, è possibile automatizzare le distribuzioni. Automazione prevede l'esecuzione di processi ripetibili, ad esempio programmi e script che gestiscono il lavoro sporco automaticamente.

::: zone pivot="windows-cloud"

In questo caso, si configurerà IIS in modalità remota dalla sessione di Cloud Shell usando la funzionalità basata su Windows macchine virtuali di Azure denominato l'estensione Script personalizzato.

::: zone-end

::: zone pivot="linux-cloud"

In questo caso, si configurerà Nginx in remoto dalla sessione di Cloud Shell usando la funzionalità basati su Linux, macchine virtuali di Azure denominato l'estensione Script personalizzato.

::: zone-end

::: zone pivot="windows-cloud"

## <a name="what-is-iis"></a>Che cos'è IIS?

Internet Information Services o IIS, è un server web che viene eseguito in Windows. È possibile utilizzare IIS per rendere disponibile il contenuto web standard (HTML, CSS e JavaScript) o eseguire altri tipi di applicazioni web e ASP.NET. IIS viene fornito con Windows Server, ma è necessario attivarlo per iniziare a servire le pagine web.

## <a name="whats-the-custom-script-extension"></a>Che cos'è l'estensione Script personalizzata?

L'estensione Script personalizzata è un modo semplice per scaricare ed eseguire script in macchine virtuali di Azure. È solo uno dei modi in cui che è possibile configurare la macchina virtuale dopo che la macchina virtuale sia in esecuzione.

È possibile archiviare gli script in archiviazione di Azure o in una posizione pubblica, ad esempio GitHub. È possibile eseguire gli script manualmente o come parte di una distribuzione più automatizzata. In questo caso, si eseguirà un comando di Azure per scaricare uno script di PowerShell da GitHub ed eseguirlo nella macchina virtuale. Lo script configura IIS.

## <a name="configure-iis"></a>Configurare IIS

In questo caso si userà l'estensione Script personalizzato per configurare IIS in modalità remota nella macchina virtuale da Cloud Shell. Inoltre, si configurerà il firewall per consentire l'accesso di rete in ingresso sulla porta 80 (HTTP).

1. Da Cloud Shell, eseguire questo `az vm extension set` comando per scaricare ed eseguire uno script di PowerShell che consente di installare IIS e configura una base di home page.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myWindowsVM \
      --name CustomScriptExtension \
      --publisher Microsoft.Compute \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1"]}' \
      --protected-settings '{"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File configure-iis.ps1"}'
    ```

    Il processo richiede un paio di minuti per il completamento.

    Nel frattempo, è possibile [esaminare lo script di PowerShell](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/69e5d9250b9dcd7e7eece4b0ea3c3a8cd1b4fcd7/configure-iis.ps1?azure-portal=true) da una scheda separata del browser se si desidera. Lo script installa IIS e configura la home page per visualizzare un messaggio di benvenuto insieme a nome computer della macchina virtuale, "myWindowsVM".

1. Eseguire questo `az vm open-port` comando per aprire la porta 80 (HTTP) attraverso il firewall.

    ```azurecli
    az vm open-port \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Verificare la configurazione

Ora che IIS è configurato, è possibile verificare che sia in esecuzione.

1. Eseguire questo `az vm list-ip-addresses` gli indirizzi di comando per elencare l'indirizzo IP pubblico della VM.

    ```azurecli
    az vm list-ip-addresses \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > Questo comando è piuttosto complesso, soprattutto `--query` argomento. Ma sarà un vero professionista questo Approfondisci le tue conoscenze ed esplorare Azure.

    Noterete che indirizzo IP pubblico della VM. Ecco un esempio.

    ```console
    104.211.9.245
    ```

1. In una nuova scheda del browser, passare all'indirizzo IP della VM. Viene visualizzato il messaggio di benvenuto e il nome della VM.

    ![](../media/iis-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## <a name="what-is-nginx"></a>Che cos'è Nginx?

Nginx (pronunciato "motore-x") è un server web più diffusi, gratuito, open source che viene eseguito in Windows, Linux, macOS e Unix. In questo caso si userà Nginx per servire una pagina web di base.

## <a name="whats-the-custom-script-extension"></a>Che cos'è l'estensione Script personalizzata?

L'estensione Script personalizzata è un modo semplice per scaricare ed eseguire script in macchine virtuali di Azure. È solo uno dei modi in cui che è possibile configurare la macchina virtuale dopo che la macchina virtuale sia in esecuzione.

È possibile archiviare gli script in archiviazione di Azure o in una posizione pubblica, ad esempio GitHub. È possibile eseguire gli script manualmente o come parte di una distribuzione più automatizzata. In questo caso, si eseguirà un comando di Azure per scaricare uno script Bash da GitHub ed eseguirlo nella macchina virtuale. Lo script configura Nginx.

## <a name="configure-nginx"></a>Configurare Nginx

In questo caso si userà l'estensione Script personalizzato per configurare in remoto Nginx nella macchina virtuale da Cloud Shell. Inoltre, si configurerà il firewall per consentire l'accesso di rete in ingresso sulla porta 80 (HTTP).

1. Da Cloud Shell, eseguire questo `az vm extension set` comando per scaricare ed eseguire uno script Bash che installa Nginx e configura una base di home page.

    ```azurecli
    az vm extension set \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --vm-name myLinuxVM \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
    ```

    Il processo richiede un paio di minuti per il completamento.

    Nel frattempo, è possibile [esaminare lo script Bash](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/20fca2517fa5913150abab66b51b0d88aa3077d8/configure-nginx.sh?azure-portal=true) da una scheda separata del browser se si desidera. Lo script installa Nginx e configura la home page per visualizzare un messaggio di benvenuto insieme a nome computer della macchina virtuale, "myLinuxVM".

1. Eseguire questo `az vm open-port` comando per aprire la porta 80 (HTTP) attraverso il firewall.

    ```azurecli
    az vm open-port \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --port 80
    ```

## <a name="verify-the-configuration"></a>Verificare la configurazione

Ora che Nginx è configurato, è possibile verificare che sia in esecuzione.

1. Eseguire questo `az vm list-ip-addresses` gli indirizzi di comando per elencare l'indirizzo IP pubblico della VM.

    ```azurecli
    az vm list-ip-addresses \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" \
      --output tsv
    ```

    > [!NOTE]
    > Questo comando è piuttosto complesso, soprattutto `--query` argomento. Ma sarà un vero professionista questo Approfondisci le tue conoscenze ed esplorare Azure.

    Noterete che indirizzo IP pubblico della VM. Ecco un esempio.

    ```console
    137.135.110.210
    ```

1. In una nuova scheda del browser, passare all'indirizzo IP della VM. Viene visualizzato il messaggio di benvenuto e il nome della VM.

    ![](../media/nginx-browser.png)

::: zone-end

## <a name="summary"></a>Riepilogo

La macchina virtuale è in esecuzione e può ora essere usato le pagine web, ma che cosa significa per te?

Tenere presente che ogni percorso inizia con le nozioni di base e quasi tutte le innovazioni eccezionali nate nel cloud, da aziende di ogni dimensione, a una configurazione simile a quella dell'utente. Evoluzione della tua idea, inizia a effettuare un impatto positivo sulla tua azienda e gli utenti.