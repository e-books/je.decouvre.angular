#Utilisation d'un routeur

J'aimerais pouvoir afficher au choix, dans ma webapp, les actualités ou les "coups de cœur" en cliquant sur un lien et ça sans recharger ma page et sans que l'état des autres composants ne soient impactés (mon formulaire de saisie ou ma liste de livres).

Pour cela il existe dans Angular le composant `nRoute` et nous allons voir comment nous en servir.

##Préparation

Nous avons besoin d'une nouvelle dépendance : `angular-route.min.js`

Donc modifiez votre fichier `bower.json` de la manière suivante:

    {
      "name": "books",
      "version": "0.0.0",
      "dependencies": {
        "angular" : "1.2.16",
        "angular-resource" : "1.2.16",
        "angular-route": "1.2.16",
        "uikit" : "2.6.0"
      }
    }

Et lancez la commande `bower update` pour télécharger dans votre projet `angular-route`.

Ensuite dans la page `index.html` ajoutez en bas de page avec les autres définitions de script, la définition suivante:

    <script src="bower_components/angular-route/angular-route.min.js"></script>

###Qu'est qu'un routeur selon Angular?

Un routeur dans Angular est un composant qui est capable de détecter la modification d'une url par une saisie dans la zone d'url du navigateur ou par le biais d'un click sur un lien et de déclencher une action en fonction et mettre à jour le chargement d'une vue par le biais de la directive `ng-view`. C'est à dire, si vous avez dans votre page html un tag comme celui-ci `<div ng-view></div>`, le routeur pourra non seulement déclencher une action, mais aussi charger un template à l'intérieur de ce tag (de la même manière que la directive `ng-include`). Pas clair?

Ok, rien de mieux qu'un exemple, alors.

##Modifions légèrement notre IHM

Modifion `index.html` comme suit:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>livres</title>
    <link rel="stylesheet" href="bower_components/uikit/dist/css/uikit.almost-flat.min.css" />

  </head>

  <body ng-app="booksApp" style="padding: 10px">
```

Insérez la liste ci-dessous :

```html
    <div>
      <ul class="uk-breadcrumb">
        <li><a href="#/actualites">Actualités</a></li>
        <li><a href="#/coupsdecoeur">Coup de coeur</a></li>
      </ul>
    </div>
```

**Remarquez** de quelle manière sont renseignées les attributs `href` : on fait précéder l'url par `#/`.

```html
    <div class="uk-container uk-container-center">

      <div class="uk-grid" ng-controller="MainCtrl">

        <div class="uk-width-4-10">

          <div class="uk-panel" ng-include="'books/bookForm.html'" onload="whenFormIsLoaded()"></div>

        </div>

        <div class="uk-width-6-10">

          <div class="uk-panel" ng-include="'books/booksList.html'" onload="whenListIsLoaded()"></div>

        </div>
      </div>
```

**Ajoutez** cette balise:

```html
      <div ng-view></div>
```

Et le **reste** ne change pas:

```html
    </div>

    <script src="bower_components/angular/angular.min.js"></script>
    <script src="bower_components/angular-resource/angular-resource.min.js"></script>
    <script src="bower_components/angular-route/angular-route.min.js"></script>
    <script src="js/main.js"></script>

  </body>
</html>
```


##Notre 1er routeur

###`ngRoute`

Commençons par ajouter la dépendance `ngRoute` à notre module principal

```javascript
var booksApp = angular.module("booksApp", ["ngResource", "ngRoute"]);
```

Puis définissons notre routeur et son fonctionnement avec la méthode `config` de notre module principal en lui passant en paramètre `$routeProvider`:

```javascript
booksApp.config(["$routeProvider",
  function($routeProvider) {

    $routeProvider.
      when('/actualites', {
        templateUrl: "templates/actualites.html",
        controller: "ActualitesCtrl"
      })
      .when('/coupsdecoeur', {
        templateUrl: "templates/coupsdecoeur.html",
        controller: "CoupsDeCoeurCtrl"
      })
      .otherwise({
        redirectTo: "/"
      })

  }
]);
```

