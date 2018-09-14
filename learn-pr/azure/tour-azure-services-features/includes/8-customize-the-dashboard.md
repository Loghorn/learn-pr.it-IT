<span data-ttu-id="fceda-101">Qui di seguito viene descritto come creare e modificare alcuni dashboard usando il portale di Azure e modificando direttamente il file con estensione json sottostante.</span><span class="sxs-lookup"><span data-stu-id="fceda-101">Next, let's look at how to create and modify dashboards using the Azure Portal, and by editing the underlying JSON file directly.</span></span>

## <a name="what-is-a-dashboard"></a><span data-ttu-id="fceda-102">Che cos'è un dashboard?</span><span class="sxs-lookup"><span data-stu-id="fceda-102">What is a dashboard?</span></span>

<span data-ttu-id="fceda-103">Un _dashboard_ è una raccolta personalizzabile di riquadri di interfaccia utente visualizzata nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="fceda-103">A _dashboard_ is a customizable collection of UI tiles displayed in the Azure portal.</span></span> <span data-ttu-id="fceda-104">È possibile aggiungere, rimuovere e posizionare i riquadri per creare la visualizzazione esatta desiderata e quindi salvarla come dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-104">You add, remove, and position tiles to create the exact view you want, and then save that view as a dashboard.</span></span> <span data-ttu-id="fceda-105">Sono supportati più dashboard ed è possibile passare tra un dashboard e l'altro a seconda delle esigenze.</span><span class="sxs-lookup"><span data-stu-id="fceda-105">Multiple dashboards are supported, and you can switch between them as needed.</span></span> <span data-ttu-id="fceda-106">È anche possibile condividere i dashboard con altri membri del team.</span><span class="sxs-lookup"><span data-stu-id="fceda-106">You can even share your dashboards with other team members.</span></span>

<span data-ttu-id="fceda-107">I dashboard offrono una notevole flessibilità in termini di modalità di gestione di Azure.</span><span class="sxs-lookup"><span data-stu-id="fceda-107">Dashboards give you considerable flexibility regarding how you manage Azure.</span></span> <span data-ttu-id="fceda-108">Ad esempio, è possibile creare dashboard per ruoli specifici all'interno dell'organizzazione, quindi usare il controllo degli accessi in base al ruolo per specificare quali utenti possono accedere ai dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-108">For example, you can create dashboards for specific roles within the organization, and then use role-based access control (RBAC) to control who can access that dashboard.</span></span> <span data-ttu-id="fceda-109">L'amministratore del database avrebbe quindi accesso a un dashboard che contiene le visualizzazioni del servizio di Database SQL, mentre l'amministratore di Azure Active Directory avrebbe accesso alle visualizzazioni degli utenti e dei gruppi di Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fceda-109">Hence, your database administrator would have a dashboard that contains views of the SQL database service, whereas your Azure Active Directory administrator would have views of the users and groups within Azure AD.</span></span> <span data-ttu-id="fceda-110">È anche possibile personalizzare il portale tra gli ambienti di produzione e sviluppo all'interno del portale, creando un dashboard specifico per ogni ambiente che si gestisce.</span><span class="sxs-lookup"><span data-stu-id="fceda-110">You can even customize the portal between your production and development environments within the portal - creating a specific dashboard for each environment you are managing.</span></span>

<span data-ttu-id="fceda-111">I dashboard vengono archiviati come file JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="fceda-111">Dashboards are stored as JavaScript Object Notation (JSON) files.</span></span> <span data-ttu-id="fceda-112">Ciò significa che possono essere caricati e scaricati in altri computer o essere condivisi con i membri della directory di Azure.</span><span class="sxs-lookup"><span data-stu-id="fceda-112">This means they can be uploaded and downloaded to other computers, or shared with members of the Azure directory.</span></span> <span data-ttu-id="fceda-113">Azure archivia i dashboard all'interno di gruppi di risorse, proprio come le macchine virtuali o gli account di archiviazione che è possibile gestire all'interno del portale.</span><span class="sxs-lookup"><span data-stu-id="fceda-113">Azure stores dashboards within resource groups, just like virtual machines or storage accounts that you can manage within the portal.</span></span>

