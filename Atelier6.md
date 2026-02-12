# Atelier 7 : Authentification, Sessions et Sécurité des accès

## 1. Fiche d'identité

* **Thème :** Création d'un espace administrateur sécurisé.
* **Durée :** 3 heures.
* **Compétences visées :** * Gérer le maintien de l'état utilisateur via les **Sessions**.
* Comprendre et implémenter le **hachage de mots de passe** (bcrypt).
* Créer une barrière d'accès (Access Control) sur des pages sensibles.



## 2. Préparation (10 min)

1. **Dossier :** Travaillez dans `~/Documents/mini-news`.
2. **Base de données :** Ajoutez une table `utilisateurs` via phpMyAdmin :
```sql
CREATE TABLE utilisateurs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pseudo VARCHAR(50) NOT NULL,
    mot_de_passe VARCHAR(255) NOT NULL -- 255 car le hash est long
);

```


3. **Référence :** [Pierre Giraud - Les Sessions PHP](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/session-cookie/) et [Hachage des mots de passe](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/mot-de-passe-hachage-securite/).

## 3. Quiz : La mémoire du Web (10 min)

1. HTTP est un protocole "stateless" (sans mémoire). Comment PHP fait-il pour savoir qu'un utilisateur est le même entre deux clics ?
2. En C, on ne stocke jamais un mot de passe en clair. Quelle est la différence entre le **chiffrement** (réversible) et le **hachage** (irréversible) ?
3. Où sont stockées les données de `$_SESSION` ? (Côté client ou côté serveur ?)
4. Quelle fonction doit être appelée impérativement avant tout code HTML pour activer la mémoire du serveur ?

## 4. Mise en situation

En langage C, un programme s'exécute d'un bloc. En Web, chaque page est un nouveau programme qui repart de zéro. Pour créer un espace "Admin" où l'on peut ajouter/supprimer des news, nous devons créer un "badge" (le cookie de session) que le navigateur présente au serveur à chaque page.

## 5. Exploration guidée (30 min)

### Le hachage (Sécurité PHP)

Ne faites jamais `if ($pass == "1234")`. En PHP, on utilise `password_hash()` pour stocker et `password_verify()` pour comparer. C'est une protection contre les fuites de base de données.

### Mécanisme de Session

```php
session_start(); // On allume la mémoire
$_SESSION['user'] = "Admin"; // On stocke
echo $_SESSION['user']; // On récupère sur n'importe quelle autre page

```

## 6. Production principale (1h15)

**Objectif : Sécuriser l'ajout d'articles.**

1. **Inscription (Initialisation) :** Créez un script `create_user.php` qui insère un utilisateur dans la table avec un mot de passe haché :
```php
$hash = password_hash("mon_code_secret", PASSWORD_DEFAULT);
// INSERT en base...

```


2. **Connexion :** Créez `login.php` avec un formulaire. Le script doit vérifier le pseudo et le mot de passe (via `password_verify`). Si c'est bon, on remplit `$_SESSION['admin_id'] = $user['id'];`.
3. **Protection :** Créez un fichier `auth_check.php`. Si la session `admin_id` n'existe pas, il redirige vers `login.php`.
4. **Application :** Incluez `auth_check.php` en haut de votre fichier `ajouter.php`. Désormais, seul l'admin peut écrire des news !

## 7. Exercices progressifs (45 min)

### Niveau 1 : Le bouton Déconnexion

Créez `logout.php` qui utilise `session_destroy()`. Vérifiez que l'accès à `ajouter.php` devient impossible après avoir cliqué dessus.

### Niveau 2 : Personnalisation de l'interface

Dans le `header.php`, si l'utilisateur est connecté, affichez "Bonjour [Pseudo] (Déconnexion)". Sinon, affichez un lien "Connexion".

### Niveau 3 : Expiration de session

Utilisez `$_SESSION['last_activity'] = time();`. Dans votre check d'auth, si l'activité remonte à plus de 10 minutes, détruisez la session pour forcer une reconnexion (Sécurité accrue).

## 8. Difficulté encadrée

**L'erreur "Headers already sent" :**
C'est le cauchemar des débutants. Elle arrive si vous appelez `session_start()` ou `header()` alors qu'un espace ou un caractère HTML a déjà été envoyé au navigateur.
**Sous Ubuntu / VS Code :** Assurez-vous que `session_start()` est bien à la **ligne 1, colonne 1**, avant même le `<!DOCTYPE>`.

## 9. Correction détaillée (Extrait du Login)

```php
// login.php (simplifié)
if (password_verify($password_saisi, $user_bdd['mot_de_passe'])) {
    session_start();
    $_SESSION['admin'] = $user_bdd['pseudo'];
    header("Location: admin_dashboard.php");
    exit;
} else {
    echo "Identifiants invalides.";
}

```

## 10. Challenge autonome (20 min)

**"Le Gardien du Dashboard"**

1. Créez une page `admin_stats.php` qui affiche le nombre total de news et le nombre de caractères total rédigés sur le blog.
2. Sécurisez cette page pour qu'elle ne soit accessible **que si** l'utilisateur a le pseudo "admin".

## 11. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*   Différencier les variables locales, globales et superglobales (Sessions).
*   Hacher correctement un mot de passe avec les standards actuels.
*   Utiliser les sessions pour protéger l'accès à des scripts sensibles.
*   Gérer le cycle de vie d'une connexion (Connexion / Vérification / Déconnexion).
