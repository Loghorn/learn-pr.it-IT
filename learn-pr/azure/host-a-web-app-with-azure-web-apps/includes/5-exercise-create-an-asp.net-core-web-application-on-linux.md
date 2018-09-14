In questa unità verrà creata un'app web ASP.NET Core tramite l'interfaccia della riga di comando di .NET in un computer Ubuntu.

## <a name="aspnet-core-installation-on-linux-environment"></a>Installazione di ASP.NET Core in ambiente Linux

Visitare la [pagina dei download di .NET](https://www.microsoft.com/net/download) di Microsoft e seguire gli stessi passaggi indicati nella pagina di .NET Core SDK. Sono riportati di seguito. Per eseguire i comandi, è necessario aprire una nuova istanza della riga di comando del **terminale**.

### <a name="register-microsoft-key-and-feed"></a>Registrare la chiave Microsoft e il feed

Prima di installare .NET è necessario registrare la chiave Microsoft, registrare il repository dei prodotti e installare le dipendenze necessarie. Questa operazione deve essere eseguita una volta sola per ogni computer.

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-sdk"></a>Installare .NET SDK

Aggiornare i prodotti disponibili per l'installazione, quindi installare .NET SDK.

Al prompt dei comandi eseguire i comandi seguenti:

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

Durante il processo di installazione potrebbe essere richiesto di immettere la password dell'account. Specificare la password e premere INVIO per continuare.

Digitare quanto segue per verificare l'installazione:

```console
dotnet --version
```

L'output restituito è:

```console
2.1.302
```

Ora che .NET Core SDK è installato, è stato installato anche ASP.NET Core. A questo punto verrà illustrato come usare l'interfaccia della riga di comando di .NET per creare un nuovo progetto ASP.NET Core usando i comandi appena appresi.

## <a name="open-a-terminal-window"></a>Aprire una finestra terminale

In primo luogo, è necessario aprire la finestra Terminale. La finestra terminale consente di eseguire comandi e ha un ruolo simile alla finestra del prompt dei comandi di Windows.

## <a name="create-a-new-web-project"></a>Creare un nuovo progetto Web

Il fulcro degli strumenti dell'interfaccia della riga di comando di .NET è lo strumento driver *dotnet*. Tramite questo comando, si creerà un nuovo progetto Web ASP.NET Core.

Per creare una nuova applicazione ASP.NET Core MVC, è sufficiente digitare i comandi seguenti:

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

Con i comandi precedenti si passa alla cartella radice *Documenti* e quindi si crea una nuova cartella. Si passa quindi all'interno di questa cartella e infine si invia il comando dell'interfaccia della riga di comando di .NET per generare una nuova applicazione ASP.NET MVC con tutti i pacchetti ripristinati:

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

Ora è sufficiente eseguire l'applicazione inviando questo comando:

```console
dotnet run
```

Nel *terminale* il comando *dotnet* consente di visualizzare alcune informazioni utili per l'app in esecuzione:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

L'output descrive la situazione dopo l'avvio dell'app: l'applicazione è in esecuzione e in ascolto sulla porta 5001 e 5002 (tramite HTTPS).

Per eseguire localmente HTTPS nel computer in modalità di sviluppo, è necessario creare e considerare attendibile un **certificato autofirmato**.

## <a name="create-a-self-signed-certificate"></a>Creare un certificato autofirmato

Eseguire il comando seguente per generare un certificato autofirmato di sviluppo:

```console
dotnet dev-certs https -v
```

L'esecuzione del comando restituisce l'output seguente:

```console
The HTTPS developer certificate was generated successfully.
```

## <a name="run-the-application"></a>Eseguire l'applicazione

Per questa dimostrazione verrà usato il browser Firefox.

Aprire il browser e digitare l'indirizzo `http://localhost:5001` per verificare che l'applicazione funzioni correttamente.

![Screenshot che mostra la pagina Web predefinita del modello ASP.NET Core MVC nel Web browser.](../media/5-asp-core-mvc-default-template.PNG)

> [!NOTE]
> È necessario **aggiungere un'eccezione** per l'URL dell'applicazione perché Firefox non è stato in grado di verificare il certificato autofirmato per lo sviluppo.
