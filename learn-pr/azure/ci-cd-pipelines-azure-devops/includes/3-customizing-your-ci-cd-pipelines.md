<span data-ttu-id="7399d-101">L'esperienza predefinita del progetto Azure DevOps crea pipeline di compilazione e versione adeguate per le tecnologie selezionate.</span><span class="sxs-lookup"><span data-stu-id="7399d-101">The out of the box experience with an Azure DevOps Project creates build and release pipelines that make sense for the technologies picked.</span></span> <span data-ttu-id="7399d-102">Per questo modulo, è stata creata una pipeline di compilazione e versione adeguata per un'app node.js in esecuzione in un contenitore ospitato in un cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="7399d-102">For this module, you created a build and release pipeline that makes sense for a node.js app running in a container hosted in a Kubernetes cluster.</span></span> 

<span data-ttu-id="7399d-103">Spesso è necessario personalizzare le pipeline di compilazione e versione per eseguire operazioni specifiche per il progetto.</span><span class="sxs-lookup"><span data-stu-id="7399d-103">Often times we need to customize the build and release pipelines to do specific things for our project.</span></span> <span data-ttu-id="7399d-104">Le pipeline di compilazione e di versione in VSTS sono personalizzabili al 100%.</span><span class="sxs-lookup"><span data-stu-id="7399d-104">The build and release pipelines in VSTS are 100% customizable.</span></span> <span data-ttu-id="7399d-105">È possibile controllare le operazioni eseguite dalle pipeline.</span><span class="sxs-lookup"><span data-stu-id="7399d-105">You can make the pipelines do whatever you need it to do.</span></span>

<span data-ttu-id="7399d-106">In questa unità si apprenderà come personalizzare le pipeline di compilazione e di versione.</span><span class="sxs-lookup"><span data-stu-id="7399d-106">In this unit, learn how to customize your build and release pipelines.</span></span>

## <a name="customize-the-build-pipeline"></a><span data-ttu-id="7399d-107">Personalizzare la pipeline di compilazione</span><span class="sxs-lookup"><span data-stu-id="7399d-107">Customize the build pipeline</span></span>

<span data-ttu-id="7399d-108">Il motore di compilazione in VSTS è solo uno strumento di esecuzione attività, che esegue un'attività dopo l'altra.</span><span class="sxs-lookup"><span data-stu-id="7399d-108">The build engine in VSTS is just a task runner, doing one task, after another.</span></span> <span data-ttu-id="7399d-109">Per personalizzare la compilazione, è sufficiente aggiungere o rimuovere le attività e inserire i parametri corretti per l'attività.</span><span class="sxs-lookup"><span data-stu-id="7399d-109">To customize the build, you just add or remove tasks and fill out the correct parameters for the task.</span></span>

<span data-ttu-id="7399d-110">Per impostazione predefinita, VSTS include circa 100 attività che è possibile usare.</span><span class="sxs-lookup"><span data-stu-id="7399d-110">Out of the box, VSTS comes with about 100 tasks that you can use.</span></span> <span data-ttu-id="7399d-111">Se è necessario eseguire un'operazione non predefinita, controllare nel marketplace in cui sono presenti oltre 700 attività di compilazione e versione, pronte per essere scaricate e usate.</span><span class="sxs-lookup"><span data-stu-id="7399d-111">If you need to do something that doesn't exist out of the box, check the marketplace where there are over 700 build and release tasks ready to be downloaded and used.</span></span> <span data-ttu-id="7399d-112">È anche possibile scrivere attività personalizzate.</span><span class="sxs-lookup"><span data-stu-id="7399d-112">You also have the ability to write your own custom tasks.</span></span>

<span data-ttu-id="7399d-113">Per questa unità, si personalizzerà la pipeline di compilazione installando le attività di marketplace WhiteSource Bolt per eseguire l'analisi di sicurezza del codice.</span><span class="sxs-lookup"><span data-stu-id="7399d-113">For this unit, you will customize the build pipeline by installing the marketplace tasks WhiteSource Bolt to do security scanning of our code.</span></span>

