## <a name="docker"></a>Docker

![Logo di Docker](../media/3-image1.PNG)

Docker consente agli sviluppatori di creare un pacchetto, distribuire ed eseguire tutte le applicazioni come un contenitore leggero, portatile e autosufficiente che può essere eseguito praticamente ovunque. Se l'immagine di base della DSVM offre i più popolari framework di apprendimento avanzato preinstallati, perché c'è bisogno di inserire i client in contenitori come Docker?

Spesso quando si prova a eseguire attività di apprendimento avanzato, gli sviluppatori devono affrontare alcuni problemi in termini di dipendenze, ad esempio: 

- La necessità di creare pacchetti personalizzati: i ricercatori dell'apprendimento avanzato tendono a concentrarsi meno sulla produzione al momento della pubblicazione di codice su Github. Se riescono a ottenere un pacchetto che funziona nel loro ambiente di sviluppo, spesso pensano che anche gli altri utenti saranno in grado di farlo.
- Controllo delle versioni del driver GPU: CUDA è una piattaforma di calcolo parallela e un'API (Application Programming Interface) sviluppata da Nvidia. Consente a sviluppatori di software e ingegneri software di usare un'unità di elaborazione grafica (GPU) basata su CUDA per l'elaborazione per uso generico. Alcune versioni di Tensorflow non funzionano con le versioni di CUDA successive alla 9.1, mentre altri framework, come ad esempio PyTorch, dovrebbero offrire prestazioni migliori con le versioni successive di CUDA.

Per aggirare questi problemi e aumentare le possibilità di uso del codice, è possibile usare Docker o la sua variante GPU Nvidia-Docker per gestire ed eseguire i progetti di apprendimento avanzato. 

<!--Quiz 
What is CUDA? 
What versioning issues do deep learning engineers deal with? -->