## <a name="intro-to-deep-learning"></a>Introduzione all'apprendimento profondo

L'obiettivo dell'apprendimento automatico (ML, Machine Learning) è quello di trovare le funzioni per eseguire il training di un modello che trasforma i dati di input (ad esempio immagini, serie temporali o audio) in un determinato output (ad esempio didascalie, valori dei prezzi, trascrizioni). In uno scenario di data science tradizionale, le funzioni vengono spesso selezionate manualmente.

![Esempio canonico di rete neurale profonda di tipo feed-forward.](../media/2-image1.PNG)

Nell'apprendimento profondo (DL, Deep Learning), il processo di estrazione delle funzioni viene appreso tramite la rappresentazione degli input come vettori e la loro trasformazione, con una serie di operazioni algebriche lineari intelligenti, in un determinato output.  

L'output del modello viene quindi confrontato con l'output previsto usando un'equazione detta funzione di perdita. Il valore restituito dalla funzione di perdita di ciascun input di training viene usato per guidare il modello nell'estrazione delle funzioni, con una conseguente riduzione del valore della perdita al passaggio successivo.  
 
La serie di operazioni di matrice che vengono calcolate come parte del componente algebrico lineare tende a essere costosa dal punto di vista del calcolo e spesso consente un livello elevato di parallelizzazione. Sono quindi necessari strumenti specialistici, ad esempio unità di elaborazione grafica (GPU), per eseguire il calcolo in modo efficiente.

## <a name="data-science-virtual-machine"></a>Data Science Virtual Machine

![Opzioni delle DSVM](../media/2-image2.PNG)

Le DSVM sono immagini di macchine virtuali di Azure preinstallate, configurate e testate con alcuni degli strumenti più diffusi usati comunemente per analisi dei dati, apprendimento automatico e training di apprendimento profondo.

Offrono le caratteristiche seguenti:

- Configurazione coerente tra i team, semplificazione di condivisione e collaborazione, scalabilità e gestione di Azure, configurazione ridotta al minimo, desktop completo basato sul cloud per attività di data science.
- Capacità elastica on demand con la possibilità di eseguire analisi in tutte le configurazioni hardware di Azure con scalabilità orizzontale e verticale. Pagamento solo delle risorse usate nel momento in cui vengono usate.
- Apprendimento profondo con GPU e cluster di GPU subito disponibili con strumenti di apprendimento profondo già preconfigurati. 

La DSVM contiene diversi strumenti per l'intelligenza artificiale, tra cui popolari edizioni GPU dei framework di apprendimento profondo e strumenti come Microsoft R Server Developer Edition, Anaconda Python, notebook Jupyter per Python e R, IDE per Python e R, database SQL e molti altri strumenti di data science e apprendimento automatico.

È possibile eseguire la DSVM in istanze di macchine virtuali serie NC con GPU di Azure. Queste GPU usano l'assegnazione discreta dei dispositivi con conseguenti prestazioni quasi di livello bare metal e sono particolarmente adatte per i problemi di apprendimento profondo.

<!--### Quiz? 

What is the goal of machine learning? 
How is traditional machine learning different from deep learning? 
Why are GPU's often used for deep learning? 
What does the DSVM provide? -->
