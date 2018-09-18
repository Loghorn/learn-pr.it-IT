<span data-ttu-id="dabb7-101">Dopo aver configurato un account, è possibile accedere al **portale di Azure**.</span><span class="sxs-lookup"><span data-stu-id="dabb7-101">Now that we have an account, we can sign into the **Azure portal**.</span></span> <span data-ttu-id="dabb7-102">Il portale è un sito di amministrazione basata sul Web che consente di interagire con tutte le sottoscrizioni e le risorse create.</span><span class="sxs-lookup"><span data-stu-id="dabb7-102">The portal is a web-based administration site that lets you interact with all of your subscriptions and resources you have created.</span></span> <span data-ttu-id="dabb7-103">Quasi tutte le operazioni eseguite con Azure possono essere completate tramite questa interfaccia Web.</span><span class="sxs-lookup"><span data-stu-id="dabb7-103">Almost everything you do with Azure can be done through this web interface.</span></span>

## <a name="azure-portal-layout"></a><span data-ttu-id="dabb7-104">Layout del portale di Azure</span><span class="sxs-lookup"><span data-stu-id="dabb7-104">Azure portal layout</span></span>

<span data-ttu-id="dabb7-105">Il portale di Azure è l'interfaccia utente grafica (GUI) principale per il controllo di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="dabb7-105">The Azure portal is the primary graphical user interface (GUI) for controlling Microsoft Azure.</span></span> <span data-ttu-id="dabb7-106">Il portale consente di eseguire la maggior parte delle azioni di gestione ed è in genere l'interfaccia migliore per l'esecuzione di singole attività o la visualizzazione dettagliata delle opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dabb7-106">You can carry out the majority of management actions in the portal, and it is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.</span></span>

![Portale di Azure](../media-draft/5-portal.png)

:::row:::

    :::column:::
    ![The resources and favorites](../media-draft/5-favorites.png)
    :::column-end:::

    :::column span="3":::
    **Resource Panel**
    
    In the left-hand sidebar of the portal is the resource pane, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_. 

    You can customize this with the specific resource types you tend to create or administer most often. 

    You can also collapse this pane; with the **<<** caret. This will minimize it to just icons which can be convenient if you are working with limited screen real-estate.
    :::column-end:::

:::row-end:::

<span data-ttu-id="dabb7-108">La parte restante della visualizzazione del portale è riservata agli elementi specifici con cui si sta lavorando.</span><span class="sxs-lookup"><span data-stu-id="dabb7-108">The remainder of the portal view is for the specific elements you are working with.</span></span> <span data-ttu-id="dabb7-109">La pagina predefinita (principale) è il _dashboard_.</span><span class="sxs-lookup"><span data-stu-id="dabb7-109">The default (main) page is the _dashboard_.</span></span> <span data-ttu-id="dabb7-110">Verrà illustrato più avanti, ma offre una visualizzazione panoramica delle risorse.</span><span class="sxs-lookup"><span data-stu-id="dabb7-110">We'll cover this a bit later, but this represents a customizable birds-eye-view of your resources.</span></span> <span data-ttu-id="dabb7-111">È possibile usarlo per passare a risorse specifiche in modo da poterle gestire o per cercare risorse con l'opzione **Tutte le risorse** nel pannello delle risorse.</span><span class="sxs-lookup"><span data-stu-id="dabb7-111">You can use it to jump into specific resources you want to manage, or search for resources with the **All resources** entry in the resource panel.</span></span> <span data-ttu-id="dabb7-112">Per gestire una risorsa, come una macchina virtuale o un'app Web, si usa un _pannello_ che presenta informazioni specifiche sulla risorsa.</span><span class="sxs-lookup"><span data-stu-id="dabb7-112">When you are managing a resource, such as a virtual machine or a web app, you will work with a _blade_ that presents specific information about the resource.</span></span>

## <a name="what-is-a-blade"></a><span data-ttu-id="dabb7-113">Che cos'è un pannello?</span><span class="sxs-lookup"><span data-stu-id="dabb7-113">What is a blade?</span></span>

<span data-ttu-id="dabb7-114">Il portale di Azure usa un modello a pannelli per lo spostamento.</span><span class="sxs-lookup"><span data-stu-id="dabb7-114">The Azure portal uses a blades model for navigation.</span></span> <span data-ttu-id="dabb7-115">Un _pannello_ è un elemento scorrevole contenente l'interfaccia utente per un singolo livello in una sequenza di spostamento.</span><span class="sxs-lookup"><span data-stu-id="dabb7-115">A _blade_ is a slide-out panel containing the UI for a single level in a navigation sequence.</span></span> <span data-ttu-id="dabb7-116">Ad esempio, ognuno degli elementi in questa sequenza è rappresentato da un pannello: **Macchine virtuali** > **Calcolo** > **Server Ubuntu**.</span><span class="sxs-lookup"><span data-stu-id="dabb7-116">For example, each of these elements in this sequence would be represented by a blade: **Virtual machines** > **Compute** > **Ubuntu Server**.</span></span>

