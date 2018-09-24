Il server Web è pronto e in esecuzione, ma ci si rende conto che è necessaria più potenza di calcolo per offrire un'esperienza ottimale agli utenti. Come è possibile velocizzare l'esecuzione della macchina virtuale?

Nel data center locale si potrebbe possibile spostare il server Web in hardware più potente per risolvere i problemi di prestazioni. Il problema è però la necessità di acquistare nuovi elementi per il sistema, installarli nel rack e alimentarli. Con Azure, la risposta è molto più semplice.

Prima di aumentare le prestazioni della macchina virtuale passando a una dimensione più potente, verrà esaminato il significato di scalabilità.

## <a name="what-is-scale"></a>Cosa è la scalabilità?

Il termine _scalabilità_ indica la possibilità di aggiungere larghezza di banda di rete, memoria, spazio di archiviazione o potenza di calcolo per ottenere prestazioni migliori.  

Probabilmente si è già sentito parlare di _scalabilità verticale_ e _scalabilità orizzontale_.

Con scalabilità verticale si intende l'aumento della memoria, dello spazio di archiviazione o della potenza di calcolo in una macchina virtuale esistente. Ad esempio, è possibile aggiungere ulteriore memoria a un server Web o di database per renderlo più veloce.

Con scalabilità orizzontale si intende l'aggiunta di macchine virtuali per eseguire l'applicazione. Ad esempio, si potrebbero creare molte macchine virtuali configurate esattamente allo stesso modo e usare il bilanciamento del carico per distribuire il lavoro tra di esse.

> [!TIP]
> Il cloud è elastico. È anche possibile _ridurre le prestazioni_ o _ridurre il numero di istanze_ della distribuzione, se l'aumento delle prestazioni o delle istanze è stato necessario solo temporaneamente. Riducendo le prestazioni o le istanze, è possibile risparmiare denaro.<br><br>**Azure Advisor** e **Gestione costi di Azure** sono due servizi che aiutano a ottimizzare la spesa per il cloud. È possibile usare questi servizi per identificare i casi in cui si stanno usando più risorse del necessario e quindi tornare alla capacità effettivamente usata.

## <a name="scale-up-your-vm"></a>Aumentare le prestazioni della macchina virtuale

Ricordarsi che è stata specificata la dimensione **Standard_DS2_v2** al momento della creazione della macchina virtuale. La macchina virtuale dispone attualmente di due CPU virtuali e 7 GB di memoria.

È possibile passare alla dimensione successiva, **Standard_DS3_v2**. La macchina virtuale disporrà quindi di quattro CPU virtuali e 14 GB di memoria.

::: zone pivot="windows-cloud"

1. In Cloud Shell eseguire `az vm resize` per aumentare le dimensioni della macchina virtuale a **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    Il processo di aggiornamento richiede circa un minuto. Durante questo processo, la macchina virtuale viene riavviata.

1. Eseguire `az vm show` per verificare che la macchina virtuale sia in esecuzione con la nuova dimensione.

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Verrà visualizzata la nuova dimensione della macchina virtuale: **Standard_DS3_v2**.
    ```output
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. In Cloud Shell eseguire `az vm resize` per aumentare le dimensioni della macchina virtuale a **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    Il processo di aggiornamento richiede circa un minuto. Durante questo processo, la macchina virtuale viene riavviata.

1. Eseguire `az vm show` per verificare che la macchina virtuale sia in esecuzione con la nuova dimensione.

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Verrà visualizzata la nuova dimensione della macchina virtuale: **Standard_DS3_v2**.
    ```output
    Standard_DS3_v2
    ```

::: zone-end

## <a name="summary"></a>Riepilogo

L'operazione è stata completata. Con un solo comando è stato possibile raddoppiare la potenza della macchina virtuale.

La scalabilità verticale e la scalabilità orizzontale sono due modi per migliorare le prestazioni. In questo modulo si è visto come ridimensionare la macchina virtuale per aumentarne la potenza di calcolo.