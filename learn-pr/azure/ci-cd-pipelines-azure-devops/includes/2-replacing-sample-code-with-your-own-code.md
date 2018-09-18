<span data-ttu-id="fa23b-101">Se dopo avere creato un progetto di Azure DevOps si ricevono prima di tutto domande su come sostituire l'app di esempio con un'app personalizzata,</span><span class="sxs-lookup"><span data-stu-id="fa23b-101">After creating an Azure DevOps project, the first thing everyone ask is how do you replace the sample app with your own app?</span></span> <span data-ttu-id="fa23b-102">si tratta di una procedura abbastanza semplice e in questa unità verranno illustrati due modi per eseguirla.</span><span class="sxs-lookup"><span data-stu-id="fa23b-102">It's quite simple and in this unit, you will learn two ways to do it.</span></span>

1. <span data-ttu-id="fa23b-103">Sostituzione del codice nel repository Git per Visual Studio Team Services con codice effettivo</span><span class="sxs-lookup"><span data-stu-id="fa23b-103">Replacing the code in the VSTS git repository with your real code</span></span>

2. <span data-ttu-id="fa23b-104">Specificare come riferimento per la pipeline di compilazione un repository Git esterno contenente il codice effettivo</span><span class="sxs-lookup"><span data-stu-id="fa23b-104">Pointing your build pipeline to an external git repo holding your real code</span></span>

## <a name="replacing-code-in-vsts-git-repository"></a><span data-ttu-id="fa23b-105">Sostituzione del codice nel repository Git di Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="fa23b-105">Replacing code in VSTS git repository</span></span>

<span data-ttu-id="fa23b-106">Un approccio semplice consiste nel clonare il repository Git in Visual Studio Team Services sul disco rigido, sostituendo tutto con il codice personalizzato e infine ricaricando il repository in Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="fa23b-106">One simple way is by cloning the git repo in VSTS onto your hard drive, replacing everything with your own code, uploading back to VSTS and voila.</span></span> <span data-ttu-id="fa23b-107">Il codice verrà compilato e distribuito mediante la pipeline CI/CD.</span><span class="sxs-lookup"><span data-stu-id="fa23b-107">Your code will now be built and deployed through the CI/CD pipeline.</span></span>

<span data-ttu-id="fa23b-108">Per questa unità verranno prima di tutto eseguiti il download e l'archiviazione del codice sorgente dell'app Node.js sul disco rigido.</span><span class="sxs-lookup"><span data-stu-id="fa23b-108">For this unit, you will start by downloading and storing the source code to the node.js app onto your hard drive.</span></span>

1. <span data-ttu-id="fa23b-109">Scaricare il codice sorgente da <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip></span><span class="sxs-lookup"><span data-stu-id="fa23b-109">Download the source code from <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip></span></span>

