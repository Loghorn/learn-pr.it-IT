In questa unità, si creerà un'app web ASP.NET Core.

## <a name="create-a-new-web-project"></a>Creare un nuovo progetto Web

La funzionalità degli strumenti della CLI di .NET si il `dotnet` strumento da riga di comando. Tramite questo comando, si creerà un nuovo progetto Web ASP.NET Core.

1. In Cloud Shell a destra, creare una nuova applicazione ASP.NET Core MVC. Denominarla "BestBikeApp".

```bash
dotnet new mvc -name BestBikeApp
```

1. Il comando creerà una nuova cartella denominata "BestBikeApp" per contenere il progetto. Passare alla cartella.

1. Compilare ed eseguire l'applicazione per verificare che sia completata.

```bash
dotnet run
```

Dovrebbe essere simile a:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

L'output descrive la situazione dopo avere avviato l'app: l'applicazione è in esecuzione e in ascolto sulla porta 5000.

Se fosse in esecuzione l'app nel nostro computer saremo in grado di aprire un browser per http://localhost:5000. Poiché si usa Azure Cloud Shell, è necessario distribuire l'app a una destinazione con un endpoint pubblico. È ideale per tale istanza del servizio App creato in precedenza.