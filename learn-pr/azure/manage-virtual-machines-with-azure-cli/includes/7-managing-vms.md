Avvio e arresto sono tra le attività principali da eseguire con le macchine virtuali.

## <a name="stopping-a-vm"></a>Arresto di una macchina virtuale

È possibile arrestare una macchina virtuale in esecuzione con il comando `vm stop`. È necessario passare il nome e il gruppo di risorse oppure l'ID univoco della macchina virtuale:

```azurecli
az vm stop -n SampleVM -g <rgn>[sandbox resource group name]</rgn>
```

Per verificare che la macchina sia stata arrestata, è possibile eseguire il ping dell'indirizzo IP pubblico, usare `ssh` oppure il comando `vm get-instance-view`. Quest'ultimo approccio restituisce sostanzialmente gli stessi dati del comando `vm show` ma include informazioni sull'istanza. Per visualizzare lo stato di esecuzione corrente della macchina virtuale inserire il comando seguente in Azure Cloud Shell:

```azurecli
az vm get-instance-view -n SampleVM -g <rgn>[sandbox resource group name]</rgn> --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

Il comando dovrebbe restituire come risultato `VM stopped`.

## <a name="starting-a-vm"></a>Avvio di una macchina virtuale

È possibile eseguire l'operazione inversa con il comando `vm start`.

```azurecli
az vm start -n SampleVM -g <rgn>[sandbox resource group name]</rgn>
```

Questo comando avvia una macchina virtuale arrestata. Per verificare l'esecuzione, inviare la query `vm get-instance-view`, che dovrebbe restituire `VM running`.

## <a name="restarting-a-vm"></a>Riavvio di una macchina virtuale

Infine, se sono state apportate modifiche che richiedono un riavvio, è possibile riavviare una macchina virtuale usando il comando `vm restart`. È possibile aggiungere il flag `--no-wait` per tornare immediatamente all'interfaccia della riga di comando di Azure senza attendere il riavvio della macchina virtuale.