2. <span data-ttu-id="fa23b-110">Estrarre i contenuti di MicrosoftLearnDevOps.zip in una posizione sul disco rigido.</span><span class="sxs-lookup"><span data-stu-id="fa23b-110">Extract the contents of MicrosoftLearnDevOps.zip somewhere on your hard drive.</span></span> <span data-ttu-id="fa23b-111">Per questo esempio è stato usato `C:\users\abel\Downloads\MicrosoftLearnDevOps`</span><span class="sxs-lookup"><span data-stu-id="fa23b-111">For this example `C:\users\abel\Downloads\MicrosoftLearnDevOps` was used</span></span>  
<span data-ttu-id="fa23b-112">![Directory non compressa](/media-draft/2-unzippedfolder.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-112">![Unzipped Directory](/media-draft/2-unzippedfolder.png)</span></span>

<span data-ttu-id="fa23b-113">È quindi necessario clonare il repository sul disco rigido e sostituire l'app di esempio con l'app Node.js effettiva.</span><span class="sxs-lookup"><span data-stu-id="fa23b-113">Next, you need to clone the repo onto your hard drive and replace the sample app with the real node.js app.</span></span> <span data-ttu-id="fa23b-114">In questa unità si presuppone che Git sia già installato nel computer.</span><span class="sxs-lookup"><span data-stu-id="fa23b-114">This unit assumes you already have git installed on your computer.</span></span>

1. <span data-ttu-id="fa23b-115">Dal portale di Azure passare al progetto di Azure DevOps e quindi fare clic sul collegamento del repository di codice.</span><span class="sxs-lookup"><span data-stu-id="fa23b-115">From Azure portal browse to your Azure DevOps Project and click on the code repository link.</span></span>  
![](/media-draft/2-browsetorepolink.png)

2. <span data-ttu-id="fa23b-116">Fare clic su Clona e copiare l'URL del repository Git disponibile in alto a destra.</span><span class="sxs-lookup"><span data-stu-id="fa23b-116">Click on Clone and copy the url for the git repo in the upper right-hand side.</span></span>  
![Copiare l'URL di clonazione](/media-draft/2-copycloneurl.png)  
<span data-ttu-id="fa23b-118">L'URL del repository verrà copiato negli Appunti</span><span class="sxs-lookup"><span data-stu-id="fa23b-118">This will copy the repo url to your clipboard</span></span>

3. <span data-ttu-id="fa23b-119">Clonare il repository sul disco rigido</span><span class="sxs-lookup"><span data-stu-id="fa23b-119">Clone the repo to your hard drive</span></span>  
![Git Clone](/media-draft/2-gitclone.png)  
<span data-ttu-id="fa23b-121">In questo esempio il repository è stato clonato in C:\Users\abel\Source\TripleCrown\DevOps</span><span class="sxs-lookup"><span data-stu-id="fa23b-121">In this example the repo was cloned to C:\Users\abel\Source\TripleCrown\DevOps</span></span>

4. <span data-ttu-id="fa23b-122">Eliminare tutti gli elementi dal repository locale, ad eccezione della directory `.git`</span><span class="sxs-lookup"><span data-stu-id="fa23b-122">Delete everything from your local repo except for the `.git` directory</span></span>  
<span data-ttu-id="fa23b-123">![Eliminare tutti gli elementi dal repository](/media-draft/2-deleterepoofeverything.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-123">![Delete Repo of Everything](/media-draft/2-deleterepoofeverything.png)</span></span>

5. <span data-ttu-id="fa23b-124">Copiare il codice sorgente per l'app Node.js scaricata nella cartella del repository</span><span class="sxs-lookup"><span data-stu-id="fa23b-124">Copy the source code for the downloaded node.js app into the repo folder</span></span>  
![Codice sostituito](/media-draft/2-replacedeverything.png)

6. <span data-ttu-id="fa23b-126">Aggiungere tutte le modifiche al repository locale digitando `git add *` dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="fa23b-126">Add all the changes to your local repo by typing `git add *` from the command line</span></span>  
<span data-ttu-id="fa23b-127">![Git Add All](/media-draft/2-gitaddall.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-127">![Git Add All](/media-draft/2-gitaddall.png)</span></span>

7. <span data-ttu-id="fa23b-128">Eseguire il commit delle modifiche nel repository locale digitando `git commit -m "replace sample app with real code"`</span><span class="sxs-lookup"><span data-stu-id="fa23b-128">Commit changes to your local repo by typing `git commit -m "replace sample app with real code"`</span></span>  
<span data-ttu-id="fa23b-129">![Git Commit](/media-draft/2-gitcommit.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-129">![Git Commit](/media-draft/2-gitcommit.png)</span></span>

8. <span data-ttu-id="fa23b-130">Eseguire il push delle modifiche nel repository Git in Visual Studio Team Services con `git push`</span><span class="sxs-lookup"><span data-stu-id="fa23b-130">Push changes back to the git repo in VSTS with `git push`</span></span>  
<span data-ttu-id="fa23b-131">![Git Push](/media-draft/2-gitpush.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-131">![Git Push](/media-draft/2-gitpush.png)</span></span>

9. <span data-ttu-id="fa23b-132">Dopo il push delle modifiche in Visual Studio Team Services, il codice dell'applicazione effettiva dovrebbe essere sottoposto a compilazione</span><span class="sxs-lookup"><span data-stu-id="fa23b-132">After pushing changes back to VSTS, this should send the real app code through the build</span></span>  
<span data-ttu-id="fa23b-133">![Compilazione attivata](/media-draft/2-buildkickedoff.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-133">![Build Kicked Off](/media-draft/2-buildkickedoff.png)</span></span>  
<span data-ttu-id="fa23b-134">![Compilazione in corso](/media-draft/2-buildinaction.png) e pipeline di versione fino ad Azure</span><span class="sxs-lookup"><span data-stu-id="fa23b-134">![Build in Action](/media-draft/2-buildinaction.png) and release pipeline all the way to azure</span></span>  
 <span data-ttu-id="fa23b-135">![Versione in esecuzione](/media-draft/2-releaserunning.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-135">![Release Running](/media-draft/2-releaserunning.png)</span></span>

 <span data-ttu-id="fa23b-136">Al termine della distribuzione è possibile verificare che l'app effettiva sia stata distribuita tornando nel portale di Azure</span><span class="sxs-lookup"><span data-stu-id="fa23b-136">After the deployment finishes, you can verify the real app was deployed by going back to the Azure portal</span></span>

 1. <span data-ttu-id="fa23b-137">Passare al portale di Azure, passare al progetto di Azure DevOps e fare clic sull'app distribuita, disponibile a destra</span><span class="sxs-lookup"><span data-stu-id="fa23b-137">Go to the Azure portal, browse to your Azure DevOps project, and click on your deployed app on the right-hand side</span></span>  
 ![Collegamento per l'avvio dell'app di esempio](/media-draft/2-launchapp.png)

 2. <span data-ttu-id="fa23b-139">L'app in esecuzione verrà avviata nel browser</span><span class="sxs-lookup"><span data-stu-id="fa23b-139">This launches the running app in your browser</span></span>  
 ![App in esecuzione](/media-draft/2-apprunning.png)

## <a name="using-external-git-repo"></a><span data-ttu-id="fa23b-141">Uso di un repository Git esterno</span><span class="sxs-lookup"><span data-stu-id="fa23b-141">Using external git repo</span></span>

<span data-ttu-id="fa23b-142">Un altro modo per sostituire l'app di esempio con il codice effettivo dell'app consiste nello specificare come riferimento per la pipeline di compilazione un repository Git esterno che include il codice dell'app.</span><span class="sxs-lookup"><span data-stu-id="fa23b-142">Another way to swap out the sample app with your real app code is by pointing the build pipeline to an external git repository that holds your app code.</span></span> <span data-ttu-id="fa23b-143">Per questo esempio, caricare il codice effettivo dell'app in un repository di GitHub.</span><span class="sxs-lookup"><span data-stu-id="fa23b-143">For this example, upload the real app code to a github repository.</span></span>

<span data-ttu-id="fa23b-144">Dopo il caricamento del codice effettivo in GitHub, seguire questa procedura per specificare tale repository di GitHub come riferimento per la pipeline di compilazione</span><span class="sxs-lookup"><span data-stu-id="fa23b-144">After uploading the real code to github, do the following to point the build pipeline to this github repository</span></span>

1. <span data-ttu-id="fa23b-145">Dal portale di Azure passare al progetto di Azure DevOps e fare clic sul collegamento per la compilazione</span><span class="sxs-lookup"><span data-stu-id="fa23b-145">From the Azure portal, browse to your Azure DevOps project and click on the build link</span></span>  
![Collegamento alla compilazione](/media-draft/2-buildlink.png)

2. <span data-ttu-id="fa23b-147">Verranno visualizzate le pipeline di compilazione. Fare clic sulla pipeline di compilazione specifica e quindi su `Edit`</span><span class="sxs-lookup"><span data-stu-id="fa23b-147">This takes you to the build pipelines, click your build pipeline and then `Edit`</span></span>  
<span data-ttu-id="fa23b-148">![Fare clic sull'opzione di modifica della compilazione](/media-draft/2-clickeditbuildlink.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-148">![Click Edit Build](/media-draft/2-clickeditbuildlink.png)</span></span>

3. <span data-ttu-id="fa23b-149">Verrà visualizzato l'editor compilazioni. Fare clic su `Get sources`</span><span class="sxs-lookup"><span data-stu-id="fa23b-149">This takes you to the build editor, click on `Get sources`</span></span>  
<span data-ttu-id="fa23b-150">![Fare clic sull'opzione per il recupero dell'origine](/media-draft/2-clickgetsource.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-150">![Click Get Source](/media-draft/2-clickgetsource.png)</span></span>

4. <span data-ttu-id="fa23b-151">Verrà visualizzata la pagina Selezionare un'origine.</span><span class="sxs-lookup"><span data-stu-id="fa23b-151">This takes you to the Select a source page.</span></span> <span data-ttu-id="fa23b-152">Si noti che per la pipeline di compilazione è possibile usare non solo Visual Studio Team Services, ma anche GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud e qualsiasi altro repository esterno basato su Git.</span><span class="sxs-lookup"><span data-stu-id="fa23b-152">Notice how you can not only use VSTS Git, but also GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud, and any external Git based repo for the build pipeline.</span></span> <span data-ttu-id="fa23b-153">Per questo esercizio, selezionare GitHub</span><span class="sxs-lookup"><span data-stu-id="fa23b-153">For this exercise, select GitHub</span></span>  
![Selezionare GitHub](/media-draft/2-selectgithub.png)

5. <span data-ttu-id="fa23b-155">Verrà visualizzata la pagina di connessione a GitHub.</span><span class="sxs-lookup"><span data-stu-id="fa23b-155">This takes you to the GitHub connection page.</span></span> <span data-ttu-id="fa23b-156">Usare OAuth o un token di accesso personale per connettersi con il proprio account GitHub</span><span class="sxs-lookup"><span data-stu-id="fa23b-156">Either use OAuth or a Personal Access Token to connect with your GitHub account</span></span>

6. <span data-ttu-id="fa23b-157">Selezionare il repository di GitHub che include il codice effettivo dell'app e quindi fare clic su `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="fa23b-157">Select the github repo holding the real app code and click `Save & queue`</span></span>  
<span data-ttu-id="fa23b-158">![Salvare e accodare](/media-draft/2-saveandqueue.png)</span><span class="sxs-lookup"><span data-stu-id="fa23b-158">![Save and Queue](/media-draft/2-saveandqueue.png)</span></span>

7. <span data-ttu-id="fa23b-159">Fare clic sul pulsante `Save & queue`</span><span class="sxs-lookup"><span data-stu-id="fa23b-159">Click the `Save & queue` button</span></span>  
![](/media-draft/2-saveandqueuedialog.png)

8. <span data-ttu-id="fa23b-160">Questa azione consente di salvare e attivare la compilazione, inviando il codice effettivo dell'app ospitato in GitHub tramite le pipeline di compilazione e di versione fino ad Azure</span><span class="sxs-lookup"><span data-stu-id="fa23b-160">This action saves and kicks off the build, sending the real app code hosted in GitHub through our build and release pipelines all the way to Azure</span></span>  
![Compilazione in esecuzione](/media-draft/2-buildrunning.png)

## <a name="summary"></a><span data-ttu-id="fa23b-162">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="fa23b-162">Summary</span></span>

<span data-ttu-id="fa23b-163">In questa unità sono stati appresi due modi diversi per sostituire il codice di esempio del progetto DevOps con il codice effettivo dell'app.</span><span class="sxs-lookup"><span data-stu-id="fa23b-163">In this unit, you learned two different ways to replace the sample code in the DevOps project with real app code.</span></span> <span data-ttu-id="fa23b-164">È possibile eseguire questa operazione sostituendo il codice nel repository Git di Visual Studio Team Services oppure collegando la pipeline di compilazione con un altro repository esterno che include il codice dell'app.</span><span class="sxs-lookup"><span data-stu-id="fa23b-164">This can be done either by replacing the code in the VSTS git repo, or by linking the build pipeline with another external repo which holds your app code.</span></span>

<span data-ttu-id="fa23b-165">Verrà quindi illustrato come personalizzare le pipeline di compilazione e di versione.</span><span class="sxs-lookup"><span data-stu-id="fa23b-165">Next learn how to customize the build and release pipelines.</span></span>