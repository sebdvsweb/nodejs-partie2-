# TP Node.js avec Express et Mustache

## Exercice 1

### Objectif

Créer une application Express simple qui utilise Mustache pour le rendu des vues et bodyParser pour lire les données POST des formulaires.

### Étapes d'installation

1. **Installation des dépendances :**
   
   - `mustache-express` : Moteur de templates pour rendre les vues.
   - `body-parser` : Middleware pour lire les données des formulaires POST.
   - `nodemon` : Outil pour redémarrer automatiquement le serveur lorsque des fichiers sont modifiés.
   
   ```bash
   npm install mustache-express body-parser
   npm install --save-dev nodemon
   ```

2. **Configuration de Nodemon :**

Dans le fichier package.json, ajoutez un script pour démarrer le serveur avec Nodemon :

```json
"scripts": {
  "start": "nodemon index.js"
}
```

### Étapes et Explications

1. Création du fichier index.js :

```javascript
const express = require('express');
const mustacheExpress = require('mustache-express');
const bodyParser = require('body-parser');

const app = express();

// Configuration du moteur de vues
app.engine('mustache', mustacheExpress());
app.set('view engine', 'mustache');
app.set('views', __dirname + '/views');

// Middleware pour lire les données POST
app.use(bodyParser.urlencoded({ extended: false }));

// Route GET pour la page d'accueil
app.get('/', (req, res) => {
    res.render('mon-template', { MyName: 'Maxime', MyImage: 'https://example.com/my-image.jpg' });
});

// Démarrage du serveur
app.listen(3000, () => {
    console.log('Serveur démarré sur http://localhost:3000');
});
```

2. Création du dossier views et du fichier mon-template.mustache :

Créez un dossier views dans votre répertoire de projet et ajoutez-y le fichier mon-template.mustache :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Accueil</title>
</head>
<body>
    <h1>{{ MyName }}</h1>
    <img src="{{ MyImage }}" alt="Image de {{ MyName }}">
</body>
</html>
```

Rôle de chaque élément :

`express` : Crée et configure le serveur web.
`mustache-express` : Permet de rendre des templates Mustache.
`body-parser` : Permet de lire les données des formulaires envoyées en POST.
`app.engine` et `app.set` : Configurent le moteur de vues Mustache.
`app.use` : Ajoute le middleware pour parser les données POST.
`app.get` : Définit la route GET pour la page d'accueil qui rend le template mon-template.mustache avec les variables MyName et MyImage.


## Exercice 2

### Objectif

Créer une application Express pour visualiser et ajouter des personnages Marvel. Les personnages sont stockés dans un fichier JSON.

### Étapes et Explications

1. Création du fichier Marvel.json :
Créez un fichier Marvel.json dans votre répertoire de projet avec le contenu suivant :

```json
[
    {
        "nom": "Iron Man",
        "serie": "Iron Man",
        "image": "https://is3-ssl.mzstatic.com/image/thumb/ZypiNEdbU0wwCF0GMJ3zoA/1200x675mf.jpg",
        "description": "Iron Man est un super-héros de bande dessinée américaine créé en 1963 par Stan Lee, Larry Lieber, Don Heck et Jack Kirby. Il est apparu pour la première fois dans Tales of Suspense #39."
    },
    {
        "nom": "Spider-Man",
        "serie": "The Amazing Spider-Man",
        "image": "https://upload.wikimedia.org/wikipedia/en/thumb/2/21/Web_of_Spider-Man_Vol_1_129-1.png/220px-Web_of_Spider-Man_Vol_1_129-1.png",
        "description": "Spider-Man est un super-héros de bande dessinée américaine créé en 1962 par Stan Lee et Steve Ditko. Il est apparu pour la première fois dans Amazing Fantasy #15."
    }
]
```

2. Création du fichier index.mustache dans le dossier views :

Ajoutez un fichier index.mustache dans le dossier views :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="https://unpkg.com/@picocss/pico@1.3.0/css/pico.min.css">
    <title>Personnages Marvel</title>
</head>
<body>
    <h1>Ajouter un personnage Marvel</h1>
    <form action="/add" method="POST" class="container">
        <label for="nom">Nom:</label>
        <input type="text" id="nom" name="nom" required>
        <label for="serie">Série:</label>
        <input type="text" id="serie" name="serie" required>
        <label for="image">Image (URL):</label>
        <input type="text" id="image" name="image" required>
        <label for="description">Description:</label>
        <textarea id="description" name="description" required></textarea>
        <button type="submit">Ajouter</button>
    </form>
    <h2>Liste des personnages</h2>
    <ul class="container">
        {{#personnages}}
        <li>
            <h3>{{ nom }}</h3>
            <p>{{ serie }}</p>
            <img src="{{ image }}" alt="{{ nom }}">
            <p>{{ description }}</p>
        </li>
        {{/personnages}}
    </ul>
    {{#success}}
    <p>Personnage ajouté avec succès !</p>
    {{/success}}
</body>
</html>
```

3. Création du fichier index.js :

Créez ou mettez à jour le fichier index.js avec le contenu suivant :

```javascript
const express = require('express');
const mustacheExpress = require('mustache-express');
const bodyParser = require('body-parser');
const fs = require('fs');

const app = express();

// Configuration du moteur de vues
app.engine('mustache', mustacheExpress());
app.set('view engine', 'mustache');
app.set('views', __dirname + '/views');

// Middleware pour lire les données POST
app.use(bodyParser.urlencoded({ extended: false }));

// Lecture du fichier JSON
const lirePersonnages = () => {
    const data = fs.readFileSync('Marvel.json');
    return JSON.parse(data);
};

// Route GET pour afficher le formulaire et la liste des personnages
app.get('/', (req, res) => {
    const personnages = lirePersonnages();
    res.render('index', { personnages: personnages });
});

// Route POST pour ajouter un personnage
app.post('/add', (req, res) => {
    const personnages = lirePersonnages();
    const nouveauPersonnage = {
        nom: req.body.nom,
        serie: req.body.serie,
        image: req.body.image,
        description: req.body.description
    };
    personnages.push(nouveauPersonnage);
    fs.writeFileSync('Marvel.json', JSON.stringify(personnages, null, 4));
    res.render('index', { personnages: personnages, success: true });
});

// Démarrage du serveur
app.listen(3000, () => {
    console.log('Serveur démarré sur http://localhost:3000');
});
```

Rôle de chaque élément :

`fs.readFileSync` : Lit le fichier JSON pour récupérer les personnages.
`app.get` : Affiche la page d'accueil avec le formulaire et la liste des personnages.
`app.post` : Ajoute un nouveau personnage à la liste et met à jour le fichier JSON.
`fs.writeFileSync` : Écrit les nouveaux personnages dans le fichier JSON avec une indentation automatique.