<span data-ttu-id="dabb7-117">Ogni pannello contiene alcune informazioni e opzioni configurabili.</span><span class="sxs-lookup"><span data-stu-id="dabb7-117">Each blade contains some information and configurable options.</span></span> <span data-ttu-id="dabb7-118">Alcune di queste opzioni generano un altro pannello, che viene visualizzato a destra di quelli esistenti.</span><span class="sxs-lookup"><span data-stu-id="dabb7-118">Some of these options generate another blade, which reveals itself to the right of any existing blade.</span></span> <span data-ttu-id="dabb7-119">Nel nuovo pannello le eventuali altre opzioni configurabili si espanderanno in un altro pannello e così via.</span><span class="sxs-lookup"><span data-stu-id="dabb7-119">On the new blade, any further configurable options will spawn another blade, and so on.</span></span> <span data-ttu-id="dabb7-120">In poco tempo si avranno quindi diversi pannelli aperti nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="dabb7-120">Pretty soon, you can end up with several blades open at the same time.</span></span> <span data-ttu-id="dabb7-121">È possibile ingrandire i pannelli in modo da occupare l'intera schermata.</span><span class="sxs-lookup"><span data-stu-id="dabb7-121">You can maximize blades as well so that they fill the entire screen.</span></span>

<span data-ttu-id="dabb7-122">Poiché i nuovi pannelli vengono sempre aggiunti a destra del proprietario, è possibile usare la barra di scorrimento in fondo alla finestra per tornare indietro e vedere come si è arrivati a questo punto della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dabb7-122">Since new blades are always added to the right of the owner, you can use the scrollbar at the bottom of the window to go backwards to see how you got to this spot in the configuration.</span></span> <span data-ttu-id="dabb7-123">In alternativa, è possibile chiudere i pannelli singolarmente facendo clic sul pulsante `X` nell'angolo in alto del pannello.</span><span class="sxs-lookup"><span data-stu-id="dabb7-123">Alternatively, you can close blades individually by clicking the `X` button in the top corner of the blade.</span></span> <span data-ttu-id="dabb7-124">Se sono presenti modifiche non salvate, Azure informa che, se si continua, queste modifiche andranno perse.</span><span class="sxs-lookup"><span data-stu-id="dabb7-124">If you have unsaved changes, Azure will prompt you to let you know that the changes will be lost if you continue.</span></span>

## <a name="configuring-settings-in-the-azure-portal"></a><span data-ttu-id="dabb7-125">Configurazione delle impostazioni nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="dabb7-125">Configuring settings in the Azure portal</span></span>

<span data-ttu-id="dabb7-126">Il portale di Azure visualizza diverse opzioni di configurazione, principalmente nella barra di stato nella parte superiore destra della schermata.</span><span class="sxs-lookup"><span data-stu-id="dabb7-126">The Azure portal displays several configuration options, mostly in the status bar at the top-right of the screen.</span></span>

### <a name="notifications"></a><span data-ttu-id="dabb7-127">Notifiche</span><span class="sxs-lookup"><span data-stu-id="dabb7-127">Notifications</span></span>

<span data-ttu-id="dabb7-128">Per visualizzare il riquadro **Notifiche** fare clic sull'icona a forma di campanello.</span><span class="sxs-lookup"><span data-stu-id="dabb7-128">Clicking the bell icon displays the **Notifications** pane.</span></span> <span data-ttu-id="dabb7-129">Questo riquadro elenca le ultime azioni eseguite con il relativo stato.</span><span class="sxs-lookup"><span data-stu-id="dabb7-129">This pane lists the last actions that have been carried out, along with their status.</span></span>

### <a name="cloud-shell"></a><span data-ttu-id="dabb7-130">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="dabb7-130">Cloud Shell</span></span>

<span data-ttu-id="dabb7-131">Selezionando l'icona di **Cloud Shell** (>_) viene creata una nuova sessione di Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="dabb7-131">If you click the **Cloud Shell** icon (>_), you will create a new Azure Cloud Shell session.</span></span> <span data-ttu-id="dabb7-132">Azure Cloud Shell è una shell interattiva accessibile dal browser per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="dabb7-132">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span> <span data-ttu-id="dabb7-133">Consente di scegliere in maniera flessibile l'esperienza shell più adatta al proprio modo di lavorare.</span><span class="sxs-lookup"><span data-stu-id="dabb7-133">It provides the flexibility of choosing the shell experience that best suits the way you work.</span></span> <span data-ttu-id="dabb7-134">Gli utenti Linux possono scegliere un'esperienza Bash, mentre gli utenti Windows possono scegliere PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dabb7-134">Linux users can opt for a Bash experience, while Windows users can opt for PowerShell.</span></span> <span data-ttu-id="dabb7-135">Questo terminale basato su browser consente di controllare e amministrare tutte le risorse di Azure nella sottoscrizione corrente tramite un'interfaccia della riga di comando incorporata nel portale.</span><span class="sxs-lookup"><span data-stu-id="dabb7-135">This browser-based terminal lets you control and administer all of your Azure resources in the current subscription through a command-line interface built right into the portal.</span></span>

