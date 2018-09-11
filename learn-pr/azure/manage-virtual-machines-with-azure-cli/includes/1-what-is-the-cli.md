Si supponga di dover gestire un set di macchine virtuali di Azure in esecuzione nell'infrastruttura Web aziendale, che include più siti Web e server di database eseguiti su diverse piattaforme. 

Il portale di Azure è facile da usare, ma i vari spostamenti da un pannello all'altro aumentano il tempo necessario per eseguire alcune attività. 

Tra le possibili alternative è disponibile lo strumento di interfaccia della riga di comando di Azure.

Tramite l'interfaccia della riga di comando di Azure, è possibile scrivere script per controllare lo stato dei server, distribuire una nuova configurazione, aprire una porta o connettersi a una macchina virtuale per modificare un'impostazione.

È lo strumento ideale per automatizzare le attività in un ambiente cloud. Di seguito si vedrà come usare l'interfaccia della riga di comando di Azure per creare e gestire macchine virtuali ospitate in Azure. 

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> Attualmente sono disponibili due versioni dello strumento di interfaccia della riga di comando di Azure: 1.0 e 2.0. In questa unità si userà l'interfaccia della riga di comando di Azure 2.0, ovvero la versione più recente e consigliata, a meno che non si debbano eseguire script legacy. La versione 1.0 dell'interfaccia della riga di comando di Azure viene avviata con il comando `azure`, mentre la versione 2.0 viene avviata con il comando `az`. 

L'interfaccia della riga di comando di Azure può essere utile per gestire le risorse di Azure, ad esempio macchine virtuali e dischi, dalla riga di comando o tramite script. Per iniziare, si analizzerà cosa è possibile fare.

## <a name="learning-objectives"></a>Obiettivi di apprendimento

Contenuto del modulo:

- Creare una macchina virtuale con l'interfaccia della riga di comando di Azure
- Ridimensionare macchine virtuali con l'interfaccia della riga di comando di Azure.
- Eseguire attività di gestione di base tramite l'interfaccia della riga di comando di Azure.
- Connettersi a una macchina virtuale in esecuzione con SSH e l'interfaccia della riga di comando di Azure.
