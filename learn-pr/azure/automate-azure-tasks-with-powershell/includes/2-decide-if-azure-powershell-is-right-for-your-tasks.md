Si supponga che sia necessario scegliere uno strumento per amministrare le risorse di Azure usate per testare il sistema CRM (Customer Relationship Management). I test richiedono consente di creare gruppi di risorse ed eseguire il provisioning di macchine virtuali (VM).

Si desidera qualcosa che è facile per gli amministratori di conoscere, ma sufficientemente potente per automatizzare l'installazione e configurazione di più macchine virtuali o creare degli script di un ambiente completo dell'applicazione. Sono disponibili diversi strumenti. È necessario trovare quello più adatto per le specifiche persone e attività.

## <a name="what-tools-are-available"></a>Quali strumenti sono disponibili?
Azure offre tre strumenti di amministrazione tra cui scegliere:

- Portale di Azure
- Interfaccia della riga di comando di Azure
- Azure PowerShell

Tutti offrono approssimativamente lo stesso livello di controllo: qualsiasi attività che è possibile eseguire con uno degli strumenti, in genere può essere eseguita anche con gli altri due. Tutte e tre sono multipiattaforma, poiché possono essere eseguiti in Windows, macOS e Linux. Si differenziano a livello di sintassi, requisiti di installazione e supporto per l'automazione.

Di seguito verrà descritta ognuna di queste tre opzioni e saranno fornite alcune indicazioni per la scelta di quella più appropriata. 

## <a name="what-is-the-azure-portal"></a>Che cos'è il portale di Azure?
Il portale di Azure è un sito Web che consente di creare, configurare e modificare le risorse nella sottoscrizione di Azure. Il portale è un'interfaccia utente grafica che rende più semplice individuare le risorse necessarie e apportare le modifiche richieste. Fornisce inoltre assistenza per le attività amministrative complesse, attraverso procedure guidate e descrizioni comandi.

Il portale non fornisce alcun sistema per automatizzare le attività ripetitive. Ad esempio, per configurare 15 macchine virtuali, è necessario crearle una alla volta completando la procedura guidata per ogni macchina virtuale. Può trattarsi di un processo che richiede molto tempo e soggetto a errori, in caso di attività complesse. 

## <a name="what-is-the-azure-cli"></a>Che cos'è l'interfaccia della riga di comando di Azure?
L'interfaccia della riga di comando di Azure è un programma della riga di comando multipiattaforma per connettersi ad Azure ed eseguire comandi amministrativi sulle risorse di Azure. Per creare una macchina virtuale, ad esempio, si userebbe un comando simile al seguente:

```azurecli
az vm create \
  --resource-group CrmTestingResourceGroup \
  --name CrmUnitTests \
  --image UbuntuLTS
  ...
```

L'interfaccia della riga di comando di Azure è disponibile in due modalità: all'interno di un browser tramite Azure Cloud Shell o con un'installazione locale in Windows, Mac o Linux. In entrambi i casi, può essere usata in modo interattivo o tramite script. Per l'uso interattivo, si avvia prima di tutto una shell, ad esempio `cmd.exe` in Windows o Bash in Linux o macOS, quindi si esegue il comando al prompt della shell. Per automatizzare attività ripetitive, i comandi vengono assemblati in uno script della shell tramite la sintassi per gli script della shell prescelta, quindi si esegue lo script.

## <a name="what-is-azure-powershell"></a>Informazioni su Azure PowerShell.
Azure PowerShell è un modulo che viene aggiunto a Windows PowerShell o PowerShell Core per consentire di connettersi alla sottoscrizione di Azure e gestire le risorse. Per il funzionamento di Azure PowerShell è necessario PowerShell. PowerShell fornisce servizi come la finestra della shell, l'analisi dei comandi e così via. Azure PowerShell aggiunge i comandi specifici di Azure.

