<span data-ttu-id="2f456-101">In questo esercizio si creerà un'applicazione AngularJS semplice ospitata in Node.js e si userà Express per il routing.</span><span class="sxs-lookup"><span data-stu-id="2f456-101">In this exercise, you are going to create a simple AngularJS application hosted in Node.js and using Express for routing.</span></span> <span data-ttu-id="2f456-102">Sul lato back-end, MongoDB fungerà da archivio dati.</span><span class="sxs-lookup"><span data-stu-id="2f456-102">On the back-end, MongoDB will serve as your data store.</span></span> <span data-ttu-id="2f456-103">L'applicazione è un database di libri in cui sarà possibile elencare, aggiungere ed eliminare libri.</span><span class="sxs-lookup"><span data-stu-id="2f456-103">The application is a book database, where you will be able to list, add, and delete books.</span></span>

> [!Important]
> <span data-ttu-id="2f456-104">Si tratta di un'applicazione molto semplice il cui unico scopo è testare lo stack MEAN appena installato.</span><span class="sxs-lookup"><span data-stu-id="2f456-104">This application is a very simple application whose sole purpose is to test the newly installed MEAN stack.</span></span> <span data-ttu-id="2f456-105">Questa applicazione non è sufficientemente sicura o pronta per l'uso in un ambiente di produzione, pertanto si raccomanda di usarla nel modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="2f456-105">This application is not sufficiently secure or ready for production use, so please use it accordingly.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="2f456-106">Connettersi alla macchina virtuale</span><span class="sxs-lookup"><span data-stu-id="2f456-106">Connect to the VM</span></span>

<span data-ttu-id="2f456-107">Se non ci si è ancora connessi alla macchina virtuale, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="2f456-107">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="2f456-108">Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della macchina virtuale precedenti.</span><span class="sxs-lookup"><span data-stu-id="2f456-108">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="create-the-back-end"></a><span data-ttu-id="2f456-109">Creare il back-end</span><span class="sxs-lookup"><span data-stu-id="2f456-109">Create the back-end</span></span>

1. <span data-ttu-id="2f456-110">Creare una struttura di cartelle per la nuova applicazione di esempio usando il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="2f456-110">Create a folder structure for your new sample application with the following command.</span></span>

    ```bash
    mkdir ~/Books
    mkdir ~/Books/app
    mkdir ~/Books/public
    ```

    <span data-ttu-id="2f456-111">Nella home directory dell'utente amministratore è stata creata una cartella denominata "Books" in cui inserire l'app del progetto e le sue dipendenze.</span><span class="sxs-lookup"><span data-stu-id="2f456-111">In your admin user's home location, you created a folder called "Books" to contain your project's app and its dependencies.</span></span> <span data-ttu-id="2f456-112">All'interno di questa cartella è stata creata una cartella "app" in cui inserire tutte le risorse e gli script dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f456-112">Within that folder, you created an "app" folder to contain all your application resources and scripts.</span></span> <span data-ttu-id="2f456-113">Infine si creerà anche una cartella "public" in cui inserire tutti i file del lato client che verranno usati direttamente per soddisfare le richieste HTTP appropriate.</span><span class="sxs-lookup"><span data-stu-id="2f456-113">Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.</span></span>

1. <span data-ttu-id="2f456-114">Installare **Express** per gestire il routing delle richieste HTTP, decidendo quale contenuto restituire a un utente dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2f456-114">Install **Express** to handle routing of your HTTP requests, deciding what content to return to a user of your web application.</span></span>

    <span data-ttu-id="2f456-115">Eseguire il comando seguente per aggiungere Express come pacchetto che possa essere usato dall'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2f456-115">Run the following command to add Express as a package for your web application to use.</span></span>

        ```bash
        npm install express
        ```

