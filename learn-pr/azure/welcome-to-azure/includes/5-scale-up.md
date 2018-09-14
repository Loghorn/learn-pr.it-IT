Il server web sia attivo e in esecuzione, ma si rende che la necessità di potenza di calcolo per rendere l'esperienza ottimale per gli utenti. Come è possibile prendere la macchina virtuale vengono eseguite più velocemente?

Nel data center, è possibile spostare il server web in hardware più potente per risolvere i problemi di prestazioni. Il problema è che necessario acquistare, installare in rack e il nuovo sistema di alimentazione. Con Azure, la risposta è molto più semplice.

A questo punto si verrà aumenta la macchina virtuale a una dimensione più potente. Prima di modificare le dimensioni della VM, è necessario arrestarlo o deallocarla.

In primo luogo, definiamo significa che la scala e ciò che accade quando si dealloca la macchina virtuale.

## <a name="what-is-scale"></a>Che cos'è la scala?

_Scalabilità_ si intende l'aggiunta della larghezza di banda di rete, memoria, archiviazione o potenza di calcolo per ottenere prestazioni migliori.  

Si è già i termini _aumentare_ e _scalare in orizzontale_.

Scalabilità verticale o la scalabilità verticale, significa che aumentare la memoria, archiviazione, o calcolo accendere una macchina virtuale esistente. Ad esempio, è possibile aggiungere ulteriore memoria a un server di database web o per accelerare l'operazione.

> [!TIP]
> È anche possibile _ridurle_ sistema se è necessario per la scalabilità verticale solo temporaneamente.

Scalabilità orizzontale o la scalabilità orizzontale, significa che alle macchine virtuali aggiuntive per l'applicazione. Ad esempio, si potrebbe creare molte macchine virtuali configurate in esattamente allo stesso modo e Usa un bilanciamento del carico per distribuire il lavoro tra di essi.

## <a name="what-is-deallocation"></a>Che cos'è la deallocazione?

Deallocazione è il processo che la macchina virtuale viene arrestata e rilascia le risorse di calcolo.

Quando Arresta il PC al lavoro o a casa, del sistema operativo dei programmi di chiude e quindi invia una notifica dell'hardware di gestione power per disattivare energia.

Deallocazione di una macchina virtuale è simile. Dopo che la macchina virtuale viene arrestata, Azure viene recuperato l'hardware usato per funzionare. I dischi dati e l'archiviazione rimangono invariate. Quando si avvia la macchina virtuale eseguire il backup, prelevare in cui è stata interrotta, proprio come con il PC.

Quando è stato deallocato, non vengono fatturate per le risorse di calcolo e rete che usa la macchina virtuale. Paghi per eventuali dischi associati a cui si trovano nello spazio di memorizzazione, ma il costo complessivo è molto inferiore rispetto a come sarebbe se la macchina virtuale in esecuzione.

In questo caso, si sarà deallocare la macchina virtuale brevemente in modo che è possibile ridimensionarlo. Ma è anche possibile deallocare le macchine virtuali per un periodo più lungo per risparmiare sui costi. Si supponga di avere una banca di macchine virtuali che usano per il test durante le ore lavorative. È possibile pianificare le macchine virtuali automaticamente da deallocare in notti e fine settimana. Se è necessario rimanere in ritardo, è possibile riavviarle manualmente.

## <a name="scale-up-your-vm"></a>Aumentare le prestazioni della macchina virtuale

È importante ricordare che è specificata la dimensione **Standard_DS2_v2** durante la creazione di una macchina virtuale. La macchina virtuale dispone attualmente di due CPU e 7 GB di memoria virtuale.

È possibile promuovere a quella successiva, **Standard_DS3_v2**. La macchina virtuale avrà quindi quattro CPU e 14 GB di memoria virtuale.

::: zone pivot="windows-cloud"

1. Da Cloud Shell, eseguire `az vm deallocate` per deallocare o arrestare, la macchina virtuale.

    ```azurecli
    az vm deallocate \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM
    ```
    Il processo richiede alcuni minuti.
1. Eseguire `az vm resize` per aumentare le dimensioni della VM per **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM \
      --size Standard_DS3_v2
    ```
    Il processo di aggiornamento richiede circa un minuto.
1. Eseguire `az vm start` per riavviare la macchina virtuale.

    ```azurecli
    az vm start \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM
    ```
    Il processo richiede circa un minuto.
1. Eseguire `az vm show` per verificare che la macchina virtuale sia in esecuzione la nuova dimensione.

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myWindowsVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Noterete che la nuova dimensione di macchina virtuale, **Standard_DS3_v2**.
    ```console
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Da Cloud Shell, eseguire `az vm deallocate` per deallocare o arrestare, la macchina virtuale.

    ```azurecli
    az vm deallocate \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM
    ```
    Il processo richiede alcuni minuti.
1. Eseguire `az vm resize` per aumentare le dimensioni della VM per **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM \
      --size Standard_DS3_v2
    ```
    Il processo di aggiornamento richiede circa un minuto.
1. Eseguire `az vm start` per riavviare la macchina virtuale.

    ```azurecli
    az vm start \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM
    ```
    Il processo richiede circa un minuto.
1. Eseguire `az vm show` per verificare che la macchina virtuale sia in esecuzione la nuova dimensione.

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myLinuxVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Noterete che la nuova dimensione di macchina virtuale, **Standard_DS3_v2**.
    ```console
    Standard_DS3_v2
    ```

::: zone-end

> [!NOTE]
> Per impostazione predefinita, Azure assegna un nuovo indirizzo IP pubblico alla macchina virtuale quando si arresta e riavviarlo. Ai fini dell'apprendimento in questo stato è OK. In pratica, è possibile riservare un indirizzo IP pubblico che rimane con la macchina virtuale anche quando la macchina virtuale viene riavviata.

## <a name="summary"></a>Riepilogo

Ottimo lavoro! Con alcuni comandi, la macchina virtuale è due volte potente.

Scalabilità verticale e orizzontale in due modi per migliorare le prestazioni. Qui è aumentato di una macchina virtuale per aumentare la potenza di calcolo.

Deallocare una macchina virtuale prima che è possibile ridimensionarlo. È anche possibile deallocare le macchine virtuali quando non si trovano in uso per ridurre i costi.