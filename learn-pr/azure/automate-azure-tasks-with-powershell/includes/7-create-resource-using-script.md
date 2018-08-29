Le attività complesse o ripetitive spesso richiedono una notevole quantità di tempo di amministrazione. Le organizzazioni preferiscono automatizzare queste attività per ridurre i costi ed evitare gli errori.

Questo aspetto è importante nell'esempio relativo all'azienda CRM (Customer Relationship Management). In questo caso, il software viene testato su più macchine virtuali Linux, che è necessario eliminare e ricreare continuamente. Si vuole usare uno script di PowerShell per automatizzare la creazione delle macchine virtuali.

Oltre al funzionamento di base per la creazione di una macchina virtuale, sono previsti alcuni requisiti aggiuntivi per lo script. 
- Poiché verranno create più macchine virtuali, si vuole inserire la creazione all'interno di un ciclo
- È necessario creare le macchine virtuali in tre diversi gruppi di risorse, pertanto il nome del gruppo di risorse deve essere passato allo script come parametro

In questa sezione verrà descritto come scrivere ed eseguire uno script di Azure PowerShell che soddisfi questi requisiti.

## <a name="what-is-a-powershell-script"></a>Che cos'è uno script di PowerShell?
Uno script di PowerShell è un file di testo contenente comandi e costrutti di controllo. I comandi sono chiamate di cmdlet. I costrutti di controllo sono funzionalità di programmazione fornite da PowerShell, quali cicli, variabili, parametri, commenti e così via.

I file di script di PowerShell hanno un'estensione **ps1**. È possibile creare e salvare questi file con qualsiasi editor di testo. 

> [!TIP]
> Se si scrivono script di PowerShell in Windows, è possibile usare l'ambiente Windows PowerShell Integrated Scripting Environment (ISE). Questo editor offre funzionalità come la colorazione della sintassi e un elenco dei cmdlet disponibili.
>
>![Windows PowerShell ISE](../media-drafts/7-windows-powershell-ise-screenshot.png)

Dopo avere scritto lo script, eseguirlo dalla riga di comando di PowerShell passando il nome del file preceduto da un punto e una barra rovesciata:

```powershell
.\myScript.ps1
```

## <a name="powershell-techniques"></a>Tecniche di PowerShell
PowerShell offre numerose funzionalità disponibili nei tipici linguaggi di programmazione. È possibile definire variabili, usare rami e cicli, acquisire parametri della riga di comando, scrivere funzioni, aggiungere commenti e così via. Saranno necessarie tre funzionalità per il nostro script: variabili, cicli e parametri.

### <a name="variables"></a>Variabili
PowerShell supporta le variabili. Usare **$** per dichiarare una variabile e **=** per assegnare un valore. Ad esempio:

```powershell
$loc = "East US"
$iterations = 3
```

Le variabili possono contenere oggetti. Ad esempio, la definizione seguente imposta la variabile **adminCredential** sull'oggetto restituito dal cmdlet **Get-Credential**.

```powershell
$adminCredential = Get-Credential
```

Per ottenere il valore archiviato in una variabile, usare il prefisso **$** e il relativo nome, come mostrato di seguito: 

```powershell
$loc = "East US"
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location $loc
```

### <a name="loops"></a>Loop
PowerShell dispone di diversi cicli: **For**, **Do...While**, **For...Each** e così via. Il ciclo **For** è quello più adatto alle nostre esigenze, perché eseguirà un cmdlet per un numero fisso di volte.

La sintassi di base è illustrata di seguito. L'esempio viene eseguito per due iterazioni e ogni volta visualizza il valore di **i**. Gli operatori di confronto sono **-lt** per "minore di", **-le** per "minore o uguale a", **eq** per "uguale a", **ne** per "diverso da" e così via.

```powershell
For ($i = 1; $i -lt 3; $i++)
{
    $i
}
```

### <a name="parameters"></a>Parametri
Quando si esegue uno script, è possibile passare argomenti dalla riga di comando. È possibile specificare nomi per ogni parametro in modo da consentire allo script di estrarre i valori. Ad esempio:

```powershell
.\setupEnvironment.ps1 -size 5 -location "East US"
```

Nello script, i valori vengono acquisiti nelle variabili. In questo esempio, viene stabilita la corrispondenza dei parametri in base al nome:

```powershell
param([string]$location, [int]$size)
```

È possibile omettere i nomi dalla riga di comando. Ad esempio:

```powershell
.\setupEnvironment.ps1 5 "East US"
```

Nello script è possibile basarsi sulla posizione per la corrispondenza quando i parametri non sono denominati:

```powershell
param([int]$size, [string]$location)
```

## <a name="how-to-create-a-linux-virtual-machine"></a>Come creare una macchina virtuale Linux
Azure PowerShell fornisce il cmdlet **New-AzureRmVm** per creare una macchina virtuale. Il cmdlet dispone di diversi parametri che consentono di gestire le numerose impostazioni di configurazione della macchina virtuale. La maggior parte dei parametri ha valori predefiniti accettabili, pertanto è necessario specificare solo cinque elementi:
- **ResourceGroupName**: il gruppo di risorse in cui verrà inserita la nuova macchina virtuale.
- **Name**: il nome della macchina virtuale in Azure.
- **Location**: la posizione geografica in cui verrà eseguito il provisioning della macchina virtuale.
- **Credential**: un oggetto contenente il nome utente e la password per l'account di amministratore della macchina virtuale. Useremo il cmdlet **Get-Credential** per la richiesta del nome utente e della password. **Get-Credential** inserisce il nome utente e la password in un oggetto credenziali, che viene restituito come risultato.
- **Image**: identità del sistema operativo da usare per la macchina virtuale. In questo caso, verrà usato "UbuntuLTS".

La sintassi per il cmdlet è illustrata di seguito:

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

## <a name="summary"></a>Riepilogo
La combinazione di PowerShell e Azure PowerShell offre tutti gli strumenti necessari per l'automazione di Azure. Nell'esempio relativo a CRM, sarà possibile creare più macchine virtuali Linux usando un parametro per mantenere lo script generico e un ciclo per evitare la ripetizione del codice. Questo significa che un'operazione precedentemente complessa ora può essere eseguita in un unico passaggio.