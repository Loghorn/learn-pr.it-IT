In questa unità si creerà un servizio API Visione artificiale usando l'interfaccia della riga di comando di Azure.

# <a name="exercise-create-a-computer-vision-service"></a>Esercizio: Creare un servizio Visione artificiale

[Visione artificiale](/azure/cognitive-services/computer-vision/home) fa parte di [Servizi cognitivi Microsoft](/azure/cognitive-services/welcome), un set completo di algoritmi di apprendimento automatico disponibile come servizio che chiunque può usare. Invece di generare dati sufficienti per il training di un modello da zero, vengono resi disponibili modelli di cui è già stato eseguito il training, che chiunque può usare.

Tra i molti disponibili, vengono offerti servizi in grado di gestire voce, immagini, video, ricerca, linguaggio e altro ancora.

Questo esercizio è incentrato sulla creazione del servizio necessario per gestire immagini. Al termine dell'esercizio, si sarà in grado di creare un endpoint API capace di identificare immagini.

# <a name="create-a-computer-vision-api-service"></a>Creare un servizio API Visione artificiale

Prima di creare il servizio API Visione artificiale, è necessario un *gruppo di risorse* in cui distribuire il servizio. Un gruppo di risorse è una raccolta logica in cui vengono distribuite e gestite tutte le risorse di Azure.

```azurecli
az group create --name ComputerVisionRG --location westus2
```

Dopo aver creato il gruppo di risorse, creare il servizio con il comando `az cognitiveservices account create`. Nome del servizio 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
