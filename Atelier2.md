#  Atelier 2 : Tableaux Dynamiques et Boucles (Projet News)

## 1. Fiche d'identité

* **Thème :** Structures de contrôle et gestion de collections de données.
* **Durée :** 3 heures.
* **Compétences visées :** * Manipuler les tableaux (indexés et associatifs).
* Maîtriser la boucle `foreach` (la révolution par rapport au C).
* Utiliser les conditions pour l'affichage sélectif.



## 2. Préparation

2. **VS Code :** Installez l'extension **"PHP Intelephense"** pour l'autocomplétion.
3. **Terminal :** Ouvrez le terminal intégré dans VS Code (`Ctrl + ` `     `).
1. **Serveur :** Sous Ubuntu, assurez-vous que vous avez demarrer le serveur interne de php : `php -S localhost:8080`.
4. **Référence :** [Pierre Giraud - Les tableaux en PHP](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/tableau-indexe-associatif/)

## 3. Quiz : Du Tableaux C au Tableaux PHP (10 min)

1. En C, on déclare `int tab[5];`. En PHP, doit-on définir la taille du tableau à l'avance ?
2. En C, un tableau ne contient qu'un seul type. Est-ce vrai en PHP ?
3. Quelle est la différence entre un tableau indexé (chiffres) et un tableau associatif ?
4. Si j'ai 100 articles, quelle boucle est la plus efficace pour les afficher tous sans utiliser de compteur `i` ?

## 4. Mise en situation

En langage C, gérer une liste de chaînes de caractères (les titres des news) demanderait des tableaux à deux dimensions ou des pointeurs complexes. En PHP, nous allons utiliser des tableaux "élastiques" qui peuvent s'agrandir tout seuls.
L'objectif est d'afficher automatiquement une liste de news à partir d'une seule source de données.

## 5. Exploration guidée

### L'évolution du tableau

En PHP, un tableau est une structure de type "Dictionnaire".

```php
// Tableau indexé (comme en C)
$categories = ["Tech", "Science"]; 

// Tableau associatif (Nouveau !)
$article = [
    "titre" => "Linux sur Mars",
    "auteur" => "Linus",
    "vues" => 500
];
echo $article["titre"]; // Affiche : Linux sur Mars

```

### La boucle Foreach

En C, vous feriez : `for(int i=0; i<taille; i++) { printf(tab[i]); }`.
En PHP, on utilise `foreach` :

```php
foreach ($categories as $cat) {
    echo "<li>$cat</li>";
}

```

## 6. Production principale (1h00)

**Objectif : Automatiser l'affichage des news.**

1. Dans votre fichier `index.php`, créez un tableau associatif nommé `$news` contenant :
* Un titre, un contenu court, et un nom d'auteur.


2. Affichez ces informations proprement dans une `<div>` HTML.
3. **Le saut qualitatif :** Créez maintenant un tableau "multidimensionnel" (un tableau qui contient plusieurs tableaux associatifs) nommé `$listeArticles`.
4. Utilisez une boucle `foreach` pour générer une carte HTML pour **chaque** article présent dans `$listeArticles`.

## 7. Exercices progressifs

### Niveau 1 : Le Badge de Catégorie

Ajoutez une variable `$priorite` (allant de 1 à 3). Utilisez un `switch` (identique au C) pour afficher un badge : "Urgent" (si 1), "Important" (si 2) ou "Info" (si 3).

### Niveau 2 : Filtrage d'affichage

Modifiez votre boucle `foreach` pour qu'elle n'affiche que les articles dont le titre contient plus de 10 caractères (utilisez la fonction `strlen()`).

### Niveau 3 : Compteur de News

Avant la boucle, créez une variable `$totalVues = 0;`. Dans la boucle, additionnez les vues de chaque article. Affichez le total en bas de page.

## 8. Difficulté encadrée

**L'erreur "Array to string conversion" :**
Cela arrive si vous faites `echo $listeArticles;` au lieu d'accéder à un index ou d'utiliser une boucle.
**Sous Ubuntu / VS Code :** Utilisez la fonction `var_dump($votreTableau);` pour "déboguer" et voir ce que contient votre variable. C'est l'équivalent survitaminé de vos `printf` de debug.

## 9. Challenge autonome (20 min)

**"Le tri sélectif"**

1. Ajoutez un champ `"publie" => true` ou `false` dans vos articles.
2. Modifiez la boucle pour que seuls les articles dont `"publie"` est à `true` s'affichent sur le site.
3. Si aucun article n'est publié, affichez un message : "Maintenance du flux en cours".

## 10. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*  Déclarer et lire un tableau associatif (Clé => Valeur).
*  Parcourir une collection de données avec `foreach`.
*  Imbriquer une structure conditionnelle (`if`) à l'intérieur d'une boucle.
*  Utiliser `var_dump()` pour inspecter une structure de données complexe.