### <a name="settings"></a><span data-ttu-id="dabb7-136">Impostazioni</span><span class="sxs-lookup"><span data-stu-id="dabb7-136">Settings</span></span>

<span data-ttu-id="dabb7-137">Per modificare le impostazioni del portale di Azure fare clic sull'icona a forma di **ingranaggio**.</span><span class="sxs-lookup"><span data-stu-id="dabb7-137">Click the **gear** icon to change the Azure portal settings.</span></span> <span data-ttu-id="dabb7-138">Le impostazioni includono:</span><span class="sxs-lookup"><span data-stu-id="dabb7-138">These settings include:</span></span>

- <span data-ttu-id="dabb7-139">Tempo di disconnessione</span><span class="sxs-lookup"><span data-stu-id="dabb7-139">Logout time</span></span>
- <span data-ttu-id="dabb7-140">Temi a colori e a contrasto</span><span class="sxs-lookup"><span data-stu-id="dabb7-140">Color and contrast themes</span></span>
- <span data-ttu-id="dabb7-141">Avvisi popup (per un dispositivo mobile)</span><span class="sxs-lookup"><span data-stu-id="dabb7-141">Toast notifications (to a mobile device)</span></span>
- <span data-ttu-id="dabb7-142">Lingua e formato locale</span><span class="sxs-lookup"><span data-stu-id="dabb7-142">Language and regional format</span></span>

![Impostazioni del portale](../media-draft/5-settings-blade.png)

<span data-ttu-id="dabb7-144">Dopo aver modificato le impostazioni, fare clic su **Applica** per accettare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="dabb7-144">When you have changed settings, click **Apply** to accept your changes.</span></span>

### <a name="feedback-blade"></a><span data-ttu-id="dabb7-145">Pannello Commenti e suggerimenti</span><span class="sxs-lookup"><span data-stu-id="dabb7-145">Feedback blade</span></span>

<span data-ttu-id="dabb7-146">L'icona a forma di **faccina sorridente** apre il pannello **Inviare commenti e suggerimenti**.</span><span class="sxs-lookup"><span data-stu-id="dabb7-146">The **smiley face** icon opens the **Send us feedback** blade.</span></span> <span data-ttu-id="dabb7-147">In quest'area è possibile inviare a Microsoft commenti e suggerimenti su Azure.</span><span class="sxs-lookup"><span data-stu-id="dabb7-147">Here you can send feedback to Microsoft about Azure.</span></span> <span data-ttu-id="dabb7-148">Si noti che è possibile specificare se Microsoft può rispondere al feedback tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="dabb7-148">Note that you can specify whether Microsoft can respond to your feedback by email.</span></span>

### <a name="help-blade"></a><span data-ttu-id="dabb7-149">Pannello Guida e supporto</span><span class="sxs-lookup"><span data-stu-id="dabb7-149">Help blade</span></span>

<span data-ttu-id="dabb7-150">Fare clic sull'icona del **punto interrogativo** per visualizzare il pannello **Guida e supporto**.</span><span class="sxs-lookup"><span data-stu-id="dabb7-150">Click the **question mark** icon to show the **Help** blade.</span></span> <span data-ttu-id="dabb7-151">Qui è possibile scegliere tra diverse opzioni, tra cui:</span><span class="sxs-lookup"><span data-stu-id="dabb7-151">Here you choose from several options, including:</span></span>

- <span data-ttu-id="dabb7-152">Novità</span><span class="sxs-lookup"><span data-stu-id="dabb7-152">What's new</span></span>
- <span data-ttu-id="dabb7-153">Roadmap per Azure</span><span class="sxs-lookup"><span data-stu-id="dabb7-153">Azure roadmap</span></span>
- <span data-ttu-id="dabb7-154">Avvia presentazione guidata</span><span class="sxs-lookup"><span data-stu-id="dabb7-154">Launch guided tour</span></span>
- <span data-ttu-id="dabb7-155">Scelte rapide da tastiera</span><span class="sxs-lookup"><span data-stu-id="dabb7-155">Keyboard shortcuts</span></span>
- <span data-ttu-id="dabb7-156">Visualizza diagnostica</span><span class="sxs-lookup"><span data-stu-id="dabb7-156">Show diagnostics</span></span>
- <span data-ttu-id="dabb7-157">Privacy e condizioni</span><span class="sxs-lookup"><span data-stu-id="dabb7-157">Privacy + terms</span></span>