1. <span data-ttu-id="2f456-116">Installare **Mongoose** per facilitare l'inoltro dei dati relativi ai libri tra MongoDB e il routing delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f456-116">Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.</span></span>

    <span data-ttu-id="2f456-117">Le informazioni relative ai libri verranno sottoposte a query tramite richieste all'API REST.</span><span class="sxs-lookup"><span data-stu-id="2f456-117">The book information will be queried via REST API requests.</span></span> <span data-ttu-id="2f456-118">Per semplificare il trasferimento dei dati interni ed esterni a MongoDB all'API verrà usato Mongoose.</span><span class="sxs-lookup"><span data-stu-id="2f456-118">To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose.</span></span> <span data-ttu-id="2f456-119">Mongoose è un sistema basato su schema per la modellazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="2f456-119">Mongoose is a schema-based system for modeling data.</span></span> <span data-ttu-id="2f456-120">Verrà usato nell'applicazione di esempio per garantire la coerenza dei modelli di dati tra le varie richieste HTTP GET, POST e DELETE.</span><span class="sxs-lookup"><span data-stu-id="2f456-120">We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.</span></span>

    <span data-ttu-id="2f456-121">Eseguire il comando seguente per aggiungere Mongoose come pacchetto che possa essere usato dall'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2f456-121">Run the following command to add Mongoose as a package for your web application to use.</span></span>

        ```bash
        npm install mongoose
        ```

1. <span data-ttu-id="2f456-122">Installare **body-parser** per pre-elaborare i dati delle richieste JSON da usare nel routing tramite Express.</span><span class="sxs-lookup"><span data-stu-id="2f456-122">Install **body-parser** to pre-process JSON request data for use in our Express routing.</span></span>

    <span data-ttu-id="2f456-123">Sul lato back-end, body-parser fungerà da middleware tra Node.js ed Express per l'analisi dei dati delle richieste JSON in ingresso.</span><span class="sxs-lookup"><span data-stu-id="2f456-123">On the back-end, body-parser will serve as a middleware between Node.js and Express for parsing incoming JSON request data.</span></span>

    <span data-ttu-id="2f456-124">Eseguire il comando seguente per aggiungere body-parser come pacchetto che possa essere usato dall'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2f456-124">Run the following command to add body-parser as a package for your web application to use.</span></span>

        ```bash
        npm install body-parser
        ```

    > [!Note]
    > <span data-ttu-id="2f456-125">Quando si installano più pacchetti npm, è possibile includerli tutti in un unico comando come questo.</span><span class="sxs-lookup"><span data-stu-id="2f456-125">When installing multiple npm packages, you can also include them all in a single command such as this.</span></span>
    > ```bash
    > npm install express mongoose body-parser
    > ```

1. <span data-ttu-id="2f456-126">Creare il back-end del modello di dati per l'applicazione Web Books usando Mongoose.</span><span class="sxs-lookup"><span data-stu-id="2f456-126">Create the data model back-end for your books web application using Mongoose.</span></span>

    1. <span data-ttu-id="2f456-127">All'interno della cartella **app** nella cartella **Books** dell'applicazione creare un nuovo file JavaScript denominato **model.js** che contenga il modello di dati basato su Mongoose dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f456-127">Within the **app** folder within your **Books** application folder, create a new JavaScript file called **model.js** to contain your book's Mongoose-based data model.</span></span>

        ```bash
        nano ~/Books/app/model.js
        ```

    1. <span data-ttu-id="2f456-128">Incollare il codice seguente in questo nuovo file per creare lo schema dell'applicazione Books usando Mongoose.</span><span class="sxs-lookup"><span data-stu-id="2f456-128">Paste the following code into this new file to create our book schema using Mongoose.</span></span>

        ```javascript
        var mongoose = require('mongoose');
        var dbHost = 'mongodb://localhost:27017/Books';
        mongoose.connect(dbHost,  { useNewUrlParser: true } );
        mongoose.connection;
        mongoose.set('debug', true);
        var bookSchema = mongoose.Schema( {
            name: String,
            isbn: {type: String, index: true},
            author: String,
            pages: Number
        });
        var Book = mongoose.model('Book', bookSchema);
        module.exports = Book // mongoose.model('Book', bookSchema);
        ```

        > [!Note]
        > <span data-ttu-id="2f456-129">Per salvare il file corrente in **nano** è necessario premere **CTRL**+**O** e per uscire da **nano** occorre premere **CTRL**+**X**</span><span class="sxs-lookup"><span data-stu-id="2f456-129">To save the current file in **nano** you need to press **Ctrl**+**O**, and to exit **nano** you need to press **Ctrl**+**X**</span></span>

        <span data-ttu-id="2f456-130">Questo codice si connette a un database denominato `Books` nel server MongoDB della macchina virtuale locale.</span><span class="sxs-lookup"><span data-stu-id="2f456-130">This code is connecting to a database called `Books` on the local VM's MongoDB server.</span></span> <span data-ttu-id="2f456-131">Crea quindi un documento di database denominato `Book` con lo schema definito dalla variabile `bookSchema`.</span><span class="sxs-lookup"><span data-stu-id="2f456-131">It then creates a database document called `Book` with the schema defined by the `bookSchema` variable.</span></span>

