I contenitori si stanno affermando come soluzione preferita per la creazione di pacchetti, la distribuzione e la gestione di applicazioni cloud. Istanze di contenitore di Azure rappresenta il modo più semplice e rapido per eseguire un contenitore in Azure, senza dover gestire macchine virtuali né dover adottare un servizio di livello superiore.

Istanze di contenitore di Azure è un'ottima soluzione per qualsiasi scenario e funziona anche in contenitori isolati, inclusi i processi di compilazione, l'automazione di attività e le applicazioni semplici. Per gli scenari in cui è necessaria l'orchestrazione completa dei contenitori, quali il rilevamento dei servizi tra più contenitori, la scalabilità automatica e gli aggiornamenti coordinati delle applicazioni, è consigliabile usare Azure Kubernetes Service (AKS).

La soluzione Istanze di contenitore di Azure offre i vantaggi seguenti:

- **Avvio rapido**: Istanze di contenitore di Azure può avviare i contenitori in Azure in pochi secondi.
- **Fatturazione al secondo**: viene addebitato solo il tempo in cui il contenitore è in esecuzione.
- **Sicurezza di livello hypervisor**: Istanze di contenitore di Azure garantisce all'applicazione in un contenitore lo stesso livello di isolamento di cui usufruirebbe in una macchina virtuale.
- **Dimensioni personalizzate**: Istanze di contenitore di Azure garantisce un utilizzo ottimale grazie alla possibilità di specificare esattamente la quantità di memoria e di core della CPU.
- **Archiviazione permanente**: per recuperare e salvare lo stato in modo permanente con Istanze di contenitore di Azure, è disponibile il montaggio diretto di condivisioni di File di Azure.
- **Linux e Windows**: Istanze di contenitore di Azure consente di pianificare contenitori Windows e Linux con la stessa API.

## <a name="learning-objectives"></a>Obiettivi di apprendimento  

In questo modulo verrà descritto come:

- Eseguire un contenitore in Istanze di contenitore di Azure.
- Gestire il comportamento di riavvio dei contenitori.
- Usare variabili di ambiente nei contenitori.
- Collegare volumi di dati a Istanze di contenitore di Azure.
- Acquisire informazioni sulla risoluzione dei problemi per Istanze di contenitore di Azure.
