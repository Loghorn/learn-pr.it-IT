L'interfaccia della riga di comando di Azure è un programma della riga di comando per connettersi ad Azure ed eseguire comandi amministrativi sulle risorse di Azure. Viene eseguita in Linux, macOS e Windows e consente ad amministratori e sviluppatori di eseguire i comandi tramite un terminale o un prompt della riga di comando (o script) anziché tramite un Web browser. Per riavviare una macchina virtuale, ad esempio, si userebbe un comando simile al seguente:

 ```bash
 az vm restart -g MyResourceGroup -n MyVm
 ```

L'interfaccia della riga di comando di Azure offre strumenti da riga di comando multipiattaforma per la gestione delle risorse di Azure e può essere installata in locale in computer Linux, Mac o Windows. L'interfaccia della riga di comando di Azure può anche essere usata da un browser tramite Azure Cloud Shell. In entrambi i casi, può essere usata in modo interattivo o tramite script. Per l'uso interattivo, si avvia prima di tutto una shell, ad esempio cmd.exe in Windows o Bash in Linux o macOS, quindi si esegue il comando al prompt della shell. Per automatizzare attività ripetitive, i comandi dell'interfaccia della riga di comando vengono assemblati in uno script della shell tramite la sintassi per gli script della shell prescelta, quindi si esegue lo script.

## <a name="how-to-install-azure-cli"></a>Come installare l'interfaccia della riga di comando di Azure

In Linux e macOS, si usa uno strumento di gestione pacchetti per installare PowerShell Core. Lo strumento di gestione pacchetti consigliato è diverso in base al sistema operativo e alla distribuzione:
- Linux: **apt-get** su Ubuntu, **yum** su Red Hat e **zypper** su OpenSUSE
- Mac: **Homebrew**

L'interfaccia della riga di comando di Azure è disponibile nel repository Microsoft, quindi è prima necessario aggiungere tale repository allo strumento di gestione dei pacchetti.

Per installare l'interfaccia della riga di comando di Azure in Windows, occorre scaricare ed eseguire un file MSI.

## <a name="using-the-azure-cli-in-scripts"></a>Uso dell'interfaccia della riga di comando di Azure negli script

Se si vogliono usare i comandi dell'interfaccia della riga di comando di Azure negli script, è necessario essere a conoscenza di eventuali problemi correlati alla "shell" o all'ambiente usato per l'esecuzione dello script. In una shell bash, ad esempio, viene usata la sintassi seguente per l'impostazione delle variabili:

 ```bash
 variable="value"
 variable=integer
 ```

Se si usa un ambiente di PowerShell per eseguire gli script dell'interfaccia della riga di comando di Azure, è necessario usare questa sintassi per le variabili:

 ```powershell
 $variable="value"
 $variable=integer
 ```

## <a name="summary"></a>Riepilogo

L'interfaccia della riga di comando di Azure deve essere installata prima di poterla usare per gestire le risorse di Azure da un computer locale. La procedura di installazione varia per Windows, Linux e macOS, ma dopo l'installazione i comandi sono comuni tra le piattaforme. 