### <a name="directory-and-subscription"></a><span data-ttu-id="dabb7-158">Directory e sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="dabb7-158">Directory and subscription</span></span>

<span data-ttu-id="dabb7-159">Fare clic sull'icona a forma di **libro e filtro** per visualizzare il pannello **Directory e sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="dabb7-159">Click the **Book and Filter** icon to show the **Directory + subscription** blade.</span></span>

<span data-ttu-id="dabb7-160">Azure consente di avere più sottoscrizioni associate a una stessa directory.</span><span class="sxs-lookup"><span data-stu-id="dabb7-160">Azure allows you to have more than one subscription associated with one directory.</span></span> <span data-ttu-id="dabb7-161">Nel pannello **Directory e sottoscrizione** è possibile passare da una sottoscrizione all'altra.</span><span class="sxs-lookup"><span data-stu-id="dabb7-161">On the **Directory + subscription** blade, you can change between subscriptions.</span></span> <span data-ttu-id="dabb7-162">In quest'area è possibile cambiare la sottoscrizione o passare a un'altra directory.</span><span class="sxs-lookup"><span data-stu-id="dabb7-162">Here, you can change your subscription or change to another directory.</span></span>

![Directory](../media-draft/5-directory-blade.png)

### <a name="profile-settings"></a><span data-ttu-id="dabb7-164">Impostazioni del profilo</span><span class="sxs-lookup"><span data-stu-id="dabb7-164">Profile settings</span></span>

<span data-ttu-id="dabb7-165">Se si fa clic sul nome nell'angolo superiore destro, è possibile modificare le impostazioni del profilo.</span><span class="sxs-lookup"><span data-stu-id="dabb7-165">If you click on your name in the top right-hand corner, you can then change your profile settings.</span></span>
<span data-ttu-id="dabb7-166">Le opzioni includono:</span><span class="sxs-lookup"><span data-stu-id="dabb7-166">Options include:</span></span>

- <span data-ttu-id="dabb7-167">Disconnetti da Azure</span><span class="sxs-lookup"><span data-stu-id="dabb7-167">Sign out of Azure</span></span>
- <span data-ttu-id="dabb7-168">Cambia password</span><span class="sxs-lookup"><span data-stu-id="dabb7-168">Change password</span></span>
- <span data-ttu-id="dabb7-169">Modifica informazioni di contatto</span><span class="sxs-lookup"><span data-stu-id="dabb7-169">Change contact information</span></span>
- <span data-ttu-id="dabb7-170">Visualizza autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="dabb7-170">View permissions</span></span>
- <span data-ttu-id="dabb7-171">Invia un'idea al team di Azure</span><span class="sxs-lookup"><span data-stu-id="dabb7-171">Submit an idea to the Azure team</span></span>
- <span data-ttu-id="dabb7-172">Visualizzare la fattura</span><span class="sxs-lookup"><span data-stu-id="dabb7-172">View your bill</span></span>
- <span data-ttu-id="dabb7-173">Cambia directory (viene visualizzato il pannello **Directory e sottoscrizione** come nella sezione precedente)</span><span class="sxs-lookup"><span data-stu-id="dabb7-173">Switch directory (shows the **Directory + subscription** blade as in the previous section)</span></span>

<span data-ttu-id="dabb7-174">Se si seleziona **Visualizza fattura**, viene visualizzata la pagina **Gestione dei costi e fatturazione - Fatture** che consente di analizzare i costi generati da Azure.</span><span class="sxs-lookup"><span data-stu-id="dabb7-174">If you now click **View my bill**, Azure takes you to the **Cost Management + Billing - Invoices** page, which helps you analyze where Azure is generating costs.</span></span>

<span data-ttu-id="dabb7-175">Azure è una piattaforma di grandi dimensioni, come testimonia l'interfaccia utente del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="dabb7-175">Azure is a large product, and the Azure portal user interface (UI) reflects this.</span></span> <span data-ttu-id="dabb7-176">La struttura a pannelli consente di spostarsi avanti e indietro tra le varie attività di amministrazione con facilità.</span><span class="sxs-lookup"><span data-stu-id="dabb7-176">The sliding blade approach allows you to navigate back and forth through the various administration tasks with ease.</span></span> <span data-ttu-id="dabb7-177">Nei prossimi esercizi faremo pratica con questa interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="dabb7-177">Let's experiment a bit with this UI so you get some practice.</span></span>