Per i reparti IT è abbastanza comune gestire un vasto set di risorse di Azure, dalle macchine virtuali di Azure ai siti Web gestiti.

Anche se il portale di Azure è facile da usare per attività occasionali, lo spostamento tra i vari pannelli richiede tuttavia tempo aggiuntivo quando è necessario creare, modificare o eliminare più elementi. In questo caso la riga di comando rappresenta uno strumento estremamente vantaggioso perché consente di eseguire comandi in modo rapido ed efficiente o anche di usare gli script per eseguire attività ripetitive. In Azure sono disponibili due strumenti da riga di comando, ovvero Azure PowerShell e l'interfaccia della riga di comando.

Con uno di questi strumenti è possibile scrivere script per controllare lo stato del server del cloud, distribuire nuove configurazioni, aprire le porte nel firewall o connettersi a una macchina virtuale per modificare un'impostazione. Gli amministratori di Windows, in genere, preferiscono usare Azure PowerShell, mentre gli sviluppatori e gli amministratori di Linux usano spesso l'interfaccia della riga di comando di Azure.

Questo modulo è incentrato sull'uso dell'interfaccia della riga di comando di Azure per creare e gestire macchine virtuali ospitate in Azure. Se si vuole ottenere una panoramica dell'interfaccia della riga di comando di Azure, sulle modalità di installazione e sul funzionamento con le sottoscrizioni di Azure, vedere il modulo di formazione **Controllare i servizi di Azure con l'interfaccia della riga di comando**.

## <a name="what-is-the-azure-cli"></a>Che cos'è l'interfaccia della riga di comando di Azure?

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

L'interfaccia della riga di comando di Azure può essere utile per gestire le risorse di Azure, ad esempio macchine virtuali e dischi, dalla riga di comando o tramite script. Esaminiamo le operazioni che può eseguire con le macchine virtuali di Azure.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

- Creare una macchina virtuale con l'interfaccia della riga di comando di Azure
- Ridimensionare macchine virtuali con l'interfaccia della riga di comando di Azure.
- Eseguire attività di gestione di base tramite l'interfaccia della riga di comando di Azure.
- Connettersi a una macchina virtuale in esecuzione con SSH e l'interfaccia della riga di comando di Azure.

## <a name="prerequsites"></a>Prerequisiti

- Conoscenza di base dello strumento dell'interfaccia della riga di comando di Azure dal modulo **Controllare i servizi di Azure con l'interfaccia della riga di comando**.