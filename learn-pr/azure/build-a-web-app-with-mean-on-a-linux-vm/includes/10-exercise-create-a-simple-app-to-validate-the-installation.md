<span data-ttu-id="022e0-101">In questa unità si creerà un'applicazione AngularJS semplice ospitata in Node.js e si userà Express per il routing.</span><span class="sxs-lookup"><span data-stu-id="022e0-101">In this unit, you're going to create a simple AngularJS application hosted in Node.js and use Express for routing.</span></span> <span data-ttu-id="022e0-102">Sul lato back-end, MongoDB fungerà da archivio dati.</span><span class="sxs-lookup"><span data-stu-id="022e0-102">On the back end, MongoDB will serve as your data store.</span></span> <span data-ttu-id="022e0-103">L'applicazione è un database di libri in cui sarà possibile elencare, aggiungere ed eliminare libri.</span><span class="sxs-lookup"><span data-stu-id="022e0-103">The application is a book database, where you will be able to list, add, and delete books.</span></span>

> [!Important]
> <span data-ttu-id="022e0-104">Questa è un'applicazione semplice.</span><span class="sxs-lookup"><span data-stu-id="022e0-104">This is a simple application.</span></span> <span data-ttu-id="022e0-105">La finalità dell'applicazione consiste nel testare lo stack MEAN appena installato.</span><span class="sxs-lookup"><span data-stu-id="022e0-105">Its purpose is to test the newly installed MEAN stack.</span></span> <span data-ttu-id="022e0-106">Questa applicazione non è sufficientemente sicura o pronta per l'uso in un ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="022e0-106">This application is not sufficiently secure or ready for production use.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="022e0-107">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="022e0-107">Create the application</span></span>

<span data-ttu-id="022e0-108">Prima di tutto verranno creati i file di codice, script e HTML per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="022e0-108">First, we're going to create the code, script and HTML files for our application.</span></span> <span data-ttu-id="022e0-109">Queste operazioni verranno eseguite nell'editor Cloud Shell e i file verranno poi copiati nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="022e0-109">We'll do this in the Cloud Shell editor then copy the files to the VM.</span></span>

1. <span data-ttu-id="022e0-110">In Cloud Shell, se si è ancora connessi sulla macchina virtuale con SSH, usare `exit` per tornare al file system di Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="022e0-110">In Cloud Shell, if you are still SSHed into your VM, use `exit` to return to the Cloud Shell filesystem.</span></span>

    ```bash
    exit
    ```

1. <span data-ttu-id="022e0-111">Creare le cartelle e i file per l'applicazione e aprirli nell'editor Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="022e0-111">Create the folders and files for your application and open them in the Cloud Shell editor.</span></span>

    ```bash
    cd ~
    mkdir Books
    mkdir Books/app
    mkdir Books/public
    touch Books/app/model.js
    touch Books/app/routes.js
    touch Books/server.js
    touch Books/public/script.js
    touch Books/public/index.html
    code Books
    ```

    <span data-ttu-id="022e0-112">In questo modo viene creata una cartella denominata "Books" per contenere l'app del progetto e le relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="022e0-112">This creates a folder called "Books" to contain your project's app and its dependencies.</span></span> <span data-ttu-id="022e0-113">All'interno di questa cartella è stata creata una cartella "app" in cui inserire tutte le risorse e gli script dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="022e0-113">Within that folder, you created an "app" folder to contain all your application resources and scripts.</span></span> <span data-ttu-id="022e0-114">Infine si creerà anche una cartella "public" in cui inserire tutti i file del lato client che verranno usati direttamente per soddisfare le richieste HTTP appropriate.</span><span class="sxs-lookup"><span data-stu-id="022e0-114">Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="022e0-115">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="022e0-115">Create the application</span></span>

