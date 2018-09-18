In questo modulo è stata illustrata la memorizzazione nella cache del disco di Azure e si è appreso come può migliorare potenzialmente le prestazioni. Per gestire la memorizzazione nella cache del disco per la macchina virtuale sono stati usati il portale di Azure e Azure PowerShell. 

Dopo che è stata adottata la strategia di memorizzazione nella cache del disco della macchina virtuale di Azure, è possibile distribuire in modo semplice e rapido le nuove macchine virtuali e il disco con le impostazioni ottimali della cache su disco usando gli script e i modelli.

## <a name="further-reading"></a>Altre informazioni

- [Archiviazione Premium di Azure: progettata per prestazioni elevate](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance)
- [Guida introduttiva ad Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.8.1)
- [Informazioni di riferimento sui cmdlet del computer di Azure](https://docs.microsoft.com/powershell/module/azurerm.compute/?view=azurermps-6.8.1#vm_disks)


## <a name="cleanup"></a>Pulizia
<!---TODO: Update for sandbox?--->

L'esecuzione delle macchine virtuali di Azure comporta costi per la sottoscrizione. Rimuovere le risorse non necessarie per evitare inutili addebiti. Il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. L'eliminazione di un gruppo di risorse determina l'eliminazione di tutte le risorse in esso contenute. Dopo aver completato questo modulo, eseguire il cmdlet seguente di Azure PowerShell:

    ```powershell
    Remove-AzureRmResourceGroup -Name "fotoshare-rg"
    ```

Quando viene richiesto di confermare l'eliminazione, rispondere **Sì**. Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse.
