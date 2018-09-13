In questo modulo si è visto come le code negli account di archiviazione di Azure vengono usate per passare messaggi tra i componenti in un'applicazione distribuita. L'uso delle code in questo modo può contribuire a rendere un'applicazione distribuita più affidabile e resiliente agli errori e ai periodi di domanda elevata. Se si usa la libreria client Archiviazione di Microsoft Azure per .NET, è possibile scrivere facilmente codice C# o VB.NET per creare code, aggiungere messaggi o recuperare e rimuovere i messaggi dalle code.

## <a name="clean-up-the-resources"></a>Pulire le risorse

Gli account di archiviazione che contengono dati possono generare costi in relazione alla sottoscrizione ad Azure. La quantità di dati usati è minima, per cui i costi non dovrebbero essere ingenti; tuttavia, è sempre bene rimuovere tutte le risorse.

Poiché tutte le risorse sono state create nello stesso gruppo di risorse, il modo più semplice per pulire la sottoscrizione di Azure consiste nel rimuovere il gruppo di risorse. Verranno così eliminati tutti i contenuti:

```azurecli
az group delete --name ExerciseResources --yes --no-wait
```

L'opzione `--no-wait` permette di passare al modulo successivo senza attendere il completamento del comando.