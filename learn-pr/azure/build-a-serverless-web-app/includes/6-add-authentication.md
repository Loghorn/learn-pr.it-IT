<span data-ttu-id="6b6c4-101">L'autenticazione del servizio app di Azure abilita il supporto dell'autenticazione chiavi in mano nell'app Funzioni di Azure.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-101">Azure App Service authentication enables turn-key authentication support in an Azure Functions app.</span></span> <span data-ttu-id="6b6c4-102">Si integra perfettamente con Facebook, Twitter, account Microsoft, Google e Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-102">It integrates seamlessly with Facebook, Twitter, Microsoft accounts, Google, and Azure Active Directory.</span></span> <span data-ttu-id="6b6c4-103">È possibile aggiungere l'autenticazione del servizio app per proteggere le API back-end dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-103">You'll add App Service authentication to protect the back-end APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="6b6c4-104">Abilitare l'autenticazione del servizio app</span><span class="sxs-lookup"><span data-stu-id="6b6c4-104">Enable App Service authentication</span></span>

1. <span data-ttu-id="6b6c4-105">Accedere al [portale di Azure](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) usando lo stesso account con cui si è attivata la sandbox.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-105">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="6b6c4-106">Aprire l'app per le funzioni.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-106">Open the function app.</span></span>

1. <span data-ttu-id="6b6c4-107">In **Funzionalità della piattaforma** selezionare **Autenticazione/Autorizzazione**.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-107">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Selezionare Autenticazione e Autorizzazione](../media/6-authorization.jpg)

1. <span data-ttu-id="6b6c4-109">Selezionare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="6b6c4-109">Select the following values:</span></span>

    | <span data-ttu-id="6b6c4-110">Impostazione</span><span class="sxs-lookup"><span data-stu-id="6b6c4-110">Setting</span></span>      |  <span data-ttu-id="6b6c4-111">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="6b6c4-111">Suggested value</span></span>   | <span data-ttu-id="6b6c4-112">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6b6c4-112">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6b6c4-113">**Autenticazione servizio app**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-113">**App Service Authentication**</span></span> | <span data-ttu-id="6b6c4-114">Attivato</span><span class="sxs-lookup"><span data-stu-id="6b6c4-114">On</span></span> | <span data-ttu-id="6b6c4-115">Abilitare l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-115">Enable authentication.</span></span> |
    | <span data-ttu-id="6b6c4-116">**Azione da eseguire quando la richiesta non è autenticata**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-116">**Action when request is not authenticated**</span></span> | <span data-ttu-id="6b6c4-117">Accedere con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-117">Sign in with Azure Active Directory.</span></span> | <span data-ttu-id="6b6c4-118">Selezionare un metodo di autenticazione configurato (vedere qui di seguito).</span><span class="sxs-lookup"><span data-stu-id="6b6c4-118">Select a configured authentication method (See below).</span></span> |
    | <span data-ttu-id="6b6c4-119">**Provider di autenticazione**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-119">**Authentication Providers**</span></span> | <span data-ttu-id="6b6c4-120">Vedere qui di seguito.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-120">See below.</span></span> | <span data-ttu-id="6b6c4-121">Vedere qui di seguito.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-121">See below.</span></span> |
    | <span data-ttu-id="6b6c4-122">**Archivio token**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-122">**Token store**</span></span> | <span data-ttu-id="6b6c4-123">Attivato</span><span class="sxs-lookup"><span data-stu-id="6b6c4-123">On</span></span> | <span data-ttu-id="6b6c4-124">Consente al servizio app di archiviare e gestire i token.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-124">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="6b6c4-125">**URL di reindirizzamento esterni consentiti**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-125">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="6b6c4-126">L'URL dell'applicazione, ad esempio https://firstserverlessweb.z4.web.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-126">The URL of your application, for example https://firstserverlessweb.z4.web.core.windows.net/.</span></span> | <span data-ttu-id="6b6c4-127">URL a cui può essere reindirizzato il servizio app dopo l'autenticazione di un utente.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-127">URLs that App Service is allowed to redirect to, after a user is authenticated.</span></span> |

