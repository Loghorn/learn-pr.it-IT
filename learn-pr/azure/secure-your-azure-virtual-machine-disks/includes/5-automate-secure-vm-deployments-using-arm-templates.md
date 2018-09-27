Si supponga che l'azienda voglia distribuire numerosi server come parte della transizione al cloud. I dischi delle macchine virtuali devono essere crittografati durante la distribuzione, per evitare periodi di vulnerabilità dei dischi stessi. Si vuole automatizzare questo processo ed è necessario modificare i modelli di Azure Resource Manager (ARM) per abilitare automaticamente la crittografia.

In questo caso si illustrerà come usare un modello di Azure Resource Manager per abilitare automaticamente la crittografia per le nuove macchine virtuali Windows.

## <a name="what-are-azure-resource-manager-templates"></a>Cosa sono i modelli di Azure Resource Manager?

I modelli di Azure Resource Manager sono file JSON usati per definire un set di risorse da distribuire in Azure. È possibile scriverli da zero e per alcune risorse di Azure, incluse le macchine virtuali, è possibile generarli tramite il portale di Azure. Sarà necessario immettere le informazioni necessarie per una distribuzione di macchina virtuale manuale, ma anziché distribuire la macchina virtuale in Azure, si salverà il modello. È quindi possibile _riusare_ il modello per creare la configurazione di macchina virtuale specifica.

Sono disponibili [modelli di esempio in docs](https://azure.microsoft.com/resources/templates) per automatizzare tutti i tipi di attività amministrative. Avremmo infatti potuto usare uno di questi modelli per crittografare la macchina virtuale che abbiamo appena creato manualmente.

![Screenshot che mostra i modelli di Azure](../media/5-browse-templates.png)

## <a name="using-github-templates"></a>Uso dei modelli di GitHub

L'origine del modello viene archiviata in GitHub. È possibile passare a un modello in GitHub e distribuirlo direttamente in Azure dalla stessa pagina.

![Screenshot che mostra il modello GitHub con il pulsante Distribuisci in Azure evidenziato](../media/5-deploy-from-github.png)

Quando viene distribuito il modello, Azure visualizza l'elenco dei campi di input obbligatori.

![Screenshot del modello nel portale di Azure](../media/5-fill-in-template.png)

È quindi possibile eseguire il modello per creare, modificare o rimuovere le risorse.

### <a name="running-templates-in-the-azure-portal"></a>Esecuzione di modelli nel portale di Azure

Se si conosce già il modello che si desidera utilizzare oppure se ci si sono modelli salvati nell'account di Azure, è possibile usare la risorsa **Crea una risorsa** > **Distribuzione modello** per individuare ed eseguire i modelli definiti nel portale. È possibile eseguire ricerche nei modelli in base al nome, modificare un modello per cambiare i parametri o il comportamento ed eseguire il modello direttamente dall'interfaccia utente grafica.

### <a name="running-templates-from-the-command-line"></a>Esecuzione di modelli dalla riga di comando

Quando a un modello viene assegnato un URL, il modello può essere eseguito con Azure PowerShell. Ad esempio, si potrebbe eseguire il modello di crittografia dischi con il comando PowerShell seguente:

```powershell
New-AzureRmResourceGroupDeployment `
    -Name encrypt-disk `
    -ResourceGroupName <resource-group-name> `
    -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

In alternativa, se i preferisce l'interfaccia della riga di comando di Azure, con il comando `group deployment create`.

```azurecli
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> \ 
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

