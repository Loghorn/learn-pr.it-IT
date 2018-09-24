La gestione dell'infrastruttura di qualsiasi tipo di carico di lavoro comporta attività di configurazione. Questa configurazione può essere eseguita manualmente, ma i passaggi manuali possono richiedere molto personale ed essere soggetti a errori e inefficienti. E se si venisse assegnati alla gestione di un progetto che ha richiesto la distribuzione di centinaia di sistemi in Azure? Come si compilerebbero e configurerebbero queste risorse? Quanto tempo servirebbe? Sarebbe possibile fare in modo che ogni sistema venisse configurato correttamente, senza varianze tra di essi? Usando l'automazione nella progettazione dell'architettura, è possibile superare queste difficoltà. Verranno ora illustrati alcuni modi per automatizzare i passaggi in Azure.

## <a name="infrastructure-as-code"></a>Infrastruttura come codice

L'infrastruttura come codice è la gestione dell'infrastruttura (reti, macchine virtuali, servizi di bilanciamento del carico e topologia di connessione) in un modello descrittivo, usando un sistema di controllo delle versioni simile a quello usato per il codice sorgente. Analogamente al principio per cui lo stesso codice sorgente genera lo stesso file binario, un modello di infrastruttura come codice genera lo stesso ambiente ogni volta che viene applicato. L'infrastruttura come codice è una procedura DevOps essenziale e viene spesso usata in combinazione con il recapito continuo.

L'infrastruttura come codice si è evoluta per risolvere il problema della trasformazione degli ambienti. Senza l'infrastruttura come codice, i team devono gestire le impostazioni dei singoli ambienti di distribuzione. Nel tempo, ogni ambiente diventa un fiocco di neve, ovvero una configurazione univoca che non può essere riprodotta automaticamente. Le incoerenze tra ambienti causano problemi durante le distribuzioni. Con questo tipo di scenario, l'amministrazione e la manutenzione dell'infrastruttura richiede processi manuali difficili da monitorare e soggetti a errori.

Quando si automatizza la distribuzione dei servizi e dell'infrastruttura, esistono due diversi approcci che è possibile adottare: quello imperativo e quello dichiarativo. In un approccio imperativo si determinano in modo esplicito i comandi che vengono eseguiti per ottenere il risultato desiderato. Con un approccio dichiarativo, si specifica quale deve essere il risultato e non come ottenerlo. Entrambi gli approcci sono validi, quindi nessuna scelta è sbagliata. Questi diversi approcci come appaiono in Azure e come li si usa?

### <a name="imperative-automation"></a>Automazione imperativa

Si inizierà con l'automazione imperativa. Con l'automazione imperativa, si specifica _come_ eseguire i passaggi. Questa operazione viene in genere eseguita a livello di codice tramite un linguaggio di scripting o un SDK. Per le risorse di Azure, è possibile usare l'interfaccia della riga di comando di Azure o Azure PowerShell. Verrà ora esaminato un esempio che usa l'interfaccia della riga di comando di Azure per creare un account di archiviazione.

```azure-cli
az group create --name storage-resource-group \
        --location eastus

az storage account create --name mystorageaccount \
        --resource-group storage-resource-group \
        --kind BlobStorage \
        --access-tier hot
```

In questo esempio verrà specificato come creare queste risorse. Eseguire un comando per creare un gruppo di risorse. Eseguire un altro comando per creare un account di archiviazione. Verrà indicato in modo esplicito ad Azure quali comandi eseguire per produrre l'output necessario.

Con questo approccio, è possibile automatizzare completamente l'infrastruttura. È possibile fornire aree per l'input e l'output e assicurarsi che vengano eseguiti ogni volta gli stessi comandi. Automatizzando le risorse, i passaggi manuali sono stati eliminati dal processo, rendendo l'amministrazione delle risorse più efficiente dal punto di vista operativo. Esistono tuttavia alcuni svantaggi in questo approccio. Se l'architettura diventa più complessa, anche gli script per creare le risorse lo diventano rapidamente. Potrebbe essere necessario aggiungere la gestione degli errori e la convalida dell'input per garantire l'esecuzione completa. I comandi potrebbero essere modificati e richiedere quindi la manutenzione degli script.

### <a name="declarative-automation"></a>Automazione dichiarativa

Con l'automazione dichiarativa, si specifica _quale_ deve essere il risultato, affidando al sistema in uso la procedura per ottenerlo. In Azure l'automazione dichiarativa viene eseguita tramite l'uso di modelli di Azure Resource Manager.

