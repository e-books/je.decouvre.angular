#Filter et Filters

Les filtres dans Angular, le petit truc sympa qui simplifie grandement la vie (je commence à comprendre de plus en plus pourquoi les développeurs java aiment Angular: tous des faignasses, moi aussi, donc j'aime Angular). 

##Filter ... dans les templates : on filtre `ng-repeat`

Imaginons que je veuille ajouter au-dessus de ma liste de livre une zone de texte qui me permette de filtrer ma liste en fonction du texte saisi, et ceci de manière dynamique... et sans écrire une ligne de javascript :)

Actuellement notre liste ressemble à ceci:

```html
<div class="uk-panel">

  <h3 class="uk-panel-title">Liste des Livres</h3>

  <table class="uk-table uk-table-hover">
    <thead>
    <tr>
      <th>Titre</th>
      <th>Description</th>
      <th>Niveau</th>
      <th>id</th>
    </tr>
    </thead>

    <tbody ng-repeat="book in books" ng-click="selectBook(book)">
    <tr>
      <td>{{book.title}}</td>
      <td>{{book.description}}</td>
      <td>{{book.level}}</td>
      <td>{{book._id}}</td>
    </tr>
    </tbody>
  </table>

</div>
```

###Filter them all

Nous allons modifier le code de la façon suivante:

```html
<div class="uk-panel">

  <h3 class="uk-panel-title">Liste des Livres</h3>
```

**On ajoute la zone de texte de recherche** et on lui affecte un modèle `searchText` (pas besoin de le déclarer dans le code, la directive suffit):

```html
  <form class="uk-form">
    <input ng-model="searchText" placeholder="chercher un livre">
  </form>
```

Ensuite on ne change rien ...

```html
  <table class="uk-table uk-table-hover">
    <thead>
    <tr>
      <th>Titre</th>
      <th>Description</th>
      <th>Niveau</th>
      <th>id</th>
    </tr>
    </thead>
```

... **sauf au niveau de la directive** `ng-repeat` à laquelle on ajoute un filtre sur la valeur de `searchText`:

```html
    <tbody ng-repeat="book in books | filter:searchText" ng-click="selectBook(book)">
    <tr>
      <td>{{book.title}}</td>
      <td>{{book.description}}</td>
      <td>{{book.level}}</td>
      <td>{{book._id}}</td>
    </tr>
    </tbody>
  </table>

</div>
```

Nous avons donc ajouté 3 lignes, et modifié une 4ème. Faites l'exercice d'aller demander à un développeur "pas Angular", à combien de temps il chiffrerait cette fonctionnalité ;). *(mais ça c'est un autre sujet ...)*

Vous n'avez plus qu'à tester, ça fonctionne très bien mais par contre cela fait de la recherche sur l'ensemble des colonnes. Je voudrais pouvoir faire des recherches par champs.

###Filter ... Better

Avant tout, allez modifier notre factory `Models`, on ajoute un niveau `""` qui permettra de faire des recherches "tous niveaux" (ou autrement dit sans filtre sur le niveau): 

```javascript
booksApp.factory("Models", function() {
  var levels = function() {
    return [
      "", "très bon", "bon", "débutant"
    ];
  }

  return {
    levels : levels
  }
});
``` 

**Modifions le formulaire de recherche** de la manière suivante:

```html
<form class="uk-form">
  <input ng-model="search.$" placeholder="chercher un livre">
  <input ng-model="search.title" placeholder="par titre">
  <input ng-model="search.description" placeholder="par description">
  <select ng-model="search.level"
          ng-options="level for level in levels">
  </select>
</form>
```

Donc nous avons ajouté des champs de recherche et modifié les directives `ng-model` :

- `ng-model="search.$"` : on cherche dans tous les champs
- `ng-model="search.title"` : on cherche dans le titre
- etc. ...

**Modifions ensuite la directive** `ng-repeat` pour prendre en compte les changements:

```html
<tbody ng-repeat="book in books | filter:search:strict" ng-click="selectBook(book)">
```

Donc, pas beaucoup plus compliqué que toute à l'heure. Vous pouvez tester, et vous aller remarquer que les filtres de recherches sont cumulatifs! Donc une joli fonctionnalité de recherche à pas cher ;).

Vous pouvez aussi utiliser l'API `filter` en "pur" javascript si vous devez filtrer des tableaux, mais là je vous laisse jeter un coup d'oeil à la documentation d'Angular [https://docs.angularjs.org/api/ng/filter/filter](https://docs.angularjs.org/api/ng/filter/filter).

##Filters : les "filtres de formatage"

Si vous faites des recherches dans Google sur `angular + filter`, vous tomberez sur le sujet que nous venons de traiter mais aussi sur la notion de filtre de format d'affichage : on définit la manière dont on veut afficher les données (dates, nombre, monnaie, ...). 

###Afficher en majuscules

Par exemple, toujours dans ma liste, je souhaite afficher les niveaux en majuscules. Il suffit de modifier :

```html
<td>{{book.level}}</td>
```

par :

```html
<td>{{book.level | uppercase}}</td>
```

Là aussi je vous engage à aller voir la documentation : [https://docs.angularjs.org/guide/filter](https://docs.angularjs.org/guide/filter)

###Faire son propre filtre d'affichage : `stars`

Je souhaite afficher un certain nombre d'étoiles en fonction du niveau, plutôt que d'afficher le niveau en toutes lettres. Pour cela, il suffit de quelques lignes de javascript pour créer son propre filtre :

Allons éditer `public/js/main.js` pour ajouter le code suivant:

```javascript
booksApp.filter("stars", function() {
  return function(data) {
    switch (data) {
      case "très bon":
        return "***";
        break;
      case "bon":
        return "**";
        break;
      case "débutant":
        return "*";
        break;
      default:
        return "";
    }
  }
});
```

Nous avons donc notre filtre, et pour l'utiliser il suffit de modifier côté html la ligne de toute à l'heure:

```html
<td>{{book.level | uppercase}}</td>
```

en remplaçant le filtre `uppercase` par `stars` :

```html
<td>{{book.level | stars}}</td>
```

Vous n'avez plus qu'à vérifier. Pratique non?