> [!TIP]
> <span data-ttu-id="fceda-114">Poiché i dashboard sono file JSON, è possibile anche [personalizzarli a livello di programmazione](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), trasformandoli quindi in validi strumenti di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="fceda-114">Because dashboards are JSON files, you can also [customize them programmatically](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically), making them compelling administrative tools.</span></span> <span data-ttu-id="fceda-115">Alcuni tipi di riquadri possono anche essere basati su query, in modo da essere aggiornati automaticamente quando i dati di origine cambiano.</span><span class="sxs-lookup"><span data-stu-id="fceda-115">Also, some tile types can be query-based, so they update automatically when the source data changes.</span></span>

## <a name="explore-the-default-dashboard"></a><span data-ttu-id="fceda-116">Esplorare il dashboard predefinito</span><span class="sxs-lookup"><span data-stu-id="fceda-116">Explore the default dashboard</span></span>

<span data-ttu-id="fceda-117">Il dashboard predefinito è denominato "Dashboard".</span><span class="sxs-lookup"><span data-stu-id="fceda-117">The default dashboard is named "Dashboard".</span></span> <span data-ttu-id="fceda-118">Quando si accede al portale, viene visualizzato questo dashboard contenente cinque Web part.</span><span class="sxs-lookup"><span data-stu-id="fceda-118">When you log into the portal, you are presented with this dashboard containing five web parts.</span></span>

![Web part predefinite](../media-draft/8-dashboard-default-webparts.png)

<span data-ttu-id="fceda-120">Le Web part predefinite sono</span><span class="sxs-lookup"><span data-stu-id="fceda-120">These default web parts are</span></span>

1. <span data-ttu-id="fceda-121">Tutte le risorse</span><span class="sxs-lookup"><span data-stu-id="fceda-121">All resources</span></span>

1. <span data-ttu-id="fceda-122">Guide introduttive ed esercitazioni</span><span class="sxs-lookup"><span data-stu-id="fceda-122">Quickstarts + tutorials</span></span>

1. <span data-ttu-id="fceda-123">Integrità dei servizi</span><span class="sxs-lookup"><span data-stu-id="fceda-123">Service Health</span></span>

1. <span data-ttu-id="fceda-124">Marketplace</span><span class="sxs-lookup"><span data-stu-id="fceda-124">Marketplace</span></span>

1. <span data-ttu-id="fceda-125">Introduzione ad Azure</span><span class="sxs-lookup"><span data-stu-id="fceda-125">Azure Getting Started</span></span>

## <a name="creating-and-managing-dashboards"></a><span data-ttu-id="fceda-126">Creazione e gestione dei dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-126">Creating and managing dashboards</span></span>

<span data-ttu-id="fceda-127">Nella parte superiore del dashboard sono presenti i controlli che consentono di creare, caricare, scaricare, modificare e condividere un dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-127">Along the top of the dashboard are the controls that enable you to create, upload, download, edit, and share a dashboard.</span></span> <span data-ttu-id="fceda-128">Sono presenti anche i controlli per visualizzare un dashboard a schermo intero, clonarlo o eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="fceda-128">You can also switch a dashboard to full screen, clone it, or delete it.</span></span>

![Personalizzare i controlli del dashboard](../media-draft/8-customise-dashboard-controls.png)

