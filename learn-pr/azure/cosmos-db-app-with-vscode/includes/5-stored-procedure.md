<span data-ttu-id="58b8c-101">Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="58b8c-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="58b8c-102">Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.</span><span class="sxs-lookup"><span data-stu-id="58b8c-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="58b8c-103">Creare una stored procedure nell'app</span><span class="sxs-lookup"><span data-stu-id="58b8c-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="58b8c-104">In questa stored procedure OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine.</span><span class="sxs-lookup"><span data-stu-id="58b8c-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="58b8c-105">Il totale degli ordini viene calcolato sommando gli articoli nell'ordine, sottraendo eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.</span><span class="sxs-lookup"><span data-stu-id="58b8c-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="58b8c-106">Nella scheda Azure in Visual Studio Code espandere il **modulo di apprendimento (SQL)** > **Utenti** > **WebCustomers**, fare clic con il pulsante destro del mouse su **Stored procedure**, quindi fare clic su **Crea Stored Procedure**.</span><span class="sxs-lookup"><span data-stu-id="58b8c-106">In Visual Studio Code, in the Azure tab, expand the **learning-module (SQL)** > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="58b8c-107">Nella casella di testo nella parte superiore della schermata digitare *UpdateOrderTotal* e premere Invio per assegnare un nome alla stored procedure.</span><span class="sxs-lookup"><span data-stu-id="58b8c-107">In the text box at the top of the screen, type *UpdateOrderTotal* and click Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="58b8c-108">Espandere **Stored procedure** e fare clic su **UpdateOrderTotal**.</span><span class="sxs-lookup"><span data-stu-id="58b8c-108">Expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

1. <span data-ttu-id="58b8c-109">Per impostazione predefinita, viene offerta una stored procedure che recupera il primo elemento.</span><span class="sxs-lookup"><span data-stu-id="58b8c-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="58b8c-110">Per eseguire questa stored procedure dall'applicazione, aggiungere il codice seguente al file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="58b8c-110">To run this stored procedure from your application, add the following code to the Program.cs file.</span></span>

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "sample"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```
    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->

1. <span data-ttu-id="58b8c-111">Copiare ora il codice seguente e incollarlo alla fine del metodo **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="58b8c-111">Now copy the following code and paste it into the end of the **BasicOperations** method.</span></span>

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="58b8c-112">Eseguire il comando seguente nel terminale integrato per eseguire l'esempio con la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="58b8c-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="58b8c-113">La console visualizza l'output indicando il completamento della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="58b8c-113">The console displays output indicating that the stored procedure was completed.</span></span>

## <a name="clean-up"></a><span data-ttu-id="58b8c-114">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="58b8c-114">Clean up</span></span>

<span data-ttu-id="58b8c-115">Se si prevede di continuare a utilizzare i moduli in questo percorso di apprendimento, ignorare il processo di pulizia, altrimenti usare la procedura seguente per eliminare le risorse per evitare di incorrere in addebiti per l'uso del servizio.</span><span class="sxs-lookup"><span data-stu-id="58b8c-115">If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.</span></span>

1. <span data-ttu-id="58b8c-116">Nel portale di Azure selezionare **Gruppi di risorse** all'estrema sinistra, quindi selezionare il gruppo di risorse creato.</span><span class="sxs-lookup"><span data-stu-id="58b8c-116">In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.</span></span>  

    <span data-ttu-id="58b8c-117">Se il menu a sinistra è compresso, fare clic sul</span><span class="sxs-lookup"><span data-stu-id="58b8c-117">If the left menu is collapsed, click</span></span> ![pulsante Espandi](../media/5-javascript-programming/expand.png) <span data-ttu-id="58b8c-119">per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="58b8c-119">to expand it.</span></span>

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources-select.png)

1. <span data-ttu-id="58b8c-121">Nella nuova finestra selezionare il gruppo di risorse e fare clic su **Elimina gruppo di risorse**.</span><span class="sxs-lookup"><span data-stu-id="58b8c-121">In the new window select the resource group, and then click **Delete resource group**.</span></span>

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources.png)

1. <span data-ttu-id="58b8c-123">Nella nuova finestra digitare il nome del gruppo di risorse da eliminare, quindi fai clic su **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="58b8c-123">In the new window, type the name of the resource group to delete, and then click **Delete**.</span></span>

## <a name="summary"></a><span data-ttu-id="58b8c-124">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="58b8c-124">Summary</span></span>

<span data-ttu-id="58b8c-125">In questo modulo è stata creata un'applicazione console .NET Core che crea, aggiorna ed elimina i record utente, esegue query per gli utenti con SQL e LINQ ed esegue una stored procedure per eseguire query per gli elementi nel database.</span><span class="sxs-lookup"><span data-stu-id="58b8c-125">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to query items in the database.</span></span>