A questo punto è possibile eseguire l'app in Azure. È necessario creare un'app del Servizio app di Azure, configurarla con un'identità gestita e la configurazione dell'insieme di credenziali e quindi distribuire il codice.

## <a name="create-the-app-service-plan-and-app"></a>Creare l'app e il piano di servizio app

La creazione di un'app di Servizio app è un processo che si svolge in due passaggi: per prima cosa, creare il *piano* e poi l'*app*.

Il nome del *piano* deve solamente essere univoco all'interno della sottoscrizione, quindi è possibile usare lo stesso nome usato da noi: **insieme di credenziali delle chiavi-esercizio-piano**. Il nome dell'app deve essere globalmente univoco, ma è necessario sceglierne uno proprio.

In Azure Cloud Shell, eseguire il comando seguente:

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

## <a name="add-configuration-to-the-app"></a>Aggiungere una configurazione per l'app

Per la distribuzione in Azure, si seguirà la procedura consigliata del servizio app per inserire la configurazione VaultName nelle impostazioni dell'applicazione, anziché in un file di configurazione.

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a>Abilitare l'identità gestita

L'abilitazione dell'identità gestita in un'app richiede una singola riga di codice:

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

Nell'output JSON risultante, copiare il valore **principalId**. Proprietà PrincipalId è l'ID univoco della nuova identità dell'app in Azure Active Directory e dobbiamo usarlo nel passaggio successivo.

## <a name="grant-access-to-the-vault"></a>Concedere l'accesso all'insieme di credenziali

A questo punto è necessario concedere all'app le autorizzazioni di identità per ottenere ed elencare i segreti dall'insieme di credenziali di ambiente di produzione. Usare la **principalId** copiata nel passaggio precedente come valore per **id dell'oggetto** nel comando seguente.

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-managed-identity-principleid> --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a>Distribuire l'app e provarla

Tutte le configurazioni sono impostate e i può avviare la distribuzione. I comandi seguenti pubblicheranno il sito nella cartella `pub`, la comprimeranno in `site.zip` e quindi distribuiranno il file zip nel Servizio app.

> [!NOTE]
> Sarà necessario `cd` indietro alla directory di KeyVaultDemoApp, se non è già stato fatto.

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

Dopo aver ottenuto un risultato che indica che il sito è stato distribuito, aprire `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in un browser. Il valore del segreto, **reindeer_flotilla** dovrebbe essere visualizzato.

L'app è stata completata e distribuita.