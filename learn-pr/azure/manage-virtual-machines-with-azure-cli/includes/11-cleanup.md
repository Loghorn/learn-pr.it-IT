Si è creata una nuova macchina virtuale Linux e se ne è modificata la dimensione, quindi si è arrestata e avviata la macchina e si è aggiornata la configurazione con l'interfaccia della riga di comando di Azure.

## <a name="cleanup"></a>Pulizia

Ora che le procedure sono state completate e la macchina virtuale non è più necessaria, è possibile eliminare le risorse. La pulizia è importante per le risorse di calcolo e archiviazione il cui costo può continuare a essere addebitato in base all'account. 

È possibile eliminare singole risorse con il comando `delete`, ma il modo più semplice per rimuovere tutte le risorse consiste nell'uso di `az group delete`. Eseguire il comando seguente nell'interfaccia della riga di comando di Azure:

```azurecli
az group delete --name ExerciseResources --no-wait
```

Rispondere "yes" alla richiesta di conferma visualizzata oppure usare il parametro `--yes` per impostare la risposta automatica.

Questo comando elimina tutte le risorse associate al gruppo e le dealloca nell'ordine corretto. Il parametro `--no-wait` impedisce all'interfaccia della riga di comando di Azure di bloccarsi durante l'eliminazione. Lasciarlo disattivato per attendere l'eliminazione delle risorse. In alternativa, usare il comando `az group wait -n ExerciseResources --deleted` più avanti nello script per attendere che l'eliminazione sia completata.


## <a name="further-reading"></a>Altre informazioni

- [Panoramica dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Riferimento ai comandi dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