I modelli di Resource Manager sono file con struttura JSON che specificano ciò che si vuole creare. Nell'esempio seguente viene indicato ad Azure di creare un account di archiviazione con i nomi e le proprietà specificati. I passaggi veri e propri eseguiti per creare questo account di archiviazione vengono effettuati da Azure. I modelli hanno quattro sezioni: parametri, variabili, risorse e output. I parametri gestiscono l'input da usare nel modello. Le variabili consentono di archiviare i valori da usare nell'intero modello. Le risorse sono ciò che viene creato e gli output consentono di fornire all'utente i dettagli di ciò che è stato creato.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "accountType": {
            "type": "string",
            "defaultValue": "Standard_RAGRS"
        },
        "kind": {
            "type": "string"
        },
        "accessTier": {
            "type": "string"
        },
        "httpsTrafficOnlyEnabled": {
            "type": "bool",
            "defaultValue": true
        }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('name')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "[parameters('accountType')]"
            },
            "kind": "[parameters('kind')]",
            "properties": {
                "supportsHttpsTrafficOnly": "[parameters('httpsTrafficOnlyEnabled')]",
                "accessTier": "[parameters('accessTier')]",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "storageAccountName": {
            "type": "string",
            "value": "[parameters('name')]"
        }
    }
}
```

I modelli possono essere usati per creare e modificare la maggior parte dei servizi in Azure. Possono essere archiviati in repository di codice, inclusi nel controllo del codice sorgente e condivisi tra gli ambienti per garantire che l'infrastruttura in fase di sviluppo corrisponda a quella effettivamente in produzione. Sono un ottimo modo per automatizzare le distribuzioni e garantire la coerenza, eliminano gli errori di configurazione delle distribuzioni e possono aumentare la velocità operativa.

L'automazione della distribuzione dell'infrastruttura è una valida opzione per iniziare, ma per la distribuzione delle macchine virtuali sono necessarie altre operazioni. Verranno ora esaminati due approcci per automatizzare la configurazione dopo la distribuzione.

## <a name="vm-customization-images-vs-post-deployment-configuration"></a>Personalizzazione della VM: immagini e configurazione post-distribuzione

In molte distribuzioni di macchine virtuali, il processo non viene eseguito quando il computer è in esecuzione. È probabile che siano necessarie altre operazioni di configurazione prima la VM possa effettivamente essere usata per lo scopo designato. Potrebbe essere necessario formattare altri dischi, aggiungere a un dominio la VM, installare un agente per un software di gestione e anche installare e configurare il carico di lavoro effettivo.

Sono due le strategie comunemente applicate per le operazioni di configurazione, considerate parte della configurazione della VM stessa, che presentano entrambe vantaggi e svantaggi:

- Immagini personalizzate
- Scripting post-distribuzione

Le immagini personalizzate vengono generate distribuendo una macchina virtuale e quindi configurando o installando il software in tale istanza in esecuzione. Quando tutto è configurato correttamente, il computer può essere arrestato e viene creata un'immagine dalla VM. Da tale immagine possono quindi essere create altre nuove macchine virtuali. L'uso di immagini personalizzate può ridurre il tempo complessivo richiesto dalla distribuzione perché, quando la macchina virtuale è distribuita e in esecuzione, non sarà necessaria alcuna configurazione aggiuntiva. Se la velocità di distribuzione è un fattore importante, è sicuramente opportuno prendere in considerazione le immagini personalizzate.

Lo scripting post-distribuzione in genere sfrutta un'immagine di base e quindi usa lo scripting o una piattaforma di gestione della configurazione per eseguire la configurazione dopo che la VM è stata distribuita. Lo scripting post-distribuzione può essere realizzato eseguendo uno script nella VM tramite l'estensione script di Azure o sfruttando una soluzione più affidabile, ad esempio Automation DSC (Desired State Configuration) per Azure.

Per ogni approccio è bene ricordare alcune considerazioni. Quando si usano le immagini, assicurarsi che esista un processo per gestire gli aggiornamenti delle immagini, le patch di sicurezza e la gestione dell'inventario delle immagini stesse. Con lo scripting post-distribuzione, i tempi di compilazione possono aumentare perché la VM non può essere aggiunta ai carichi di lavoro in tempo reale fino al completamento della compilazione. Questo potrebbe non essere un problema significativo per i sistemi autonomi, ma, quando si usano servizi che vengono ridimensionati automaticamente (ad esempio, i set di scalabilità di macchine virtuali), i tempi di compilazione estesi possono comprometta la velocità di ridimensionamento. Con entrambi gli approcci, è importante correggere eventuali deviazioni della configurazione. Non appena la nuova configurazione viene implementata, è necessario assicurarsi che i sistemi esistenti vengano aggiornati di conseguenza.

L'automazione della distribuzione delle risorse può rappresentare un enorme vantaggio per l'ambiente. Il risparmio di tempo e la riduzione degli errori possono portare le funzionalità operative a un altro livello.

## <a name="automation-of-operational-tasks"></a>Automazione delle attività operative

Quando le soluzioni sono operative, è possibile automatizzare anche le attività operative continuative. L'automazione di queste attività con Automazione di Azure riduce i carichi di lavoro manuali, abilita la gestione della configurazione e degli aggiornamenti delle risorse di calcolo, centralizza le risorse condivise, ad esempio pianificazioni, credenziali e certificati, e fornisce un framework per l'esecuzione di qualsiasi tipo di attività di Azure.

Per le operazioni relative a Lamna Healthcare, sono incluse:

- Ricerca periodica di dischi orfani.
- Installazione delle patch di sicurezza più recenti nelle VM.
- Ricerca e arresto delle macchine virtuali durante gli orari di minore attività.
- Esecuzione di report giornalieri e generazione di un dashboard per inviare i report ai dirigenti.

Ad esempio, si supponga di voler eseguire una macchina virtuale solo durante l'orario lavorativo. È possibile scrivere uno script per avviare la macchina virtuale la mattina e spegnerla la sera. È possibile configurare Automazione di Azure per eseguire lo script a orari definiti. L'illustrazione seguente mostra il ruolo di Automazione di Azure in questo processo.

![Illustrazione che mostra il ruolo di Automazione di Azure nella gestione di un processo aziendale ripetitivo.](../media/automation-vm-power-state.png)

## <a name="automating-development-environments"></a>Automazione degli ambienti di sviluppo

All'altra estremità della pipeline dell'infrastruttura cloud si trovano i computer di sviluppo usati dagli sviluppatori per scrivere le applicazioni e i servizi che sono alla base dell'azienda. È possibile usare Azure DevTest Labs per identificare le VM con tutti gli strumenti corretti e i repository necessari. Gli sviluppatori che usano più servizi possono passare da un ambiente di sviluppo a un altro senza dover effettuare manualmente il provisioning di un nuovo computer. Questi ambienti di sviluppo possono essere arrestati quando non sono in uso e riavviati quando sono di nuovo necessari.

## <a name="automation-at-lamna-healthcare"></a>Automazione in Lamna Healthcare

Verrà ora illustrato come Lamna Healthcare abbia registrato un miglioramento grazie all'automazione. All'inizio, la distribuzione dell'infrastruttura e le compilazioni dei server erano interamente manuali. I tecnici distribuivano tutto tramite il portale. In questo modo venivano introdotti varianze ed errori tra gli ambienti di test e di produzione e le differenze impedivano di rilevare i problemi prima che il codice venisse usato nell'ambiente di produzione.

L'intera infrastruttura viene ora distribuita tramite modelli di Resource Manager. Questi modelli sono archiviati in un repository GitHub e, prima del rilascio per la distribuzione, viene eseguita una revisione del codice. Viene inoltre creata la stessa infrastruttura negli ambienti di sviluppo, test e produzione, per garantire la convalida della configurazione in tutti gli ambienti.

Per la maggior parte dei servizi che usano macchine virtuali, è disponibile un'immagine di base standard e viene usato DSC per configurare i sistemi dopo la distribuzione. Per le Web farm in cui è necessaria la scalabilità dei set di scalabilità di macchine virtuali, è disponibile un processo completamente automatizzato per archiviare il codice e compilare una nuova immagine con tutta la configurazione richiesta predefinita prima di renderla disponibile nei set di scalabilità.

È disponibile un processo di Automazione per arrestare durante gli orari di minore attività le macchine virtuali identificate per poter ridurre i costi. È stata anche automatizzata l'applicazione di patch alle VM.

Gli sviluppatori hanno ora un ambiente self-service in DevTest Labs in cui possono sviluppare con le immagini e la configurazione più recenti, assicurandosi che gli elementi usati per lo sviluppo corrispondano alla configurazione usata nell'ambiente di produzione.

Tutto ciò ha richiesto uno sforzo iniziale, ma ne sono derivati vantaggi a lungo termine. Sono stati ridotti drasticamente gli errori e lo sforzo richiesto ai team operativi per gestire gli ambienti. Per gli sviluppatori è un vantaggio poter passare facilmente al provisioning delle risorse per lo sviluppo, eliminando tutte le operazioni necessarie per creare gli ambienti.

## <a name="summary"></a>Riepilogo

Sono stati illustrati alcuni modi per introdurre le funzionalità di automazione nell'architettura. Dedicando tempo all'automazione dell'ambiente si ottengono numerosissimi vantaggi, dalla distribuzione dell'infrastruttura come codice al miglioramento della produttività degli sviluppatori con gli ambienti lab. La riduzione degli errori e della varianza e il risparmio sui costi operativi possono essere un vantaggio significativo per l'organizzazione e consentono di portare l'ambiente cloud al livello successivo.