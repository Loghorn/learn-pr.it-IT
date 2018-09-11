Esistono diversi strumenti per creare un account di archiviazione. In genere si sceglie in base a se si desidera un'interfaccia utente grafica e se è necessaria l'automazione.

## <a name="available-tools"></a>Strumenti disponibili

Gli strumenti disponibili sono:

- Portale
- Interfaccia della riga di comando (CLI)
- Azure PowerShell
- Librerie di gestione client.

Il portale offre un'interfaccia utente grafica con le descrizioni comandi per ogni impostazione. Questo lo rende facile da usare e utile per conoscere le opzioni.

Gli altri strumenti supportano tutti l'automazione. Interfaccia della riga di comando e Azure PowerShell consentono di scrivere script mentre le librerie di gestione consentono di incorporare la creazione in un'app client.

## <a name="how-to-choose-a-tool"></a>Come scegliere uno strumento

Gli account di archiviazione sono generalmente basati su un'analisi dei dati, quindi tendono a essere relativamente stabili. Questo significa che la creazione di un account di archiviazione è solitamente un'operazione una tantum eseguita all'inizio di un progetto. Per le attività una tantum, il portale è la scelta più comune.

Nei rari casi in cui è necessaria l'automazione, la scelta è tra un'API programmatica o una soluzione di scripting. Gli script sono in genere più veloci da creare e meno impegnativi da mantenere perché non c'è bisogno di un pacchetto IDE, NuGet o di passaggi di compilazione. Se si dispone di un'applicazione client esistente, le librerie di gestione possono essere una scelta interessante, altrimenti gli script sono probabilmente un'opzione migliore.