1. <span data-ttu-id="7399d-114">Passare al progetto di Azure DevOps nel portale di Azure e fare clic sul collegamento alla definizione di compilazione</span><span class="sxs-lookup"><span data-stu-id="7399d-114">Browse to the Azure DevOps Project in the Azure portal and click on the build definition link</span></span>  
![Collegamento alla compilazione](/media-draft/3-buildlink.png)

2. <span data-ttu-id="7399d-116">Verrà visualizzata la pagina delle pipeline di compilazione.</span><span class="sxs-lookup"><span data-stu-id="7399d-116">This takes you to the build pipelines page.</span></span> <span data-ttu-id="7399d-117">Fare clic sulla compilazione e selezionare `Edit`</span><span class="sxs-lookup"><span data-stu-id="7399d-117">Click on the build and select `Edit`</span></span>  
<span data-ttu-id="7399d-118">![Modifica compilazione](/media-draft/3-editbuild.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-118">![Edit Build](/media-draft/3-editbuild.png)</span></span>

3. <span data-ttu-id="7399d-119">Verrà visualizzata la definizione di compilazione.</span><span class="sxs-lookup"><span data-stu-id="7399d-119">This takes you to the build definition.</span></span> <span data-ttu-id="7399d-120">Fare clic su `+` per aggiungere un'attività al Processo agente 1</span><span class="sxs-lookup"><span data-stu-id="7399d-120">Click on the `+` to add a task to Agent Job 1</span></span>  
<span data-ttu-id="7399d-121">![Aggiungi attività](/media-draft/3-addtask.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-121">![Add Task](/media-draft/3-addtask.png)</span></span>

4. <span data-ttu-id="7399d-122">Nel campo di testo digitare `bolt` e fare clic su `Get it free`</span><span class="sxs-lookup"><span data-stu-id="7399d-122">In the text field, type `bolt` and click `Get it free`</span></span>  
<span data-ttu-id="7399d-123">![Scarica gratuitamente](/media-draft/3-getitfree.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-123">![Get it free](/media-draft/3-getitfree.png)</span></span>

5. <span data-ttu-id="7399d-124">Consente di andare alla pagina del Marketplace di WhiteSource Bolt.</span><span class="sxs-lookup"><span data-stu-id="7399d-124">This takes you to the WhiteSource Bolt Marketplace page.</span></span> <span data-ttu-id="7399d-125">Fare clic su `Get it free`</span><span class="sxs-lookup"><span data-stu-id="7399d-125">Click `Get it free`</span></span>  
<span data-ttu-id="7399d-126">![Scarica WhiteSource Bolt gratuitamente](/media-draft/3-getwhitesourceboltfree.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-126">![Get White Source Bolt Free](/media-draft/3-getwhitesourceboltfree.png)</span></span>

6. <span data-ttu-id="7399d-127">Scegliere l'organizzazione di VSTS e quindi fare clic su `Install`</span><span class="sxs-lookup"><span data-stu-id="7399d-127">Choose your VSTS organization and then click `Install`</span></span>  
<span data-ttu-id="7399d-128">![Installa](/media-draft/3-install.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-128">![Install](/media-draft/3-install.png)</span></span>

7. <span data-ttu-id="7399d-129">Attivare WhiteSource Bolt seguendo le istruzioni riportate qui <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate></span><span class="sxs-lookup"><span data-stu-id="7399d-129">Activate WhiteSource Bolt by following the directions here <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate></span></span>

8. <span data-ttu-id="7399d-130">Tornare al portale di Azure con il progetto DevOps caricato e fare clic sul collegamento alla pipeline di compilazione</span><span class="sxs-lookup"><span data-stu-id="7399d-130">Go back to your Azure portal with the DevOps project loaded and click the build pipeline link</span></span>  
![Collegamento alla pipeline di compilazione](/media-draft/3-buildpipelinelink.png)

9. <span data-ttu-id="7399d-132">Selezionare la compilazione e fare clic su `Edit`</span><span class="sxs-lookup"><span data-stu-id="7399d-132">Select your build and click `Edit`</span></span>  
<span data-ttu-id="7399d-133">![Modifica compilazione](/media-draft/3-editbuild.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-133">![Edit Build](/media-draft/3-editbuild.png)</span></span>

10. <span data-ttu-id="7399d-134">Fare clic su `+` Aggiungi un'attività a Processo agente 1, digitare `bolt` nel campo di ricerca e fare clic su `Add`</span><span class="sxs-lookup"><span data-stu-id="7399d-134">Click the `+` Add a task to Agent job 1, type in `bolt` in the search field and click `Add`</span></span>  
<span data-ttu-id="7399d-135">![Aggiungi Bolt](/media-draft/3-addbolt.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-135">![Add Bolt](/media-draft/3-addbolt.png)</span></span>

11. <span data-ttu-id="7399d-136">Verrà aggiunta l'attività di WhiteSource Bolt nella parte inferiore dell'elenco attività, trascinarla nella parte superiore</span><span class="sxs-lookup"><span data-stu-id="7399d-136">This adds the WhiteSource Bolt task to the bottom of the task list, drag it to the top</span></span>  
![Bolt in alto](/media-draft/3-boltattop.png)

12. <span data-ttu-id="7399d-138">Fare clic su `Save & queue` e selezionare `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="7399d-138">Click `Save & queue` and select `Save & queue`</span></span>  
<span data-ttu-id="7399d-139">![Salvare e accodare](/media-draft/3-saveandqueue.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-139">![Save and Queue](/media-draft/3-saveandqueue.png)</span></span>

13. <span data-ttu-id="7399d-140">Fare clic su `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="7399d-140">Click `Save & queue`</span></span>  
<span data-ttu-id="7399d-141">![Salva e accoda](/media-draft/3-saveandqueuedialog.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-141">![Save and Queue Dialog](/media-draft/3-saveandqueuedialog.png)</span></span>

<span data-ttu-id="7399d-142">La pipeline di compilazione modificata viene salvata e la compilazione viene messa in coda.</span><span class="sxs-lookup"><span data-stu-id="7399d-142">This saves the modified build pipeline and queues the build.</span></span> <span data-ttu-id="7399d-143">Al termine della compilazione, esaminando la compilazione `WhiteSource Bolt Build Report` è possibile osservare che il codice sorgente è stato analizzato da WhiteSource Bolt alla ricerca di vulnerabilità della sicurezza.</span><span class="sxs-lookup"><span data-stu-id="7399d-143">After the build finishes, looking at the build `WhiteSource Bolt Build Report`, you can see the source code was scanned by WhiteSource Bolt looking for security vulnerabilities.</span></span>

![Report di compilazione](/media-draft/3-buildreport.png)

## <a name="customize-the-release-pipeline"></a><span data-ttu-id="7399d-145">Personalizzare la pipeline di versione</span><span class="sxs-lookup"><span data-stu-id="7399d-145">Customize the release pipeline</span></span>

<span data-ttu-id="7399d-146">Proprio come per la compilazione, anche la pipeline di versione è uno strumento di esecuzione attività, che può essere personalizzato in modo analogo.</span><span class="sxs-lookup"><span data-stu-id="7399d-146">Like the build, the release pipeline is task runner and can be customized the same way.</span></span> <span data-ttu-id="7399d-147">Per questa unità, alla fine della versione si aggiungerà un test delle prestazioni Web,</span><span class="sxs-lookup"><span data-stu-id="7399d-147">For this unit, you will add a web performance test at the end of the release.</span></span> <span data-ttu-id="7399d-148">che consente di verificare che l'app sia distribuita e correttamente eseguita nel cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="7399d-148">This will verifying that your app is deployed and running successfully in the Kubernetes cluster.</span></span>

1. <span data-ttu-id="7399d-149">Passare al progetto DevOps nel portale di Azure e fare clic sul collegamento alla pipeline di versione</span><span class="sxs-lookup"><span data-stu-id="7399d-149">Browse to the DevOps project in the Azure Portal and click on the link for the release pipeline</span></span>  
![Collegamento alla versione](/media-draft/3-releaselink.png)

2. <span data-ttu-id="7399d-151">Verrà visualizzata la pagina della pipeline di versione.</span><span class="sxs-lookup"><span data-stu-id="7399d-151">This takes you to the release pipeline page.</span></span> <span data-ttu-id="7399d-152">Scegliere la pipeline di versione e fare clic su `Edit`</span><span class="sxs-lookup"><span data-stu-id="7399d-152">Click your release pipeline and click on `Edit`</span></span>  
<span data-ttu-id="7399d-153">![Modifica la pipeline di versione](/media-draft/3-editreleasepipeline.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-153">![Edit Release Pipeline](/media-draft/3-editreleasepipeline.png)</span></span>

3. <span data-ttu-id="7399d-154">Fare clic sulle attività nella fase `Dev` della versione</span><span class="sxs-lookup"><span data-stu-id="7399d-154">Click on the tasks in your release `Dev` stage</span></span>  
<span data-ttu-id="7399d-155">![Fase di versione](/media-draft/3-releasestage.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-155">![Release Stage](/media-draft/3-releasestage.png)</span></span>

4. <span data-ttu-id="7399d-156">Fare clic su `+` Aggiungi un'attività alla Fase1, digitare `web test` nel campo di ricerca e fare clic su `Add` per l'attività di test delle prestazioni Web basato sul cloud</span><span class="sxs-lookup"><span data-stu-id="7399d-156">Click on the `+` Add a task to Phase1, type `web test` in the search field and click `Add` for the Cloud-based Web Performance Test task</span></span>  
<span data-ttu-id="7399d-157">![Aggiungi test Web](/media-draft/3-addwebtest.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-157">![Add Web Test](/media-draft/3-addwebtest.png)</span></span>

5. <span data-ttu-id="7399d-158">Modificare l'attività Test prestazioni Web rapido facendo clic su di essa e aggiungendo l'URL alla propria app nell'URL del sito Web (per trovare l'URL, passare alla pagina del progetto DevOps del portale di Azure e, a destra, fare clic con il pulsante destro del mouse sull'endpoint esterno dell'app di esempio e copiare il collegamento), quindi per TestName immettere `Ping Test`</span><span class="sxs-lookup"><span data-stu-id="7399d-158">Edit the Quick Web Performance Test task by clicking on it and adding the url to your app in the Website URL (to find the url, go to the Azure portal DevOps project page and on the right-hand side, right-click your sample app external endpoint and copy link) and then for TestName, enter in `Ping Test`</span></span>  
<span data-ttu-id="7399d-159">![Copia URL](/media-draft/3-copyurl.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-159">![Copy URL](/media-draft/3-copyurl.png)</span></span>  
<span data-ttu-id="7399d-160">![Modificare l'attività di test Web](/media-draft/3-editwebtesttask.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-160">![Edit Web Test Task](/media-draft/3-editwebtesttask.png)</span></span>

6. <span data-ttu-id="7399d-161">Fare clic su `Save`</span><span class="sxs-lookup"><span data-stu-id="7399d-161">Click `Save`</span></span>  
<span data-ttu-id="7399d-162">![Salvare la versione](/media-draft/3-saverelease.png)</span><span class="sxs-lookup"><span data-stu-id="7399d-162">![Save Release](/media-draft/3-saverelease.png)</span></span>

<span data-ttu-id="7399d-163">A questo punto, quando si esegue una versione, dopo aver distribuito il nuovo pacchetto Helm, viene eseguito un test Web per raggiungere correttamente l'URL dell'app.</span><span class="sxs-lookup"><span data-stu-id="7399d-163">Now, when you run a release, after deploying the new helm package, a web test is run hitting the app url successfully.</span></span>

![Test Web](/media-draft/3-webtest.png)


## <a name="summary"></a><span data-ttu-id="7399d-165">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7399d-165">Summary</span></span>

<span data-ttu-id="7399d-166">In questa unità si è appreso come personalizzare le pipeline di compilazione e di versione.</span><span class="sxs-lookup"><span data-stu-id="7399d-166">In this unit, you learned how to customize your build and release pipelines.</span></span> <span data-ttu-id="7399d-167">Si è anche appreso come installare e usare le attività dal Marketplace.</span><span class="sxs-lookup"><span data-stu-id="7399d-167">You also learned how to install and use tasks from the Marketplace.</span></span>