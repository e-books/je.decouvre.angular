#Préparation

##Pré-requis

- nodejs
- npm
- bower

Dans un répertoire créez les fichiers suivants (à la racine) :

##`bower.json`

    {
      "name": "books",
      "version": "0.0.0",
      "dependencies": {
        "angular" : "1.2.16"
      }
    }

##`.bowerrc`

    {
      "directory": "public/bower_components"
    }

Ensuite créez un répertoire `public` avec le fichier suivant dans ce répertoire:

##`public/index.html`

```hml
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>livres</title>
</head>
<body>

  <script src="bower_components/angular/angular.min.js"></script>
  <script src="js/main.js"></script>

</body>
</html>
```

Ensuite créez un sous-répertoire `js` dans `public` avec le fichier suivant dans ce répertoire:

##`public/js/main.js`

```javascript
// Foo ...
```

Ensuite lancez la commande `bower install` pour télécharger la librairie Angular.

Nous avons donc la structure de projet suivante:

    votre_projet/
    ├── public/
    |   ├── bower_components/ /* angular est par ici */
    |   ├── js/      
    |   |    └── main.js
    |   └── index.html
    ├── bower.json
    └── .bowerrc
