In questa unità verrà creata un'app Web ASP.NET Core.

## <a name="create-a-new-web-project"></a>Creare un nuovo progetto Web

Il nucleo degli strumenti dell'interfaccia della riga di comando .NET è rappresentato dallo strumento da riga di comando `dotnet`. Tramite questo comando si creerà un nuovo progetto Web ASP.NET Core.

In Cloud Shell a destra, creare una nuova applicazione ASP.NET Core MVC. Denominarla "BestBikeApp".

```bash
dotnet new mvc --name BestBikeApp
```

Il comando creerà una nuova cartella denominata "BestBikeApp" per contenere il progetto. `cd`, quindi compilare ed eseguire l'applicazione per verificare che sia completa.

```bash
cd BestBikeApp
dotnet run
```

L'output dovrebbe essere simile al seguente:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

L'output descrive la situazione dopo l'avvio dell'app: l'applicazione è in esecuzione e in ascolto sulla porta 5000.

Se l'app fosse in esecuzione nel computer locale, sarebbe possibile aprire una finestra del browser per http://localhost:5000. Dato che si usa Azure Cloud Shell, però, sarà necessario distribuire l'app in una posizione con un endpoint pubblico. L'istanza del servizio app creata in precedenza è perfetta a tale scopo.