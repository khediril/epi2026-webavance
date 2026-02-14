
#  Atelier 1 : Développement Web coté serveur en php avec le Serveur Intégré

Bienvenue dans le monde du Web. Vous allez découvrir que PHP, bien que proche du C par sa syntaxe, offre une flexibilité déconcertante. Fini la compilation manuelle à chaque modification : ici, on rafraîchit la page et le serveur interprète le code instantanément.

## 1. Fiche d'identité

* **Thème :** Syntaxe de base et environnement CLI (Command Line Interface).
* **Durée :** 3 heures.
* **Compétences visées :** * Lancer et manipuler le serveur de développement intégré de PHP.
* Comprendre le rendu côté serveur (Server-Side Rendering).
* Gérer les variables et le typage dynamique.



## 2. Préparation (15 min)

1. **Dossier de travail :** Ouvrez votre terminal (`Ctrl+Alt+T`) et créez votre projet :
```bash
mkdir ~/Documents/mini-news
cd ~/Documents/mini-news
code .

```


2. **Vérification PHP :** Dans le terminal de VS Code, vérifiez que PHP est installé : `php -v`.
3. **Le Serveur Intégré :** Pour lancer votre site, tapez la commande suivante :
```bash
php -S localhost:8000

```


*Laissez ce terminal ouvert.* Votre site est désormais accessible sur `http://localhost:8000`.

## 3. Quiz : Le choc des cultures

1. En C, on utilise `printf()`. En PHP, quelle instruction affiche du contenu dans la page ?
2. En PHP, comment le serveur sait-il qu'il doit arrêter d'interpréter du code et redonner la main au HTML ?
3. Pourquoi n'y a-t-il pas de fichier `.out` ou d'exécutable après avoir écrit du PHP ?
4. Quel symbole est le "préfixe" obligatoire de toute variable ?

## 4. Mise en situation

En PHP, vous allez créer un un afficheur de news.

## 5. Exploration guidée (30 min)

### Le Serveur de Développement

Contrairement à Apache qui tourne en tâche de fond, le serveur intégré (`php -S`) affiche les logs de connexion directement dans votre terminal. C'est votre meilleur allié pour le débogage.

### La syntaxe "Embedded"

En C, vous écrivez du code qui produit du texte. En PHP, vous écrivez du texte (HTML) qui contient des îlots de code.

```php
<h1>Bienvenue</h1>
<?php echo "<p>Ceci est généré par PHP</p>"; ?>

```

### 🔍 Focus : Activer le "Mode Debug" (Affichage des erreurs)

Contrairement au C où les erreurs sont détectées à la compilation, PHP est interprété ligne par ligne. Si vous faites une erreur à la ligne 10, le serveur s'arrêtera brusquement. Par défaut, pour des raisons de sécurité, PHP cache ces erreurs et vous risquez de vous retrouver face à une "Page Blanche" (le fameux *White Screen of Death*), ce qui est très frustrant pour un débutant.

**Pour le développement, nous allons forcer PHP à tout nous dire.**

#### 1. Configuration via le code (La méthode "In-App")

Ajoutez ces deux lignes tout en haut de votre bloc PHP (juste après `<?php`) :

```php
ini_set('display_errors', 1);      // Affiche les erreurs directement dans le navigateur
ini_set('display_startup_errors', 1); // Affiche les erreurs qui surviennent au démarrage de PHP
error_reporting(E_ALL);            // Rapporte absolument tous les types d'erreurs (warnings, notices, etc.)

```

#### 2. Pourquoi est-ce vital pour un habitué du C ?

En PHP, les erreurs les plus courantes sont :

* **Parse Error :** Un point-virgule oublié ou une parenthèse non fermée (l'équivalent de vos erreurs `gcc`).
* **Warning :** Le script continue, mais quelque chose cloche (ex: inclure un fichier inexistant).
* **Fatal Error :** Le script s'arrête immédiatement (ex: appeler une fonction qui n'existe pas).

> [!IMPORTANT]
> **Le conseil du pro :** Dans le terminal où tourne votre serveur intégré (`php -S`), les erreurs s'affichent aussi ! Si votre page web est vide, jetez toujours un œil aux logs de votre terminal VS Code.

---

### "Production principale"


1. Ouvrez `index.php`.
2. **Bloc de configuration :** Commencez votre fichier par le bloc de débogage pour être paré à toute éventualité :
```php
<?php
// 1. Activation des erreurs pour le développement
ini_set('display_errors', 1);
error_reporting(E_ALL);

// 2. Vos variables
$nomSite = "Ubuntu News";
// ... reste du code
?>

```

3. **Test volontaire :** Supprimez un point-virgule intentionnellement ou essayez d'afficher une variable qui n'existe pas (`echo $variableInconnue;`).
* *Observez le résultat :* PHP va vous donner le fichier, la ligne et la raison de l'erreur. 

---

### 8. Difficulté encadrée 

**L'erreur masquée :**
Si vous avez une erreur de syntaxe fatale (ex: oublier le `<?php`), même `ini_set` ne pourra pas l'afficher car le script ne pourra même pas démarrer.
**Solution :** Vérifiez toujours la coloration syntaxique dans VS Code. Si le texte ne change pas de couleur, c'est que votre balise PHP est mal ouverte.

---

**Objectif : Créer l'identité visuelle de votre "Mini-News".**

1. Créez un fichier `index.php` dans VS Code.
2. Ajoutez la structure HTML5 de base (`! + Tab`).
3. Tout en haut du fichier, créez un bloc PHP pour définir :
* `$nomSite` : "Ubuntu News".
* `$slogan` : "Le PHP en ligne de commande".
* `$annee` : Utilisez la fonction `date('Y')`.


4. Dans le HTML, affichez ces variables aux bons endroits (Titre, Header, Footer).

## 7. Exercices progressifs

### Niveau 1 : Arithmétique et Concaténation

Créez `$versionPHP = phpversion();`. Affichez dans le footer : "Propulsé par PHP version X" en utilisant la concaténation (le point `.`).

### Niveau 2 : Typage Faible

Déclarez `$a = "5 articles";` et `$b = 10;`. Essayez de faire `$total = $a + $b;`.
*Question :* Qu'affiche PHP ? Pourquoi un compilateur C aurait-il hurlé à la mort ?

### Niveau 3 : Les Constantes

Utilisez `define('SITEROOT', '/home/user/news');`. Affichez cette constante. Essayez de la modifier plus bas. Observez l'erreur dans les logs du terminal VS Code.

## 8. Difficulté encadrée

**L'erreur 404 sur le serveur intégré :**
Si vous essayez d'accéder à `localhost:8000/contact.php` alors que le fichier n'existe pas, le terminal affichera `[404] /contact.php - No such file or directory`.
**Astuce :** Le serveur intégré ne gère pas la réécriture d'URL compliquée sans script de routage. Pour l'instant, restez sur des noms de fichiers simples.

## 9. Challenge autonome

**"Le Compteur de mots"**

1. Créez une variable `$articleTest` contenant un long paragraphe de texte.
2. Utilisez la fonction `str_word_count()` pour calculer le nombre de mots.
3. Affichez une bannière en bas de l'article : "Temps de lecture estimé : X minutes" (en comptant 200 mots par minute).

## 10. Bilan de compétences

À la fin de cet atelier, vous devez être capable de :

*  Lancer et arrêter le serveur de développement PHP en ligne de commande.
*  Interpréter les logs du serveur dans le terminal.
*  Manipuler les chaînes de caractères sans gestion manuelle de la mémoire.
*  Utiliser les balises courtes `<?= $var ?>` pour l'affichage rapide.