Ad esempio, Azure PowerShell fornisce il **New-AzureRmVM** comando che crea una macchina virtuale per l'utente all'interno della sottoscrizione di Azure. Per usarlo, è necessario avviare l'applicazione PowerShell e quindi eseguire un comando simile al seguente:

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Anche Azure PowerShell è disponibile in due modalità: all'interno di un browser tramite Azure Cloud Shell o con un'installazione locale in Windows, Mac o Linux. In entrambi i casi, è possibile scegliere tra due modalità. È possibile usarlo in modalità interattiva, in cui si esegue manualmente un comando alla volta, o in modalità di scripting, in cui si esegue uno script costituito da più comandi.

## <a name="how-to-choose-an-administrative-tool"></a>Come scegliere uno strumento di amministrazione
Il portale, l'interfaccia della riga di comando di Azure e Azure PowerShell sono sostanzialmente equivalenti per quanto riguarda gli oggetti di Azure che possono amministrare e le configurazioni che possono creare. Sono anche tutti multipiattaforma. Questo significa che in genere sarà necessario prendere in considerazione altri fattori per decidere quali usare:

- **Automazione**: è necessario automatizzare una serie di attività complesse o ripetitive? Azure PowerShell e l'interfaccia della riga di comando di Azure supportano questa operazione, a differenza del portale.

- **Curva di apprendimento**: è necessario completare un'attività rapidamente senza dover apprendere nuovi comandi o una nuova sintassi? Il portale di Azure non richiede di apprendere una sintassi o di memorizzare comandi. In Azure PowerShell e nell'interfaccia della riga di comando di Azure è necessario conoscere la sintassi dettagliata per ogni comando usato.

- **Set di competenze del team**: il team dispone di competenze esistenti? Il team potrebbe ad esempio avere usato PowerShell per amministrare Windows. In questo caso, potrà acquisire rapidamente familiarità con l'uso di Azure PowerShell.

## <a name="example"></a>Esempio
È importante ricordare che è necessario scegliere uno strumento di amministrazione per creare gli ambienti di test per l'applicazione CRM. Gli amministratori dovranno eseguire due specifiche attività di Azure:

1. Creare un gruppo di risorse per ogni categoria di test (unit test, integrazione e accettazione).
2. Creare più macchine virtuali in ogni gruppo di risorse prima di ogni ciclo di test.

Per creare i gruppi di risorse, il portale di Azure è una scelta ragionevole. Si tratta di attività da effettuare una sola volta, pertanto non sono necessari gli script per eseguirle.

Individuare lo strumento migliore per creare le macchine virtuali è una decisione più complessa. È necessario crearne diverse e questa operazione deve essere eseguita ripetutamente, presumibilmente più volte alla settimana. Sarà pertanto necessaria l'automazione, quindi il portale di Azure non è una scelta valida. In questo caso, è possibile usare Azure PowerShell o l'interfaccia della riga di comando di Azure per soddisfare le esigenze. Se i membri del team hanno già una certa conoscenza di PowerShell, Azure PowerShell è probabilmente la soluzione più adatta. Azure PowerShell è disponibile nei sistemi operativi usati dal team di amministrazione, supporta l'automazione e dovrebbe essere molto rapido da apprendere per il team.

## <a name="summary"></a>Riepilogo
Per la maggior parte degli amministratori, la prima esperienza con Azure avviene nel portale. È un ottimo punto di partenza perché offre un'interfaccia grafica pulita e ben strutturata, ma fornisce opzioni limitate per l'automazione. Quando è necessaria l'automazione, Azure offre due opzioni: Azure PowerShell per gli amministratori che hanno esperienza con PowerShell e l'interfaccia della riga di comando di Azure per tutti gli altri utenti.

Nella pratica, generalmente le aziende devono eseguire una combinazione di attività occasionali e ripetitive. Per questo motivo, è piuttosto comune usare sia il portale che una soluzione di scripting. Nell'esempio relativo a CRM, è possibile creare i gruppi di risorse tramite il portale e automatizzare la creazione delle macchine virtuali con PowerShell.

Il resto di questo modulo è dedicato all'installazione e all'uso di Azure PowerShell.
