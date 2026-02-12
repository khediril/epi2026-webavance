#  Atelier 3 : Modularité, Inclusions et Fonctions

## 1. Fiche d'identité

* **Thème :** Organisation du code et réutilisation (Principe DRY : *Don't Repeat Yourself*).
* **Durée :** 3 heures.
* **Compétences visées :** * Structurer un projet web en composants (`include`, `require`).
* Créer et appeler des fonctions avec paramètres et valeurs de retour.
* Comprendre la portée des variables (Scope) par rapport au langage C.

## 2. Préparation 

1. **Dossier :** Continuez dans `~/Documents/mini-news`.
2. **Serveur :** Relancez le serveur si nécessaire : `php -S localhost:8000`.
3. **Debug :** Prévoyez un fichier `config.php` pour centraliser l'affichage des erreurs.
4. **Référence :** [Pierre Giraud - Les fonctions PHP](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/fonction-utilisateur/).

## 3. Quiz : Du "Header C" à l'Inclusion PHP (10 min)

1. En C, on utilise `#include <stdio.h>`. En PHP, quelle est la différence majeure entre `include` et `require` ?
2. En C, les fonctions doivent être déclarées avant d'être utilisées (prototypes). Est-ce le cas en PHP ?
3. Que signifie le suffixe `_once` dans `require_once` ?
4. Comment fait-on pour qu'une fonction retourne une valeur (comme en C) ?

## 4. Mise en situation

En langage C, vous séparez les prototypes (`.h`) de l'implémentation (`.c`). En Web, nous allons séparer la **structure** (menu, pied de page) de la **logique métier** (calculs, formatage).
Imaginez devoir changer le lien "Contact" sur 20 pages de news. Sans modularité, vous feriez 20 copier-coller. Avec PHP, vous allez modifier **un seul fichier**.

## 5. Exploration guidée

### L'inclusion de composants

Au lieu d'un seul fichier `index.php` géant, nous allons assembler des pièces de puzzle.

### La portée des variables (Scope)

**Attention :** Contrairement au C, une variable globale définie dans `index.php` n'est **pas** automatiquement visible à l'intérieur d'une fonction PHP. Il faut utiliser le mot-clé `global` (bien que ce soit déconseillé) ou, mieux, passer la variable en paramètre.

### Syntaxe des fonctions

```php
function genererLien($url, $nom = "Lien") {
    return "<a href='$url'>$nom</a>";
}

```

## 6. Production principale

**Objectif : Découper l'architecture du projet News.**

1. **Le noyau :** Créez un fichier `functions.php`. Placez-y les lignes `ini_set` pour l'affichage des erreurs afin qu'elles soient actives partout.
2. **Le Design :**
* Créez `header.php` : Coupez-y tout le haut de votre HTML (jusqu'à l'ouverture de la balise `main` ou `body`).
* Créez `footer.php` : Coupez-y tout le bas (fermetures des balises et copyright).


3. **L'assemblage :** Dans `index.php`, utilisez `require_once` pour importer `functions.php`, `header.php` et `footer.php`.
4. **Test :** Ajoutez un paramètre dans l'URL (ex: `?page=accueil`). Votre site doit rester visuellement identique, mais le code est désormais "propre".

## 7. Exercices progressifs

### Niveau 1 : Le Formateur de Titres

Dans `functions.php`, créez une fonction `formaterTitre($titre)`. Elle doit retourner le titre en majuscules (utilisez `mb_strtoupper()`) entouré d'une balise `<h2>`. Appelez-la pour chaque news de votre tableau de l'Atelier 2.

### Niveau 2 : Le "Read More" (Tronquage)

En C, tronquer une chaîne demande de gérer le caractère nul `\0`. En PHP, créez une fonction `tronquer($texte, $max = 100)`. Elle doit retourner les  premiers caractères suivis de "...".
*Indice : Utilisez `mb_substr()`.*

### Niveau 3 : Le Helper de Navigation

Créez une fonction `genererMenu($liens)` qui prend un tableau associatif en paramètre et génère une liste `<ul><li>` de liens HTML automatiquement.

## 8. Difficulté encadrée

**L'erreur "Function already declared" :**
Si vous incluez deux fois un fichier contenant une fonction (par exemple `include` au lieu de `include_once`), PHP s'arrêtera avec une erreur fatale.
**Sous Ubuntu / VS Code :** Surveillez votre terminal. Si vous voyez `Fatal error: Cannot redeclare...`, c'est que vous avez un doublon d'inclusion. Utilisez toujours `require_once` pour vos bibliothèques de fonctions.

## 9. Correction détaillée (Extrait)

```php
// functions.php
function extraireContenu($chaine, $taille = 50) {
    if (mb_strlen($chaine) > $taille) {
        return mb_substr($chaine, 0, $taille) . "...";
    }
    return $chaine;
}

// index.php
require_once 'functions.php';
require_once 'header.php';

foreach ($articles as $article) {
    echo "<div>";
    echo "<h3>" . $article['titre'] . "</h3>";
    echo "<p>" . extraireContenu($article['texte']) . "</p>";
    echo "</div>";
}

require_once 'footer.php';

```

## 10. Challenge autonome

**"Le Mode Nuit intelligent"**

1. Dans `functions.php`, créez une fonction `estModeNuit()`.
2. Elle doit retourner `true` si l'heure actuelle est supérieure à 20h ou inférieure à 7h.
3. Dans `header.php`, injectez une classe CSS `dark-mode` dans la balise `<body>` si la fonction retourne vrai :
`<body class="<?= estModeNuit() ? 'dark' : 'light' ?>">`

## 11. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*  Modulariser une application web en séparant la structure du contenu.
*  Utiliser `require_once` pour sécuriser les inclusions de fonctions.
*  Créer des fonctions avec des paramètres optionnels (valeurs par défaut).
*  Comprendre que le code PHP est exécuté de haut en bas, rendant l'ordre des inclusions crucial.
