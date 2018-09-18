È stato illustrato come usare un servizio di bilanciamento del carico nell'ambito di una soluzione per garantire la disponibilità elevata delle macchine virtuali ospitate in Azure. Sono stati esaminati il servizio di bilanciamento del carico, le reti virtuali associate, le regole per controllare l'algoritmo di bilanciamento e i probe di integrità che identificano le macchine virtuali correttamente in esecuzione.

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox?--->

L'esecuzione di macchine virtuali e set di scalabilità comporta costi per la sottoscrizione. È consigliabile rimuovere le risorse non necessarie per evitare addebiti superflui. Il modo più semplice per eseguire la pulizia della sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse, rimuovendo così tutte le risorse nel gruppo. Dopo aver completato questo modulo, eseguire questo cmdlet di Azure PowerShell:

```powershell
Remove-AzureRmResourceGroup -Name woodgrove-RG
```

Quando viene richiesto di confermare l'eliminazione, rispondere **Yes**. Il completamento di questo comando può richiedere alcuni minuti mentre vengono eliminate le risorse.