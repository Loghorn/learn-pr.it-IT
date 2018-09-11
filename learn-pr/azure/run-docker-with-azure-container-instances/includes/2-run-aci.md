Istanze di contenitore di Azure semplifica la creazione e gestione di contenitori Docker in Azure, senza dover eseguire il provisioning di macchine virtuali o adottare un servizio di livello superiore. In questa unità viene creato un contenitore in Azure, che viene quindi esposto a Internet con un nome di dominio completo (FQDN).

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Le istanze di contenitore di Azure, come tutte le risorse di Azure, devono essere inserite in un gruppo di risorse, una raccolta logica in cui le risorse di Azure vengono distribuite e gestite.

Creare un gruppo di risorse con il comando `az group create`.

L'esempio seguente crea un gruppo di risorse denominato *myResourceGroup* nella posizione *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Creare un contenitore

È possibile creare un contenitore specificando un nome, un'immagine Docker e un gruppo di risorse di Azure al comando **az container create**. Facoltativamente, è possibile esporre il contenitore in Internet specificando un'etichetta del nome DNS. In questo esempio si distribuisce un contenitore che ospita un'app Web di piccole dimensioni.

Eseguire il comando seguente per avviare un'istanza di contenitore. Il valore *--dns-name-label* deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza, quindi potrebbe essere necessario modificare questo valore per garantire l'univocità:

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

In pochi secondi, verrà visualizzata una risposta alla richiesta. Il contenitore inizialmente presenta lo stato **Creazione in corso**, ma viene avviato entro alcuni secondi. È possibile controllare lo stato usando il comando `az container show`:

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

Quando si esegue il comando, vengono visualizzati il nome di dominio completo (FQDN) del contenitore e il relativo stato di provisioning:

```bash
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

Quando il contenitore passa allo stato **Completato**, passare al relativo FQDN nel browser:

![Screenshot del browser che mostra un'applicazione in esecuzione in un'istanza di contenitore di Azure](../media-draft/aci-app-browser.png)

## <a name="summary"></a>Riepilogo

In questa unità è stata creata un'istanza di contenitore di Azure che esegue un server Web e un'applicazione. L'accesso all'applicazione è stato inoltre eseguito mediante l'FQDN dell'istanza di contenitore.

Nell'unità successiva si configureranno i criteri di riavvio dell'istanza di contenitore.