1. <span data-ttu-id="022e0-116">Creare il modello di dati di Mongoose.</span><span class="sxs-lookup"><span data-stu-id="022e0-116">Create the Mongoose data model.</span></span> <span data-ttu-id="022e0-117">Nell'editor aprire `app/model.js` e incollare il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="022e0-117">In the editor, open `app/model.js` and paste in the following code.</span></span>

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

    > [!IMPORTANT]
    > <span data-ttu-id="022e0-118">Ogni volta che si incolla o si modifica codice in un file nell'editor, assicurarsi di salvare le modifiche tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux o <kbd>Comando+S</kbd> in macOS).</span><span class="sxs-lookup"><span data-stu-id="022e0-118">Whenever you paste or change code into a file in the editor, make sure to save afterwards using the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

    <span data-ttu-id="022e0-119">Questo codice si connette a un database denominato "Books" nel server MongoDB della macchina virtuale locale.</span><span class="sxs-lookup"><span data-stu-id="022e0-119">This code is connecting to a database called "Books" on the local VM's MongoDB server.</span></span> <span data-ttu-id="022e0-120">Crea quindi un documento di database denominato "Book" con lo schema definito dalla variabile `bookSchema`.</span><span class="sxs-lookup"><span data-stu-id="022e0-120">It then creates a database document called "Book" with the schema defined by the `bookSchema` variable.</span></span>

2. <span data-ttu-id="022e0-121">Creare le ExpressRoute che gestiranno le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="022e0-121">Create the Express routes that will handle HTTP requests.</span></span> <span data-ttu-id="022e0-122">Aprire `app/routes.js` nell'editor e incollare il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="022e0-122">Open `app/routes.js` in the editor and paste in the following code.</span></span>

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

    <span data-ttu-id="022e0-123">Questo codice creerà quattro route per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="022e0-123">This code will create four routes for our application.</span></span> <span data-ttu-id="022e0-124">Le prime tre specificano cosa fare quando qualcuno invia una richiesta API GET, POST o DELETE alla risorsa `/book`.</span><span class="sxs-lookup"><span data-stu-id="022e0-124">The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource.</span></span> <span data-ttu-id="022e0-125">L'ultima è una route generale che invia il richiedente alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="022e0-125">The last one is a catch-all route to send the requester to the index page.</span></span>

    <span data-ttu-id="022e0-126">Express può fornire le risposte HTTP direttamente nel codice di gestione delle route oppure può fornire il contenuto statico dei file.</span><span class="sxs-lookup"><span data-stu-id="022e0-126">Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files.</span></span> <span data-ttu-id="022e0-127">Entrambe le operazioni verranno eseguite in questa applicazione Web di esempio.</span><span class="sxs-lookup"><span data-stu-id="022e0-127">We are doing both in this sample web application.</span></span> <span data-ttu-id="022e0-128">Verrà inviata una risposta con dati JSON per le richieste API relative ai libri e con dati HTML direttamente dal file index.html.</span><span class="sxs-lookup"><span data-stu-id="022e0-128">We respond with JSON data for book API requests and with HTML data direct from the index.html file.</span></span>

3. <span data-ttu-id="022e0-129">Creare il server Express per ospitare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="022e0-129">Create the Express server to host the application.</span></span> <span data-ttu-id="022e0-130">Aprire `server.js` nell'editor e inserire il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="022e0-130">Open `server.js` in the editor and paste in the following code:</span></span>

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

    <span data-ttu-id="022e0-131">Questo codice crea l'applicazione Web stessa.</span><span class="sxs-lookup"><span data-stu-id="022e0-131">This code creates the web application itself.</span></span> <span data-ttu-id="022e0-132">Servirà file statici da una cartella denominata **public** (creata successivamente) e userà le route definite nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="022e0-132">It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.</span></span>