1. <span data-ttu-id="2f456-132">Creare le route Express per consentire all'applicazione di gestire le varie richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f456-132">Create the Express routes for the application to handle the various HTTP request.</span></span>

    1. <span data-ttu-id="2f456-133">Nella cartella **app** creare un nuovo file JavaScript denominato **routes.js**.</span><span class="sxs-lookup"><span data-stu-id="2f456-133">Within the **app** folder, create a new JavaScript file called **routes.js**.</span></span>

        ```bash
        nano ~/Books/app/routes.js
        ```

    1. <span data-ttu-id="2f456-134">Incollare il codice seguente in questo nuovo file per stabilire le route mediante Express.</span><span class="sxs-lookup"><span data-stu-id="2f456-134">Paste the following code into this new file to establish the routes using Express.</span></span>

        ```javascript
        var path = require('path');
        var Book = require('./model');
        var routes = function(app) {
            app.get('/book', function(req, res) {
                Book.find({}, function(err, result) {
                    if ( err ) throw err;
                    res.json(result);
                });
            });
            app.post('/book', function(req, res) {
                var book = new Book( {
                    name:req.body.name,
                    isbn:req.body.isbn,
                    author:req.body.author,
                    pages:req.body.pages
                });
                book.save(function(err, result) {
                    if ( err ) throw err;
                    res.json( {
                        message:"Successfully added book",
                        book:result
                    });
                });
            });
            app.delete("/book/:isbn", function(req, res) {
                Book.findOneAndRemove(req.query, function(err, result) {
                    if ( err ) throw err;
                    res.json( {
                        message: "Successfully deleted the book",
                        book: result
                    });
                });
            });
            app.get('*', function(req, res) {
                res.sendFile(path.join(__dirname + '/public', 'index.html'));
            });
        };
        module.exports = routes;
        ```

        <span data-ttu-id="2f456-135">Questo codice creerà quattro route per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f456-135">This code will create four routes for our application.</span></span> <span data-ttu-id="2f456-136">Le prime tre specificano cosa fare quando qualcuno invia una richiesta API GET, POST o DELETE alla risorsa `/book`.</span><span class="sxs-lookup"><span data-stu-id="2f456-136">The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource.</span></span> <span data-ttu-id="2f456-137">L'ultima è una route generale che invia il richiedente alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="2f456-137">The last one is a catch all route to send the requester to the index page.</span></span>

        <span data-ttu-id="2f456-138">Express può fornire le risposte HTTP direttamente nel codice di gestione delle route oppure può fornire il contenuto statico dei file.</span><span class="sxs-lookup"><span data-stu-id="2f456-138">Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files.</span></span> <span data-ttu-id="2f456-139">In questa applicazione Web di esempio vengono eseguite entrambe le operazioni, ossia rispondere con dati JSON alle richieste API di libri e rispondere con dati HTML direttamente dal file index.html.</span><span class="sxs-lookup"><span data-stu-id="2f456-139">We are doing both in this sample web application, responding with JSON data for book API requests and responding with HTML data direct from the index.html file.</span></span>

