Si supponga di dover gestire un set di macchine virtuali di Azure in esecuzione nell'infrastruttura Web aziendale, che include più siti Web e server di database eseguiti su diverse piattaforme. 

Il portale di Azure è facile da usare, ma i vari spostamenti da un pannello all'altro aumentano il tempo necessario per eseguire alcune attività. 

Tra le possibili alternative è disponibile lo strumento della riga di comando di Azure.

Tramite l'interfaccia della riga di comando di Azure, è possibile scrivere script per controllare lo stato dei server, distribuire una nuova configurazione, aprire una porta o connettersi a una macchina virtuale per modificare un'impostazione.

È lo strumento ideale per automatizzare le attività in un ambiente cloud. Di seguito si vedrà come usare l'interfaccia della riga di comando di Azure per creare e gestire macchine virtuali ospitate in Azure. 

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

L'interfaccia della riga di comando di Azure è lo strumento da riga di comando multipiattaforma di Microsoft per la gestione delle risorse di Azure. È disponibile per macOS, Linux e Windows oppure nel browser, tramite [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> Sono attualmente disponibili due versioni dell'interfaccia della riga di comando di Azure: 1.0 e 2.0. In questa unità si userà l'interfaccia della riga di comando di Azure 2.0, ovvero la versione più recente e consigliata, a meno che non si debbano eseguire script legacy. La versione 1.0 dell'interfaccia della riga di comando di Azure viene avviata con il comando `azure`, mentre la versione 2.0 viene avviata con il comando `az`. 

L'interfaccia della riga di comando di Azure può essere utile per gestire le risorse di Azure, ad esempio macchine virtuali e dischi, dalla riga di comando o tramite script. Per iniziare, di seguito sono elencate le attività che è possibile svolgere.

## <a name="learning-objectives"></a>Obiettivi di apprendimento
In questo modulo verrà descritto come:

- Creare una macchina virtuale di Azure con l'interfaccia della riga di comando.
- Ridimensionare le macchine virtuali con l'interfaccia della riga di comando.
- Eseguire attività di gestione e query su una macchina virtuale dalla riga di comando.
- Installare software in una macchina virtuale.
