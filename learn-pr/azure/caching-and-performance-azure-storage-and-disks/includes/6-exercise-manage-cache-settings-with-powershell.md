Nell'esercizio precedente, abbiamo eseguito le attività seguenti usando il portale di Azure:

- Stato di visualizzazione del sistema operativo del disco della cache
- Modificare le impostazioni della cache del disco del sistema operativo
- Aggiungere un disco dati alla macchina virtuale
- Modifica tipo di memorizzazione nella cache in un nuovo disco dati

È possibile applicare queste operazioni usando Azure PowerShell. Si userà la macchina virtuale create nell'esercizio precedente. Si supponga che le operazioni in questo laboratorio:

- La macchina virtuale esista e viene chiamata **fotoshareVM**
- La macchina virtuale si trova in un gruppo di risorse denominato  **<rgn>[nome gruppo di risorse di tipo Sandbox]</rgn>**

Se l'utente ha eseguito con un diverso set di nomi, sostituire semplicemente questi valori con i propri.

Di seguito è riportato lo stato corrente dei nostri dischi delle macchine Virtuali nell'ultimo esercizio:

![Screenshot del nostro sistema operativo e dischi dati, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media/disks-final-config-portal.PNG)

È stato usato il portale per impostare il **memorizzazione nella cache HOST** campo per i dischi del sistema operativo sia i dati. Tenere presente questo stato iniziale mentre si procede alla seguente procedura.

### <a name="set-up-some-variables"></a>Impostare alcune variabili

In primo luogo, è possibile archiviare alcuni nomi di risorse in modo che è possibile usarle in un secondo momento.

Usare il terminale Azure Cloud Shell a destra per eseguire i comandi di PowerShell seguenti:

> [!NOTE]
> Opzione della sessione Cloud Shell **PowerShell** prima di provare questi comandi, se non è già.

```powershell
$myRgName = "<rgn>[Sandbox resource group name]</rgn>"
$myVMName = "fotoshareVM"
```

> [!TIP]
> È possibile impostare queste variabili di nuovo se scade la sessione Cloud Shell. Pertanto, se possibile, funzionano tramite questa esercitazione completa in una singola sessione.

### <a name="get-info-about-our-vm"></a>Ottenere informazioni sulle VM

Eseguire il comando seguente per ottenere le proprietà della macchina virtuale:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
```

Archiviamo la risposta nel nostro `$myVM` variabile. È possibile eseguire il comando seguente per farti vedere le proprietà che si specifica qui:

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

Come illustrato nella schermata seguente, questa macchina virtuale è effettivamente la VM si intende eseguire. Pertanto, è possibile passare.

![Screenshot della console di PowerShell di Azure che mostra i risultati di comandi ultime quattro.](../media/6-ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a>Stato di visualizzazione del sistema operativo del disco della cache

È possibile controllare l'impostazione di memorizzazione nella cache tramite il `StorageProfile` dell'oggetto, come indicato di seguito:

```powershell
$myVM.StorageProfile.OsDisk.Caching
```

In questo esempio, il valore corrente è `None`. È possibile modificarlo nuovamente allo stato predefinito per un disco del sistema operativo.

![Screenshot della console di PowerShell di Azure che mostra il disco del sistema operativo con un valore di memorizzazione nella cache "None".](../media/6-ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a>Modificare le impostazioni della cache del disco del sistema operativo

È possibile impostare il valore per il tipo di cache usando la stessa `StorageProfile` dell'oggetto, come indicato di seguito:

```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

Questo comando esegue veloci e che deve informare l'utente che sta eseguendo un'operazione in locale. Il comando Cambia semplicemente la proprietà sul `myVM` oggetto. Come mostrato nella schermata seguente, se si aggiorna il `$myVM` variabile, il valore di memorizzazione nella cache non sono state modificate nella macchina virtuale:

![Screenshot della console di PowerShell di Azure che illustra che l'aggiornamento l'oggetto "myVM" Reimposta la memorizzazione nella cache su "none" perché non si aggiorna la macchina virtuale.](../media/6-ps-commands-2.PNG)

Per apportare la modifica nella VM stessa, chiamare `Update-AzureRmVM`, come indicato di seguito:

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Si noti che questa chiamata richiede alcuni minuti. Ciò avviene perché stiamo aggiornando la VM effettiva e Azure riavvia la macchina virtuale per apportare la modifica.

![Screenshot della console di PowerShell di Azure che mostra il disco del sistema operativo con un valore di memorizzazione nella cache "None".](../media/6-ps-oscaching-rw.PNG)

Se si aggiorna il `$myVM` variabile anche in questo caso, si noterà la modifica nell'oggetto. Esaminando il disco nel portale, si sarebbe vedere anche la modifica non esiste. È possibile passare alla creazione di un nuovo disco dati.

### <a name="list-data-disk-info"></a>Informazioni del disco dati elenco

Per visualizzare i dischi dati sono disponibili per la macchina virtuale, eseguire il comando seguente:

```powershell
$myVM.StorageProfile.DataDisks
```

Nel momento in cui è disponibile solo un disco dati. Il `Lun` campo è importante. L'univocità **L**ogico **U**nit **N**umero. Quando si aggiunge un altro disco dati, viene assegnato un valore univoco `Lun` valore.

### <a name="add-a-new-data-disk-to-our-vm"></a>Aggiungere un nuovo disco dati alla VM

Per praticità, verrà archiviato il nuovo nome del disco:

```powershell
$newDiskName = "fotoshareVM-data2"
```

Eseguire il comando seguente `Add-AzureRmVMDataDisk` comando per definire un nuovo disco:

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

Ti forniamo tale disco un' `Lun` pari a `1` perché non viene preso. È stato definito il disco si creeranno, pertanto è possibile eseguire `Update-AzureRmVM` apportare la modifica effettiva:

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Esaminiamo le informazioni del disco i dati anche in questo caso:

```powershell
$myVM.StorageProfile.DataDisks
```

![Screenshot della console di PowerShell di Azure che mostra i due dischi dati.](../media/2-data-disks-part1.png)

Ora abbiamo due dischi. Il nuovo disco è un `Lun` dei `1` e il valore predefinito per `Caching` è `None`. È possibile modificare tale valore.

### <a name="change-cache-settings-of-new-data-disk"></a>Modificare le impostazioni della cache del nuovo disco dati

È modificare le proprietà di un disco dati della macchina virtuale con il `Set-AzureRmVMDataDisk` cmdlet, come indicato di seguito:

```powershell
Set-AzureRmVMDataDisk -VM $myVM -Lun "1" -Caching ReadWrite
```

Come sempre, eseguire il commit delle modifiche con `Update-AzureRmVM`:

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Ecco una visualizzazione dal portale di ciò che è stata eseguita in questo esercizio. La macchina virtuale ha ora due dischi dati e si abbiamo modificato tutti **memorizzazione nella cache HOST** impostazioni. Abbiamo fatto tutto ciò con alcuni comandi. Che sarà la potenza di Azure PowerShell.

![Screenshot del portale di Azure che illustra la sezione dischi del nostro pannello macchina virtuale con due dischi dati.](../media/disks-final-config-portal2.png)
