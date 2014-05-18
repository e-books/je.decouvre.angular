#Ajouter un formulaire de saisie pour modifier les livres

##Modifier les livres

Commençons par ajouter 2 propriétés `levels`, `selectedBook` et une méthode `selectBook`

```javascript
var MainCtrl = booksApp.controller("MainCtrl", function($scope) {
  $scope.books = [
      {title:"Backbone c'est de la balle", description:"tutorial bb", level:"très bon"}
    , {title:"React ça dépote", description:"se perfectionner avec React", level:"bon"}
    , {title:"J'apprends Angular", description:"from scratch", level:"débutant"}
  ];

  $scope.levels = [
    "très bon", "bon", "débutant"
  ];

  $scope.selectedBook = null;

  $scope.selectBook = function(book) {
    $scope.selectedBook = book;
  }

})
```

Et dans notre vue ajoutons le formulaire suivant :

```html
<div ng-controller="MainCtrl">
  <!-- mon joli formulaire -->
  <hr>
  <div>
    <input ng-model="selectedBook.title">
    <input ng-model="selectedBook.description">
    <div>
      <select id="inputType"
              ng-model="selectedBook.level"
              ng-options="level for level in levels"></select>
    </div>
  </div>
  <hr>
  <!-- fin de mon joli formulaire -->
```

Et modifions la portion de code précédente en y ajoutant la directive `ng-click="selectBook(book)"`

```html
  <h2>Books</h2>
  <div ng-repeat="book in books" ng-click="selectBook(book)">
    <h4>{{book.title}}</h4>
    <p>{{book.description}} - Niveau <b>{{book.level}}</b>
    <hr>
  </div>
</div>
```

Donc à chaque fois que nous "cliquerons" sur la ligne d'un livre la méthode `selectBook(book)` du contrôleur sera appelée et mettra à jour la propriété `$scope.selectedBook` avec le livre sélectionné.

Et le formulaire de saisie sera mis à jour automatiquement grâce aux directives :

- `ng-model="selectedBook.title"` 
- `ng-model="selectedBook.description"`
- `ng-model="selectedBook.level"`
- `ng-options="level for level in levels"`

Vous pouvez dès maintenant essayer, et vous allez vous apercevoir que lorsque vous sélectionnez un livre, le formulaire se met à jour, et lorsque vous modifiez les données dans le formulaire, la liste se met à jour automatiquement de manière assez magique, je dois bien le reconnaître.

##Ajouter un livre

C'est encore plus simple, ajoutez la méthode `createBook` à votre contrôleur :

```javascript
$scope.createBook = function() {
  $scope.books.push({
    title : "This is a new Book",
    description : "...",
    level: "???"
  });
}
```

Puis ajouter un bouton (`<a href="#" ng-click="createBook()">Ajouter un livre</a>`) dans votre page en dessous de la liste des livres:

```html
<div ng-controller="MainCtrl">
  <hr>
  <div>
    <input ng-model="selectedBook.title">
    <input ng-model="selectedBook.description">
    <div>
      <select id="inputType"
              ng-model="selectedBook.level"
              ng-options="level for level in levels"></select>
    </div>
  </div>
  <hr>
  <h2>Livres</h2>
  <div ng-repeat="book in books" ng-click="selectBook(book)">
    <h4>{{book.title}}</h4>
    <p>{{book.description}} - level <b>{{book.level}}</b>
    <hr>
  </div>
  <!-- le bouton est ici !!! -->
  <hr>
  <a href="#" ng-click="createBook()">Ajouter un livre</a>
  <hr>
</div>
```

Le bouton est un lien (pourquoi pas?) avec une directive `ng-click="createBook()"` qui permettra d'appeler la méthode `createBook()` de notre contrôleur lorsque l'on clique sur le lien et d'ajouter un nouveau livre que vous pourrez ensuite modifier (ok mon workflow n'est pas forcément au top mais ça vous donne l'idée).

Vous pouvez tester :)

C'est tout pour aujourd'hui. Dans le chapitre suivant je vous montre comment faire "plus propre" en utilisant les **services**, **les factories** puis nous attaquerons la partie "ajax" et serveur. 

