Viene qui creato un contenitore in Azure, che viene quindi esposto a Internet con un nome di dominio completo (FQDN).

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="why-use-azure-container-instances"></a>Perché usare Istanze di contenitore di Azure?

Istanze di contenitore di Azure è utile per gli scenari che possono funzionare anche in contenitori isolati, inclusi i processi di compilazione, l'automazione di attività e le applicazioni semplici. La soluzione Istanze di contenitore di Azure offre i vantaggi seguenti:

- **Avvio rapido**: i contenitori vengono avviati in pochi secondi.
- **Fatturazione al secondo**: i costi vengono addebitati solo mentre il contenitore è in esecuzione.
- **Sicurezza a livello di hypervisor**: l'applicazione viene isolata completamente come in una macchina virtuale.
- **Dimensioni personalizzate**: è possibile specificare i valori esatti per memoria e core CPU.
- **Archivio permanente**: è possibile montare le condivisioni di File di Azure direttamente in un contenitore per recuperare e rendere persistente lo stato.
- **Linux e Windows**: consente di pianificare contenitori sia Windows che Linux usando la stessa API.

Per gli scenari in cui è necessaria l'orchestrazione completa dei contenitori, quali il rilevamento dei servizi tra più contenitori, il ridimensionamento automatico e gli aggiornamenti coordinati delle applicazioni, è consigliabile usare il servizio Kubernetes di Azure (AKS).

## <a name="create-a-container"></a>Creare un contenitore

Si crea un contenitore specificando un nome, un'immagine Docker e un gruppo di risorse di Azure nel comando **az container create**. Facoltativamente, è possibile esporre il contenitore in Internet specificando un'etichetta del nome DNS. In questo esempio si distribuisce un contenitore che ospita un'app Web di piccole dimensioni. È anche possibile selezionare la località in cui posizionare l'immagine: per impostazione predefinita qui viene usata l'opzione "Stati Uniti orientali", ma è possibile modificarla selezionando dall'elenco una località più vicina.

<!-- TODO: fix region list so it's not hardcoded here --> L'ambiente sandbox gratuito consente di creare risorse in un subset di aree globali di Azure. Selezionare un'area nell'elenco seguente durante la creazione delle risorse:

:::row:::
    :::column:::
        - westus2 - centralus - eastus - westeurope - southeastasia :::column-end:::
:::row-end:::

Eseguire il comando seguente in Cloud Shell per avviare un'istanza di contenitore. Il valore `--dns-name-label` deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza, quindi sarà necessario sostituire `[dns-name]` con un valore univoco.

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label [dns-name] \
    --location eastus
```

In pochi secondi, verrà visualizzata una risposta alla richiesta. Il contenitore inizialmente presenta lo stato **Creazione in corso**, ma viene avviato entro alcuni secondi. È possibile controllare lo stato usando il comando `az container show`:

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

Quando si esegue il comando, vengono visualizzati il nome di dominio completo (FQDN) del contenitore e il relativo stato di provisioning:

```output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

Quando il contenitore passa allo stato **Completato**, passare al relativo nome FQDN nel browser per verificare l'esito positivo.

È stata creata un'istanza di contenitore di Azure per eseguire un server Web e un'applicazione. L'accesso all'applicazione è stato inoltre eseguito mediante il nome FQDN dell'istanza di contenitore.