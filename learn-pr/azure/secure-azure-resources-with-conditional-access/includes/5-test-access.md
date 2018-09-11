<span data-ttu-id="37d82-101">Negli esercizi precedenti si è acquisita una conoscenza di base di Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37d82-101">In the previous exercises, we gathered a basic understanding of Azure AD.</span></span> <span data-ttu-id="37d82-102">Sono stati creati una directory, un utente e un gruppo e quindi è stata creata una regola di accesso condizionale che richiede Azure Multi-Factor Authentication quando si accede al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="37d82-102">We created a directory, created a user and group, and then created a conditional access rule that requires Azure Multi-Factor Authentication when accessing the Azure portal.</span></span>

<span data-ttu-id="37d82-103">Verrà quindi testato se è possibile accedere alle risorse.</span><span class="sxs-lookup"><span data-stu-id="37d82-103">Now, we'll test if we can access our resources.</span></span>

## <a name="test-access-to-resources"></a><span data-ttu-id="37d82-104">Testare l'accesso alle risorse</span><span class="sxs-lookup"><span data-stu-id="37d82-104">Test access to resources</span></span>

<span data-ttu-id="37d82-105">Gli utenti eseguiranno l'accesso a tutte le applicazioni SaaS tramite il portale delle app personali, quindi è questo che verrà testato.</span><span class="sxs-lookup"><span data-stu-id="37d82-105">You know that your users will sign in and access all their SaaS applications using the MyApps portal, so this is what we'll test.</span></span>

1. <span data-ttu-id="37d82-106">Aprire una finestra del browser InPrivate.</span><span class="sxs-lookup"><span data-stu-id="37d82-106">Open an InPrivate browser window.</span></span>

1. <span data-ttu-id="37d82-107">Passare a https://myapps.microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="37d82-107">Browse to https://myapps.microsoft.com.</span></span>

1. <span data-ttu-id="37d82-108">Accedere usando le credenziali dell'utente creato nell'unità 2.</span><span class="sxs-lookup"><span data-stu-id="37d82-108">Sign in as the user that we created in Unit 2.</span></span>

   * <span data-ttu-id="37d82-109">Si noti che si esegue l'accesso al portale senza l'autenticazione a più fattori.</span><span class="sxs-lookup"><span data-stu-id="37d82-109">Notice that you're signed in to the portal without requiring Multi-Factor Authentication.</span></span>

1. <span data-ttu-id="37d82-110">Nella stessa finestra del browser passare a https://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="37d82-110">In the same browser window, browse to https://portal.azure.com.</span></span>

   * <span data-ttu-id="37d82-111">Si noti che verrà richiesto di fornire altre informazioni per mantenere sicuro l'account.</span><span class="sxs-lookup"><span data-stu-id="37d82-111">Notice that you're now required to provide more information to keep your account secure.</span></span> <span data-ttu-id="37d82-112">Questa interruzione è dovuta all'avvio di Azure Multi-Factor Authentication a causa dei criteri di accesso condizionale creati.</span><span class="sxs-lookup"><span data-stu-id="37d82-112">This interrupt is Azure Multi-Factor Authentication kicking in because of the conditional access policy we created.</span></span> <span data-ttu-id="37d82-113">A questo punto è possibile interrompere e chiudere la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="37d82-113">You can stop at this point and close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="37d82-114">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="37d82-114">Summary</span></span>

<span data-ttu-id="37d82-115">In questa unità si è appreso come concedere a un utente le autorizzazioni di accesso per creare e gestire macchine virtuali in un gruppo di risorse con Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="37d82-115">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell.</span></span> <span data-ttu-id="37d82-116">Nell'unità successiva si imparerà a creare un ruolo personalizzato e a definire le autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="37d82-116">In the next unit, you look at how to create a custom role and define your own permissions.</span></span>