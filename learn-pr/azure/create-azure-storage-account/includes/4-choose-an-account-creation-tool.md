Esistono diversi strumenti per creare un account di archiviazione. La scelta in genere si basa sulla preferenza o meno di un'interfaccia utente grafica e sulla necessità o meno di funzionalità di automazione.

## <a name="available-tools"></a>Strumenti disponibili

Gli strumenti disponibili sono:

- Portale di Azure
- Interfaccia della riga di comando di Azure
- Azure PowerShell
- Librerie client di gestione

Il portale offre un'interfaccia utente grafica con le spiegazioni per ogni impostazione. Di conseguenza, il portale è facile da usare e utile per conoscere le opzioni.

Gli altri strumenti nell'elenco precedente supportano tutti l'automazione. L'interfaccia della riga di comando di Azure e Azure PowerShell consentono di scrivere script mentre le librerie di gestione consentono di incorporare la creazione in un'app client.

## <a name="how-to-choose-a-tool"></a>Come scegliere uno strumento

Gli account di archiviazione sono generalmente basati su un'analisi dei dati, quindi tendono a essere relativamente stabili. Di conseguenza, la creazione di un account di archiviazione è solitamente un'operazione una tantum eseguita all'inizio di un progetto. Per le attività una tantum, il portale è la scelta più comune.

Nei rari casi in cui è necessaria l'automazione, la scelta è tra un'API programmatica o una soluzione di scripting. Gli script sono in genere più veloci da creare e meno impegnativi da mantenere perché non c'è bisogno di un pacchetto IDE, NuGet o di passaggi di compilazione. Se si dispone di un'applicazione client esistente, le librerie di gestione possono essere una scelta interessante, altrimenti gli script sono probabilmente un'opzione migliore.