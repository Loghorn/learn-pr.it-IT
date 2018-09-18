È stata creata un'applicazione serverless completa tramite i servizi di Azure.

## <a name="clean-up"></a>Eseguire la pulizia
<!---TODO: Update for sandbox--->

Dopo aver usato questa applicazione, è possibile eseguire il comando seguente per eliminare tutte le risorse create durante l'esercitazione:

```azurecli
az group delete --name first-serverless-app
```

Quando richiesto, digitare `y`.  

## <a name="next-steps"></a>Passaggi successivi

In questo modulo si è appreso come:
  - Configurare l'archiviazione BLOB di Azure per ospitare un sito Web statico e le immagini caricate.
  - Caricare le immagini nell'archiviazione BLOB di Azure con Funzioni di Azure.
  - Ridimensionare le immagini con Funzioni di Azure.
  - Archiviare i metadati delle immagini in Azure Cosmos DB. 
  - Usare l'API Visione artificiale di Servizi cognitivi per generare automaticamente le didascalie delle immagini.
  - Usare Azure Active Directory per proteggere l'app Web tramite l'autenticazione degli utenti.

Per informazioni su come connettere ancora più servizi a Funzioni, continuare con l'esercitazione [Funzioni con app per la logica](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email).