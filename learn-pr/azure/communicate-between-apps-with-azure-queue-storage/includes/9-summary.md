In questo modulo si è visto come le code negli account di archiviazione di Azure vengono usate per passare messaggi tra i componenti in un'applicazione distribuita. L'uso delle code in questo modo può contribuire a rendere un'applicazione distribuita più affidabile e resiliente agli errori e ai periodi di domanda elevata. Se si usa la libreria client di Archiviazione di Microsoft Azure per .NET, è possibile scrivere facilmente codice C# o VB.NET per creare code, aggiungere messaggi o recuperare e rimuovere i messaggi nelle code.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

Quando si usa la propria sottoscrizione, è possibile eseguire il comando seguente dell'interfaccia della riga di comando di Azure per eliminare il gruppo di risorse e tutte le risorse associate.

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

L'opzione facoltativa `--no-wait` indica alla shell di non attendere che Azure completi l'operazione.