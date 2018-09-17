<span data-ttu-id="0def0-101">Dopo aver configurato un account, è possibile accedere al **portale di Azure**.</span><span class="sxs-lookup"><span data-stu-id="0def0-101">Now that we have an account, we can sign into the **Azure portal**.</span></span> <span data-ttu-id="0def0-102">Il portale è un sito di amministrazione basata sul Web che consente di interagire con tutte le sottoscrizioni e le risorse create.</span><span class="sxs-lookup"><span data-stu-id="0def0-102">The portal is a web-based administration site that lets you interact with all of your subscriptions and resources you have created.</span></span> <span data-ttu-id="0def0-103">Quasi tutte le operazioni eseguite con Azure possono essere completate tramite questa interfaccia Web.</span><span class="sxs-lookup"><span data-stu-id="0def0-103">Almost everything you do with Azure can be done through this web interface.</span></span>

## <a name="azure-portal-layout"></a><span data-ttu-id="0def0-104">Layout del portale di Azure</span><span class="sxs-lookup"><span data-stu-id="0def0-104">Azure portal layout</span></span>

<span data-ttu-id="0def0-105">Il portale di Azure è l'interfaccia utente grafica (GUI) principale per il controllo di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0def0-105">The Azure portal is the primary graphical user interface (GUI) for controlling Microsoft Azure.</span></span> <span data-ttu-id="0def0-106">Il portale consente di eseguire la maggior parte delle azioni di gestione ed è in genere l'interfaccia migliore per l'esecuzione di singole attività o la visualizzazione dettagliata delle opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="0def0-106">You can carry out the majority of management actions in the portal, and it is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.</span></span>

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

<span data-ttu-id="0def0-108">La parte restante della visualizzazione del portale è riservata agli elementi specifici con cui si sta lavorando.</span><span class="sxs-lookup"><span data-stu-id="0def0-108">The remainder of the portal view is for the specific elements you are working with.</span></span> <span data-ttu-id="0def0-109">La pagina predefinita (principale) è il _dashboard_.</span><span class="sxs-lookup"><span data-stu-id="0def0-109">The default (main) page is the _dashboard_.</span></span> <span data-ttu-id="0def0-110">Verrà illustrato più avanti, ma offre una visualizzazione panoramica personalizzabile delle risorse.</span><span class="sxs-lookup"><span data-stu-id="0def0-110">We'll cover this a bit later, but this represents a customizable birds-eye-view of your resources.</span></span> <span data-ttu-id="0def0-111">È possibile usarlo per passare alle risorse specifiche che si vogliono gestire o per cercare risorse con l'opzione **Tutte le risorse** nel pannello delle risorse.</span><span class="sxs-lookup"><span data-stu-id="0def0-111">You can use it to jump into specific resources you want to manage, or search for resources with the **All resources** entry in the resource panel.</span></span> <span data-ttu-id="0def0-112">Per gestire una risorsa, come una macchina virtuale o un'app Web, si usa un _pannello_ che presenta informazioni specifiche sulla risorsa.</span><span class="sxs-lookup"><span data-stu-id="0def0-112">When you are managing a resource, such as a virtual machine or a web app, you will work with a _blade_ that presents specific information about the resource.</span></span>

## <a name="what-is-a-blade"></a><span data-ttu-id="0def0-113">Che cos'è un pannello?</span><span class="sxs-lookup"><span data-stu-id="0def0-113">What is a blade?</span></span>

<span data-ttu-id="0def0-114">Il portale di Azure usa un modello a pannelli per lo spostamento.</span><span class="sxs-lookup"><span data-stu-id="0def0-114">The Azure portal uses a blades model for navigation.</span></span> <span data-ttu-id="0def0-115">Un _pannello_ è un elemento scorrevole contenente l'interfaccia utente per un singolo livello in una sequenza di spostamento.</span><span class="sxs-lookup"><span data-stu-id="0def0-115">A _blade_ is a slide-out panel containing the UI for a single level in a navigation sequence.</span></span> <span data-ttu-id="0def0-116">Ad esempio, ognuno degli elementi in questa sequenza è rappresentato da un pannello: **Macchine virtuali** > **Calcolo** > **Server Ubuntu**.</span><span class="sxs-lookup"><span data-stu-id="0def0-116">For example, each of these elements in this sequence would be represented by a blade: **Virtual machines** > **Compute** > **Ubuntu Server**.</span></span>

