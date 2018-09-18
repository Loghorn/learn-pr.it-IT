In questa unità si creerà un'applicazione AngularJS semplice ospitata in Node.js e si userà Express per il routing. Sul lato back-end, MongoDB fungerà da archivio dati. L'applicazione è un database di libri in cui sarà possibile elencare, aggiungere ed eliminare libri.

> [!Important]
> Questa è un'applicazione semplice. La finalità dell'applicazione consiste nel testare lo stack MEAN appena installato. Questa applicazione non è sufficientemente sicura o pronta per l'uso in un ambiente di produzione.

## <a name="connect-to-the-vm"></a>Connettersi alla macchina virtuale

Se non ci si è ancora connessi alla macchina virtuale, eseguire il comando seguente. Sostituire i segnaposto `<vm-admin-username>` e `<vm-public-ip>` con il nome utente amministratore e l'indirizzo IP pubblico della VM precedenti.

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="create-the-back-end"></a>Creare il back-end

1. Creare una struttura di cartelle per la nuova applicazione di esempio usando il comando seguente.

    ```bash
    mkdir ~/Books
    mkdir ~/Books/app
    mkdir ~/Books/public
    ```

    Nella home directory dell'utente amministratore è stata creata una cartella denominata "Books" in cui inserire l'app del progetto e le sue dipendenze. All'interno di questa cartella è stata creata una cartella "app" in cui inserire tutte le risorse e gli script dell'applicazione. Infine si creerà anche una cartella "public" in cui inserire tutti i file del lato client che verranno usati direttamente per soddisfare le richieste HTTP appropriate.

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

1. Creare il back-end del modello di dati per l'applicazione Web Books usando Mongoose.

    1. Nella cartella **app** entro la cartella **Books** dell'applicazione creare un nuovo file JavaScript denominato **model.js** che contenga il modello di dati basato su Mongoose dell'applicazione.

        ```bash
        nano ~/Books/app/model.js
        ```

    1. Incollare il codice seguente in questo nuovo file per creare lo schema dell'applicazione Books usando Mongoose.

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

        > [!TIP]
        > Per salvare il file corrente in **nano**, è necessario premere **CTRL**+**O**. Per uscire da **nano**, è necessario premere **CTRL**+**X**.

        Questo codice si connette a un database denominato "Books" nel server MongoDB della macchina virtuale locale. Crea quindi un documento di database denominato "Book" con lo schema definito dalla variabile `bookSchema`.

1. Creare le route Express per consentire all'applicazione di gestire le varie richieste HTTP.

    1. Nella cartella **app** creare un nuovo file JavaScript denominato **routes.js**.

        ```bash
        nano ~/Books/app/routes.js
        ```

    1. Incollare il codice seguente in questo nuovo file per stabilire le route mediante Express.

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

1. Creare un file **server.js** nella cartella **Books** per configurare l'hosting di Node.js (tramite le route Express).

    1. Tornare nella cartella radice dell'applicazione **Books** e creare un nuovo file JavaScript denominato **server.js**.

        ```bash
        nano ~/Books/server.js
        ```

    1. Incollare il codice seguente in questo nuovo file per configurare l'applicazione Web e mettersi in ascolto sulla porta HTTP predefinita.

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

1. Creare l'HTML front-end e l'applicazione JavaScript lato client.

    1. Nella cartella dati **public** creare un nuovo file JavaScript denominato **script.js**.

        ```bash
        nano ~/Books/public/script.js
        ```

    1. Incollare il codice seguente in questo nuovo file per configurare l'applicazione Web lato client in modo che gestisca la comunicazione con il server Web.

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

    1. Sempre nella cartella dati **public**, creare un nuovo file HTML denominato **index.html**.

        ```bash
        nano ~/Books/public/index.html
        ```

    1. Incollare il markup seguente in questo nuovo file per configurare l'interfaccia utente HTML dell'applicazione Web.

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

1. Testare l'applicazione Web Books completa.

    1. Avviare l'applicazione con Node.js con il comando seguente.

        ```bash
        sudo node ~/Books/server.js
        ```

        Verrà avviato il back-end dell'applicazione, che si metterà quindi in ascolto delle richieste HTTP in ingresso sulla porta 80.

    1. Testare il funzionamento dell'applicazione.

        Aprire il browser preferito e passare all'indirizzo IP pubblico della macchina virtuale Azure come URL.

        ```bash
        http://<vm-public-ip>
        ```

        Se tutto è in ordine, dovrebbe essere visualizzata una schermata simile alla seguente:

        Lo screenshot seguente mostra l'interfaccia utente per l'inserimento dei dettagli sui libri da archiviare nel database MongoDB.


        ![Screenshot di un Web browser che mostra il modulo di immissione dati per l'aggiunta di un libro.](../media-draft/10-book-page.png)

    Ora dovrebbe essere possibile inserire dati sui libri da salvare nel database MongoDB. Si può vedere anche l'elenco completo dei libri caricati dal database.