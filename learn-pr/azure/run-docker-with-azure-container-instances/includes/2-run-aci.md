In questo caso, si crea un contenitore in Azure ed esporlo a internet con un nome di dominio completo (FQDN).

## <a name="why-use-azure-container-instances"></a>Perché usare istanze di contenitore di Azure?

Istanze di contenitore di Azure è utile per scenari in cui possono operare in contenitori isolati, tra cui applicazioni semplici, automazione di attività e i processi di compilazione. La soluzione Istanze di contenitore di Azure offre i vantaggi seguenti:

- **Avvio rapido**: avviare i contenitori in pochi secondi.
- **Fatturazione al secondo**: addebitato un costo solo mentre è in esecuzione il contenitore.
- **Sicurezza a livello di hypervisor**: isolare l'applicazione completamente come lo sarebbe in una macchina virtuale.
- **Dimensioni personalizzate**: specificare valori esatti per la memoria e core CPU.
- **Un archivio permanente**: montare file di Azure condivide direttamente a un contenitore per recuperare e rendere persistente lo stato.
- **Linux e Windows**: pianificare contenitori sia Windows che Linux usando l'API stessa.

Per gli scenari in cui è necessaria l'orchestrazione completa dei contenitori, quali il rilevamento dei servizi tra più contenitori, la scalabilità automatica e gli aggiornamenti coordinati delle applicazioni, è consigliabile usare Azure Kubernetes Service (AKS).

## <a name="create-a-container"></a>Creare un contenitore

[!include[](../../../includes/azure-sandbox-activate.md)]

Per creare un contenitore specificando un nome, un'immagine Docker e un gruppo di risorse di Azure per il **contenitore di az creare** comando. Facoltativamente, è possibile esporre il contenitore in Internet specificando un'etichetta del nome DNS. In questo esempio si distribuisce un contenitore che ospita un'app Web di piccole dimensioni.

Eseguire il comando seguente in Cloud Shell per avviare un'istanza di contenitore. Il valore *--dns-name-label* deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza, quindi potrebbe essere necessario modificare questo valore per garantire l'univocità:

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

In pochi secondi, verrà visualizzata una risposta alla richiesta. Il contenitore inizialmente presenta lo stato **Creazione in corso**, ma viene avviato entro alcuni secondi. È possibile controllare lo stato usando il comando `az container show`:

```azurecli
az container show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
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

Quando il contenitore passa per la **Succeeded** nello stato, passare al suo FQDN nel browser per verificare l'esito positivo.

In questo caso, è creata un'istanza di contenitore di Azure per l'esecuzione di un server web e un'applicazione. L'accesso all'applicazione è stato inoltre eseguito mediante l'FQDN dell'istanza di contenitore.