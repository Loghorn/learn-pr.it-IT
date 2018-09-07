<span data-ttu-id="2b256-101">L'autenticazione del servizio app di Azure abilita il supporto dell'autenticazione chiavi in mano nell'app Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="2b256-101">Azure App Service authentication enables turn-key authentication support in an Azure Functions app.</span></span> <span data-ttu-id="2b256-102">Si integra perfettamente con Facebook, Twitter, account Microsoft, Google e Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2b256-102">It integrates seamlessly with Facebook, Twitter, Microsoft accounts, Google, and Azure Active Directory.</span></span> <span data-ttu-id="2b256-103">È possibile aggiungere l'autenticazione del servizio app per proteggere le API back-end dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="2b256-103">You'll add App Service authentication to protect the back-end APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="2b256-104">Abilitare l'autenticazione del servizio app</span><span class="sxs-lookup"><span data-stu-id="2b256-104">Enable App Service authentication</span></span>

1. <span data-ttu-id="2b256-105">Aprire l'app per le funzioni nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="2b256-105">Open the function app in the Azure portal.</span></span>

1. <span data-ttu-id="2b256-106">In **Funzionalità della piattaforma** selezionare **Autenticazione/Autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="2b256-106">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Selezionare Autenticazione e Autorizzazione](../media/6-authorization.jpg)


1. <span data-ttu-id="2b256-108">Selezionare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="2b256-108">Select the following values:</span></span>
    
    | <span data-ttu-id="2b256-109">Impostazione</span><span class="sxs-lookup"><span data-stu-id="2b256-109">Setting</span></span>      |  <span data-ttu-id="2b256-110">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="2b256-110">Suggested value</span></span>   | <span data-ttu-id="2b256-111">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2b256-111">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="2b256-112">**Autenticazione servizio app**</span><span class="sxs-lookup"><span data-stu-id="2b256-112">**App Service Authentication**</span></span> | <span data-ttu-id="2b256-113">Attivato</span><span class="sxs-lookup"><span data-stu-id="2b256-113">On</span></span> | <span data-ttu-id="2b256-114">Abilitare l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2b256-114">Enable authentication.</span></span> |
    | <span data-ttu-id="2b256-115">**Azione da eseguire quando la richiesta non è autenticata**</span><span class="sxs-lookup"><span data-stu-id="2b256-115">**Action when request is not authenticated**</span></span> | <span data-ttu-id="2b256-116">Accedere con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2b256-116">Sign in with Azure Active Directory.</span></span> | <span data-ttu-id="2b256-117">Selezionare un metodo di autenticazione configurato (vedere qui di seguito).</span><span class="sxs-lookup"><span data-stu-id="2b256-117">Select a configured authentication method (See below).</span></span> |
    | <span data-ttu-id="2b256-118">**Provider di autenticazione**</span><span class="sxs-lookup"><span data-stu-id="2b256-118">**Authentication Providers**</span></span> | <span data-ttu-id="2b256-119">Vedere qui di seguito.</span><span class="sxs-lookup"><span data-stu-id="2b256-119">See below.</span></span> | <span data-ttu-id="2b256-120">Vedere qui di seguito.</span><span class="sxs-lookup"><span data-stu-id="2b256-120">See below.</span></span> |
    | <span data-ttu-id="2b256-121">**Archivio token**</span><span class="sxs-lookup"><span data-stu-id="2b256-121">**Token store**</span></span> | <span data-ttu-id="2b256-122">Attivato</span><span class="sxs-lookup"><span data-stu-id="2b256-122">On</span></span> | <span data-ttu-id="2b256-123">Consente al servizio app di archiviare e gestire i token.</span><span class="sxs-lookup"><span data-stu-id="2b256-123">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="2b256-124">**URL di reindirizzamento esterni consentiti**</span><span class="sxs-lookup"><span data-stu-id="2b256-124">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="2b256-125">L'URL dell'applicazione, ad esempio https://firstserverlessweb.z4.web.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="2b256-125">The URL of your application, for example https://firstserverlessweb.z4.web.core.windows.net/.</span></span> | <span data-ttu-id="2b256-126">URL a cui può essere reindirizzato il servizio app dopo l'autenticazione di un utente.</span><span class="sxs-lookup"><span data-stu-id="2b256-126">URLs that App Service is allowed to redirect to, after a user is authenticated.</span></span> |

1. <span data-ttu-id="2b256-127">Selezionare **Azure Active Directory** per visualizzare **Impostazioni di Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2b256-127">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="2b256-128">Selezionare **Rapida** come **Modalità di gestione** e inserire le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="2b256-128">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="2b256-129">Impostazione</span><span class="sxs-lookup"><span data-stu-id="2b256-129">Setting</span></span>      |  <span data-ttu-id="2b256-130">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="2b256-130">Suggested value</span></span>   | <span data-ttu-id="2b256-131">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2b256-131">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="2b256-132">**Modalità di gestione**</span><span class="sxs-lookup"><span data-stu-id="2b256-132">**Management mode**</span></span> | <span data-ttu-id="2b256-133">Rapida, Crea nuova app AD</span><span class="sxs-lookup"><span data-stu-id="2b256-133">Express, Create new AD app</span></span> | <span data-ttu-id="2b256-134">Configurare automaticamente un'entità servizio e l'autenticazione di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2b256-134">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="2b256-135">**Creare un'app**</span><span class="sxs-lookup"><span data-stu-id="2b256-135">**Create app**</span></span> | <span data-ttu-id="2b256-136">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="2b256-136">my-serverless-webapp</span></span> | <span data-ttu-id="2b256-137">Immettere un nome applicazione univoco.</span><span class="sxs-lookup"><span data-stu-id="2b256-137">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="2b256-138">Fare clic su **OK** per salvare le impostazioni di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2b256-138">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Impostazioni di Autenticazione, Autorizzazione e Azure Active Directory](../media/6-create-aad.png)


1. <span data-ttu-id="2b256-140">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="2b256-140">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="2b256-141">Modificare l'app Web per abilitare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="2b256-141">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="2b256-142">In Cloud Shell assicurarsi che la directory corrente sia la cartella **www/dist**.</span><span class="sxs-lookup"><span data-stu-id="2b256-142">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="2b256-143">Per abilitare l'autenticazione nell'app per le funzioni, aggiungere la riga di codice seguente al file **settings.js**:</span><span class="sxs-lookup"><span data-stu-id="2b256-143">To enable authentication in your function app, append the following line of code to the **settings.js** file:</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="2b256-144">Effettuare la modifica e visualizzare il risultato eseguendo i comandi seguenti o usando un editor da riga di comando come VIM.</span><span class="sxs-lookup"><span data-stu-id="2b256-144">Make the change and view the result by using the following commands, or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="2b256-145">Verificare che la modifica sia stata effettuata nel file.</span><span class="sxs-lookup"><span data-stu-id="2b256-145">Confirm that the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="2b256-146">Caricare il file nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="2b256-146">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="2b256-147">Test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="2b256-147">Test the application</span></span>

1. <span data-ttu-id="2b256-148">Aprire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="2b256-148">Open the application in a browser.</span></span> <span data-ttu-id="2b256-149">Fare clic su **Accedi** ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="2b256-149">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="2b256-150">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="2b256-150">Select an image file and upload it.</span></span>

    ![Pagina di accesso](../media/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="2b256-152">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2b256-152">Summary</span></span>

<span data-ttu-id="2b256-153">In questa unità è stato descritto come aggiungere l'autenticazione all'applicazione usando l'autenticazione del servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="2b256-153">In this unit, you learned how to add authentication to the application using Azure App Service authentication.</span></span>