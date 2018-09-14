<span data-ttu-id="7ee68-101">Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="7ee68-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="7ee68-102">Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.</span><span class="sxs-lookup"><span data-stu-id="7ee68-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="7ee68-103">Creare una stored procedure nell'app</span><span class="sxs-lookup"><span data-stu-id="7ee68-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="7ee68-104">In questa stored procedure, OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine.</span><span class="sxs-lookup"><span data-stu-id="7ee68-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="7ee68-105">Il totale degli ordini viene calcolato dalla somma degli articoli nell'ordine, meno eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.</span><span class="sxs-lookup"><span data-stu-id="7ee68-105">The order total is calculated from the sum of the items in the order, less any dividend (credit) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="7ee68-106">Copiare il codice seguente e incollarlo alla fine del file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="7ee68-106">Copy the following code and paste it into the end of the Program.cs file.</span></span>

    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->
    ```csharp
    private async Task UpdateOrderTotal(string databaseName, string collectionName, Order orderId)
    {
    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
    }
    ```

2. <span data-ttu-id="7ee68-107">Copiare ora il codice seguente e incollarlo alla fine del metodo **BasicOperations**.</span><span class="sxs-lookup"><span data-stu-id="7ee68-107">Now copy the following code and paste it into the end of the **BasicOperations** method.</span></span>

    ```
    await this.UpdateOrderTotal("Users", "WebCustomers", "5-6235");
    ```

3. <span data-ttu-id="7ee68-108">Salvare il file Program.cs e quindi eseguire il comando seguente nel terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="7ee68-108">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="7ee68-109">La console visualizza l'output seguente.</span><span class="sxs-lookup"><span data-stu-id="7ee68-109">The console displays the following output.</span></span>

    ```
    Order total is 90.85.
    Press any key to continue ...
    ```

## <a name="clean-up"></a><span data-ttu-id="7ee68-110">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="7ee68-110">Clean up</span></span>

<span data-ttu-id="7ee68-111">Se si prevede di continuare a utilizzare i moduli in questo percorso di apprendimento, ignorare il processo di pulizia, altrimenti usare la procedura seguente per eliminare le risorse per evitare di incorrere in addebiti per l'uso del servizio.</span><span class="sxs-lookup"><span data-stu-id="7ee68-111">If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.</span></span>

1. <span data-ttu-id="7ee68-112">Nel portale di Azure selezionare **Gruppi di risorse** all'estrema sinistra, quindi selezionare il gruppo di risorse creato.</span><span class="sxs-lookup"><span data-stu-id="7ee68-112">In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.</span></span>  

    <span data-ttu-id="7ee68-113">Se il menu a sinistra è compresso, fare clic sul</span><span class="sxs-lookup"><span data-stu-id="7ee68-113">If the left menu is collapsed, click</span></span> ![pulsante Espandi](../media/5-javascript-programming/expand.png) <span data-ttu-id="7ee68-115">per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="7ee68-115">to expand it.</span></span>

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources-select.png)

2. <span data-ttu-id="7ee68-117">Nella nuova finestra selezionare il gruppo di risorse e fare clic su **Elimina gruppo di risorse**.</span><span class="sxs-lookup"><span data-stu-id="7ee68-117">In the new window select the resource group, and then click **Delete resource group**.</span></span>

   ![Metriche nel portale di Azure](../media/5-javascript-programming/delete-resources.png)

3. <span data-ttu-id="7ee68-119">Nella nuova finestra digitare il nome del gruppo di risorse da eliminare, quindi fai clic su **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="7ee68-119">In the new window, type the name of the resource group to delete, and then click **Delete**.</span></span>

## <a name="summary"></a><span data-ttu-id="7ee68-120">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7ee68-120">Summary</span></span>

<span data-ttu-id="7ee68-121">In questo modulo è stata creata un'applicazione console .NET Core che crea, aggiorna ed elimina i record utente, esegue query per gli utenti con SQL e LINQ ed esegue una stored procedure per creare un totale degli ordini che tiene conto dei bonus degli utenti e dei coupon.</span><span class="sxs-lookup"><span data-stu-id="7ee68-121">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to create an order total that takes the users dividends and coupons into account.</span></span>