4. <span data-ttu-id="022e0-133">Creare l'applicazione JavaScript lato client.</span><span class="sxs-lookup"><span data-stu-id="022e0-133">Create the client-side JavaScript application.</span></span> <span data-ttu-id="022e0-134">Aprire `public/script.js` nell'editor e incollare questo codice:</span><span class="sxs-lookup"><span data-stu-id="022e0-134">Open `public/script.js` in the editor and paste in this code:</span></span>

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

    <span data-ttu-id="022e0-135">Questo codice AngularJS lato client crea una nuova applicazione Angular `myApp` che contiene un controller `myCtrl`.</span><span class="sxs-lookup"><span data-stu-id="022e0-135">This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`.</span></span> <span data-ttu-id="022e0-136">Quando l'applicazione viene eseguita nel browser del visualizzatore, invia una richiesta HTTP GET per recuperare l'elenco dei libri nel database.</span><span class="sxs-lookup"><span data-stu-id="022e0-136">When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.</span></span>

5. <span data-ttu-id="022e0-137">Creare l'interfaccia utente per l'app.</span><span class="sxs-lookup"><span data-stu-id="022e0-137">Create the user interface for the app.</span></span> <span data-ttu-id="022e0-138">Aprire `public/index.html` nell'editor e incollare questo codice:</span><span class="sxs-lookup"><span data-stu-id="022e0-138">Open `public/index.html` in the editor and paste in this code:</span></span>

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

    <span data-ttu-id="022e0-139">Questo codice creerà un modulo HTML semplice con quattro campi in cui inserire nuovi dati sui libri e una tabella in cui visualizzare tutti i libri già archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="022e0-139">This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database.</span></span> <span data-ttu-id="022e0-140">I vari attributi HTML `ng-` collegheranno il codice AngularJS all'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="022e0-140">The various `ng-` HTML attributes will wire up the AngularJS code to the UI.</span></span>

6. <span data-ttu-id="022e0-141">La modifica dei file è stata completata.</span><span class="sxs-lookup"><span data-stu-id="022e0-141">We're done editing files.</span></span> <span data-ttu-id="022e0-142">Assicurarsi di averli salvati tutti, quindi eseguire il comando seguente per copiarli nella macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="022e0-142">Make sure you have saved all of them, then run the following command to copy them to the VM.</span></span> <span data-ttu-id="022e0-143">Immettere la password quando viene richiesto.</span><span class="sxs-lookup"><span data-stu-id="022e0-143">Enter your password when prompted.</span></span>

    ```bash
    scp -r ~/Books <vm-admin-username>@<vm-public-ip>:~/Books
    ```

## <a name="install-node-packages"></a><span data-ttu-id="022e0-144">Installare i pacchetti di Node</span><span class="sxs-lookup"><span data-stu-id="022e0-144">Install Node packages</span></span>

1. <span data-ttu-id="022e0-145">Usare SSH per connettersi nuovamente alla macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="022e0-145">SSH back into your VM.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="022e0-146">Passare alla directory `Books`.</span><span class="sxs-lookup"><span data-stu-id="022e0-146">Change directories to the `Books` directory.</span></span>

    ```bash
    cd ~/Books
    ```

1. <span data-ttu-id="022e0-147">Installare **Express** per gestire il routing delle richieste HTTP, in modo da decidere quale contenuto restituire a un utente dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="022e0-147">Install **Express** to handle routing of your HTTP requests, to decide what content to return to a user of your web application.</span></span>

    <span data-ttu-id="022e0-148">Eseguire il comando seguente per aggiungere Express come pacchetto che possa essere usato dall'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="022e0-148">Run the following command to add Express as a package for your web application to use.</span></span>

    ```bash
    npm install express
    ```

1. <span data-ttu-id="022e0-149">Installare **Mongoose** per facilitare l'inoltro dei dati relativi ai libri tra MongoDB e il routing delle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="022e0-149">Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.</span></span>

    <span data-ttu-id="022e0-150">Le informazioni relative ai libri verranno sottoposte a query tramite richieste all'API REST.</span><span class="sxs-lookup"><span data-stu-id="022e0-150">The book information will be queried via REST API requests.</span></span> <span data-ttu-id="022e0-151">Per semplificare il trasferimento dei dati interni ed esterni a MongoDB all'API verrà usato Mongoose.</span><span class="sxs-lookup"><span data-stu-id="022e0-151">To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose.</span></span> <span data-ttu-id="022e0-152">Mongoose è un sistema basato su schema per la modellazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="022e0-152">Mongoose is a schema-based system for modeling data.</span></span> <span data-ttu-id="022e0-153">Verrà usato nell'applicazione di esempio per garantire la coerenza dei modelli di dati tra le varie richieste HTTP GET, POST e DELETE.</span><span class="sxs-lookup"><span data-stu-id="022e0-153">We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.</span></span>

    <span data-ttu-id="022e0-154">Eseguire il comando seguente per aggiungere Mongoose come pacchetto che possa essere usato dall'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="022e0-154">Run the following command to add Mongoose as a package for your web application to use.</span></span>

      ```bash
      npm install mongoose
      ```

1. <span data-ttu-id="022e0-155">Installare **body-parser** per pre-elaborare i dati delle richieste JSON da usare nel routing tramite Express.</span><span class="sxs-lookup"><span data-stu-id="022e0-155">Install **body-parser** to pre-process JSON request data for use in our Express routing.</span></span>

    <span data-ttu-id="022e0-156">Sul lato back-end, `body-parser` fungerà da middleware tra Node.js ed Express per l'analisi dei dati delle richieste JSON in ingresso.</span><span class="sxs-lookup"><span data-stu-id="022e0-156">On the back end, `body-parser` will serve as a middleware between Node.js and Express for parsing incoming JSON request data.</span></span>

    <span data-ttu-id="022e0-157">Eseguire il comando seguente per aggiungere `body-parser` come pacchetto che possa essere usato dall'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="022e0-157">Run the following command to add `body-parser` as a package for your web application to use.</span></span>

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > <span data-ttu-id="022e0-158">Quando si installano più pacchetti npm, è possibile includerli tutti in un unico comando come questo:</span><span class="sxs-lookup"><span data-stu-id="022e0-158">When installing multiple npm packages, you can include them all in a single command such as this:</span></span>
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

## <a name="test-the-application"></a><span data-ttu-id="022e0-159">Test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="022e0-159">Test the application</span></span>

1. <span data-ttu-id="022e0-160">Avviare l'applicazione con Node.js con il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="022e0-160">Start the application with Node.js with the following command.</span></span>

    ```bash
    sudo node server.js
    ```

    <span data-ttu-id="022e0-161">Verrà avviato il back-end dell'applicazione, che si metterà quindi in ascolto delle richieste HTTP in ingresso sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="022e0-161">This will start the back end of our application, which will then start listening on port 80 for incoming HTTP requests.</span></span>

1. <span data-ttu-id="022e0-162">Testare il funzionamento dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="022e0-162">Test the application functionality.</span></span>

    <span data-ttu-id="022e0-163">Aprire il browser preferito e passare all'indirizzo IP pubblico della macchina virtuale Azure come URL.</span><span class="sxs-lookup"><span data-stu-id="022e0-163">Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL.</span></span>

    ```bash
    http://<vm-public-ip>
    ```

    <span data-ttu-id="022e0-164">Se tutto è in ordine, dovrebbe essere visualizzata una schermata simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="022e0-164">If everything is in order, you should see a screen similar to this:</span></span>

    <span data-ttu-id="022e0-165">Lo screenshot seguente mostra l'interfaccia utente per l'inserimento dei dettagli sui libri da archiviare nel database MongoDB.</span><span class="sxs-lookup"><span data-stu-id="022e0-165">The following screenshot displays the user interface to submit book details for storage in the MongoDB database.</span></span>

    ![Screenshot di un Web browser che mostra il modulo di immissione dati per l'aggiunta di un libro.](../media/10-book-page.png)

    <span data-ttu-id="022e0-167">Ora dovrebbe essere possibile inserire dati sui libri da salvare nel database MongoDB.</span><span class="sxs-lookup"><span data-stu-id="022e0-167">You should now be able to submit books to save to the MongoDB database.</span></span> <span data-ttu-id="022e0-168">Si può vedere anche l'elenco completo dei libri caricati dal database.</span><span class="sxs-lookup"><span data-stu-id="022e0-168">As well, you can see the full list of books loaded from the database.</span></span>