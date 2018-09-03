L'azienda ha deciso di gestire i dati video dalle fotocamere del traffico in Azure mediante VM. Per eseguire più codec, è necessario innanzitutto creare le VM, nonché connetterle e interagirvi. In questa unità si apprenderà come creare una VM usando il portale di Azure. Verrà illustrato come configurare la VM per RDP, selezionare un'immagine di VM e scegliere l'opzione di archiviazione appropriata.

## <a name="introduction-to-windows-virtual-machines-in-azure"></a>Introduzione alle macchine virtuali Windows in Azure

Le VM di Azure sono risorse di cloud computing scalabili su richiesta. Sono simili alle macchine virtuali ospitate in Windows Hyper-V. Includono un processore, memoria, archiviazione e risorse di rete. È possibile avviare e arrestare le macchine virtuali in base alle esigenze come con Hyper-V e gestirle dal portale o mediante l'interfaccia della riga di comando di Azure. È anche possibile usare un client RDP per connettersi direttamente all'interfaccia utente del desktop di Windows e usare la VM come dopo aver eseguito l'accesso a un computer Windows locale.

## <a name="create-resources-for-a-windows-vm"></a>Creare risorse per una VM di Windows

Quando si crea una VM di Windows in Azure, è anche possibile creare risorse per ospitarla. Come per altri servizi di Azure, è necessario un **gruppo di risorse**. È prassi comune includere altri servizi correlati alla VM nello stesso gruppo di risorse, tra cui:

* Macchina virtuale
* Archiviazione
* Reti virtuali 
* Interfacce di rete
* Subnet
* Indirizzo IP pubblico

Quando si crea una nuova VM, è possibile usare un gruppo di risorse esistente o crearne uno nuovo.

## <a name="choose-the-vm-image"></a>Scegliere l'immagine della VM

La scelta di un'immagine è una delle decisioni principali e più importanti da prendere in fase di creazione di una VM. Un'immagine è un modello usato per creare una VM. Questi modelli includono un sistema operativo e, spesso, anche altri software, ad esempio strumenti di sviluppo o ambienti di hosting Web.

Tutto ciò che può essere installato in un computer può essere incluso in un'immagine. È possibile creare una VM da un'immagine preconfigurata esattamente per le attività necessarie, ad esempio l'hosting di un'app ASP.Net Core.

Facoltativamente, è possibile creare e caricare immagini personalizzate, ma questo argomento esula dal presente modulo.

> [!Note] 
> È possibile notare una funzionalità denominata "Macchina virtuale (versione classica)". Si tratta di una funzionalità legacy supportata per i clienti che hanno già creato istanze. Per i nuovi carichi di lavoro è consigliabile la funzionalità "Macchina virtuale di Azure" o, per brevità, "Macchine virtuali".

## <a name="sizing-your-vm"></a>Dimensionamento di una VM

Proprio come un computer fisico dispone di una determinata quantità di memoria e potenza della CPU, lo stesso vale per una macchina virtuale. Azure offre una serie di VM con dimensioni e prezzi diversi. Queste VM iniziano con immagini serie A di sviluppo e test di fascia bassa fino alla serie D per calcoli generici. Includono la serie H per esigenze di calcolo a prestazioni elevate e la serie N per ruoli ottimizzati per la GPU.

Come verrà affrontato più avanti nel modulo, è possibile modificare le dimensioni di una VM dopo averla creata, ma è necessario arrestarla, il che potrebbe determinare tempi di inattività per i carichi di lavoro.

## <a name="choosing-storage-options"></a>Scelta delle opzioni di archiviazione

La scelta successiva quando si specifica una VM è la tecnologia del disco rigido. Le opzioni includono un'unità disco rigido tradizionale basata su platter o un'unità SSD più moderna. L'archiviazione SSD è più dispendiosa, ma offre prestazioni migliori, in particolare in ambienti che supportano livelli elevati di trasferimento dati oppure I/O frequenti.

> [!Note] 
> Le applicazioni nelle macchine virtuali di Azure possono connettersi anche ad altre forme di persistenza in Azure, ad esempio Archiviazione BLOB di Azure e Azure Cosmos DB.