1. <span data-ttu-id="6b6c4-128">Selezionare **Azure Active Directory** per visualizzare **Impostazioni di Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-128">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="6b6c4-129">Selezionare **Rapida** come **Modalità di gestione** e inserire le informazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-129">Select **Express** as the **Management Mode** and fill in the following information.</span></span>

        | <span data-ttu-id="6b6c4-130">Impostazione</span><span class="sxs-lookup"><span data-stu-id="6b6c4-130">Setting</span></span>      |  <span data-ttu-id="6b6c4-131">Valore consigliato</span><span class="sxs-lookup"><span data-stu-id="6b6c4-131">Suggested value</span></span>   | <span data-ttu-id="6b6c4-132">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6b6c4-132">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="6b6c4-133">**Modalità di gestione**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-133">**Management mode**</span></span> | <span data-ttu-id="6b6c4-134">Rapida, Crea nuova app AD</span><span class="sxs-lookup"><span data-stu-id="6b6c4-134">Express, Create new AD app</span></span> | <span data-ttu-id="6b6c4-135">Configurare automaticamente un'entità servizio e l'autenticazione di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-135">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="6b6c4-136">**Creare un'app**</span><span class="sxs-lookup"><span data-stu-id="6b6c4-136">**Create app**</span></span> | <span data-ttu-id="6b6c4-137">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="6b6c4-137">my-serverless-webapp</span></span> | <span data-ttu-id="6b6c4-138">Immettere un nome applicazione univoco.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-138">Enter a unique application name.</span></span> |

    1. <span data-ttu-id="6b6c4-139">Fare clic su **OK** per salvare le impostazioni di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-139">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Impostazioni di Autenticazione, Autorizzazione e Azure Active Directory](../media/6-create-aad.png)

1. <span data-ttu-id="6b6c4-141">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-141">Click **Save**.</span></span>

## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="6b6c4-142">Modificare l'app Web per abilitare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="6b6c4-142">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="6b6c4-143">In Cloud Shell assicurarsi che la directory corrente sia la cartella **www/dist**.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-143">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="6b6c4-144">Abilitare l'autenticazione nell'app per le funzioni modificando **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-144">You enable authentication in your function app by modifying **settings.js**.</span></span> <span data-ttu-id="6b6c4-145">Aprire il file nell'editor Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-145">Open the file in Cloud Shell Editor.</span></span>

    ```azurecli
    code settings.js
    ```

1. <span data-ttu-id="6b6c4-146">Aggiungere l riga seguente al file e salvarlo.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-146">Append the following line to the file and save it.</span></span>

    ```azurecli
    window.authEnabled = true
    ```

1. <span data-ttu-id="6b6c4-147">Verificare che la modifica sia stata effettuata nel file.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="6b6c4-148">Caricare il file nell'archivio BLOB.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload \
        -c \$web \
        --account-name <storage account name> \
        -f settings.js \
        -n settings.js
    ```

## <a name="test-the-application"></a><span data-ttu-id="6b6c4-149">Test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="6b6c4-149">Test the application</span></span>

1. <span data-ttu-id="6b6c4-150">Aprire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-150">Open the application in a browser.</span></span> <span data-ttu-id="6b6c4-151">Fare clic su **Accedi** ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="6b6c4-152">Selezionare un file di immagine e caricarlo.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-152">Select an image file and upload it.</span></span>

    ![Pagina di accesso](../media/6-aad-auth.png)

## <a name="summary"></a><span data-ttu-id="6b6c4-154">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6b6c4-154">Summary</span></span>

<span data-ttu-id="6b6c4-155">In questa unità è stato descritto come aggiungere l'autenticazione all'applicazione usando l'autenticazione del servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="6b6c4-155">In this unit, you learned how to add authentication to the application using Azure App Service authentication.</span></span>