1. <span data-ttu-id="2f456-140">Creare un file **server.js** nella cartella **Books** per configurare l'hosting di Node.js (tramite le route Express).</span><span class="sxs-lookup"><span data-stu-id="2f456-140">Create a **server.js** file in the **Books** folder to configure the Node.js hosting (using the Express routes).</span></span>

    1. <span data-ttu-id="2f456-141">Tornare nella cartella radice dell'applicazione **Books** e creare un nuovo file JavaScript denominato **server.js**.</span><span class="sxs-lookup"><span data-stu-id="2f456-141">Back in the application root **Books** folder, create a new JavaScript file called **server.js**.</span></span>

        ```bash
        nano ~/Books/server.js
        ```

    1. <span data-ttu-id="2f456-142">Incollare il codice seguente in questo nuovo file per configurare l'applicazione Web e mettersi in ascolto sulla porta HTTP predefinita.</span><span class="sxs-lookup"><span data-stu-id="2f456-142">Paste the following code into this new file to configure your web application and start listening to the default HTTP port.</span></span>

        ```javascript
        var express = require('express');
        var bodyParser = require('body-parser');
        var app = express();
        app.use(express.static(__dirname + '/public'));
        app.use(bodyParser.json());
        require('./app/routes')(app);
        app.set('port', 80);
        app.listen(app.get('port'), function() {
            console.log('Server up: http://localhost:' + app.get('port'));
        });
        ```

        <span data-ttu-id="2f456-143">Questo codice crea l'applicazione Web stessa.</span><span class="sxs-lookup"><span data-stu-id="2f456-143">This code creates the web application itself.</span></span> <span data-ttu-id="2f456-144">Servirà file statici da una cartella denominata **public** (creata successivamente) e userà le route definite nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="2f456-144">It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.</span></span>

