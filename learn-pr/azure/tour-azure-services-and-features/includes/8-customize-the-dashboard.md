<span data-ttu-id="ba42d-101">Qui di seguito viene descritto come creare e modificare alcuni dashboard usando il portale di Azure e modificando direttamente il file con estensione json sottostante.</span><span class="sxs-lookup"><span data-stu-id="ba42d-101">Next, let's look at how to create and modify dashboards using the Azure Portal, and by editing the underlying JSON file directly.</span></span>

## <a name="what-is-a-dashboard"></a><span data-ttu-id="ba42d-102">Che cos'è un dashboard?</span><span class="sxs-lookup"><span data-stu-id="ba42d-102">What is a dashboard?</span></span>

<span data-ttu-id="ba42d-103">Un _dashboard_ è una raccolta personalizzabile di riquadri di interfaccia utente visualizzata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba42d-103">A _dashboard_ is a customizable collection of UI tiles displayed in the Azure portal.</span></span> <span data-ttu-id="ba42d-104">È possibile aggiungere, rimuovere e posizionare i riquadri per creare la visualizzazione esatta desiderata e quindi salvarla come dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-104">You add, remove, and position tiles to create the exact view you want, and then save that view as a dashboard.</span></span> <span data-ttu-id="ba42d-105">Sono supportati più dashboard ed è possibile passare tra un dashboard e l'altro a seconda delle esigenze.</span><span class="sxs-lookup"><span data-stu-id="ba42d-105">Multiple dashboards are supported, and you can switch between them as needed.</span></span> <span data-ttu-id="ba42d-106">È anche possibile condividere i dashboard con altri membri del team.</span><span class="sxs-lookup"><span data-stu-id="ba42d-106">You can even share your dashboards with other team members.</span></span>

<span data-ttu-id="ba42d-107">I dashboard offrono una notevole flessibilità in termini di modalità di gestione di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba42d-107">Dashboards give you considerable flexibility regarding how you manage Azure.</span></span> <span data-ttu-id="ba42d-108">Ad esempio, è possibile creare dashboard per ruoli specifici all'interno dell'organizzazione, quindi usare il controllo degli accessi in base al ruolo per specificare quali utenti possono accedere ai dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-108">For example, you can create dashboards for specific roles within the organization, and then use role-based access control (RBAC) to control who can access that dashboard.</span></span> <span data-ttu-id="ba42d-109">L'amministratore del database avrebbe quindi accesso a un dashboard che contiene le visualizzazioni del servizio di Database SQL, mentre l'amministratore di Azure Active Directory avrebbe accesso alle visualizzazioni degli utenti e dei gruppi di Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ba42d-109">Hence, your database administrator would have a dashboard that contains views of the SQL database service, whereas your Azure Active Directory administrator would have views of the users and groups within Azure AD.</span></span> <span data-ttu-id="ba42d-110">È anche possibile personalizzare il portale tra gli ambienti di produzione e sviluppo all'interno del portale, creando un dashboard specifico per ogni ambiente che si gestisce.</span><span class="sxs-lookup"><span data-stu-id="ba42d-110">You can even customize the portal between your production and development environments within the portal - creating a specific dashboard for each environment you are managing.</span></span>

<span data-ttu-id="ba42d-111">I dashboard vengono archiviati come file JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="ba42d-111">Dashboards are stored as JavaScript Object Notation (JSON) files.</span></span> <span data-ttu-id="ba42d-112">Ciò significa che possono essere caricati e scaricati in altri computer o essere condivisi con i membri della directory di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba42d-112">This means they can be uploaded and downloaded to other computers, or shared with members of the Azure directory.</span></span> <span data-ttu-id="ba42d-113">Azure archivia i dashboard all'interno di gruppi di risorse, proprio come le macchine virtuali o gli account di archiviazione che è possibile gestire all'interno del portale.</span><span class="sxs-lookup"><span data-stu-id="ba42d-113">Azure stores dashboards within resource groups, just like virtual machines or storage accounts that you can manage within the portal.</span></span>

