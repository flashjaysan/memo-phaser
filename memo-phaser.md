# Mémo Phaser

*par flashjaysan*

## Introduction

*Phaser* est une bibliothèque open source de création de jeu vidéo pour *JavaScript*. Elle possède de très nombreuses fonctionnalités qui simplifient la création de jeu utilisant le standard *HTML5* pour les navigateurs web. Les jeux développés avec *Phaser* peuvent être joués sur n'importe quel navigateur web.

## Téléchargement de Phaser

Téléchargez le fichier `phaser.js` à la page [https://phaser.io/download/stable](https://phaser.io/download/stable) et placez-le dans le dossier de votre projet. Je le place en général dans un sous-dossier appelé `libs`.

**Remarque :**  Le fichier [`phaser.js`](https://github.com/photonstorm/phaser/releases/download/v3.21.0/phaser.js) est la version de développement de *Phaser*. Vous pouvez l'ouvrir dans un éditeur de code et lire son contenu. Le fichier [`phaser.min.js`](https://github.com/photonstorm/phaser/releases/download/v3.20.1/phaser.min.js) est une version de production de *Phaser*. Les commentaires ainsi que les espaces ont été supprimés pour réduire la taille du fichier. Sa lecture en est en revanche bien moins facile. Utilisez la version de développement, lors du développement de vos jeux, puis utilisez la version de production lors de la publication.

## Outils nécessaires

### Navigateur

*Phaser* étant une bibliothèque *JavaScript*, vous aurez besoin d'un navigateur web pour tester vos jeux. N'importe quel navigateur web récent fera l'affaire. [Google Chrome](https://www.google.com/chrome/), [Mozilla Firefox](https://www.mozilla.org/fr/firefox/new/), [Opera](https://www.opera.com/fr) ou [Microsoft Edge](https://www.microsoft.com/fr-fr/windows/microsoft-edge) par exemple sont tous de bons choix.

### Editeur de code

Bien que vous puissiez programmer votre jeu dans un éditeur de texte basique tel que le bloc-note, un éditeur de code peut vous simplifier les choses. [Microsoft Visual Studio Code](https://code.visualstudio.com/), [Atom](https://atom.io/), [Sublime Text](https://www.sublimetext.com/), [Brackets](http://brackets.io/) sont des éditeurs populaires.

### Serveur web

Pour tester vos jeux localement lors du développement, vous aurez besoin d'un serveur web. En effet, les navigateurs web, pour des raisons de sécurité, ne permettent pas d'accéder à des fichiers locaux. Un serveur web permet cet accès en le restreignant à un dossier particulier.

#### Mongoose web server

Sur *Windows*, je vous recommande le serveur web [*Mongoose*](http://cesanta.com/downloads.html) qui est très léger et qui ne nécessite aucune installation. Placez simplement le fichier dans le dossier de votre choix qui contiendra vos projets *Phaser* et exécutez le fichier. Une fenêtre de navigateur s'ouvre et affiche les fichiers contenus dans ce dossier.

**Remarque :** Si vous fermez le navigateur, cliquez sur l'icône de *Mongoose* dans la zone de notification en bas à droite et choisissez l'option `Go to my address:...` pour ouvrir à nouveau le navigateur sur le serveur web.

Pour fermer le serveur web, cliquez sur l'icône de *Mongoose* dans la zone de notification en bas à droite et choisissez l'option `Exit`.

#### Brackets

Une autre solution très simple est d'utiliser l'éditeur de code [*Brackets*](brackets.io) qui fournit un serveur web. Ouvrez le dossier de votre projet dans *Brackets*, ouvrez le fichier *HTML* contenant votre jeu et cliquez sur l'icône en forme d'éclair en haut à droite pour ouvrir automatiquement un serveur web sur l'emplacement du fichier *HTML* en cours d'édition.

#### Autres solutions

Il existe une multitude de serveurs web différents utilisés pour divers projets. Chacun a sa propre façon de fonctionner, aussi, je ne peux pas détailler chaque outil. Vous pouvez installer un autre serveur web que ceux que je propose, car l'objectif est simplement de pouvoir accéder aux fichiers ressources d'un dossier local depuis votre navigateur.

## Fichiers de base

Pour commencer, vous devez créer un fichier *HTML* qui fera référence au fichier `phaser.js` ainsi qu'à vos fichiers sources de votre jeu.

```html
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Test1</title>
    </head>
        <script src="libs/phaser.js"></script>
        <script src="scripts/main.js"></script>
    <body>
    </body>
</html>
```

Dans l'exemple précédent, le fichier *HTML* fait référence au fichier `phaser` situé dans un dossier appelé `libs` ainsi qu'au fichier `main.js` situé dans un dossier appelé `scripts`.

**Remarque :** Notez que le fichier `main.js` est référencé **après** la bibliothèque de *Phaser*. Comme il va utiliser les fonctions de cette bibliothèque, il est évident que ces fonctions doivent être connues avant d'y faire référence.

Si vous souhaitez que votre canevas s'adapte à la largeur de la fenêtre du navigateur, ajoutez un style *CSS* à votre fichier *HTML* et définissez la propriété `width` de l'élément `canvas` sur `100%`.

```html
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Test1</title>
        <style>
            canvas {
                width: 100%;
            }
        </style>
    </head>
        <script src="libs/phaser.js"></script>
        <script src="scripts/main.js"></script>
    <body>
    </body>
</html>
```

Définir la propriété `width` de l'élément `canvas`  sur la valeur `100%` demande au navigateur de redimensionner les canevas du document pour qu'ils prennent toute la largeur de la fenêtre. Cela redimensionne également proportionnellement et automatiquement la hauteur des canevas.

## Programme de base

### Création d'un jeu

Pour le moment, votre projet n'a qu'un fichier *HTML*. Vous devez également créez un fichier *JavaScript* qui contiendra le code source de votre jeu. J'appelle généralement ce fichier `main.js` et je le place dans un dossier appelé `scripts`.

Un jeu *Phaser* est basé sur un objet `Phaser.Game` mais avant de pouvoir instancier cet objet, vous avez besoin d'une scène et d'un objet de configuration de jeu.

## Scènes

### Créer une scène

Les scènes sont les éléments de plus haut niveau que vous utiliserez dans vos jeux. Elles correspondent aux différents écrans de jeux ou aux différents niveaux. Vous devez définir au moins une scène que vous passerez à l'objet de configuration de jeu.

Pour définir une scène, utilisez le constructeur [`Phaser.Scene`](https://photonstorm.github.io/phaser3-docs/Phaser.Scene.html). Il prend en argument une chaîne de caractère unique (qui servira de clé pour faire référence à la scène) ou un [objet de configuration de scène](https://photonstorm.github.io/phaser3-docs/Phaser.Types.Scenes.html#.SettingsConfig).

```js
let scene = new Phaser.Scene('scene_principale');
```

### Définition des méthodes de la scène

Vous devez ensuite définir les méthodes essentielles de la scène.

- La méthode [`init`](https://photonstorm.github.io/phaser3-docs/Phaser.Types.Scenes.html#.SceneInitCallback__anchor) est appelée au démarrage de la scène (à sa création). Elle sert à initialiser les données nécessaires à la scène qui ne concernent pas les ressources à charger.
- La méthode [`preload`](https://photonstorm.github.io/phaser3-docs/Phaser.Types.Scenes.html#.ScenePreloadCallback__anchor) est appelée après la méthode `init`. Elle sert à charger en mémoire les ressources (graphiques, audio, etc...) nécessaires à la scène avant que la scène ne commence à afficher son contenu. Son rôle est de pré-charger les ressources avant leur utilisation pour éviter des erreurs (par exemple, afficher une image qui n'est pas encore chargée affichera un joli rectangle de couleur).
- La méthode [`create`](https://photonstorm.github.io/phaser3-docs/Phaser.Types.Scenes.html#.SceneCreateCallback__anchor) est appelée après la méthode `preload` et juste avant que la scène ne commence à afficher son contenu. Elle sert à initialiser les données nécessaires à la scène en utilisant les ressources chargées en mémoire (par exemple, les *sprites*).
- La méthode [`update`](https://photonstorm.github.io/phaser3-docs/Phaser.Scene.html#update__anchor) est appelée avant chaque affichage de la scène. Elle se répète donc en boucle tant que la scène s'exécute. Elle sert à gérer les données du jeu selon diverses conditions.

```js
scene.init = function(){
    
};


scene.preload = function(){
    
};


scene.create = function(){
    
};


scene.update = function(){
    
};
```

### Objet de configuration de jeu

Avant d'appeler le constructeur précédent, vous devez créer un [objet de configuration de jeu](https://photonstorm.github.io/phaser3-docs/Phaser.Types.Core.html#.GameConfig). En général, on l'appelle `config`.

```js
let config = {
    type: Phaser.AUTO,
    width: 640,
    height: 360,
    zoom: 1,
    scene: scene
};
```

Cet objet sert à paramétrer le jeu.

- La propriété `type` définit le type de rendu que le navigateur doit utiliser pour le jeu.
  - La valeur `Phaser.AUTO` indique au navigateur de choisir le rendu *WebGL* en priorité ou *Canvas* si le rendu *WebGL* est inaccessible. Utilisez de préférence cette valeur pour la propriété `type`.
  - La valeur `Phaser.WEBGL` indique au navigateur de choisir le rendu *WebGL* (meilleure performance mais n'est pas disponible sur tous les navigateurs).
  - La valeur `Phaser.CANVAS` indique au navigateur de choisir le rendu *Canvas* (moins bonne performance mais fonctionne sur tous les navigateurs).
  - La valeur `Phaser.HEADLESS` indique au navigateur que le programme n'a pas besoin de fenêtre de rendu. Cette valeur est utilisée dans des environnements sans carte graphique pour effectuer des traitements sans avoir besoin d'afficher d'éléments graphiques. Cette valeur est rarement utilisée, mais peut servir pour faire des simulations ou des tests.
- La propriété `scene` définit la scène à lancer au démarrage du jeu. Attribuez-lui l'objet scène que vous souhaitez définir comme scène de démarrage. Consultez la section sur les scènes pour plus d'information.
- La propriété `width` définit la largeur du rectangle de rendu utilisée par le jeu.
- La propriété `height` définit la hauteur du rectangle de rendu utilisée par le jeu.
- La propriété `zoom` définit le facteur d'agrandissement des éléments dans la fenêtre de rendu. Pour un jeu en faible résolution, augmentez cette valeur pour agrandir la taille du canevas.

**Remarque :** Vous pouvez également définir une scène anonyme directement dans le fichier de configuration de jeu. Vous devrez alors définir des fonctions pour chaque méthode de la scène.

```js
let config = {
    type: Phaser.AUTO,
    width: 640,
    height: 360,
    zoom: 1,
    scene: {
        init: init,
        preload: preload,
        create: create,
        update: update
    }
};


function init(){
    
}


function preload(){
    
}


function create(){
    
}


function update(){
    
}
```


## Instancier le jeu

Une fois la scène principale et le fichier de configuration de jeu définis, pour créer un jeu avec *Phaser*, il vous suffit d'appeler le constructeur [`Phaser.Game`](https://photonstorm.github.io/phaser3-docs/Phaser.Game.html). Ce dernier prend comme argument l'objet de configuration de jeu définit précédemment.

```js
let game = new Phaser.Game(config);
```

## Charger des ressources

Pour chargez des ressources, vous devez le faire dans la méthode de scène `preload`. Celle-ci s'assure que les ressources nécessaires à la scène sont toutes chargées en mémoire avant que la scène ne démarre (lors de l'appel à la méthode `create`).

### Charger une image

Pour charger une image, utilisez la méthode [`Scene.load.image`](https://photonstorm.github.io/phaser3-docs/Phaser.Loader.LoaderPlugin.html#image__anchor) dans la méthode `preload`. Cette méthode prend comme arguments une chaîne de caractère unique (qui servira de clé pour faire référence à l'image) et une chaîne de caractère correspondant au chemin vers le fichier image relativement au fichier *HTML*. *Phaser* prend en charge tous les formats qu'un navigateur peut lire mais je vous conseille de rester sur les format `.png` et `.jpg` ou à défaut `.gif`.

```js
function preload ()
{
    this.load.image('logo', 'images/phaserLogo.png');
}
```

### Charger une spritesheet

Pour charger une spritesheet, utilisez la méthode [`Scene.load.spritesheet`](https://photonstorm.github.io/phaser3-docs/Phaser.Loader.LoaderPlugin.html#spritesheet__anchor). Cette méthode prend une chaîne de caractère unique (qui servira de clé pour faire référence à la spritesheet), une chaîne de caractère correspondant au chemin vers le fichier image et un objet contenant les dimensions (`frameWidth` et `frameHeight`) des sprites (voir exemple). Bien évidemment, les sprites doivent tous être de même taille. *Phaser* prend en charge tous les formats qu'un navigateur peut lire mais je vous conseille de rester sur les format `.png` et `.jpg` ou à défaut `.gif`.

```js
function preload ()
{
    this.load.spritesheet('dude', 
        'assets/dude.png',
        { frameWidth: 32, frameHeight: 48 }
    );
}
```

## Afficher des éléments

Le système de coordonnées de *Phaser* inverse l'axe vertical par rapport au système mathématique habituel. Les valeurs augmentent vers le bas et l'origine du système est positionné dans le coin supérieur gauche du canevas.

### Afficher une image

Une fois vos ressources chargées depuis la fonction `preload`, vous devez ajoutez les images à la scène avec la fonction [`Scene.add.image`](https://photonstorm.github.io/phaser3-docs/Phaser.GameObjects.GameObjectFactory.html#image__anchor). Passez les coordonnées de l'image ainsi que la chaîne de caractère unique identifiant la ressource.

```js
function create()
{
    this.add.image(400, 300, 'logo');
}
```

Cette méthode renvoie l'identifiant du nouveau sprite créé. Vous pouvez donc récupérer cet identifiant dans une variable et faire des manipulations sur ce sprite.

```js
let sprite = this.add.image(400, 300, 'logo');
```

Par défaut, l'origine d'un sprite est placée en son centre.

### Déplacer l'origine d'un sprite

Pour déplacer l'origine d'un sprite, utilisez la méthode [`Phaser.GameObjects.Sprite.setOrigin`](https://photonstorm.github.io/phaser3-docs/Phaser.GameObjects.Sprite.html#setOrigin__anchor). Les arguments attendus sont deux nombres compris entre `0` et `1` correspondants aux limites de l'image source. `0, 0` correspond au coin supérieur gauche de l'image. `1, 1` correspond au coin inférieur droit de l'image. Par défaut, l'origine est placée au centre de l'image (`0.5, 0.5`).

```js
sprite.setOrigin(0.5, 0.5);
```

### Modifier la position d'un sprite

Pour modifier la position d'un sprite, utilisez la méthode [`Phaser.GameObjects.Sprite.setPosition`](https://photonstorm.github.io/phaser3-docs/Phaser.GameObjects.Sprite.html#setPosition__anchor). Les arguments attendus sont deux nombres correspondants aux coordonnées x et y du sprite dans le système de coordonnées de l'objet parent (par défaut, la scène).

```js
sprite.setOrigin(0.5, 0.5);
```

**Attention !** Chaque appel à cette méthode ajoute un élément par dessus les précédents. Vous devez donc être vigilant à l'ordre dans lequel vous souhaitez ajouter vos images à la scène.
