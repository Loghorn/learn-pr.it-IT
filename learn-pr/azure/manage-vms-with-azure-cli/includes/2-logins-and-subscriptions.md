Prima di iniziare, è consigliabile ripassare la sintassi dello strumento dell'interfaccia della riga di comando di Azure. Se è stato seguito il modulo **Controllare i servizi di Azure con l'interfaccia della riga di comando di Azure**, si sa che i comandi dell'interfaccia hanno la forma di:

```azurecli
az [command] [subcommand] [--parameter --parameter]
```

`[command]` identifica l'area specifica di Azure che si desidera controllare. Ad esempio, è possibile gestire le informazioni della sottoscrizione con il comando `account` oppure i database SQL con il comando `sql`. `[subcommand]` e `[--parameters]` dipendono quindi dal comando con cui si sta lavorando. 

È possibile visualizzare un elenco di comandi, sottocomandi e parametri digitando un comando parziale. Ad esempio, se si digita `az` dalla riga di comando verrà visualizzata la schermata di aiuto principale; digitando `az vm` si otterranno invece tutti i sottocomandi per le macchine virtuali. Questo approccio può essere un ottimo modo per esplorare lo strumento dell'interfaccia della riga di comando di Azure.

> [!NOTE]
> Si userà Azure Cloud Shell ospitata nel browser per lavorare con l'interfaccia della riga di comando di Azure. Se si preferisce lavorare dalla macchina locale, è possibile eseguire tutti i comandi menzionati anche dalla riga di comando [installando l'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="log-in-to-azure"></a>Accedere ad Azure

La prima cosa da fare quando si lavora con l'interfaccia della riga di comando di Azure è accedere al proprio account di Azure. Questa operazione viene eseguita con il comando `login`. Se si usa Cloud Shell, sarà presente un pulsante per accedere ad Azure.

```azurecli
az login
```

Questo comando avvia una finestra del browser e consente di selezionare l'account Microsoft da usare.

## <a name="working-with-subscriptions"></a>Uso di più sottoscrizioni

In questo modulo si lavorerà in una sottoscrizione temporanea creata come ambiente di prova. Solitamente, invece, l'utente userà una sottoscrizione presente nel proprio account. Se si dispone di più sottoscrizioni, è possibile ottenere un relativo elenco formattato in modo chiaro usando l'istruzione `az account list --output table`.

```
Name                                  CloudName    SubscriptionId                        State    IsDefault
------------------------------------  -----------  ------------------------------------  -------  -----------
Contoso Legacy Resources              AzureCloud   abc13b0c-d2c4-64b2-9ac5-2f4cb819b752  Enabled  True
Visual Studio Enterprise              AzureCloud   233aebce-23c2-4572-c056-c029449e93ed  Enabled  False
```

Si noti che il comando identifica anche la sottoscrizione _predefinita_ in cui verranno applicati tutti i comandi. Se si preferisce lavorare in una sottoscrizione diversa, è possibile usare il comando `az account set --subscription "[name]"`. Ad esempio, è possibile impostare la sottoscrizione corrente come `Visual Studio Enterprise` dall'elenco precedente tramite il comando seguente:

```azurecli
az account set --subscription "Visual Studio Enterprise"
```