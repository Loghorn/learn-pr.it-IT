È stato sottovaluto il fatto che alcuni caricamenti possono avere dimensioni eccessive per i dischi di caricamento in possesso. Si decide quindi di raddoppiare lo spazio da 64 a 128 GB.

## <a name="resize-the-data-disk"></a>Ridimensionare il disco dati

Per ridimensionare un disco, è necessario l'ID o il nome. In questo caso si conosce già il nome (uploadDataDisk1). Ma nel caso in cui non lo si ricordasse o se è stato creato da qualcun altro, è possibile ottenere un elenco di dischi tramite il comando `az disk list`.

1. Per iniziare, ottenere un elenco dei dischi gestiti nel gruppo di risorse. Se si hanno più VM in un unico gruppo di risorse, l'elenco può includere altri dischi. Per questo esempio, dovrà essere presente solo il server Web.

    ```azurecli
    az disk list \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

1. Successivamente, arrestare e deallocare la VM usando `az vm deallocate`. 

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```
1. A questo punto è possibile ridimensionare il disco con il comando `az disk update`.

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
1. Dopo aver completato l'operazione di ridimensionamento, riavviare la macchina virtuale.

    ```azurecli
    az vm start --name support-web-vm01
    ```

## <a name="expand-the-disk-partition"></a>Espandere la partizione del disco

Il passaggio finale consiste nell'indicare al sistema operativo lo spazio disponibile. Come per i passaggi di partizionamento e formattazione svolti in precedenza, questo processo è identico per le espansioni dei dischi locali. 

1. In primo luogo, ottenere l'indirizzo IP pubblico della macchina virtuale. Dal momento che è stata riavviata, è probabile che sia cambiato. È possibile provare ora un approccio diverso e usare `az vm show` con un filtro per restituire l'indirizzo IP pubblico.

    > [!TIP]
    > Gli indirizzi IP sono dinamici per impostazione predefinita. DNS di Azure compenserà automaticamente il cambio di indirizzo IP oppure è possibile modificare il comportamento usando indirizzi IP statici.

    ```azurecli
    az vm show --name support-web-vm01 -d --query [publicIps] --o tsv
    ```
    
1. Connettersi tramite SSH nel computer Linux. È necessario fornire l'indirizzo IP corretto.

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. Smontare il disco. Ricordarsi che si tratta di `/dev/sdc1`.

    ```bash
    sudo umount /dev/sdc1
    ```

1. Avviare `parted` in una shell con privilegi elevati

    ```bash
    sudo parted /dev/sdc
    ```
    
1. Espandere la partizione con il comando `resizepart`. Immettere la partizione 1 e le nuove dimensioni (128 GB).

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [64GB]? 128GB
    ```

    > [!WARNING]
    > Prestare attenzione alle dimensioni. Il ridimensionamento della partizione consente anche di ridurla, con la possibilità di una perdita di dati.
    
1. Chiudere lo strumento digitando `quit`.

1. Lo strumento di partizionamento _rimonterà_ automaticamente l'unità. Smontarla nuovamente in modo da poterla formattare.

    ```bash
    sudo umount /dev/sdc1
    ```
    
1. Verificare la coerenza tra le partizioni con `e2fsck`. Questo passaggio è assolutamente necessario, ma è anche consigliabile ogni volta che si intende modificare le dimensioni in un volume del disco.

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. Ridimensionare il file system con `resize2fs`.

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. Infine, rimontare l'unità sul punto di montaggio.

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```

Per verificare che il disco del sistema operativo sia stato ridimensionato, usare `df -h`. Dovrebbe ora indicare che l'unità è di 128 GB.