<span data-ttu-id="0def0-117">Ogni pannello contiene alcune informazioni e opzioni configurabili.</span><span class="sxs-lookup"><span data-stu-id="0def0-117">Each blade contains some information and configurable options.</span></span> <span data-ttu-id="0def0-118">Alcune di queste opzioni generano un altro pannello, che viene visualizzato a destra di quelli esistenti.</span><span class="sxs-lookup"><span data-stu-id="0def0-118">Some of these options generate another blade, which reveals itself to the right of any existing blade.</span></span> <span data-ttu-id="0def0-119">Nel nuovo pannello le eventuali altre opzioni configurabili si espanderanno in un altro pannello e così via.</span><span class="sxs-lookup"><span data-stu-id="0def0-119">On the new blade, any further configurable options will spawn another blade, and so on.</span></span> <span data-ttu-id="0def0-120">In poco tempo si avranno quindi diversi pannelli aperti nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="0def0-120">Pretty soon, you can end up with several blades open at the same time.</span></span> <span data-ttu-id="0def0-121">È possibile ingrandire i pannelli in modo da occupare l'intera schermata.</span><span class="sxs-lookup"><span data-stu-id="0def0-121">You can maximize blades as well so that they fill the entire screen.</span></span>

<span data-ttu-id="0def0-122">Poiché i nuovi pannelli vengono sempre aggiunti a destra del proprietario, è possibile usare la barra di scorrimento nella parte inferiore della finestra per tornare indietro e vedere come si è arrivati a questo punto della configurazione.</span><span class="sxs-lookup"><span data-stu-id="0def0-122">Since new blades are always added to the right of the owner, you can use the scrollbar at the bottom of the window to go backwards to see how you got to this spot in the configuration.</span></span> <span data-ttu-id="0def0-123">In alternativa, è possibile chiudere i pannelli singolarmente facendo clic sul pulsante `X` nell'angolo in alto del pannello.</span><span class="sxs-lookup"><span data-stu-id="0def0-123">Alternatively, you can close blades individually by clicking the `X` button in the top corner of the blade.</span></span> <span data-ttu-id="0def0-124">Se sono presenti modifiche non salvate, Azure informa che, se si continua, queste modifiche andranno perse.</span><span class="sxs-lookup"><span data-stu-id="0def0-124">If you have unsaved changes, Azure will prompt you to let you know that the changes will be lost if you continue.</span></span>

## <a name="configuring-settings-in-the-azure-portal"></a><span data-ttu-id="0def0-125">Configurazione delle impostazioni nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="0def0-125">Configuring settings in the Azure portal</span></span>

<span data-ttu-id="0def0-126">Il portale di Azure visualizza diverse opzioni di configurazione, principalmente nella barra di stato nella parte superiore destra della schermata.</span><span class="sxs-lookup"><span data-stu-id="0def0-126">The Azure portal displays several configuration options, mostly in the status bar at the top-right of the screen.</span></span>

### <a name="notifications"></a><span data-ttu-id="0def0-127">Notifiche</span><span class="sxs-lookup"><span data-stu-id="0def0-127">Notifications</span></span>

<span data-ttu-id="0def0-128">Per visualizzare il riquadro **Notifiche** fare clic sull'icona a forma di campanello.</span><span class="sxs-lookup"><span data-stu-id="0def0-128">Clicking the bell icon displays the **Notifications** pane.</span></span> <span data-ttu-id="0def0-129">Questo riquadro elenca le ultime azioni completate con il relativo stato.</span><span class="sxs-lookup"><span data-stu-id="0def0-129">This pane lists the last actions that have been carried out, along with their status.</span></span>

![Pannello Notifiche](../media-draft/5-notifications-blade.png)

### <a name="cloud-shell"></a><span data-ttu-id="0def0-131">Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="0def0-131">Cloud Shell</span></span>

<span data-ttu-id="0def0-132">Selezionando l'icona di **Cloud Shell** (>_) viene creata una nuova sessione di Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="0def0-132">If you click the **Cloud Shell** icon (>_), you will create a new Azure Cloud Shell session.</span></span> <span data-ttu-id="0def0-133">Azure Cloud Shell è una shell interattiva accessibile dal browser per la gestione delle risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="0def0-133">Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources.</span></span> <span data-ttu-id="0def0-134">Offre la flessibilità necessaria per scegliere l'esperienza shell più adatta al proprio modo di lavorare.</span><span class="sxs-lookup"><span data-stu-id="0def0-134">It provides the flexibility of choosing the shell experience that best suits the way you work.</span></span> <span data-ttu-id="0def0-135">Gli utenti Linux possono scegliere un'esperienza Bash, mentre gli utenti Windows possono scegliere PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0def0-135">Linux users can opt for a Bash experience, while Windows users can opt for PowerShell.</span></span> <span data-ttu-id="0def0-136">Questo terminale basato su browser consente di controllare e amministrare tutte le risorse di Azure nella sottoscrizione corrente tramite un'interfaccia della riga di comando incorporata nel portale.</span><span class="sxs-lookup"><span data-stu-id="0def0-136">This browser-based terminal lets you control and administer all of your Azure resources in the current subscription through a command-line interface built right into the portal.</span></span>