**Que venons nous de faire?** A chaque fois que je cliquerais sur `<a href="#/actualites">Actualités</a>` le fichier `"templates/actualites.html"` sera chargé dans `<div ng-view></div>`et le contrôleur `"ActualitesCtrl"` sera appelé.
De la même manière, lorsque je cliquerais sur `<a href="#/coupsdecoeur">Coup de coeur</a>`le fichier `"templates/coupsdecoeur.html"` sera chargé dans `<div ng-view></div>`et le contrôleur `"CoupsDeCoeurCtrl"` sera appelé.

**A noter**: si dans la barre d'url du navigateur vous saisissez [http://localhost:3000/#/actualites](http://localhost:3000/#/actualites) ou [http://localhost:3000/#/coupsdecoeur](http://localhost:3000/#/coupsdecoeur), les actions décrites ci-dessus seront déclenchées de la même façon (sans rechargement de page), ce qui vous permet de bookmarquer l'état de votre webapp (par exemple, avoir un lien direct sur l'état qui affiche les actualités).

##Définissons nos templates

Créez un sous-répertoire `templates` avec 2 fichiers `actualites.html` et `coupsdecoeur.html`:

    votre_projet/
    ├── public/
    |   ├── bower_components/ /* angular est par ici */
    |   ├── js/
    |   |    └── main.js
    |   ├── books/
    |   |    ├── bookForm.html
    |   |    └── booksList.html
    |   ├── templates/
    |   |    ├── actualites.html
    |   |    └── coupsdecoeur.html
    |   └── index.html
    |

Avec les contenus suivants:

###`actualites.html`

```html
<h2>{% raw %}{{title}}{% endraw %}</h2>
 <ul ng-repeat="new in news">
   <li>{% raw %}{{new.title}}{% endraw %}</li>
 </ul>
```

###`coupsdecoeur.html`

```html
<h2>{% raw %}{{title}}{% endraw %}</h2>
<ul ng-repeat="item in items">
  <li>{% raw %}{{item.title}}{% endraw %}</li>
</ul>
```

Et enfin, ...

##Écrivons les contrôleurs correspondants

Donc dans `/public/js/main.js`, ajoutons les contrôleurs suivants

###`ActualitesCtrl`

```javascript
var ActualitesCtrl = booksApp.controller("ActualitesCtrl", function($scope) {
  $scope.title = "Actualités";
  $scope.news = [
      { title:"Un nouveau Stephen King à venir?" }
    , { title:"Le Kindle paperwhite est juste une turie" }
    , { title:"Le format epub 3 en détail" }
  ];
});
```

###`CoupsDeCoeurCtrl`

```javascript
var CoupsDeCoeurCtrl = booksApp.controller("CoupsDeCoeurCtrl", function($scope) {
  $scope.title = "Coups de Coeur";
  $scope.items = [
      { title:"Game of Thrones" }
    , { title:"Bilbo le Hobbit" }
    , { title:"Le sorcier de TerreMère" }
  ];
});
```

Vous pouvez maintenant tester votre webapp:

<img src="images/ang-routeur-01.png" height="70%" width="70%">

<img src="images/ang-routeur-02.png" height="70%" width="70%">

##Passer des paramètres aux urls

Il est tout à fait possible de passer des paramètres via l'url. Par exemple, ajoutez la route suivante au routeur:

```javascript
.when('/addition/:a/:b', {
    templateUrl: "templates/addition.html",
    controller: "AdditionCtrl"
})
```

Créez le contrôleur `AdditionCtrl`:

```javascript
var AdditionCtrl = booksApp.controller("AdditionCtrl", function($scope, $routeParams) {
  $scope.result = parseInt($routeParams['a']) + parseInt($routeParams['b']);
});
```

Créez rapidement un template `templates/addition.html`:

```html
<h1>{% raw %}{{result}}{% endraw %}</h1>
```

Maintenant vous pouvez faire des additions de cette manière : [http://localhost:3000/#/addition/5/60](http://localhost:3000/#/addition/5/60)

Alors je trouve quelques limitations à ce routeur, j'aurais aimé pouvoir utiliser plusieurs routeurs et plusieurs `ng-view`, mais je pense qu'il faut que j'apprenne encore à l'utiliser. J'ai vu qu'il existait des directives angular dans des projets externes qui le permettait. Nous le verrons peut-être plus tard.