> [!TIP]
> <span data-ttu-id="ba42d-114">Poiché i dashboard sono file JSON, è possibile anche [personalizzarli a livello di programmazione](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), trasformandoli quindi in validi strumenti di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="ba42d-114">Because dashboards are JSON files, you can also [customize them programmatically](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), making them compelling administrative tools.</span></span> <span data-ttu-id="ba42d-115">Alcuni tipi di riquadri possono anche essere basati su query, in modo da essere aggiornati automaticamente quando i dati di origine cambiano.</span><span class="sxs-lookup"><span data-stu-id="ba42d-115">Also, some tile types can be query-based, so they update automatically when the source data changes.</span></span>

## <a name="explore-the-default-dashboard"></a><span data-ttu-id="ba42d-116">Esplorare il dashboard predefinito</span><span class="sxs-lookup"><span data-stu-id="ba42d-116">Explore the default dashboard</span></span>

<span data-ttu-id="ba42d-117">Il dashboard predefinito è denominato "Dashboard".</span><span class="sxs-lookup"><span data-stu-id="ba42d-117">The default dashboard is named "Dashboard".</span></span> <span data-ttu-id="ba42d-118">Quando si accede al portale, viene visualizzato questo dashboard contenente cinque Web part.</span><span class="sxs-lookup"><span data-stu-id="ba42d-118">When you log into the portal, you are presented with this dashboard containing five web parts.</span></span>

![Web part predefinite](../media-draft/8-dashboard-default-webparts.png)

<span data-ttu-id="ba42d-120">Le Web part predefinite sono</span><span class="sxs-lookup"><span data-stu-id="ba42d-120">These default web parts are</span></span>

1. <span data-ttu-id="ba42d-121">Tutte le risorse</span><span class="sxs-lookup"><span data-stu-id="ba42d-121">All resources</span></span>

1. <span data-ttu-id="ba42d-122">Introduzione ad Azure</span><span class="sxs-lookup"><span data-stu-id="ba42d-122">Azure Getting Started</span></span>

1. <span data-ttu-id="ba42d-123">Guide introduttive ed esercitazioni</span><span class="sxs-lookup"><span data-stu-id="ba42d-123">Quickstarts + tutorials</span></span>

1. <span data-ttu-id="ba42d-124">Marketplace</span><span class="sxs-lookup"><span data-stu-id="ba42d-124">Marketplace</span></span>

1. <span data-ttu-id="ba42d-125">Integrità dei servizi</span><span class="sxs-lookup"><span data-stu-id="ba42d-125">Service Health</span></span>

## <a name="creating-and-managing-dashboards"></a><span data-ttu-id="ba42d-126">Creazione e gestione dei dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-126">Creating and managing dashboards</span></span>

<span data-ttu-id="ba42d-127">Nella parte superiore del dashboard sono presenti i controlli che consentono di creare, caricare, scaricare, modificare e condividere un dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-127">Along the top of the dashboard are the controls that enable you to create, upload, download, edit, and share a dashboard.</span></span> <span data-ttu-id="ba42d-128">Sono presenti anche i controlli per visualizzare un dashboard a schermo intero, clonarlo o eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="ba42d-128">You can also switch a dashboard to full screen, clone it, or delete it.</span></span>

![Personalizzare i controlli del dashboard](../media-draft/8-customise-dashboard-controls.png)

