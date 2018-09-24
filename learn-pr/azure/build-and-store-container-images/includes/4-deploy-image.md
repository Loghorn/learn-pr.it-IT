È possibile eseguire il pull di immagini del contenitore dal Registro contenitori di Azure usando molte piattaforme di gestione dei contenitori, ad esempio Istanze di contenitore di Azure, il registro Kubernetes di Azure e Docker per Windows o Mac. Quando si eseguono immagini del contenitore dal Registro contenitori di Azure, potrebbero essere necessarie credenziali di autenticazione. 

È consigliabile usare un'entità servizio di Azure per l'autenticazione con Container Registry. È anche consigliabile proteggere le credenziali dell'entità servizio di Azure in Azure Key Vault. In questa unità verrà descritto l'approccio consigliato.

Tuttavia, per l'esercitazione in tempo reale verrà usato l'account amministratore predefinito che può essere abilitato in tutti i registri contenitori di Azure. L'account amministratore viene usato con le risorse della sandbox gratuita.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

Creare una variabile con il nome del registro contenitori in lettere minuscole (ad esempio, "contenitore" invece di "Contenitore"). Questa variabile verrà usata in tutta l'unità.

```azurecli
ACR_NAME=<acrName>
```

### <a name="service-principal"></a>Entità servizio

Per un'applicazione di produzione sarebbe a questo punto opportuno creare un'entità servizio. Ricordare che **questo non funzionerà nell'ambiente sandbox**, ma è una procedura consigliata da adottare nei sistemi reali. Per l'esercitazione pratica verranno usate le istruzioni dell'account amministratore riportate sotto.

Per creare l'entità servizio, è possibile usare il comando `az ad sp create-for-rbac`. L'argomento `--role` configura l'entità servizio con il ruolo *lettore*, che concede l'accesso di tipo solo pull al registro. Per concedere l'accesso sia push che pull, impostare l'argomento `--role` su *collaboratore*.

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

L'output della creazione dell'entità servizio avrà un aspetto simile al seguente. Prendere nota dei valori `appId` e `password`. Questi valori devono essere archiviati nell'insieme di credenziali delle chiavi di Azure.

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="admin-account"></a>Account amministratore

I registri contenitori di Azure sono dotati di un account amministratore predefinito. Questo account non è collegato ad Azure AD o ai controlli degli accessi in base al ruolo e di conseguenza **deve essere usato solo a scopo di test**. 

1. Abilitare l'account amministratore.
    ```azurecli
      az acr update -n $ACR_NAME --admin-enabled true
    ```

2. Eseguire una query per ottenere il nome utente e la password generati automaticamente

    ```azurecli
      az acr credential show --name $ACR_NAME
    ```

L'output è simile al seguente. Prendere nota di `username` e `value` abbinati alla "password" `name`. Salvare tali valori nell'insieme di credenziali delle chiavi.

```output
{  "passwords": [
    {
      "name": "password",
      "value": "aaaaa"
    },
    {
      "name": "password2",
      "value": "bbbbb"
    }
  ],
  "username": "ccccc"
}
```

### <a name="save-the-username-and-password-to-the-key-vault"></a>Salvare il nome utente e la password nell'insieme di credenziali delle chiavi

1. Creare un insieme di credenziali delle chiavi di Azure con il comando `az keyvault create`.

    ```azurecli
    az keyvault create --resource-group <rgn>[Sandbox resource group name]</rgn> --name $ACR_NAME-keyvault
    ```

1. Usare il comando `az keyvault secret set` per archiviare il nome utente per il registro contenitori di Azure nell'insieme di credenziali. Se si usano entità servizio, per questo valore è necessario usare l'ID dell'app. Poiché è stato usato l'account amministratore questa volta verrà salvato il nome utente dalla query eseguita in precedenza. Immettere il comando seguente senza dimenticare di sostituire `<username>`.

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <username>
    ```

1. Usare il comando `az keyvault secret set` per archiviare la *password* nell'insieme di credenziali. Sostituire `<password>` con `password` dalla query precedente.

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
    ```

A questo punto è stato creato un insieme di credenziali delle chiavi di Azure e vi sono stati archiviati due segreti:

* `$ACR_NAME-pull-usr`: il **nome utente** del registro contenitori.
* `$ACR_NAME-pull-pwd`: la **password** del registro contenitori.

Ora è possibile fare riferimento a questi segreti per nome quando gli utenti o le applicazioni e i servizi eseguono il pull di immagini dal registro.

### <a name="deploy-a-container-with-azure-cli"></a>Distribuire un contenitore con l'interfaccia della riga di comando di Azure

Ora che le credenziali dell'entità servizio sono archiviate in Azure Key Vault, le applicazioni e i servizi possono usarle per accedere al registro privato.

Eseguire il comando `az container create` seguente per distribuire un'istanza di contenitore. Il comando usa le credenziali dell'entità servizio archiviate in Azure Key Vault per eseguire l'autenticazione al registro contenitori.

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location eastus \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

Ottenere l'indirizzo IP dell'istanza di contenitore di Azure.

```azurecli
az container show --resource-group  <rgn>[Sandbox resource group name]</rgn> --name acr-build --query ipAddress.ip --output table
```

Aprire un browser e passare all'indirizzo IP del contenitore. Se tutto è configurato correttamente, si dovrebbero visualizzare i risultati seguenti:

![Applicazione Web di esempio con il testo Hello World](../media/hello.png)

