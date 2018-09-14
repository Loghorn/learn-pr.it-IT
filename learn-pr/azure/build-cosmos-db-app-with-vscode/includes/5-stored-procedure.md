<span data-ttu-id="32a53-101">Capita di frequente che più documenti nel database debbano essere aggiornati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="32a53-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="32a53-102">Questa unità illustra come creare, registrare ed eseguire stored procedure dall'applicazione console .NET.</span><span class="sxs-lookup"><span data-stu-id="32a53-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="32a53-103">Creare una stored procedure nell'app</span><span class="sxs-lookup"><span data-stu-id="32a53-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="32a53-104">In questa stored procedure OrderId, che contiene un elenco di tutti gli articoli nell'ordine, viene usato per calcolare il totale di un ordine.</span><span class="sxs-lookup"><span data-stu-id="32a53-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="32a53-105">Il totale degli ordini viene calcolato sommando gli articoli nell'ordine, sottraendo eventuali bonus (crediti) del cliente e tenendo conto di eventuali codici di coupon.</span><span class="sxs-lookup"><span data-stu-id="32a53-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="32a53-106">In Visual Studio Code nel **Azure: Cosmos DB** scheda, quindi espandere l'account Azure Cosmos DB > **Users** > **WebCustomers** e quindi fare doppio clic su  **Le stored procedure** e quindi fare clic su **Create Stored Procedure**.</span><span class="sxs-lookup"><span data-stu-id="32a53-106">In Visual Studio Code, in the **Azure: Cosmos DB** tab, expand your Azure Cosmos DB account > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="32a53-107">Nella casella di testo nella parte superiore della schermata digitare *UpdateOrderTotal* e premere Invio per assegnare un nome alla stored procedure.</span><span class="sxs-lookup"><span data-stu-id="32a53-107">In the text box at the top of the screen, type *UpdateOrderTotal* and click Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="32a53-108">Nel **Azure: Cosmos DB** scheda, quindi espandere **Stored procedure** e fare clic su **UpdateOrderTotal**.</span><span class="sxs-lookup"><span data-stu-id="32a53-108">In the **Azure: Cosmos DB** tab, expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

    <span data-ttu-id="32a53-109">Per impostazione predefinita, viene offerta una stored procedure che recupera il primo elemento.</span><span class="sxs-lookup"><span data-stu-id="32a53-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="32a53-110">Per eseguire questa stored procedure dall'applicazione, aggiungere il codice seguente al file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="32a53-110">To run this stored procedure from your application, add the following code to the Program.cs file.</span></span>

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```

1. <span data-ttu-id="32a53-111">A questo punto copiare il codice seguente e incollarlo prima la `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` riga nel **BasicOperations** (metodo).</span><span class="sxs-lookup"><span data-stu-id="32a53-111">Now copy the following code and paste it before the `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` line in the **BasicOperations** method.</span></span>

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="32a53-112">Eseguire il comando seguente nel terminale integrato per eseguire l'esempio con la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="32a53-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="32a53-113">La console visualizza l'output indicando il completamento della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="32a53-113">The console displays output indicating that the stored procedure was completed.</span></span>

## <a name="summary"></a><span data-ttu-id="32a53-114">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="32a53-114">Summary</span></span>

<span data-ttu-id="32a53-115">In questo modulo è stata creata un'applicazione console .NET Core che crea, aggiorna ed elimina i record utente, esegue query per gli utenti con SQL e LINQ ed esegue una stored procedure per eseguire query per gli elementi nel database.</span><span class="sxs-lookup"><span data-stu-id="32a53-115">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to query items in the database.</span></span>

[!include[](../../../includes/azure-sandbox-cleanup.md)]