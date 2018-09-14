Si è creata una nuova macchina virtuale Linux e se ne è modificata la dimensione, quindi si è arrestata e avviata la macchina e si è aggiornata la configurazione con l'interfaccia della riga di comando di Azure.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

Quando si lavora nella propria sottoscrizione, è possibile eseguire il comando di Azure per eliminare il gruppo di risorse e tutte le risorse associate.

```azurecli
az group delete --name [resource-group-name] --no-wait
```

Rispondere "yes" alla richiesta di conferma visualizzata oppure usare il parametro `--yes` per impostare la risposta automatica.

Questo comando elimina tutte le risorse associate al gruppo e le dealloca nell'ordine corretto. Il parametro `--no-wait` impedisce all'interfaccia della riga di comando di Azure di bloccarsi durante l'eliminazione. Lasciarlo disattivato per attendere l'eliminazione delle risorse. In alternativa, usare il comando `az group wait -n [resource-group-name] --deleted` più avanti nello script per attendere che l'eliminazione sia completata.


## <a name="further-reading"></a>Altre informazioni

- [Panoramica dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Riferimento ai comandi dell'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
