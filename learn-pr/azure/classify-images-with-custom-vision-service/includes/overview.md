[Servizi cognitivi Microsoft](https://azure.microsoft.com/en-us/services/cognitive-services/ "Servizi cognitivi Microsoft") è una famiglia di servizi e API supportati dall'apprendimento automatico che permette agli sviluppatori di integrare funzionalità intelligenti come il riconoscimento facciale in foto e video, l'analisi del sentiment nel testo e il riconoscimento vocale nelle applicazioni. Il [Servizio visione artificiale personalizzato](https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/) è uno dei membri più nuovi della famiglia Servizi cognitivi. Lo scopo di questo servizio è creare modelli di classificazione delle immagini in grado di "apprendere" da immagini con etichetta fornite. Se, ad esempio, si vuole determinare se una foto contiene un'immagine di un fiore, è possibile eseguire il training del Servizio visione artificiale personalizzato con una raccolta di immagini di fiori perché il servizio sia grado di determinare se l'immagine successiva include un fiore e addirittura il tipo di fiore.

### <a name="what-is-covered-in-this-lab"></a>Contenuto del lab
In questo lab si eseguiranno le attività seguenti:
* Creare un progetto del Servizio visione artificiale personalizzato 
* Training di un modello del Servizio visione artificiale personalizzato con immagini con tag  
* Test di un modello del Servizio visione artificiale personalizzato 
* Creazione di app che sfruttano modelli del Servizio visione artificiale personalizzato tramite la chiamata di API REST

Per completare questo lab, è necessario un account Microsoft. Se non si ha un account, è possibile [iscriversi gratuitamente](https://account.microsoft.com/account). Saranno anche necessari Visual Studio Code e Node.js.