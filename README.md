# TP Node.js avec Express et Mustache

## Exercice 1

### Objectif

Créer une application Express simple qui utilise Mustache pour le rendu des vues et bodyParser pour lire les données POST des formulaires.

### Présentation 

https://docs.google.com/presentation/d/1HSbkisJoDJxuLosDfAZUdidhvubge1-_OH1dOa4EKIU/edit#slide=id.g734747617e_0_1

### Étapes d'installation

1. **Initialisation du projet Node.js :**

Ouvrez votre projet dans VS Code et ouvrez le terminal de VS Code. 
Ensuite, exécutez la commande suivante pour créer un fichier `package.json` :

```bash
npm init -y
```

2. **Installation des dépendances :**
   
   - `mustache-express` : Moteur de templates pour rendre les vues.
   - `body-parser` : Middleware pour lire les données des formulaires POST.
   - `nodemon` : Outil pour redémarrer automatiquement le serveur lorsque des fichiers sont modifiés.


```bash
npm install mustache-express body-parser
npm install --save-dev nodemon
```

3. **Configuration de Nodemon :**

Dans le fichier `package.json`, ajoutez un script pour démarrer le serveur avec Nodemon :

```json
"scripts": {
  "start": "nodemon index.js"
}
```

### Étapes et Explications

1. Création du fichier `index.js` :

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

Créez un dossier `views` dans votre répertoire de projet et ajoutez-y le fichier `mon-template.mustache`.
Ajouter le code HTML permettant d'afficher vos variables `MyName` et `MyImage`. 

Rôle de chaque élément :

- `express` : Crée et configure le serveur web.
- `mustache-express` : Permet de rendre des templates Mustache.
- `body-parser` : Permet de lire les données des formulaires envoyées en POST.
- `app.engine` et `app.set` : Configurent le moteur de vues Mustache.
- `app.use` : Ajoute le middleware pour parser les données POST.
- `app.get` : Définit la route GET pour la page d'accueil qui rend le template mon-template.mustache avec les variables `MyName` et `MyImage`.


## Exercice 2

### Objectif

Créer une application Express pour visualiser et ajouter des personnages Marvel. Les personnages sont stockés dans un fichier JSON.

### Étapes et Explications

1. Création du fichier Marvel.json :
Créez un fichier `Marvel.json` dans votre répertoire de projet avec le contenu suivant :

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

