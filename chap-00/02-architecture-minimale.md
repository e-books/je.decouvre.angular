#Architecture minimale d'une application Angular

Une application Angular nécessite au minimum :

- un **module** principal : le code js qui représente votre application
- au moins un **contrôleur** (en js) qui contiendra les traitements
- au moins une **vue** qui est tout simplement du code html contenu dans votre page
- des **directives** qui sont des attributs des tags html et qui servent à faire le lien entre les vues et les contrôleurs

Plus concrètement en code ça donnerait ceci :

##Module principal

Dans `main.js` définissons le module de notre application `booksApp`

```javascript
var booksApp = angular.module("booksApp", []);
```

Nous devons expliquer à Angular que nous "lions" notre page à `booksApp`, donc dans `index.html`, modifier le tag `<body>` de la façon suivante avec la directive `ng-app` :

```html
<body ng-app="booksApp">
```

##Un Contrôleur

Dans `main.js` définissons le Contrôleur `MainCtrl` que l'on rattache à notre application `booksApp`:

```javascript
var MainCtrl = booksApp.controller("MainCtrl", function($scope) {
  
})
```

`$scope` est la variable de contexte qui va contenir des informations accessibles et modifiables à la fois par le contrôleur et la vue.

Modifions notre contrôleur pour qu'il puisse nous fournir une liste de livres:

```javascript
var MainCtrl = booksApp.controller("MainCtrl", function($scope) {
  $scope.books = [
      {title:"Backbone c'est de la balle", description:"tutorial bb", niveau:"très bon"}
    , {title:"React ça dépote", description:"se perfectionner avec React", niveau:"bon"}
    , {title:"J'apprends Angular", description:"from scratch", niveau:"débutant"}
  ];
})
```

##Une Vue

Et modifions notre code html de la façon suivante :

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>livres</title>
</head>
<body ng-app="booksApp">

    <div ng-controller="MainCtrl">
      <h2>Livres</h2>
      <div ng-repeat="book in books">
        <h4>{{book.title}}</h4>
        <p>{{book.description}} - Niveau <b>{{book.niveau}}</b></p>
        <hr>
      </div>
    </div>

  <script src="bower_components/angular/angular.min.js"></script>
  <script src="js/main.js"></script>

</body>
</html>
```

Nous avons donc utilisé 2 directives supplémentaires :

- `<div ng-controller="MainCtrl">` pour lier notre contrôleur au `<div>`
- `<div ng-repeat="book in books">` pour parcourir le contenu de `$scope.books`

Et du templating pour afficher tout ça :

```html
<h4>{{book.title}}</h4>
<p>{{book.description}} - Niveau <b>{{book.niveau}}</b>
```

Vous pouvez dès maintenant afficher votre page avec la liste des livres en ouvrant `index.html` dans votre navigateur préféré.