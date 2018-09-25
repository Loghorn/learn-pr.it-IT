A questo punto è possibile eseguire l'app in Azure. È necessario creare un'app del Servizio app di Azure, configurarla con un'identità gestita e la configurazione dell'insieme di credenziali e quindi distribuire il codice.

## <a name="create-the-app-service-plan-and-app"></a>Creare l'app e il piano di servizio app

La creazione di un'app di Servizio app è un processo che si svolge in due passaggi: per prima cosa, creare il *piano* e poi l'*app*.

Il nome del *piano* deve solamente essere univoco all'interno della sottoscrizione, quindi è possibile usare lo stesso nome usato da noi: **insieme di credenziali delle chiavi-esercizio-piano**. Il nome dell'app deve essere globalmente univoco, pertanto è necessario sceglierne uno proprio. Usare lo stesso percorso usato durante la creazione dell'insieme di credenziali.

In Azure Cloud Shell eseguire il comando seguente:

```azurecli
az appservice plan create \
    --name keyvault-exercise-plan \
    --resource-group <rgn>[sandbox resource group name]</rgn>
    --location eastus

az webapp create \
    --name <your-unique-app-name> \
    --plan keyvault-exercise-plan \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

## <a name="add-configuration-to-the-app"></a>Aggiungere una configurazione per l'app

Per la distribuzione in Azure si seguirà la procedura consigliata del servizio app per inserire la configurazione VaultName nelle impostazioni dell'applicazione, anziché in un file di configurazione. Eseguire questo comando per creare l'impostazione dell'applicazione:

```azurecli
az webapp config appsettings set \
    --name <your-unique-app-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a>Abilitare l'identità gestita

L'abilitazione dell'identità gestita in un'app è una singola riga di codice &mdash;. Eseguire questa riga per abilitarla nell'app:

```azurecli
az webapp identity assign \
    --name <your-unique-app-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

Nell'output JSON risultante copiare il valore di **principalId**. Proprietà PrincipalId è l'ID univoco della nuova identità dell'app in Azure Active Directory e dobbiamo usarlo nel passaggio successivo.

## <a name="grant-access-to-the-vault"></a>Concedere l'accesso all'insieme di credenziali

L'ultimo passaggio prima della distribuzione consiste nell'assegnare le autorizzazioni di Key Vault all'identità gestita dell'app. Usare il valore di **principalId** copiato dal passaggio precedente come valore per **object-id** nel comando seguente. L'esecuzione di questo comando concederà l'accesso **Get** e **List**:

```azurecli
az keyvault set-policy \
    --name <your-unique-vault-name> \
    --object-id <your-managed-identity-principleid> \
    --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a>Distribuire l'app e provarla

Tutte le configurazioni sono impostate e i può avviare la distribuzione. I comandi seguenti pubblicheranno il sito nella cartella `pub`, la comprimeranno in `site.zip` e quindi distribuiranno il file zip nel Servizio app.

> [!NOTE]
> Sarà necessario `cd` indietro alla directory di KeyVaultDemoApp, se non è già stato fatto.

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --name <your-unique-app-name> \
    --resource-group <rgn>[sandbox resource group name]</rgn>
```

Dopo aver ottenuto un risultato che indica che il sito è stato distribuito, aprire `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in un browser. Il valore del segreto, **reindeer_flotilla** dovrebbe essere visualizzato.

L'app è stata completata e distribuita.