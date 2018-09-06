L'app Web Alpine Ski House è attiva e in esecuzione, ora è necessario farla vedere al capo. È necessario aggiornare il sito e pubblicare gli aggiornamenti in Azure.

## <a name="update-your-web-app"></a>Aggiornare l'app Web

### <a name="replace-the-boilerplate-code"></a>Sostituire il codice boilerplate

1. Nella cartella **Pagine** aprire il file **About.cshtml**.

1. Nella parte inferiore del codice individuare `<p> Use this area to provide additional information. </p>`

1. Sostituire il testo boilerplate con `Welcome to the Alpine Ski House!`

1. Salvare il file.

1. Aprire il file **About.cshtml.cs**.

1. Sostituire la stringa `Message` per dire **Alpine Ski House è la località sciistica numero uno del nordest.**

1. Salvare il file.

### <a name="publish-your-updates"></a>Pubblicare gli aggiornamenti

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.

1. Selezionare **Pubblica**. Deve essere visualizzata un'opzione che include la distribuzione web di [sitoWeb].

1. Selezionare il sito. Visual Studio invierà le modifiche a Azure.

### <a name="view-your-changes"></a>Visualizzare le modifiche

Dopo aver pubblicato le modifiche, Visual Studio aprirà il sito nel browser. Passare alla pagina **Informazioni** e verificare che rifletta le modifiche apportate.

## <a name="congrats"></a>Congratulazioni.

L'app Web è stata aggiornata con Visual Studio 2017.
