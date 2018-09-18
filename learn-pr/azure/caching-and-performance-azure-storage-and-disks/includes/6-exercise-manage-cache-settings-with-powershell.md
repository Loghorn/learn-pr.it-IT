
Nell'esercizio precedente sono state eseguite le attività seguenti tramite il portale di Azure.

- Visualizzare lo stato della cache del disco del sistema operativo
- Modificare le impostazioni della cache del disco del sistema operativo
- Aggiungere un disco dati alla macchina virtuale
- Modificare il tipo di memorizzazione nella cache in un nuovo disco dati

Ora si farà pratica con queste operazioni in Azure PowerShell. Verrà usata la macchina virtuale creata nell'esercizio precedente. Per le operazioni in questo lab si presuppone che:

- La macchina virtuale esista già e sia chiamata **fotoshareVM**
- La macchina virtuale si trovi in un gruppo di risorse denominato **fotoshare-rg**

Se i nomi effettivi sono diversi, sostituire questi valori con i propri. 

Di seguito è riportato lo stato corrente dei dischi della macchina virtuale dall'ultimo esercizio. 

![Screenshot dei dischi dati e del sistema operativo, entrambi impostati per la memorizzazione nella cache di sola lettura.](../media-draft/disks-final-config-portal.PNG)

È stato usato il portale per impostare il campo ***MEMORIZZAZIONE NELLA CACHE DELL'HOST** per i dischi dati e del sistema operativo. Tenere presente questo stato iniziale mentre si esegue la procedura seguente. 

### <a name="set-up-some-variables"></a>Impostare alcune variabili
Per prima cosa, occorre archiviare alcuni nomi di risorse per poterli riusare in seguito.

Usare il terminale Azure Cloud Shell a destra per eseguire i comandi di PowerShell seguenti. 

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> Sarà necessario reimpostare queste variabili se la sessione di Cloud Shell scade. Pertanto, se possibile, eseguire interamente questo lab in una sola sessione. 

### <a name="get-info-about-our-vm"></a>Ottenere informazioni sulla macchina virtuale

Eseguire il comando seguente per ottenere le proprietà della macchina virtuale.
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
La risposta sarà archiviata nella variabile `$myVM`. È possibile eseguire il comando seguente per visualizzare le proprietà specificate.

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

Come illustrato nello screenshot seguente, questa macchina virtuale è effettivamente quella richiesta. Pertanto è possibile cominciare. 

![Console di PowerShell che mostra i risultati degli ultimi quattro comandi eseguiti.](../media-draft/ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a>Visualizzare lo stato della cache del disco del sistema operativo

È possibile controllare l'impostazione di memorizzazione nella cache tramite l'oggetto `StorageProfile` come indicato di seguito.

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
In questo esempio il valore corrente è **Nessuna**. Verrà ripristinato il valore predefinito per un disco del sistema operativo.

![Console di PowerShell che mostra il disco del sistema operativo con un valore di memorizzazione nella cache uguale a "Nessuna".](../media-draft/ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a>Modificare le impostazioni della cache del disco del sistema operativo

È possibile impostare il valore per il tipo di cache usando lo stesso oggetto StorageProfile` come indicato di seguito.
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

Questo comando viene eseguito rapidamente, il che significa che esegue un'operazione in locale. Il comando modifica semplicemente la proprietà dell'oggetto myVM. Come indicato nello screenshot seguente, se si aggiorna la variabile `$myVM`, il valore di memorizzazione nella cache non risulterà modificato nella macchina virtuale.

![Console di PowerShell che mostra che l'aggiornamento dell'oggetto "myVM" reimposta la memorizzazione nella cache su "nessuna" dal momento che la macchina virtuale non è stata effettivamente aggiornata.](../media-draft/ps-commands-2.PNG)

Per apportare la modifica nella macchina virtuale stessa, chiamare `Update-AzureRmVM` come indicato di seguito.

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Si noti che questa chiamata richiede alcuni minuti per il completamento. Ciò avviene perché è in corso l'aggiornamento della macchina virtuale effettiva e Azure riavvia la macchina virtuale per apportare la modifica.

![Console di PowerShell che mostra il disco del sistema operativo con un valore di memorizzazione nella cache uguale a "Nessuna".](../media-draft/ps-oscaching-rw.PNG)

Se si aggiorna nuovamente la variabile `$myVM`, si noterà la modifica nell'oggetto. Anche esaminando il disco nel portale, la modifica sarà visibile. Si passerà ora alla creazione di un nuovo disco dati.  

### <a name="list-data-disk-info"></a>Elencare le informazioni del disco dati

Per visualizzare i dischi dati disponibili nella macchina virtuale, eseguire il comando seguente. 

```powershell
$myVM.StorageProfile.DataDisks
```

Al momento è presente un solo disco dati. Il campo `Lun` è importante. È il numero di unità logica (**L****U****N**) univoco. Quando si aggiunge un altro disco dati, viene assegnato un valore `Lun` univoco. 

### <a name="add-a-new-data-disk-to-our-vm"></a>Aggiungere un nuovo disco dati alla macchina virtuale 

Per praticità, verrà archiviato il nuovo nome del disco.

```powershell
$newDiskName = "fotoshareVM-data2"
```

Eseguire il comando `Add-AzureRmVMDataDisk` seguente per definire un nuovo disco. 

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

A questo disco è stato assegnato un valore LUN uguale a 1, non essendo ancora preso. È stato definito il disco che si vuole creare, pertanto è il momento di eseguire `Update-AzureRmVM` per apportare la modifica effettiva. 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Esaminare nuovamente le informazioni sul disco dati.

```powershell
$myVM.StorageProfile.DataDisks
```

![Console di PowerShell che mostra i due dischi dati.](../media-draft/2-data-disks-part1.png)

Sono ora disponibili due dischi. Il nuovo disco ha un valore **LUN** pari a 1 e il valore predefinito per **Memorizzazione nella cache** è **Nessuna**. Questo valore verrà ora modificato.

### <a name="change-cache-settings-of-new-data-disk"></a>Modificare le impostazioni della cache del nuovo disco dati

Le proprietà di un disco dati della macchina virtuale possono essere modificate con il cmdlet `Set-AzureRmVMDataDisk`, come illustrato di seguito.

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

Come sempre, eseguire il commit delle modifiche con `Update-AzureRmVM`.

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Ecco una visualizzazione dal portale delle operazioni che sono state eseguite in questo esercizio. La macchina virtuale ha ora due dischi dati e sono state modificate tutte le impostazioni di **Memorizzazione nella cache dell'host**. Tutte queste operazioni sono state eseguite con pochi semplici comandi, direttamente da Azure PowerShell.

![Console di PowerShell che mostra i due dischi dati.](../media-draft/disks-final-config-portal2.png)