1. <span data-ttu-id="2f456-145">Creare l'HTML front-end e l'applicazione JavaScript lato client.</span><span class="sxs-lookup"><span data-stu-id="2f456-145">Create the front-end HTML and client-side JavaScript application.</span></span>

    1. <span data-ttu-id="2f456-146">Nella cartella dati **public** creare un nuovo file JavaScript denominato **script.js**.</span><span class="sxs-lookup"><span data-stu-id="2f456-146">Within the **public** content folder, create a new JavaScript file called **script.js**.</span></span>

        ```bash
        nano ~/Books/public/script.js
        ```

    1. <span data-ttu-id="2f456-147">Incollare il codice seguente in questo nuovo file per configurare l'applicazione Web lato client in modo che gestisca la comunicazione con il server Web.</span><span class="sxs-lookup"><span data-stu-id="2f456-147">Paste the following code into this new file to configure your client-side web application to handle communicating with your web server.</span></span>

        ```javascript
        var app = angular.module('myApp', []);
        app.controller('myCtrl', function($scope, $http) {
            var getData = function() {
                return $http( {
                    method: 'GET',
                    url: '/book'
                }).then(function successCallback(response) {
                    $scope.books = response.data;
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
            getData();
            $scope.del_book = function(book) {
                $http( {
                    method: 'DELETE',
                    url: '/book/:isbn',
                    params: {'isbn': book.isbn}
                }).then(function successCallback(response) {
                    console.log(response);
                    return getData();
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
            $scope.add_book = function() {
                var body = '{ "name": "' + $scope.Name +
                '", "isbn": "' + $scope.Isbn +
                '", "author": "' + $scope.Author +
                '", "pages": "' + $scope.Pages + '" }';
                $http({
                    method: 'POST',
                    url: '/book',
                    data: body
                }).then(function successCallback(response) {
                    console.log(response);
                    return getData();
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
        });
        ```

        <span data-ttu-id="2f456-148">Questo codice AngularJS lato client crea una nuova applicazione Angular `myApp` che contiene un controller `myCtrl`.</span><span class="sxs-lookup"><span data-stu-id="2f456-148">This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`.</span></span> <span data-ttu-id="2f456-149">Quando l'applicazione viene eseguita nel browser del visualizzatore, invia una richiesta HTTP GET per recuperare l'elenco dei libri nel database.</span><span class="sxs-lookup"><span data-stu-id="2f456-149">When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.</span></span>

    1. <span data-ttu-id="2f456-150">Sempre nella cartella dati **public**, creare un nuovo file HTML denominato **index.html**.</span><span class="sxs-lookup"><span data-stu-id="2f456-150">Also within the **public** content folder, create a new HTML file called **index.html**.</span></span>

        ```bash
        nano ~/Books/public/index.html
        ```

    1. <span data-ttu-id="2f456-151">Incollare il markup seguente in questo nuovo file per configurare l'interfaccia utente HTML dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="2f456-151">Paste the following mark-up into this new file to set up your web application's HTML user interface.</span></span>

        ```html
        <!doctype html>
        <html ng-app="myApp" ng-controller="myCtrl">
        <head>
            <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
            <script src="script.js"></script>
        </head>
        <body>
            <div>
            <table>
                <tr>
                <td>Name:</td>
                <td><input type="text" ng-model="Name"></td>
                </tr>
                <tr>
                <td>Isbn:</td>
                <td><input type="text" ng-model="Isbn"></td>
                </tr>
                <tr>
                <td>Author:</td>
                <td><input type="text" ng-model="Author"></td>
                </tr>
                <tr>
                <td>Pages:</td>
                <td><input type="number" ng-model="Pages"></td>
                </tr>
            </table>
            <button ng-click="add_book()">Add</button>
            </div>
            <hr>
            <div>
            <table>
                <tr>
                <th>Name</th>
                <th>Isbn</th>
                <th>Author</th>
                <th>Pages</th>
                </tr>
                <tr ng-repeat="book in books">
                <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
                <td>{{book.name}}</td>
                <td>{{book.isbn}}</td>
                <td>{{book.author}}</td>
                <td>{{book.pages}}</td>
                </tr>
            </table>
            </div>
        </body>
        </html>
        ```

        <span data-ttu-id="2f456-152">Questo codice creerà un modulo HTML semplice con quattro campi in cui inserire nuovi dati sui libri e una tabella in cui visualizzare tutti i libri già archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="2f456-152">This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database.</span></span> <span data-ttu-id="2f456-153">I vari attributi HTML `ng-` collegheranno il codice AngularJS all'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2f456-153">The various `ng-` HTML attributes will wire up the AngularJS code to the UI.</span></span>

1. <span data-ttu-id="2f456-154">Testare l'applicazione Web Books completa.</span><span class="sxs-lookup"><span data-stu-id="2f456-154">Test the full books web application.</span></span>

    1. <span data-ttu-id="2f456-155">Avviare l'applicazione con Node.js con il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="2f456-155">Start the application with Node.js with the following command.</span></span>

        ```bash
        sudo node ~/Books/server.js
        ```

        <span data-ttu-id="2f456-156">Verrà avviato il back-end dell'applicazione, che si metterà quindi in ascolto delle richieste HTTP in ingresso sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="2f456-156">This will start the back-end of our application, which will then start listening on port 80 for incoming HTTP requests.</span></span>

    1. <span data-ttu-id="2f456-157">Testare il funzionamento dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2f456-157">Test the application functionality.</span></span>

        <span data-ttu-id="2f456-158">Aprire il browser preferito e passare all'indirizzo IP pubblico della macchina virtuale Azure come URL.</span><span class="sxs-lookup"><span data-stu-id="2f456-158">Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL.</span></span>

        ```bash
        http://<vm-public-ip>
        ```

        <span data-ttu-id="2f456-159">Se tutto è in ordine, dovrebbe essere visualizzata una schermata simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="2f456-159">If everything is in order, you should see a screen similar to this:</span></span>

        <span data-ttu-id="2f456-160">Lo screenshot seguente mostra l'interfaccia utente per l'inserimento dei dettagli sui libri da archiviare nel database MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2f456-160">The following screenshot displays the user interface to submit book details for storage in the MongoDB database.</span></span>


        ![Screenshot di un Web browser che mostra il modulo di immissione dati per l'aggiunta di un libro.](../media-draft/10-book-page.png)

    <span data-ttu-id="2f456-162">Ora dovrebbe essere possibile inserire dati sui libri da salvare nel database MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2f456-162">You should now be able to submit books to save to the MongoDB database.</span></span> <span data-ttu-id="2f456-163">Si può vedere anche l'elenco completo dei libri caricati dal database.</span><span class="sxs-lookup"><span data-stu-id="2f456-163">As well, you can see the full list of books loaded from the database.</span></span>