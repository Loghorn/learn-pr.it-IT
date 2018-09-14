In questo modulo si è visto come le code negli account di archiviazione di Azure vengono usate per passare messaggi tra i componenti in un'applicazione distribuita. L'uso delle code in questo modo può contribuire a rendere un'applicazione distribuita più affidabile e resiliente agli errori e ai periodi di domanda elevata. Se si usa la libreria client Archiviazione di Microsoft Azure per .NET, è possibile scrivere facilmente codice C# o VB.NET per creare code, aggiungere messaggi o recuperare e rimuovere i messaggi dalle code.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

Quando si lavora nella propria sottoscrizione, è possibile eseguire il comando di Azure per eliminare il gruppo di risorse e tutte le risorse associate.

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

L'opzione facoltativa `--no-wait` opzione indica la shell non attendere per Azure completare l'operazione.