Ajoutez un fichier `index.mustache` dans le dossier `views`.
Y ajouter un formulaire HTML contenant les champs nécessaires (nom, série, image, description, et le bouton d'envoi).
Ajoutez ensuite un code permettant l'affichage des personnages présents dans le fichier json grâce aux variables définies dans le fichier `index_marvel.js`.

3. Création du fichier index_marvel.js :

Créez ou mettez à jour le fichier `index_marvel.js` avec le contenu suivant :

```javascript
// Ajouter les constantes pour `express`, `mustache-express`, `body-parser` et `fs`
// 4 lignes

const app = express();

// Configuration du moteur de vues
app.engine('mustache', mustacheExpress());
app.set('view engine', 'mustache');
app.set('views', __dirname + '/views');

// Permettre la lecture des données POST
app.use(bodyParser.urlencoded({ extended: false }));

// Lecture du fichier JSON
const lirePersonnages = () => {
    const data = fs.readFileSync('Marvel.json');
    return JSON.parse(data);
};

// Route GET pour afficher le formulaire et la liste des personnages
// Créer la route

app.get('/', (req, res) => {
    // Créer la variable personnages contenant la méthode `lirePersonnages()`
    res.render('index', { personnages: personnages });
});

// Route POST pour ajouter un personnage
app.post('/add', (req, res) => {
    // Créer la variable personnages contenant la méthode `lirePersonnages()`
    // Créer la variable objet et y inclure les champs nécessaires (nom, serie, image, description)
    // Faire un push du nouveauPersonnage dans le tableau personnages
    fs.writeFileSync('Marvel.json', JSON.stringify(personnages, null, 4));
    res.render('index', { personnages: personnages, success: true });
});

// Démarrage du serveur
app.listen(3000, () => {
    console.log('Serveur démarré sur http://localhost:3000');
});
```

Rôle de chaque élément :

- `fs.readFileSync` : Lit le fichier JSON pour récupérer les personnages.
- `app.get` : Affiche la page d'accueil avec le formulaire et la liste des personnages.
- `app.post` : Ajoute un nouveau personnage à la liste et met à jour le fichier JSON.
- `fs.writeFileSync` : Écrit les nouveaux personnages dans le fichier JSON avec une indentation automatique.

4. CSS :

Dans votre fichier `index.mustache`, lier un CSS simple du type Pico CSS pour améliorer l'affichage de votre page.


## Exercice 3

### Objectif

Créer une application Express pour visualiser, ajouter, modifier et supprimer des personnages Marvel depuis une base de données SQL

### Étapes et Explications

1. **Création de la base de données**

Dans PhpMyAdmin, créer une table personnages et ajoutez-y les personnages de base.

 - Création de la table :
   
```sql
CREATE TABLE personnages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(255) NOT NULL,
    serie VARCHAR(255) NOT NULL,
    image VARCHAR(255) NOT NULL,
    description TEXT NOT NULL
);
```

 - Ajout des personnages :

```sql
INSERT INTO personnages (nom, serie, image, description) VALUES
('Iron Man', 'Iron Man', 'https://is3-ssl.mzstatic.com/image/thumb/ZypiNEdbU0wwCF0GMJ3zoA/1200x675mf.jpg', 'Iron Man est un super-héros de bande dessinée américaine créé en 1963 par Stan Lee, Larry Lieber, Don Heck et Jack Kirby. Il est apparu pour la première fois dans Tales of Suspense #39.'),
('Spider-Man', 'The Amazing Spider-Man', 'https://upload.wikimedia.org/wikipedia/en/thumb/2/21/Web_of_Spider-Man_Vol_1_129-1.png/220px-Web_of_Spider-Man_Vol_1_129-1.png', 'Spider-Man est un super-héros de bande dessinée américaine créé en 1962 par Stan Lee et Steve Ditko. Il est apparu pour la première fois dans Amazing Fantasy #15.');
```

2. **Installation de SQL**

Ouvrez le terminal de VS Code. 
Ensuite, exécutez la commande suivante pour installer le module SQL :

```bash
npm install mysql2
```

3. **Connexion à la base de données**

Créer un fichier `database.js` et y ajouter le code suivant :

```js
const mysql = require('mysql2');

const connection = mysql.createConnection({
  host: ' ', // compléter avec vos valeurs
  port: ' ', // compléter avec vos valeurs
  user: ' ', // compléter avec vos valeurs
  password: ' ', // compléter avec vos valeurs
  database: ' ' // compléter avec vos valeurs
});

connection.connect((err) => {
  if (err) throw err;
  console.log('Connexion réussie');
});

module.exports = connection;
```

Sauvegarder et lancer `node database.js`dans votre terminal VS Code. Le message 'Connexion réussie' devrait apparaître.

4. **Connexion à la base de données**

5. **Création des routes**

Créer un fichier `index_sql.js` et y ajouter le code suivant :

```js
// Gestion des dépendances
const express = require('express');
const mustacheExpress = require('mustache-express');
const bodyParser = require('body-parser');
const db = require('./database');

// Lancement d'Express
const app = express();

// Configuration du moteur de vues
app.engine('mustache', mustacheExpress());
app.set('view engine', 'mustache');
app.set('views', __dirname + '/views');

// Lecture des données POST
app.use(bodyParser.urlencoded({ extended: false }));

// Routes pour les interactions SQL
app.get('/', (req, res) => {
    // faire la requête SQL nécessaire dans une constante pour l'affichage des personnages
    // Gérer les éventuelles erreurs
    // Rendre le résultat dans index_sql
});

app.post('/add', (req, res) => {
    const { nom, serie, image, description } = req.body;
    // Faire la requête en variable pour ajouter un personnage à la base de données
    // Ajouter les données
    // Gérer les éventuelles erreurs
    // Rediriger vers la page '/'
});

app.post('/delete/:id', (req, res) => {
    const { id } = req.params;
    // Faire la requête en variable pour supprimer un personnage via son ID
    // Gérer les éventuelles erreurs
    // Rediriger vers la page '/'
});

app.post('/edit/:id', (req, res) => {
    const { id } = req.params;
    const { nom, serie, image, description } = req.body;
    // Faire la requête en variable pour modifier un personnage via son ID
    // Gérer les éventuelles erreurs
    // Rediriger vers la page '/'
});

// Démarrage du serveur
app.listen(3002, () => {
    console.log('Serveur SQL démarré sur http://localhost:3002');
});
``` 
C'est à vous de remplir les différentes routes avec les requêtes nécessaires à la mise en place du CRUD.

6. **Création du fichier Mustache**

 - Créez le fichier `index-sql.mustache` et récupérer le code de votre fichier mustache de l'exercice 2.
 - Coller le dans ce nouveau fichier mustache, et adaptez le pour y ajouter dans la boucle `{{#personnages}}` un formulaire pour la modification d'un personnage et un autre formulaire pour la suppression d'un personnage.
 - Vos formulaires POST doivent avoir en action `/delete/{{ id }}` (suppression) et `/edit/{{ id }}` (modification) pour cibler le personnage via son ID

7. **Test**

Testez votre application !
