#Utilisation d'une factory

Cela va être court, mais nous allons voir comment commencer à ordonner notre code proprement : nous allons ajouter une factory à notre module d'application, cette factory sera une sorte d'helper destiné à nous fournir les données, nous l'appellerons `Models`.

##Factory

Pour créer une factory il suffit d'utiliser la méthode `factory` de notre module angular, de la facon suivante :

```javascript
var booksApp = angular.module("booksApp", []);

/*--- factory ---*/
booksApp.factory("Models", function() {

  return {}
});
```

Nous allons ensuite extraire la partie données de notre contrôleur `MainCtrl` :

```javascript
$scope.books = [
    {title:"Backbone c'est de la balle", description:"tutorial bb", level:"très bon"}
  , {title:"React ça dépote", description:"se perfectionner avec React", level:"bon"}
  , {title:"J'apprends Angular", description:"from scratch", level:"débutant"}
];

$scope.levels = [
  "très bon", "bon", "débutant"
];
```

Pour l'intégrer dans notre factory de la façon suivante :

```javascript
/*--- factory ---*/
booksApp.factory("Models", function() {
  var books = function() {
    return [
      {title:"Backbone c'est de la balle", description:"tutorial bb", level:"très bon"}
      , {title:"React ça dépote", description:"se perfectionner avec React", level:"bon"}
      , {title:"J'apprends Angular", description:"from scratch", level:"débutant"}
    ]
  };
  var levels = function() {
    return [
      "très bon", "bon", "débutant"
    ];
  }

  return {
    books : books, levels : levels
  }
});
```

Donc, notre factory `Models` comporte 2 méthodes `books()` et `levels()` qui vont maintenant fournir les données à notre contrôleur.

##Modification du contrôleur

Modifiez le contrôleur de la manière suivante :

```javascript
var MainCtrl = booksApp.controller("MainCtrl", function($scope, Models) {

  $scope.books = Models.books();
  $scope.levels = Models.levels();

  /*--- partie de code inchangées ---*/
  $scope.selectedBook = null;

  $scope.selectBook = function(book) {
    $scope.selectedBook = book;
  }

  $scope.createBook = function() {
    $scope.books.push({
      title : "This is a new Book",
      description : "...",
      level: "???"
    });
  }
});
```

Nous avons remplacé le code des données précédent par :

```javascript
$scope.books = Models.books();
$scope.levels = Models.levels();
```

**Et surtout, notez bien la modification des paramètres du contrôleur:**

```javascript
var MainCtrl = booksApp.controller("MainCtrl", function($scope, Models) {})
```

Vous pouvez tester à nouveau votre page `index.html` qui fonctionne de la même façon sans avoir été modifiée. Nous avons donc séparé les responsabilités entre la factory `Models` et notre contrôleur.
Nous verrons par la suite comment ajouter un service pour ensuite nous connecter à un serveur.