- [<span data-ttu-id="ba42d-130">Selezionare un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-130">Select dashboard</span></span>](#select-dashboard)
- [<span data-ttu-id="ba42d-131">Creare un nuovo dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-131">Create a new dashboard</span></span>](#create-new)
- [<span data-ttu-id="ba42d-132">Caricare e scaricare</span><span class="sxs-lookup"><span data-stu-id="ba42d-132">Upload and Download</span></span>](#upload-download)
- [<span data-ttu-id="ba42d-133">Modificare</span><span class="sxs-lookup"><span data-stu-id="ba42d-133">Edit</span></span>](#edit-dashboard)
- [<span data-ttu-id="ba42d-134">Condividere</span><span class="sxs-lookup"><span data-stu-id="ba42d-134">Share</span></span>](#share-dashboard)
- [<span data-ttu-id="ba42d-135">Schermo intero</span><span class="sxs-lookup"><span data-stu-id="ba42d-135">Full screen</span></span>](#full-screen)
- [<span data-ttu-id="ba42d-136">Clonare</span><span class="sxs-lookup"><span data-stu-id="ba42d-136">Clone</span></span>](#clone-dashboard)
- [<span data-ttu-id="ba42d-137">Eliminare</span><span class="sxs-lookup"><span data-stu-id="ba42d-137">Delete</span></span>](#delete-dashboard)

<a name="select-dashboard"></a>

## <a name="select-dashboard"></a><span data-ttu-id="ba42d-138">Selezionare un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-138">Select dashboard</span></span>

<span data-ttu-id="ba42d-139">All'estrema sinistra della barra degli strumenti è presente il controllo a discesa **Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-139">To the far left of the toolbar is the **Select Dashboard** drop-down control.</span></span> <span data-ttu-id="ba42d-140">Facendo clic su questo controllo è possibile selezionare i dashboard che sono già stati definiti per l'account.</span><span class="sxs-lookup"><span data-stu-id="ba42d-140">Clicking this control enables you to select from dashboards that you have already defined for your account.</span></span> <span data-ttu-id="ba42d-141">Grazie a questo controllo è facile definire più dashboard per scopi diversi e quindi spostarsi tra l'uno e l'altro, a seconda delle azioni che si intende eseguire al momento.</span><span class="sxs-lookup"><span data-stu-id="ba42d-141">This control makes it simple for you to define multiple dashboards for different purposes and then switch from one to another and back again, depending on what you are trying to do at the time.</span></span>

<span data-ttu-id="ba42d-142">Tutti i dashboard sono inizialmente privati, ossia sono accessibili solo a chi li ha creati.</span><span class="sxs-lookup"><span data-stu-id="ba42d-142">Note that any dashboards that you create will initially be private; that is, only you can see them.</span></span> <span data-ttu-id="ba42d-143">Per rendere un dashboard disponibile agli altri utenti in un'azienda, è necessario condividerlo.</span><span class="sxs-lookup"><span data-stu-id="ba42d-143">To make a dashboard available across your enterprise, you need to share it.</span></span> <span data-ttu-id="ba42d-144">Questa opzione verrà esaminata a breve.</span><span class="sxs-lookup"><span data-stu-id="ba42d-144">We'll look at that option shortly.</span></span>

<a name="create-new"></a>

## <a name="create-a-new-dashboard"></a><span data-ttu-id="ba42d-145">Creare un nuovo dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-145">Create a new dashboard</span></span>

<span data-ttu-id="ba42d-146">Per creare un nuovo dashboard, fare clic su **Nuovo dashboard**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-146">To create a new dashboard, click **New dashboard**.</span></span> <span data-ttu-id="ba42d-147">Verrà visualizzata l'area di lavoro del dashboard priva di riquadri.</span><span class="sxs-lookup"><span data-stu-id="ba42d-147">The dashboard workspace appears, with no tiles present.</span></span> <span data-ttu-id="ba42d-148">È quindi possibile aggiungere, rimuovere e regolare i riquadri come si preferisce.</span><span class="sxs-lookup"><span data-stu-id="ba42d-148">You can then add, remove and adjust tiles however you like.</span></span> <span data-ttu-id="ba42d-149">Dopo aver finito di personalizzare il dashboard, fare clic su **Fatto** per salvare e passare a tale dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-149">When you are finished customizing the dashboard, click **Done customizing** to save and switch to that dashboard.</span></span>

<a name="upload-download"></a>

## <a name="upload-and-download"></a><span data-ttu-id="ba42d-150">Caricare e scaricare</span><span class="sxs-lookup"><span data-stu-id="ba42d-150">Upload and Download</span></span>

<span data-ttu-id="ba42d-151">I pulsanti **Carica** e **Scarica** consentono di scaricare il dashboard corrente come file con estensione json, personalizzarlo e quindi distribuirlo e caricarlo o farlo caricare da un altro utente di nuovo nel portale di Azure, in sostituzione del dashboard corrente.</span><span class="sxs-lookup"><span data-stu-id="ba42d-151">The **Upload** and **Download** buttons enable you to download your current dashboard as a JSON file, customize it, and then distribute it and upload it or have someone else upload that file back to the Azure portal, thereby replacing their current dashboard.</span></span>

<span data-ttu-id="ba42d-152">Se si fa clic su **Scarica**, il dashboard corrente viene scaricato nella cartella dei download predefinita.</span><span class="sxs-lookup"><span data-stu-id="ba42d-152">If you click **Download**, the current dashboard downloads into your default Downloads folder.</span></span> <span data-ttu-id="ba42d-153">Quando si apre il file scaricato, viene visualizzato il codice JSON.</span><span class="sxs-lookup"><span data-stu-id="ba42d-153">Opening the downloaded file then shows the JSON code.</span></span>

![Codice JSON del dashboard](../media-draft/8-dashboard-json-code.png)

<span data-ttu-id="ba42d-155">È possibile modificare il codice manualmente, ad esempio si possono modificare le dimensioni dei riquadri, e successivamente caricarlo in Azure usando il pulsante **Carica**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-155">You can then edit that code manually (for example, by changing tile sizes) and then upload it back to Azure by clicking the **Upload** button.</span></span>

<a name="edit-dashboard"></a>

### <a name="edit-a-dashboard"></a><span data-ttu-id="ba42d-156">Modificare un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-156">Edit a dashboard</span></span>

<span data-ttu-id="ba42d-157">Sebbene sia possibile modificare un dashboard scaricando il file JSON, modificando i valori nel file e caricandolo nuovamente in Azure, questo approccio non è intuitivo per la progettazione di un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ba42d-157">Although you can edit a dashboard by downloading the JSON file, changing values in the file, and uploading the file back to Azure, that approach isn't intuitive for designing a user interface.</span></span> <span data-ttu-id="ba42d-158">Per usare l'interfaccia utente grafica per configurare il dashboard corrente, è possibile accedere alla modalità di modifica in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="ba42d-158">To use the GUI to configure your current dashboard you can enter edit mode in several ways:</span></span>

1. <span data-ttu-id="ba42d-159">Fare clic sul pulsante **Modifica**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-159">Click the **Edit** button</span></span>
1. <span data-ttu-id="ba42d-160">Fare clic con il pulsante destro del mouse sul dashboard e scegliere **Modifica**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-160">Right-click on the dashboard and click **Edit**.</span></span> 
1. <span data-ttu-id="ba42d-161">Passare il mouse su un riquadro del dashboard. Verrà visualizzato un menu `...` nell'angolo in alto a destra con le opzioni di modifica.</span><span class="sxs-lookup"><span data-stu-id="ba42d-161">Hover over a tile on the dashboard - a `...` menu will appear on the top/right corner with edit options.</span></span>

<span data-ttu-id="ba42d-162">Il dashboard passerà alla modalità di modifica.</span><span class="sxs-lookup"><span data-stu-id="ba42d-162">The dashboard switches to edit mode.</span></span>

![Modificare il dashboard](../media-draft/8-edit-dashboard.png)

<span data-ttu-id="ba42d-164">Sul lato sinistro è visualizzata la Raccolta riquadri con una serie di riquadri possibili.</span><span class="sxs-lookup"><span data-stu-id="ba42d-164">On the left-hand side appears the Tile Gallery, with several possible tiles.</span></span> <span data-ttu-id="ba42d-165">È possibile filtrare la Raccolta riquadri per categoria e tipo di risorsa:</span><span class="sxs-lookup"><span data-stu-id="ba42d-165">You can filter the Tile Gallery by category and resource type:</span></span>

![Raccolta riquadri](../media-draft/8-tile-gallery.png)

<span data-ttu-id="ba42d-167">Per aggiungere un riquadro basta selezionare quello desiderato nell'elenco a sinistra e trascinarlo nell'area di lavoro.</span><span class="sxs-lookup"><span data-stu-id="ba42d-167">Adding tiles is as easy as selecting the tile from the list on the left and then dragging it to the work area.</span></span> <span data-ttu-id="ba42d-168">È quindi possibile posizionare ogni riquadro, ridimensionarlo o modificare i dati in esso visualizzati.</span><span class="sxs-lookup"><span data-stu-id="ba42d-168">You can then move each tile about, resize it, or change the data that it displays.</span></span>

> [!TIP]
> <span data-ttu-id="ba42d-169">Una funzionalità interessante ma nota a pochi consente di inserire nel dashboard elementi dei pannelli figlio.</span><span class="sxs-lookup"><span data-stu-id="ba42d-169">One cool feature a lot of people are unaware of is that you can take elements on child blades and put them on your dashboard.</span></span> <span data-ttu-id="ba42d-170">È sufficiente passare il mouse sull'elemento e cercare il menu di modifica dei riquadri `...`, che contiene un'opzione "Aggiungi al dashboard" che consente di inserire rapidamente un riquadro da un servizio all'interno del dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-170">Just hover over the item and look for the `...` tile edit menu - this will have a "Pin to Dashboard" option which lets you quickly grab a tile from a service and put it onto the dashboard.</span></span>

<span data-ttu-id="ba42d-171">L'area di lavoro in modalità di modifica è suddivisa in sezioni quadrate.</span><span class="sxs-lookup"><span data-stu-id="ba42d-171">The work area in edit mode is divided into squares.</span></span> <span data-ttu-id="ba42d-172">Ogni riquadro deve occupare almeno un quadrato e i riquadri si allineano al set di divisori di riquadri più grande e più vicino.</span><span class="sxs-lookup"><span data-stu-id="ba42d-172">Each tile must occupy at least one square, and tiles will snap to the nearest largest set of tile dividers.</span></span> <span data-ttu-id="ba42d-173">Tutti i riquadri che si sovrappongono vengono spostati.</span><span class="sxs-lookup"><span data-stu-id="ba42d-173">Any overlapping tiles are moved out of the way.</span></span> <span data-ttu-id="ba42d-174">Quando si crea un riquadro di dimensioni più piccole, i riquadri circostanti si riallineano rispetto ad esso.</span><span class="sxs-lookup"><span data-stu-id="ba42d-174">When you make a tile smaller, the surrounding tiles will move back up against it.</span></span>

#### <a name="change-tile-sizes"></a><span data-ttu-id="ba42d-175">Modificare le dimensioni dei riquadri</span><span class="sxs-lookup"><span data-stu-id="ba42d-175">Change tile sizes</span></span>

<span data-ttu-id="ba42d-176">Alcuni riquadri hanno dimensioni definite che possono essere modificate solo a livello di programmazione.</span><span class="sxs-lookup"><span data-stu-id="ba42d-176">Some tiles have a set size, and you can edit their size only programmatically.</span></span> <span data-ttu-id="ba42d-177">I riquadri con un angolo inferiore destro grigio invece possono essere modificati trascinando l'indicatore angolare.</span><span class="sxs-lookup"><span data-stu-id="ba42d-177">However, you can edit tiles with a gray bottom right-hand corner by dragging the corner indicator.</span></span>

![Riquadro ridimensionabile](../media-draft/8-resizable-tile.png)

<span data-ttu-id="ba42d-179">In alternativa, è possibile fare clic con il pulsante destro del mouse sul menu di scelta rapida e specificare le dimensioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="ba42d-179">Alternatively, right-click the context menu and specify the size you want.</span></span>

![Dimensioni del riquadro](../media-draft/8-tile-size.png)

<span data-ttu-id="ba42d-181">Per creare il dashboard, trascinare i riquadri dalla Raccolta riquadri nell'area di lavoro e quindi disporli nell'ordine desiderato.</span><span class="sxs-lookup"><span data-stu-id="ba42d-181">To create your dashboard, pull tiles from the Tile Gallery onto the workspace and then rearrange them.</span></span>

#### <a name="change-tile-settings"></a><span data-ttu-id="ba42d-182">Modificare le impostazioni dei riquadri</span><span class="sxs-lookup"><span data-stu-id="ba42d-182">Change tile settings</span></span>

<span data-ttu-id="ba42d-183">Alcuni riquadri dispongono di impostazioni modificabili.</span><span class="sxs-lookup"><span data-stu-id="ba42d-183">Some tiles have editable settings.</span></span> <span data-ttu-id="ba42d-184">Ad esempio, quando viene trascinato nell'area di lavoro, il riquadro Orologio visualizza il riquadro **Modifica orologio**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-184">For example, with the clock tile, when you drag it onto the workspace, it opens the **Edit clock** tile.</span></span> <span data-ttu-id="ba42d-185">È quindi possibile impostare il fuso orario e anche scegliere se visualizzare l'ora in formato di 12 o 24 ore.</span><span class="sxs-lookup"><span data-stu-id="ba42d-185">You can then set the time zone, which it displays, and also set whether it displays in 12- or 24-hour format.</span></span>

![Modificare l'orologio](../media-draft/8-edit-clock.png)

<span data-ttu-id="ba42d-187">Per le aziende multinazionali o intercontinentali, è possibile aggiungere orologi, ciascuno con un fuso orario diverso.</span><span class="sxs-lookup"><span data-stu-id="ba42d-187">For multi-national or transcontinental companies, you can add clocks, each in a different time zone.</span></span>

#### <a name="accepting-your-edits"></a><span data-ttu-id="ba42d-188">Accettare le modifiche</span><span class="sxs-lookup"><span data-stu-id="ba42d-188">Accepting your edits</span></span>

<span data-ttu-id="ba42d-189">Dopo avere disposto i riquadri secondo le proprie esigenze, fare clic su **Fatto** oppure fare clic con il pulsante destro del mouse e selezionare **Fatto**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-189">When you have arranged the tiles as you want them, either click **Done customizing**, or right-click and then click **Done customizing**.</span></span>

## <a name="edit-a-dashboard-by-changing-the-json-file"></a><span data-ttu-id="ba42d-190">Modificare un dashboard modificando il file con estensione json</span><span class="sxs-lookup"><span data-stu-id="ba42d-190">Edit a dashboard by changing the JSON file</span></span>

<span data-ttu-id="ba42d-191">È possibile modificare un dashboard modificando il file con estensione json.</span><span class="sxs-lookup"><span data-stu-id="ba42d-191">You can also edit a dashboard by changing the JSON file.</span></span> <span data-ttu-id="ba42d-192">Questo approccio offre più opzioni per modificare le impostazioni, ma non è possibile vedere le modifiche fino a quando non si carica di nuovo il file in Azure.</span><span class="sxs-lookup"><span data-stu-id="ba42d-192">This approach provides more options for changing settings, but you cannot see the changes until you upload the file back into Azure.</span></span>

![Impostazioni JSON](../media-draft/8-json-code.png)

<span data-ttu-id="ba42d-194">Nell'esempio precedente, per modificare le dimensioni del riquadro, modificare le variabili **colSpan** e **rowSpan**, quindi salvare il file e caricarlo di nuovo in Azure.</span><span class="sxs-lookup"><span data-stu-id="ba42d-194">In the example above, to change the size of the tile, edit the **colSpan** and **rowSpan** variables, then save the file and upload it back to Azure.</span></span> <span data-ttu-id="ba42d-195">È anche possibile distribuire il file ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="ba42d-195">You can also distribute the file to other users.</span></span>

## <a name="reset-a-dashboard"></a><span data-ttu-id="ba42d-196">Reimpostare lo stato predefinito di un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-196">Reset a dashboard</span></span>

<span data-ttu-id="ba42d-197">È possibile reimpostare un dashboard sullo stile predefinito.</span><span class="sxs-lookup"><span data-stu-id="ba42d-197">You can reset any dashboard to the default style.</span></span> <span data-ttu-id="ba42d-198">In modalità di modifica fare clic con il pulsante destro del mouse e selezionare **Ripristina valori predefiniti**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-198">In edit mode, right-click and select **Reset to default state**.</span></span> <span data-ttu-id="ba42d-199">Verrà visualizzata una finestra di dialogo che chiederà di confermare che si intende reimpostare il dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-199">A dialog box will ask you to confirm that you want to reset that dashboard.</span></span>

<a name="share-dashboard"></a>

## <a name="share-or-unshare-a-dashboard"></a><span data-ttu-id="ba42d-200">Condividere o annullare la condivisione di un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-200">Share or unshare a dashboard</span></span>

<span data-ttu-id="ba42d-201">Un dashboard appena creato è privato e visibile solo all'account che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="ba42d-201">When you define a new dashboard, it is private and visible only to your account.</span></span> <span data-ttu-id="ba42d-202">Per renderlo visibile ad altri utenti, è necessario condividerlo.</span><span class="sxs-lookup"><span data-stu-id="ba42d-202">To make it visible to others, you need to share a dashboard.</span></span> <span data-ttu-id="ba42d-203">Come per qualsiasi altra risorsa di Azure, è tuttavia necessario specificare un gruppo di risorse (o usare un gruppo di risorse esistente) in cui memorizzare il dashboard condiviso.</span><span class="sxs-lookup"><span data-stu-id="ba42d-203">However, as with any other Azure resource, you need to specify a resource group (or use an existing resource group) to store shared dashboards in.</span></span> <span data-ttu-id="ba42d-204">Se non si dispone di un gruppo di risorse esistente, Azure creerà un gruppo di risorse *dashboard* nel percorso che si specifica.</span><span class="sxs-lookup"><span data-stu-id="ba42d-204">If you do not have an existing resource group, Azure will create a *dashboards* resource group in whichever location you specify.</span></span> <span data-ttu-id="ba42d-205">Se si dispone di gruppi di risorse esistenti, è possibile specificarne uno in cui archiviare i dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-205">If you have existing resource groups, you can specify that resource group to store the dashboards.</span></span>

![Condivisione e controllo dell'accesso 1](../media-draft/8-share-dashboards-default.png)

<span data-ttu-id="ba42d-207">Dopo avere condiviso il modello, viene visualizzato un secondo pannello **Condivisione e controllo dell'accesso**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-207">When you have shared the template, you will see a second **Sharing + access control** blade.</span></span>

![Condivisione e controllo dell'accesso 2](../media-draft/8-share-dashboards-access-control.png)

<span data-ttu-id="ba42d-209">È quindi possibile fare clic su **Gestisci utenti** per specificare gli utenti che hanno accesso a tale dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-209">You can then click **Manage users** to specify the users who have access to that dashboard.</span></span>

### <a name="switching-to-a-shared-dashboard"></a><span data-ttu-id="ba42d-210">Passaggio a un dashboard condiviso</span><span class="sxs-lookup"><span data-stu-id="ba42d-210">Switching to a shared dashboard</span></span>

<span data-ttu-id="ba42d-211">Per passare a un dashboard condiviso, fare clic sull'elenco dei dashboard, quindi su **Sfoglia tutti i dashboard**.</span><span class="sxs-lookup"><span data-stu-id="ba42d-211">To switch to a shared dashboard, you click on the list of dashboards, and then click **Browse all dashboards**.</span></span>

![Sfogliare tutti i dashboard](../media-draft/8-browse-dashboards.png)

<span data-ttu-id="ba42d-213">Verrà a questo punto visualizzato il pannello **Tutti i dashboard**, con i nomi di tutti i dashboard condivisi visualizzati.</span><span class="sxs-lookup"><span data-stu-id="ba42d-213">You will now see the **All dashboards** blade, with the names of any shared dashboards displayed.</span></span> <span data-ttu-id="ba42d-214">È sufficiente fare clic su un dashboard per visualizzarlo nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba42d-214">Just click on a dashboard to apply it to the Azure portal.</span></span>

![Dashboard condivisi](../media-draft/8-select-shared-dashboard.png)

<a name="full-screen"></a>

## <a name="display-a-dashboard-as-a-full-screen"></a><span data-ttu-id="ba42d-216">Visualizzare un dashboard in modalità schermo intero</span><span class="sxs-lookup"><span data-stu-id="ba42d-216">Display a dashboard as a full screen</span></span>

<span data-ttu-id="ba42d-217">Se si vuole visualizzare il dashboard alle massime dimensioni, fare clic sul pulsante **Schermo intero** per visualizzare il dashboard corrente senza i menu del browser.</span><span class="sxs-lookup"><span data-stu-id="ba42d-217">If you want the largest dashboard real estate, click the **Full screen** button to display your current dashboard without any browser menus.</span></span> <span data-ttu-id="ba42d-218">Se sono presenti riquadri oltre i limiti dello schermo, vengono visualizzate le barre di scorrimento a destra e in basso.</span><span class="sxs-lookup"><span data-stu-id="ba42d-218">If you have any tiles outside the boundaries of your screen display, slider bars will appear at the right and bottom of your screen.</span></span>

<span data-ttu-id="ba42d-219">Dopo avere completato le azioni necessarie nella modalità schermo intero, premere ESC o fare clic su **Chiudi la visualizzazione schermo intero** accanto al nome del dashboard in cima alla schermata.</span><span class="sxs-lookup"><span data-stu-id="ba42d-219">When you have finished working in full-screen mode, press the ESC key or click **Exit Full Screen** next to the Dashboard name at the top of the screen.</span></span>

<a name="clone-dashboard"></a>

## <a name="clone-a-dashboard"></a><span data-ttu-id="ba42d-220">Clonare un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-220">Clone a dashboard</span></span>

<span data-ttu-id="ba42d-221">Quando si clona un dashboard, viene creata una copia istantanea denominata "Duplicato di \<nome del dashboard>", che diventa automaticamente il dashboard corrente.</span><span class="sxs-lookup"><span data-stu-id="ba42d-221">Cloning a dashboard creates an instant copy called "Clone of \<dashboard name>" and switches to that copy as the current dashboard.</span></span> <span data-ttu-id="ba42d-222">La clonazione è anche un modo semplice per creare dashboard prima di condividerli.</span><span class="sxs-lookup"><span data-stu-id="ba42d-222">Cloning is also an easy way to create dashboards before sharing them.</span></span> <span data-ttu-id="ba42d-223">Se ad esempio si dispone di un dashboard che è quasi come lo si vorrebbe esattamente, è sufficiente clonarlo, apportarvi le modifiche necessarie e quindi condividerlo.</span><span class="sxs-lookup"><span data-stu-id="ba42d-223">For example, if you have a dashboard that is almost as you want it, clone it, make the changes that you need, and then share it.</span></span>

<a name="delete-dashboard"></a>

## <a name="delete-a-dashboard"></a><span data-ttu-id="ba42d-224">Eliminare un dashboard</span><span class="sxs-lookup"><span data-stu-id="ba42d-224">Delete a dashboard</span></span>

<span data-ttu-id="ba42d-225">L'eliminazione di un dashboard ne determina la rimozione dall'elenco dei dashboard disponibili.</span><span class="sxs-lookup"><span data-stu-id="ba42d-225">Deleting a dashboard removes it from your list of available dashboards.</span></span> <span data-ttu-id="ba42d-226">Viene chiesto di confermare che si intende eliminare il dashboard e non sono disponibili funzionalità per il ripristino di un dashboard che è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="ba42d-226">You are prompted to confirm that you want to delete the dashboard, but there is no facility to recover a dashboard that has been deleted.</span></span>

<span data-ttu-id="ba42d-227">È possibile provare alcune di queste opzioni tramite la creazione di un nuovo dashboard.</span><span class="sxs-lookup"><span data-stu-id="ba42d-227">Let's try out some of these options by creating a new dashboard.</span></span>
