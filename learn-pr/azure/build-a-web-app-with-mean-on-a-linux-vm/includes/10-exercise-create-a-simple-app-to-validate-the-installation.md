In questa unità si creerà un'applicazione AngularJS semplice ospitata in Node.js e si userà Express per il routing. Sul lato back-end, MongoDB fungerà da archivio dati. L'applicazione è un database di libri in cui sarà possibile elencare, aggiungere ed eliminare libri.

> [!Important]
> Questa è un'applicazione semplice. La finalità dell'applicazione consiste nel testare lo stack MEAN appena installato. Questa applicazione non è sufficientemente sicura o pronta per l'uso in un ambiente di produzione.

## <a name="create-the-application"></a>Creare l'applicazione

Prima di tutto verranno creati i file di codice, script e HTML per l'applicazione. Queste operazioni verranno eseguite nell'editor Cloud Shell e i file verranno poi copiati nella macchina virtuale.

1. In Cloud Shell, se si è ancora connessi sulla macchina virtuale con SSH, usare `exit` per tornare al file system di Cloud Shell.

    ```bash
    exit
    ```

1. Creare le cartelle e i file per l'applicazione e aprirli nell'editor Cloud Shell.

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

    In questo modo viene creata una cartella denominata "Books" per contenere l'app del progetto e le relative dipendenze. All'interno di questa cartella è stata creata una cartella "app" in cui inserire tutte le risorse e gli script dell'applicazione. Infine si creerà anche una cartella "public" in cui inserire tutti i file del lato client che verranno usati direttamente per soddisfare le richieste HTTP appropriate.

## <a name="create-the-application"></a>Creare l'applicazione

1. Creare il modello di dati di Mongoose. Nell'editor aprire `app/model.js` e incollare il codice seguente.

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
    > Ogni volta che si incolla o si modifica codice in un file nell'editor, assicurarsi di salvare le modifiche tramite il menu "..." o il tasto di scelta rapida (<kbd>CTRL+S</kbd> in Windows e Linux o <kbd>Comando+S</kbd> in macOS).

    Questo codice si connette a un database denominato "Books" nel server MongoDB della macchina virtuale locale. Crea quindi un documento di database denominato "Book" con lo schema definito dalla variabile `bookSchema`.

2. Creare le ExpressRoute che gestiranno le richieste HTTP. Aprire `app/routes.js` nell'editor e incollare il codice seguente.

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

    Questo codice creerà quattro route per l'applicazione. Le prime tre specificano cosa fare quando qualcuno invia una richiesta API GET, POST o DELETE alla risorsa `/book`. L'ultima è una route generale che invia il richiedente alla pagina di indice.

    Express può fornire le risposte HTTP direttamente nel codice di gestione delle route oppure può fornire il contenuto statico dei file. Entrambe le operazioni verranno eseguite in questa applicazione Web di esempio. Verrà inviata una risposta con dati JSON per le richieste API relative ai libri e con dati HTML direttamente dal file index.html.

3. Creare il server Express per ospitare l'applicazione. Aprire `server.js` nell'editor e inserire il codice seguente:

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

    Questo codice crea l'applicazione Web stessa. Servirà file statici da una cartella denominata **public** (creata successivamente) e userà le route definite nel passaggio precedente.

4. Creare l'applicazione JavaScript lato client. Aprire `public/script.js` nell'editor e incollare questo codice:

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

    Questo codice AngularJS lato client crea una nuova applicazione Angular `myApp` che contiene un controller `myCtrl`. Quando l'applicazione viene eseguita nel browser del visualizzatore, invia una richiesta HTTP GET per recuperare l'elenco dei libri nel database.

5. Creare l'interfaccia utente per l'app. Aprire `public/index.html` nell'editor e incollare questo codice:

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

    Questo codice creerà un modulo HTML semplice con quattro campi in cui inserire nuovi dati sui libri e una tabella in cui visualizzare tutti i libri già archiviati nel database. I vari attributi HTML `ng-` collegheranno il codice AngularJS all'interfaccia utente.

6. La modifica dei file è stata completata. Assicurarsi di averli salvati tutti, quindi eseguire il comando seguente per copiarli nella macchina virtuale. Immettere la password quando viene richiesto.

    ```bash
    scp -r ~/Books <vm-admin-username>@<vm-public-ip>:~/Books
    ```

## <a name="install-node-packages"></a>Installare i pacchetti di Node

1. Usare SSH per connettersi nuovamente alla macchina virtuale.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. Passare alla directory `Books`.

    ```bash
    cd ~/Books
    ```

1. Installare **Express** per gestire il routing delle richieste HTTP, in modo da decidere quale contenuto restituire a un utente dell'applicazione Web.

    Eseguire il comando seguente per aggiungere Express come pacchetto che possa essere usato dall'applicazione Web.

    ```bash
    npm install express
    ```

1. Installare **Mongoose** per facilitare l'inoltro dei dati relativi ai libri tra MongoDB e il routing delle richieste HTTP.

    Le informazioni relative ai libri verranno sottoposte a query tramite richieste all'API REST. Per semplificare il trasferimento dei dati interni ed esterni a MongoDB all'API verrà usato Mongoose. Mongoose è un sistema basato su schema per la modellazione dei dati. Verrà usato nell'applicazione di esempio per garantire la coerenza dei modelli di dati tra le varie richieste HTTP GET, POST e DELETE.

    Eseguire il comando seguente per aggiungere Mongoose come pacchetto che possa essere usato dall'applicazione Web.

      ```bash
      npm install mongoose
      ```

1. Installare **body-parser** per pre-elaborare i dati delle richieste JSON da usare nel routing tramite Express.

    Sul lato back-end, `body-parser` fungerà da middleware tra Node.js ed Express per l'analisi dei dati delle richieste JSON in ingresso.

    Eseguire il comando seguente per aggiungere `body-parser` come pacchetto che possa essere usato dall'applicazione Web.

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > Quando si installano più pacchetti npm, è possibile includerli tutti in un unico comando come questo:
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

## <a name="test-the-application"></a>Test dell'applicazione

1. Avviare l'applicazione con Node.js con il comando seguente.

    ```bash
    sudo node server.js
    ```

    Verrà avviato il back-end dell'applicazione, che si metterà quindi in ascolto delle richieste HTTP in ingresso sulla porta 80.

1. Testare il funzionamento dell'applicazione.

    Aprire il browser preferito e passare all'indirizzo IP pubblico della macchina virtuale di Azure come URL ([http://X.X.X.X](http://X.X.X.X)).

    Se tutto è in ordine, deve apparire una schermata simile alla seguente:

    Lo screenshot seguente mostra l'interfaccia utente per l'inserimento dei dettagli sui libri da archiviare nel database MongoDB.

    ![Screenshot di un Web browser che mostra il modulo di immissione dati per l'aggiunta di un libro.](../media/10-book-page.png)

    Ora dovrebbe essere possibile inserire dati sui libri da salvare nel database MongoDB. Si può vedere anche l'elenco completo dei libri caricati dal database.