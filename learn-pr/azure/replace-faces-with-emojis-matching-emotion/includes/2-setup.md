Prima di tutto occorre prima verificare che si sono impostati con alcuni account e abbiano il codice di avvio clonato in locale.

## <a name="create-an-azure-account"></a>Creare un Account di Azure

È necessario un account in Azure. Se si ha già uno, è possibile l'iscrizione e ottenere il patrimonio di un anno gratuito dei servizi, seguire questo collegamento:

[Creare Account di Azure](https://azure.microsoft.com/free)

## <a name="create-a-slack-workspace"></a>Creare un'area di lavoro di Slack

Per creare un comando di slack, sono necessari privilegi di amministratore in un'area di lavoro slack.

Pertanto, è necessario assicurarsi di disporre dei privilegi di amministratore per un'area di lavoro esistente o creare una nuova area di lavoro slack, nel modo seguente:

[Creare area di lavoro di Slack](https://slack.com/create)

## <a name="clone-the-starter-code"></a>Clonare il codice di avvio

Per ottenere risultati ottimali da questo modulo, consigliabile clonare il codice di avvio e seguire le istruzioni dettagliate.

Forniremo tutto il codice che necessario per completare l'applicazione, nonché un codice di bootstrap iniziale per iniziare a usare.

Per iniziare a clonare questo _stater_ codice nel computer locale.

```bash
git clone https://github.com/jawache/mojifier-slack.git
```

Quindi installare i pacchetti necessari usando:

```bash
npm install
```

È consigliabile che seguire questo modulo passo a passo. Tuttavia, se si bloccarsi e l'assistenza è possibile trovare il codice completo nel `completed` branch, come illustrato di seguito:

```bash
git checkout completed
```

Si intende scrivere l'applicazione usando `TypeScript`. NodeJS non sa come **eseguiti** `TypeScript`, pertanto è necessario convertire come sviluppare nostro `TypeScript` codice a `JavaScript`. Tutti gli strumenti necessari per eseguire la conversione è stato installato nel passaggio precedente per convertire il `TypeScript` eseguire questo comando:

```bash
npm run build
```

Se il comando precedente è stato eseguito correttamente accanto al `*.ts` file, a questo punto dovrebbe vedere anche `*.js` e `*.map` file.

> **Nota**
>
> Tenere questo comando in esecuzione in una shell del terminale, si verifica la presenza di eventuali modifiche ai file TypeScript e li converte in file JavaScript.
>
> Se per qualche motivo che i file non sono quindi Introduzione convertito controlla l'output nella finestra del terminale della console, potrebbe esserci errori nel codice TypeScript.

## <a name="install-visual-studio-code--azure-functions-extention"></a>Installare Visual Studio Code e dell'estensione di funzioni di Azure

Esistono molti modi, che è possibile interagire con Azure, in questo modulo che interazione con Azure usando Visual Studio Code e il plug-in estensione di funzioni di Azure associata.

1. Scaricare e installare visual studio code da https://code.visualstudio.com/

2. Installare le estensioni di codice di funzioni di Azure seguendo le istruzioni riportate qui: https://code.visualstudio.com/tutorials/functions-extension/getting-started