![Cloud Shell](../media-draft/5-choose-shell.png)

### <a name="settings"></a><span data-ttu-id="0def0-138">Impostazioni</span><span class="sxs-lookup"><span data-stu-id="0def0-138">Settings</span></span>

<span data-ttu-id="0def0-139">Per modificare le impostazioni del portale di Azure fare clic sull'icona a forma di **ingranaggio**.</span><span class="sxs-lookup"><span data-stu-id="0def0-139">Click the **gear** icon to change the Azure portal settings.</span></span> <span data-ttu-id="0def0-140">Le impostazioni includono:</span><span class="sxs-lookup"><span data-stu-id="0def0-140">These settings include:</span></span>

- <span data-ttu-id="0def0-141">Tempo di disconnessione</span><span class="sxs-lookup"><span data-stu-id="0def0-141">Logout time</span></span>
- <span data-ttu-id="0def0-142">Combinazione colori</span><span class="sxs-lookup"><span data-stu-id="0def0-142">Color scheme</span></span>
- <span data-ttu-id="0def0-143">Temi a contrasto elevato</span><span class="sxs-lookup"><span data-stu-id="0def0-143">High contrast themes</span></span>
- <span data-ttu-id="0def0-144">Notifiche di tipo avviso popup (per un dispositivo mobile)</span><span class="sxs-lookup"><span data-stu-id="0def0-144">Toast notifications (to a mobile device)</span></span>
- <span data-ttu-id="0def0-145">Doppio clic per modificare il tema</span><span class="sxs-lookup"><span data-stu-id="0def0-145">Double-click to change the theme</span></span>
- <span data-ttu-id="0def0-146">Lingua</span><span class="sxs-lookup"><span data-stu-id="0def0-146">Language</span></span>
- <span data-ttu-id="0def0-147">Formato regionale</span><span class="sxs-lookup"><span data-stu-id="0def0-147">Regional format</span></span>

![Impostazioni del portale](../media-draft/5-settings-blade.png)

<span data-ttu-id="0def0-149">Dopo aver modificato le impostazioni, fare clic su **Applica** per accettare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="0def0-149">When you have changed settings, click **Apply** to accept your changes.</span></span>

### <a name="feedback-blade"></a><span data-ttu-id="0def0-150">Pannello Commenti e suggerimenti</span><span class="sxs-lookup"><span data-stu-id="0def0-150">Feedback blade</span></span>

<span data-ttu-id="0def0-151">L'icona a forma di **faccina sorridente** apre il pannello **Inviare commenti e suggerimenti**.</span><span class="sxs-lookup"><span data-stu-id="0def0-151">The **smiley face** icon opens the **Send us feedback** blade.</span></span> <span data-ttu-id="0def0-152">In quest'area è possibile inviare a Microsoft commenti e suggerimenti su Azure.</span><span class="sxs-lookup"><span data-stu-id="0def0-152">Here you can send feedback to Microsoft about Azure.</span></span> <span data-ttu-id="0def0-153">Si noti che è possibile specificare se Microsoft può rispondere a commenti e suggerimenti tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="0def0-153">Note that you can specify whether Microsoft can respond to your feedback by email.</span></span>

![Commenti e suggerimenti](../media-draft/5-feedback-blade.png)

### <a name="help-blade"></a><span data-ttu-id="0def0-155">Pannello Guida e supporto</span><span class="sxs-lookup"><span data-stu-id="0def0-155">Help blade</span></span>

<span data-ttu-id="0def0-156">Fare clic sull'icona del **punto interrogativo** per visualizzare il pannello **Guida**.</span><span class="sxs-lookup"><span data-stu-id="0def0-156">Click the **question mark** icon to show the **Help** blade.</span></span> <span data-ttu-id="0def0-157">Qui è possibile scegliere tra diverse opzioni, tra cui:</span><span class="sxs-lookup"><span data-stu-id="0def0-157">Here you choose from several options, including:</span></span>

- <span data-ttu-id="0def0-158">Novità</span><span class="sxs-lookup"><span data-stu-id="0def0-158">What's new</span></span>
- <span data-ttu-id="0def0-159">Roadmap per Azure</span><span class="sxs-lookup"><span data-stu-id="0def0-159">Azure roadmap</span></span>
- <span data-ttu-id="0def0-160">Avvia presentazione guidata</span><span class="sxs-lookup"><span data-stu-id="0def0-160">Launch guided tour</span></span>
- <span data-ttu-id="0def0-161">Scelte rapide da tastiera</span><span class="sxs-lookup"><span data-stu-id="0def0-161">Keyboard shortcuts</span></span>
- <span data-ttu-id="0def0-162">Visualizza diagnostica</span><span class="sxs-lookup"><span data-stu-id="0def0-162">Show diagnostics</span></span>
- <span data-ttu-id="0def0-163">Privacy e condizioni</span><span class="sxs-lookup"><span data-stu-id="0def0-163">Privacy + terms</span></span>

