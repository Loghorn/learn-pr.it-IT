La società ha deciso di investire in un sistema migliore per rilevare i difetti nei moduli dei pannelli solari che produce. Il controllo di qualità manuale è molto utile per rilevare incrinature strutturali evidenti nei moduli. L'8% dei moduli consegnati ai clienti, tuttavia, presenta ancora difetti. È pertanto opportuno aggiungere elementi di automazione per ridurre la frequenza di errore e risparmiare denaro. Il manager ha già avviato il processo di acquisto di apparecchiature video per l'analisi dei moduli. Il compito del responsabile dello sviluppo è quello di trovare una soluzione per rilevare i difetti esaminando le immagini dei moduli. Dopo aver eseguito una ricerca, il responsabile dello sviluppo decide di provare un approccio basato su intelligenza artificiale, ovvero eseguire il training di una rete neurale per rilevare i difetti. 

La creazione della catena di strumenti adatti per l'apprendimento avanzato è onerosa in termini di tempo e denaro. L'investimento in hardware per eseguire il training di un modello è infatti costoso. L'uso combinato di Azure e di una macchina virtuale DSVM (Data Science Virtual Machine) rappresenta un'ottima opzione per controllare i costi e applicare la scalabilità in base alle esigenze. Si ottengono tutti gli strumenti di apprendimento avanzato necessari nonché la possibilità di distribuire macchine virtuali potenti per esaminare i dati ed eseguire il training di una rete neurale.  

In questo modulo viene creata una macchina virtuale DSVM e viene eseguito il training del primo modello. 

## <a name="learning-objectives"></a>Obiettivi di apprendimento

In questo modulo verrà descritto come:

  - Descrivere l'apprendimento avanzato e la macchina virtuale DSVM (Data Science Virtual Machine) di Azure
  - Creare una macchina virtuale DSVM con l'interfaccia della riga di comando di Azure
  - Informazioni su come usare Docker per configurare un ambiente di apprendimento avanzato personalizzato
  - Connettersi ai notebook di Jupyter tramite Docker in DSVM
  - Informazioni su come eseguire il primo esperimento di apprendimento avanzato con Jupyter