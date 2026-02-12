C'est l'étape de "Code Review" qui vous permettra de s'assurer que votre travail respecte les standards professionnels du Web.

---

# ✅ Check-list de Validation : "Mini-News PHP"

Avant de considérer votre projet comme terminé, passez votre code au crible de ces **4 piliers du développement PHP**.

### 1. Structure et Modularité (Ateliers 1 & 3)

*   Est-ce que mon code est séparé en fichiers logiques (`header.php`, `footer.php`, `db.php`, `functions.php`) ?
*   Ai-je utilisé `require_once` pour les fichiers vitaux et `include` pour les composants visuels ?
*   Est-ce que mon serveur intégré PHP se lance sans erreur (`php -S localhost:8000`) ?
*   Ai-je supprimé les blocs de debug (`ini_set('display_errors', 1)`) ou les ai-je centralisés dans un fichier de config ?

### 2. Logique et Données (Ateliers 2, 5 & 6)

*   Mes boucles `foreach` vérifient-elles si le tableau n'est pas vide avant de tenter un affichage ?
*   Toutes mes requêtes SQL qui dépendent d'une saisie utilisateur (ID, recherche) utilisent-elles des **Requêtes Préparées** (`prepare` / `execute`) ?
*   Ma connexion PDO utilise-t-elle le mode d'exception pour attraper les erreurs SQL ?
*   Mes variables sont-elles nommées de façon explicite (en `camelCase` ou `snake_case`) ?

### 3. Sécurité (Ateliers 7 & 8)

*   **Faille XSS :** Ai-je appliqué `htmlspecialchars()` sur **chaque** variable affichée avec `echo` ?
*   **Authentification :** Est-ce que mes pages d'administration (`ajouter.php`, `modifier.php`) commencent bien par une vérification de session ?
*   **Mots de passe :** Sont-ils hachés en base de données avec `password_hash()` (et jamais stockés en clair) ?
*   **Redirections :** Ai-je bien mis un `exit;` ou `die();` juste après chaque `header("Location: ...")` ?

### 4. Expérience Utilisateur (UX)

*   Est-ce qu'un message de confirmation (Flash Message) apparaît après un ajout, une modification ou une suppression ?
*   Si je saisis une mauvaise URL ou un ID d'article inexistant, est-ce que le site affiche une erreur propre plutôt qu'une erreur système ?
*   Mon menu de navigation permet-il de revenir facilement à l'accueil depuis n'importe quelle page ?

---

###  Pour aller plus loin (Bonus)


1. **Refactoring :** Créer une seule fonction `db_connect()` dans `functions.php` qui retourne l'objet `$pdo`, pour éviter de répéter le bloc `try...catch` partout.
2. **Upload d'image :** Permettre d'ajouter une image de couverture à chaque news (gestion du tableau superglobal `$_FILES`).