### <a name="directory-and-subscription"></a><span data-ttu-id="0def0-164">Directory e sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="0def0-164">Directory and subscription</span></span>

<span data-ttu-id="0def0-165">Fare clic sull'icona a forma di **libro e filtro** per visualizzare il pannello **Directory e sottoscrizione**.</span><span class="sxs-lookup"><span data-stu-id="0def0-165">Click the **Book and Filter** icon to show the **Directory + subscription** blade.</span></span>

<span data-ttu-id="0def0-166">Azure consente di avere più sottoscrizioni associate a una stessa directory.</span><span class="sxs-lookup"><span data-stu-id="0def0-166">Azure allows you to have more than one subscription associated with one directory.</span></span> <span data-ttu-id="0def0-167">Nel pannello **Directory e sottoscrizione** è possibile passare da una sottoscrizione all'altra.</span><span class="sxs-lookup"><span data-stu-id="0def0-167">On the **Directory + subscription** blade, you can change between subscriptions.</span></span> <span data-ttu-id="0def0-168">In quest'area è possibile cambiare la sottoscrizione o passare a un'altra directory.</span><span class="sxs-lookup"><span data-stu-id="0def0-168">Here, you can change your subscription or change to another directory.</span></span>

![Directory](../media-draft/5-directory-blade-1.png)

### <a name="profile-settings"></a><span data-ttu-id="0def0-170">Impostazioni del profilo</span><span class="sxs-lookup"><span data-stu-id="0def0-170">Profile settings</span></span>

<span data-ttu-id="0def0-171">Se si fa clic sul nome nell'angolo superiore destro, è possibile modificare le impostazioni del profilo.</span><span class="sxs-lookup"><span data-stu-id="0def0-171">If you click on your name in the top right-hand corner, you can then change your profile settings.</span></span>
<span data-ttu-id="0def0-172">Le opzioni includono:</span><span class="sxs-lookup"><span data-stu-id="0def0-172">Options include:</span></span>

- <span data-ttu-id="0def0-173">Disconnetti da Azure</span><span class="sxs-lookup"><span data-stu-id="0def0-173">Sign out of Azure</span></span>
- <span data-ttu-id="0def0-174">Cambia password</span><span class="sxs-lookup"><span data-stu-id="0def0-174">Change password</span></span>
- <span data-ttu-id="0def0-175">Modifica informazioni di contatto</span><span class="sxs-lookup"><span data-stu-id="0def0-175">Change contact information</span></span>
- <span data-ttu-id="0def0-176">Visualizza autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="0def0-176">View permissions</span></span>
- <span data-ttu-id="0def0-177">Invia un'idea al team di Azure</span><span class="sxs-lookup"><span data-stu-id="0def0-177">Submit an idea to the Azure team</span></span>
- <span data-ttu-id="0def0-178">Visualizzare la fattura</span><span class="sxs-lookup"><span data-stu-id="0def0-178">View your bill</span></span>
- <span data-ttu-id="0def0-179">Cambia directory (viene visualizzato il pannello **Directory e sottoscrizione** come nella sezione precedente)</span><span class="sxs-lookup"><span data-stu-id="0def0-179">Switch directory (shows the **Directory + subscription** blade as in the previous section)</span></span>

![Impostazioni del profilo](../media-draft/5-portal-menu.png)

<span data-ttu-id="0def0-181">Se si seleziona **Visualizza fattura**, viene visualizzata la pagina **Gestione dei costi e fatturazione - Fatture** che consente di analizzare i costi generati da Azure.</span><span class="sxs-lookup"><span data-stu-id="0def0-181">If you now click **View my bill**, Azure takes you to the **Cost Management + Billing - Invoices** page, which helps you analyze where Azure is generating costs.</span></span>

![Pagina Fatturazione](../media-draft/5-portal-billing.png)

<span data-ttu-id="0def0-183">Azure è una piattaforma di grandi dimensioni, come testimonia l'interfaccia utente del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="0def0-183">Azure is a large product, and the Azure portal user interface (UI) reflects this.</span></span> <span data-ttu-id="0def0-184">La struttura a pannelli consente di spostarsi avanti e indietro tra le varie attività di amministrazione con facilità.</span><span class="sxs-lookup"><span data-stu-id="0def0-184">The sliding blade approach allows you to navigate back and forth through the various administration tasks with ease.</span></span> <span data-ttu-id="0def0-185">Nei prossimi esercizi faremo pratica con questa interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="0def0-185">Let's experiment a bit with this UI so you get some practice.</span></span>