- [<span data-ttu-id="fceda-130">Selezionare un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-130">Select dashboard</span></span>](#select-dashboard)
- [<span data-ttu-id="fceda-131">Creare un nuovo dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-131">Create a new dashboard</span></span>](#create-new)
- [<span data-ttu-id="fceda-132">Caricare e scaricare</span><span class="sxs-lookup"><span data-stu-id="fceda-132">Upload and Download</span></span>](#upload-download)
- [<span data-ttu-id="fceda-133">Modificare</span><span class="sxs-lookup"><span data-stu-id="fceda-133">Edit</span></span>](#edit-dashboard)
- [<span data-ttu-id="fceda-134">Condividere</span><span class="sxs-lookup"><span data-stu-id="fceda-134">Share</span></span>](#share-dashboard)
- [<span data-ttu-id="fceda-135">Schermo intero</span><span class="sxs-lookup"><span data-stu-id="fceda-135">Full screen</span></span>](#full-screen)
- [<span data-ttu-id="fceda-136">Clonare</span><span class="sxs-lookup"><span data-stu-id="fceda-136">Clone</span></span>](#clone-dashboard)
- [<span data-ttu-id="fceda-137">Eliminare</span><span class="sxs-lookup"><span data-stu-id="fceda-137">Delete</span></span>](#delete-dashboard)

<a name="select-dashboard"></a>

## <a name="select-dashboard"></a><span data-ttu-id="fceda-138">Selezionare un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-138">Select dashboard</span></span>

<span data-ttu-id="fceda-139">All'estrema sinistra della barra degli strumenti è presente il controllo a discesa **Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="fceda-139">To the far left of the toolbar is the **Select Dashboard** drop-down control.</span></span> <span data-ttu-id="fceda-140">Facendo clic su questo controllo è possibile selezionare i dashboard che sono già stati definiti per l'account.</span><span class="sxs-lookup"><span data-stu-id="fceda-140">Clicking this control enables you to select from dashboards that you have already defined for your account.</span></span> <span data-ttu-id="fceda-141">Grazie a questo controllo è facile definire più dashboard per scopi diversi e quindi spostarsi tra l'uno e l'altro, a seconda delle azioni che si intende eseguire al momento.</span><span class="sxs-lookup"><span data-stu-id="fceda-141">This control makes it simple for you to define multiple dashboards for different purposes and then switch from one to another and back again, depending on what you are trying to do at the time.</span></span>

<span data-ttu-id="fceda-142">Tutti i dashboard sono inizialmente privati, ossia sono accessibili solo a chi li ha creati.</span><span class="sxs-lookup"><span data-stu-id="fceda-142">Note that any dashboards that you create will initially be private; that is, only you can see them.</span></span> <span data-ttu-id="fceda-143">Per rendere un dashboard disponibile agli altri utenti in un'azienda, è necessario condividerlo.</span><span class="sxs-lookup"><span data-stu-id="fceda-143">To make a dashboard available across your enterprise, you need to share it.</span></span> <span data-ttu-id="fceda-144">Questa opzione verrà esaminata a breve.</span><span class="sxs-lookup"><span data-stu-id="fceda-144">We'll look at that option shortly.</span></span>

<a name="create-new"></a>

## <a name="create-a-new-dashboard"></a><span data-ttu-id="fceda-145">Creare un nuovo dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-145">Create a new dashboard</span></span>

<span data-ttu-id="fceda-146">Per creare un nuovo dashboard, fare clic su **Nuovo dashboard**.</span><span class="sxs-lookup"><span data-stu-id="fceda-146">To create a new dashboard, click **New dashboard**.</span></span> <span data-ttu-id="fceda-147">Verrà visualizzata l'area di lavoro del dashboard priva di riquadri.</span><span class="sxs-lookup"><span data-stu-id="fceda-147">The dashboard workspace appears, with no tiles present.</span></span> <span data-ttu-id="fceda-148">È quindi possibile aggiungere, rimuovere e modificare i riquadri come desiderato. Queste azioni verranno esaminate a breve.</span><span class="sxs-lookup"><span data-stu-id="fceda-148">You can then add, remove and adjust tiles however you like - we'll look more at this in a bit.</span></span> <span data-ttu-id="fceda-149">Dopo aver finito di personalizzare il dashboard, fare clic su **Fatto** per salvare e passare a tale dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-149">When you are finished customizing the dashboard, click **Done customizing** to save and switch to that dashboard.</span></span>

<a name="upload-download"></a>

## <a name="upload-and-download"></a><span data-ttu-id="fceda-150">Caricare e scaricare</span><span class="sxs-lookup"><span data-stu-id="fceda-150">Upload and Download</span></span>

<span data-ttu-id="fceda-151">I pulsanti **Carica** e **Scarica** consentono di scaricare il dashboard corrente come file con estensione json, personalizzarlo e quindi distribuirlo e caricarlo o farlo caricare da un altro utente di nuovo nel portale di Azure, in sostituzione del dashboard corrente.</span><span class="sxs-lookup"><span data-stu-id="fceda-151">The **Upload** and **Download** buttons enable you to download your current dashboard as a JSON file, customize it, and then distribute it and upload it or have someone else upload that file back to the Azure portal, thereby replacing their current dashboard.</span></span>

<span data-ttu-id="fceda-152">Se si fa clic su **Scarica**, il dashboard corrente viene scaricato nella cartella dei download predefinita.</span><span class="sxs-lookup"><span data-stu-id="fceda-152">If you click **Download**, the current dashboard downloads into your default Downloads folder.</span></span> <span data-ttu-id="fceda-153">Quando si apre il file scaricato, viene visualizzato il codice JSON.</span><span class="sxs-lookup"><span data-stu-id="fceda-153">Opening the downloaded file then shows the JSON code.</span></span>

![Codice JSON del dashboard](../media-draft/8-dashboard-json-code.png)

<span data-ttu-id="fceda-155">È possibile modificare il codice manualmente, ad esempio si possono modificare le dimensioni dei riquadri, e successivamente caricarlo in Azure usando il pulsante **Carica**.</span><span class="sxs-lookup"><span data-stu-id="fceda-155">You can then edit that code manually (for example, by changing tile sizes) and then upload it back to Azure by clicking the **Upload** button.</span></span>

<a name="edit-dashboard"></a>

### <a name="edit-a-dashboard"></a><span data-ttu-id="fceda-156">Modificare un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-156">Edit a dashboard</span></span>

<span data-ttu-id="fceda-157">Sebbene sia possibile modificare un dashboard scaricando il file JSON, modificando i valori nel file e caricandolo nuovamente in Azure, questo approccio non è intuitivo per la progettazione di un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="fceda-157">Although you can edit a dashboard by downloading the JSON file, changing values in the file, and uploading the file back to Azure, that approach isn't intuitive for designing a user interface.</span></span> <span data-ttu-id="fceda-158">Per usare l'interfaccia utente grafica per configurare il dashboard corrente, è possibile accedere alla modalità di modifica in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="fceda-158">To use the GUI to configure your current dashboard you can enter edit mode in several ways:</span></span>

1. <span data-ttu-id="fceda-159">Fare clic sul pulsante **Modifica**</span><span class="sxs-lookup"><span data-stu-id="fceda-159">Click the **Edit** button</span></span>
1. <span data-ttu-id="fceda-160">Fare clic con il pulsante destro del mouse sul dashboard e scegliere **Modifica**.</span><span class="sxs-lookup"><span data-stu-id="fceda-160">Right-click on the dashboard and click **Edit**.</span></span> 
1. <span data-ttu-id="fceda-161">Passare il mouse su un riquadro del dashboard. Verrà visualizzato un menu `...` nell'angolo in alto a destra con le opzioni di modifica.</span><span class="sxs-lookup"><span data-stu-id="fceda-161">Hover over a tile on the dashboard - a `...` menu will appear on the top/right corner with edit options.</span></span>

<span data-ttu-id="fceda-162">Il dashboard passerà alla modalità di modifica.</span><span class="sxs-lookup"><span data-stu-id="fceda-162">The dashboard switches to edit mode.</span></span>

![Modificare il dashboard](../media-draft/8-edit-dashboard.png)

<span data-ttu-id="fceda-164">Sul lato sinistro viene visualizzata la Raccolta riquadri con una serie di riquadri possibili.</span><span class="sxs-lookup"><span data-stu-id="fceda-164">On the left-hand side appears the Tile Gallery, with several possible tiles.</span></span> <span data-ttu-id="fceda-165">È possibile filtrare la Raccolta riquadri in base ai criteri seguenti:</span><span class="sxs-lookup"><span data-stu-id="fceda-165">You can filter the Tile Gallery by the following criteria:</span></span>

- <span data-ttu-id="fceda-166">Generali</span><span class="sxs-lookup"><span data-stu-id="fceda-166">General</span></span>
- <span data-ttu-id="fceda-167">Tipo</span><span class="sxs-lookup"><span data-stu-id="fceda-167">Type</span></span>
- <span data-ttu-id="fceda-168">Cerca</span><span class="sxs-lookup"><span data-stu-id="fceda-168">Search</span></span>
- <span data-ttu-id="fceda-169">Gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="fceda-169">Resource Group</span></span>
- <span data-ttu-id="fceda-170">Tag</span><span class="sxs-lookup"><span data-stu-id="fceda-170">Tag</span></span>

![Raccolta riquadri](../media-draft/8-tile-gallery.png)

<span data-ttu-id="fceda-172">È possibile anche perfezionare ulteriormente ognuna di queste opzioni in base alla categoria, ad esempio Azure Active Directory, Internet delle cose (IoT), Microsoft Intune e così via.</span><span class="sxs-lookup"><span data-stu-id="fceda-172">You can also further refine each of these options by category (for example, Azure Active Directory, Internet of Things [IoT], Microsoft Intune, and so on).</span></span>

<span data-ttu-id="fceda-173">Per aggiungere un riquadro basta selezionare quello desiderato nell'elenco a sinistra e trascinarlo nell'area di lavoro.</span><span class="sxs-lookup"><span data-stu-id="fceda-173">Adding tiles is as easy as selecting the tile from the list on the left and then dragging it to the work area.</span></span> <span data-ttu-id="fceda-174">È quindi possibile posizionare ogni riquadro, ridimensionarlo o modificare i dati in esso visualizzati.</span><span class="sxs-lookup"><span data-stu-id="fceda-174">You can then move each tile about, resize it, or change the data that it displays.</span></span>

> [!TIP]
> <span data-ttu-id="fceda-175">Una funzionalità interessante ma nota a pochi consente di inserire nel dashboard elementi dei pannelli figlio.</span><span class="sxs-lookup"><span data-stu-id="fceda-175">One cool feature a lot of people are unaware of is that you can take elements on child blades and put them on your dashboard.</span></span> <span data-ttu-id="fceda-176">È sufficiente passare il mouse sull'elemento e cercare il menu di modifica dei riquadri `...`, che contiene un'opzione "Aggiungi al dashboard" che consente di inserire rapidamente un riquadro da un servizio all'interno del dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-176">Just hover over the item and look for the `...` tile edit menu - this will have a "Pin to Dashboard" option which lets you quickly grab a tile from a service and put it onto the dashboard.</span></span>

<span data-ttu-id="fceda-177">L'area di lavoro in modalità di modifica è suddivisa in sezioni quadrate.</span><span class="sxs-lookup"><span data-stu-id="fceda-177">The work area in edit mode is divided into squares.</span></span> <span data-ttu-id="fceda-178">Ogni riquadro deve occupare almeno un quadrato e i riquadri si allineano al set di divisori di riquadri più grande e più vicino.</span><span class="sxs-lookup"><span data-stu-id="fceda-178">Each tile must occupy at least one square, and tiles will snap to the nearest largest set of tile dividers.</span></span> <span data-ttu-id="fceda-179">Tutti i riquadri che si sovrappongono vengono spostati.</span><span class="sxs-lookup"><span data-stu-id="fceda-179">Any overlapping tiles are moved out of the way.</span></span> <span data-ttu-id="fceda-180">Quando si crea un riquadro di dimensioni più piccole, i riquadri circostanti si riallineano rispetto ad esso.</span><span class="sxs-lookup"><span data-stu-id="fceda-180">When you make a tile smaller, the surrounding tiles will move back up against it.</span></span>

#### <a name="change-tile-sizes"></a><span data-ttu-id="fceda-181">Modificare le dimensioni dei riquadri</span><span class="sxs-lookup"><span data-stu-id="fceda-181">Change tile sizes</span></span>

<span data-ttu-id="fceda-182">Alcuni riquadri hanno dimensioni definite che possono essere modificate solo a livello di programmazione.</span><span class="sxs-lookup"><span data-stu-id="fceda-182">Some tiles have a set size, and you can edit their size only programmatically.</span></span> <span data-ttu-id="fceda-183">I riquadri con un angolo inferiore destro grigio invece possono essere modificati trascinando l'indicatore angolare.</span><span class="sxs-lookup"><span data-stu-id="fceda-183">However, you can edit tiles with a gray bottom right-hand corner by dragging the corner indicator.</span></span>

![Riquadro ridimensionabile](../media-draft/8-resizable-tile.png)

<span data-ttu-id="fceda-185">In alternativa, è possibile fare clic con il pulsante destro del mouse sul menu di scelta rapida e specificare le dimensioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="fceda-185">Alternatively, right-click the context menu and specify the size you want.</span></span>

![Dimensioni del riquadro](../media-draft/8-tile-size.png)

<span data-ttu-id="fceda-187">Per creare il dashboard, trascinare i riquadri dalla Raccolta riquadri nell'area di lavoro e quindi disporli nell'ordine desiderato.</span><span class="sxs-lookup"><span data-stu-id="fceda-187">To create your dashboard, pull tiles from the Tile Gallery onto the workspace and then rearrange them.</span></span>

#### <a name="change-tile-settings"></a><span data-ttu-id="fceda-188">Modificare le impostazioni dei riquadri</span><span class="sxs-lookup"><span data-stu-id="fceda-188">Change tile settings</span></span>

<span data-ttu-id="fceda-189">Alcuni riquadri dispongono di impostazioni modificabili.</span><span class="sxs-lookup"><span data-stu-id="fceda-189">Some tiles have editable settings.</span></span> <span data-ttu-id="fceda-190">Ad esempio, quando viene trascinato nell'area di lavoro, il riquadro Orologio visualizza il riquadro **Modifica orologio**.</span><span class="sxs-lookup"><span data-stu-id="fceda-190">For example, with the clock tile, when you drag it onto the workspace, it opens the **Edit clock** tile.</span></span> <span data-ttu-id="fceda-191">È quindi possibile impostare il fuso orario e anche scegliere se visualizzare l'ora in formato di 12 o 24 ore.</span><span class="sxs-lookup"><span data-stu-id="fceda-191">You can then set the time zone, which it displays, and also set whether it displays in 12- or 24-hour format.</span></span>

![Modificare l'orologio](../media-draft/8-edit-clock.png)

<span data-ttu-id="fceda-193">Per le aziende multinazionali o intercontinentali, è possibile aggiungere orologi, ciascuno con un fuso orario diverso.</span><span class="sxs-lookup"><span data-stu-id="fceda-193">For multi-national or transcontinental companies, you can add clocks, each in a different time zone.</span></span>

#### <a name="accepting-your-edits"></a><span data-ttu-id="fceda-194">Accettare le modifiche</span><span class="sxs-lookup"><span data-stu-id="fceda-194">Accepting your edits</span></span>

<span data-ttu-id="fceda-195">Dopo avere disposto i riquadri secondo le proprie esigenze, fare clic su **Fatto** oppure fare clic con il pulsante destro del mouse e selezionare **Fatto**.</span><span class="sxs-lookup"><span data-stu-id="fceda-195">When you have arranged the tiles as you want them, either click **Done customizing**, or right-click and then click **Done customizing**.</span></span>

## <a name="edit-a-dashboard-by-changing-the-json-file"></a><span data-ttu-id="fceda-196">Modificare un dashboard modificando il file con estensione json</span><span class="sxs-lookup"><span data-stu-id="fceda-196">Edit a dashboard by changing the JSON file</span></span>

<span data-ttu-id="fceda-197">È possibile modificare un dashboard modificando il file con estensione json.</span><span class="sxs-lookup"><span data-stu-id="fceda-197">You can also edit a dashboard by changing the JSON file.</span></span> <span data-ttu-id="fceda-198">Questo approccio offre più opzioni per modificare le impostazioni, ma non è possibile vedere le modifiche fino a quando non si carica di nuovo il file in Azure.</span><span class="sxs-lookup"><span data-stu-id="fceda-198">This approach provides more options for changing settings, but you cannot see the changes until you upload the file back into Azure.</span></span>

![Impostazioni JSON](../media-draft/8-json-code.png)

<span data-ttu-id="fceda-200">Nell'esempio precedente, per modificare le dimensioni del riquadro, modificare le variabili **colSpan** e **rowSpan**, quindi salvare il file e caricarlo di nuovo in Azure.</span><span class="sxs-lookup"><span data-stu-id="fceda-200">In the example above, to change the size of the tile, edit the **colSpan** and **rowSpan** variables, then save the file and upload it back to Azure.</span></span> <span data-ttu-id="fceda-201">È anche possibile distribuire il file ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="fceda-201">You can also distribute the file to other users.</span></span>

## <a name="reset-a-dashboard"></a><span data-ttu-id="fceda-202">Reimpostare lo stato predefinito di un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-202">Reset a dashboard</span></span>

<span data-ttu-id="fceda-203">È possibile reimpostare un dashboard sullo stile predefinito.</span><span class="sxs-lookup"><span data-stu-id="fceda-203">You can reset any dashboard to the default style.</span></span> <span data-ttu-id="fceda-204">In modalità di modifica fare clic con il pulsante destro del mouse e selezionare **Ripristina valori predefiniti**.</span><span class="sxs-lookup"><span data-stu-id="fceda-204">In edit mode, right-click and select **Reset to default state**.</span></span> <span data-ttu-id="fceda-205">Verrà visualizzata una finestra di dialogo che chiederà di confermare che si intende reimpostare il dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-205">A dialog box will ask you to confirm that you want to reset that dashboard.</span></span>

<a name="share-dashboard"></a>

## <a name="share-or-unshare-a-dashboard"></a><span data-ttu-id="fceda-206">Condividere o annullare la condivisione di un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-206">Share or unshare a dashboard</span></span>

<span data-ttu-id="fceda-207">Un dashboard appena creato è privato e visibile solo all'account che li ha creati.</span><span class="sxs-lookup"><span data-stu-id="fceda-207">When you define a new dashboard, it is private and visible only to your account.</span></span> <span data-ttu-id="fceda-208">Per renderlo visibile ad altri utenti, è necessario condividerlo.</span><span class="sxs-lookup"><span data-stu-id="fceda-208">To make it visible to others, you need to share a dashboard.</span></span> <span data-ttu-id="fceda-209">Come per qualsiasi altra risorsa di Azure, è tuttavia necessario specificare un gruppo di risorse (o usare un gruppo di risorse esistente) in cui memorizzare il dashboard condiviso.</span><span class="sxs-lookup"><span data-stu-id="fceda-209">However, as with any other Azure resource, you need to specify a resource group (or use an existing resource group) to store shared dashboards in.</span></span> <span data-ttu-id="fceda-210">Se non si dispone di un gruppo di risorse esistente, Azure creerà un gruppo di risorse *dashboard* nel percorso che si specifica.</span><span class="sxs-lookup"><span data-stu-id="fceda-210">If you do not have an existing resource group, Azure will create a *dashboards* resource group in whichever location you specify.</span></span> <span data-ttu-id="fceda-211">Se si dispone di gruppi di risorse esistenti, è possibile specificarne uno in cui archiviare i dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-211">If you have existing resource groups, you can specify that resource group to store the dashboards.</span></span>

![Condivisione e controllo dell'accesso 1](../media-draft/8-share-dashboards-default.png)

<span data-ttu-id="fceda-213">Dopo avere condiviso il modello, viene visualizzato un secondo pannello **Condivisione e controllo dell'accesso**.</span><span class="sxs-lookup"><span data-stu-id="fceda-213">When you have shared the template, you will see a second **Sharing + access control** blade.</span></span>

![Condivisione e controllo dell'accesso 2](../media-draft/8-share-dashboards-access-control.png)

<span data-ttu-id="fceda-215">È quindi possibile fare clic su **Gestisci utenti** per specificare gli utenti che hanno accesso a tale dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-215">You can then click **Manage users** to specify the users who have access to that dashboard.</span></span>

### <a name="switching-to-a-shared-dashboard"></a><span data-ttu-id="fceda-216">Passaggio a un dashboard condiviso</span><span class="sxs-lookup"><span data-stu-id="fceda-216">Switching to a shared dashboard</span></span>

<span data-ttu-id="fceda-217">Per passare a un dashboard condiviso, fare clic sull'elenco dei dashboard, quindi su **Sfoglia tutti i dashboard**.</span><span class="sxs-lookup"><span data-stu-id="fceda-217">To switch to a shared dashboard, you click on the list of dashboards, and then click **Browse all dashboards**.</span></span>

![Sfogliare tutti i dashboard](../media-draft/8-browse-dashboards.png)

<span data-ttu-id="fceda-219">Verrà a questo punto visualizzato il pannello **Tutti i dashboard**, con i nomi di tutti i dashboard condivisi visualizzati.</span><span class="sxs-lookup"><span data-stu-id="fceda-219">You will now see the **All dashboards** blade, with the names of any shared dashboards displayed.</span></span> <span data-ttu-id="fceda-220">È sufficiente fare clic su un dashboard per visualizzarlo nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="fceda-220">Just click on a dashboard to apply it to the Azure portal.</span></span>

![Dashboard condivisi](../media-draft/8-select-shared-dashboard.png)

<a name="full-screen"></a>

## <a name="display-a-dashboard-as-a-full-screen"></a><span data-ttu-id="fceda-222">Visualizzare un dashboard in modalità schermo intero</span><span class="sxs-lookup"><span data-stu-id="fceda-222">Display a dashboard as a full screen</span></span>

<span data-ttu-id="fceda-223">Se si vuole visualizzare il dashboard alle massime dimensioni, fare clic sul pulsante **Schermo intero** per visualizzare il dashboard corrente senza i menu del browser.</span><span class="sxs-lookup"><span data-stu-id="fceda-223">If you want the largest dashboard real estate, click the **Full screen** button to display your current dashboard without any browser menus.</span></span> <span data-ttu-id="fceda-224">Se sono presenti riquadri oltre i limiti dello schermo, vengono visualizzate le barre di scorrimento a destra e in basso.</span><span class="sxs-lookup"><span data-stu-id="fceda-224">If you have any tiles outside the boundaries of your screen display, slider bars will appear at the right and bottom of your screen.</span></span>

<span data-ttu-id="fceda-225">Dopo avere completato le azioni necessarie nella modalità schermo intero, premere ESC o fare clic su **Chiudi la visualizzazione schermo intero** accanto al nome del dashboard in cima alla schermata.</span><span class="sxs-lookup"><span data-stu-id="fceda-225">When you have finished working in full-screen mode, press the ESC key or click **Exit Full Screen** next to the Dashboard name at the top of the screen.</span></span>

<a name="clone-dashboard"></a>

## <a name="clone-a-dashboard"></a><span data-ttu-id="fceda-226">Clonare un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-226">Clone a dashboard</span></span>

<span data-ttu-id="fceda-227">Quando si clona un dashboard, viene creata una copia istantanea denominata "Duplicato di \<nome del dashboard>", che diventa automaticamente il dashboard corrente.</span><span class="sxs-lookup"><span data-stu-id="fceda-227">Cloning a dashboard creates an instant copy called "Clone of \<dashboard name>" and switches to that copy as the current dashboard.</span></span> <span data-ttu-id="fceda-228">La clonazione è anche un modo semplice per creare dashboard prima di condividerli.</span><span class="sxs-lookup"><span data-stu-id="fceda-228">Cloning is also an easy way to create dashboards before sharing them.</span></span> <span data-ttu-id="fceda-229">Se ad esempio si dispone di un dashboard che è quasi come lo si vorrebbe esattamente, è sufficiente clonarlo, apportarvi le modifiche necessarie e quindi condividerlo.</span><span class="sxs-lookup"><span data-stu-id="fceda-229">For example, if you have a dashboard that is almost as you want it, clone it, make the changes that you need, and then share it.</span></span>

<a name="delete-dashboard"></a>

## <a name="delete-a-dashboard"></a><span data-ttu-id="fceda-230">Eliminare un dashboard</span><span class="sxs-lookup"><span data-stu-id="fceda-230">Delete a dashboard</span></span>

<span data-ttu-id="fceda-231">L'eliminazione di un dashboard ne determina la rimozione dall'elenco dei dashboard disponibili.</span><span class="sxs-lookup"><span data-stu-id="fceda-231">Deleting a dashboard removes it from your list of available dashboards.</span></span> <span data-ttu-id="fceda-232">Viene chiesto di confermare che si intende eliminare il dashboard e non sono disponibili funzionalità per il ripristino di un dashboard che è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="fceda-232">You are prompted to confirm that you want to delete the dashboard, but there is no facility to recover a dashboard that has been deleted.</span></span>

<span data-ttu-id="fceda-233">È possibile provare alcune di queste opzioni tramite la creazione di un nuovo dashboard.</span><span class="sxs-lookup"><span data-stu-id="fceda-233">Let's try out some of these options by creating a new dashboard.</span></span>
