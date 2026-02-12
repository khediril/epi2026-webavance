#  Atelier 4 : Formulaires et Interaction (GET & POST)

## 1. Fiche d'identité

* **Thème :** Récupération des données utilisateur et cycle de vie d'un formulaire.
* **Durée :** 3 heures.
* **Compétences visées :** * Faire la différence entre les méthodes **GET** et **POST**.
* Manipuler les tableaux superglobaux `$_GET` et `$_POST`.
* Sécuriser la réception de données (nettoyage de base).



## 2. Préparation

1. **Dossier :** Continuez dans `~/Documents/mini-news`.
2. **Nouveaux fichiers :** Créez `contact.php` et `traitement.php`.
3. **Debug :** Assurez-vous que votre `require_once 'functions.php'` (avec les `ini_set` d'erreurs) est présent sur tous vos nouveaux fichiers.
4. **Référence :** [Pierre Giraud - Les superglobales $_GET et $_POST](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/variable-superglobale-get-post/).

## 3. Quiz : Du `scanf()` au `$_POST`

1. En C, `scanf` bloque l'exécution en attendant une saisie. En PHP, quand les données deviennent-elles disponibles ?
2. Quelle méthode (GET ou POST) affiche les données directement dans la barre d'adresse de l'URL ?
3. Pourquoi utilise-t-on **toujours** POST pour un mot de passe ou un long texte ?
4. Quel attribut HTML de la balise `<input>` devient la "clé" dans le tableau PHP ?

## 4. Mise en situation

Jusqu'à présent, votre blog est un monologue : vous affichez, l'utilisateur lit. Nous allons créer un formulaire pour que l'utilisateur puisse vous envoyer un message ou rechercher une news. C'est le début de la communication bidirectionnelle.

## 5. Exploration guidée

### La Superglobale $_GET

Elle récupère ce qui est dans l'URL après le `?`.
*Exemple :* `recherche.php?mot=ubuntu&tri=date`
En PHP : `echo $_GET['mot']; // affiche ubuntu`

### La Superglobale $_POST

Elle récupère les données envoyées "silencieusement" dans le corps de la requête HTTP. C'est l'équivalent d'un envoi de colis scellé.

### L'importance de l'attribut `name`

Si vous écrivez `<input type="text" name="pseudo">`, PHP créera automatiquement la variable `$_POST['pseudo']`. **Sans l'attribut `name`, la donnée est perdue.**

## 6. Production principale

**Objectif : Créer un formulaire de contact fonctionnel.**

1. **Le formulaire :** Dans `contact.php`, créez un formulaire HTML avec les champs : `nom`, `email`, et `message` (utilisez un `<textarea>`).
2. **L'expédition :** Configurez la balise form : `<form action="traitement.php" method="POST">`.
3. **La réception :** Dans `traitement.php` :
* Vérifiez si le formulaire a bien été soumis avec `if ($_SERVER["REQUEST_METHOD"] == "POST")`.
* Récupérez les 3 variables.
* Affichez un message de confirmation personnalisé : "Merci [nom], votre message a bien été reçu."


4. **Sécurité (Habitude du C) :** Utilisez la fonction `strip_tags()` sur les données reçues pour éviter que l'utilisateur n'injecte du code HTML malveillant (votre premier pas vers la sécurité web).

## 7. Exercices progressifs (45 min)

### Niveau 1 : Le moteur de recherche (GET)

Sur votre page `index.php`, ajoutez un petit formulaire avec un seul champ `search` et une méthode **GET**. Affichez en haut de la page : "Résultats pour la recherche : [mot_cle]".

### Niveau 2 : Validation de données

Dans `traitement.php`, vérifiez que le message fait au moins 10 caractères. Si c'est trop court, affichez une erreur rouge. (En C, vous utiliseriez `strlen`, en PHP c'est `strlen()` ou `mb_strlen()`).

### Niveau 3 : Persistance temporaire

Si le formulaire est valide, au lieu d'afficher juste un texte, redirigez l'utilisateur vers l'accueil avec un message de succès dans l'URL :
`header("Location: index.php?success=1");`
Dans `index.php`, si `$_GET['success']` existe, affichez une bannière verte : "Message envoyé !".

## 8. Difficulté encadrée

**L'erreur "Undefined index" :**
C'est l'erreur la plus fréquente. Elle arrive si vous essayez d'accéder à `$_POST['message']` alors que l'utilisateur n'a pas encore cliqué sur envoyer.
**Solution :** Utilisez toujours `isset()` ou l'opérateur de fusion null `??`.
*Exemple :* `$msg = $_POST['message'] ?? '';`

## 9. Correction détaillée (Extrait de traitement.php)

```php
<?php
require_once 'functions.php';

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    // Nettoyage (Sanitization)
    $nom = strip_tags($_POST['nom'] ?? 'Anonyme');
    $msg = strip_tags($_POST['message'] ?? '');

    if (strlen($msg) < 10) {
        die("Erreur : Message trop court.");
    }

    echo "<h1>Message de $nom</h1>";
    echo "<p>$msg</p>";
} else {
    // Redirection si accès direct au fichier
    header("Location: contact.php");
    exit;
}

```

## 10. Challenge autonome

**"Le Calculateur de News"**

1. Créez un mini-formulaire qui demande le nombre d'articles par page souhaité.
2. Envoyez cette donnée à `index.php` via **GET**.
3. Dans `index.php`, utilisez cette valeur pour limiter l'affichage de votre boucle `foreach` (Indice : utilisez un compteur ou la fonction `array_slice()`).

## 11. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*  Construire un formulaire HTML compatible avec PHP.
*  Choisir la méthode HTTP appropriée (GET pour la lecture/recherche, POST pour l'écriture/envoi).
*  Récupérer et sécuriser sommairement les données entrantes.
*  Effectuer une redirection serveur avec `header()`.



