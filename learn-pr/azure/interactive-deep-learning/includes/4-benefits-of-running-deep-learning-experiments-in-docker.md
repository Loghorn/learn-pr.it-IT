![Logo di Docker](../media/3-image1.PNG)

Docker è uno strumento che consente di distribuire le applicazioni in un ambiente sandbox da eseguire in un sistema operativo di propria scelta. Consente di creare un pacchetto dell'app con tutte le relative dipendenze in un'unità standardizzata. Ma se con i framework di apprendimento avanzato più noti viene fornita l'immagine di base di DSVM già preinstallata, perché si dovrebbe usare Docker?

Quando gli sviluppatori provano a eseguire attività di apprendimento avanzato, devono affrontare alcuni problemi in termini di dipendenze, ad esempio: 

- La necessità di creare pacchetti personalizzati: i ricercatori nell'apprendimento avanzato tendono a pensare poco alla produzione quando pubblicano codice su GitHub. Se riescono a ottenere un pacchetto che funziona nell'ambiente dello sviluppo, spesso pensano che anche gli altri utenti saranno in grado di farlo.
- Controllo delle versioni del driver GPU: CUDA è una piattaforma di calcolo parallelo e un'API (Application Programming Interface) sviluppate da NVIDIA. Consente agli sviluppatori di usare un'unità di elaborazione grafica (GPU) basata su CUDA per l'elaborazione per uso generico. Alcune versioni di Tensorflow non funzionano con le versioni di CUDA successive alla 9.1. Altri framework, ad esempio PyTorch, sembrano offrire prestazioni migliori con le versioni successive di CUDA.

Per aggirare questi problemi e aumentare l'usabilità del codice, è possibile usare Docker o la variante GPU Nvidia-Docker per gestire ed eseguire i progetti di apprendimento avanzato. 

<!--Quiz 
What is CUDA? 
What versioning issues do deep learning engineers deal with? -->