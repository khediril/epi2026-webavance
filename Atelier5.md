#  Atelier 5 : La Persistance des Données et Sécurité SQL

## 1. Fiche d'identité

* **Thème :** Interaction complète avec MySQL (Lecture & Écriture) et protection du système.
* **Durée :** 3 à 4 heures.
* **Compétences visées :** * Maîtriser l'objet **PDO** pour l'interaction PHP-MySQL.
* Écrire des requêtes de lecture (`SELECT`) et d'insertion (`INSERT`).
* **Sécuriser** les données via les requêtes préparées (Protection contre l'Injection SQL).



## 2. Préparation (15 min)

1. **Outils :** Terminal Ubuntu, VS Code et accès à `http://localhost/phpmyadmin`.
2. **Base de données :** Dans phpMyAdmin, créez une base `blog_db`.
3. **Table :** Exécutez ce SQL pour créer votre structure :
```sql
CREATE TABLE articles (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titre VARCHAR(255) NOT NULL,
    contenu TEXT NOT NULL,
    date_creation DATETIME DEFAULT CURRENT_TIMESTAMP
);

```



## 3. Quiz : Du "File I/O" au SQL (10 min)

1. En C, pour modifier un fichier, vous devez le lire entièrement puis le réécrire. Quel est l'avantage du SQL pour modifier une seule ligne ?
2. Pourquoi ne doit-on jamais insérer une variable `$_POST` directement dans une chaîne SQL ?
3. Quelle est la différence entre `$pdo->query()` et `$pdo->prepare()` ?
4. Quel "jeton" (placeholder) utilise-t-on dans une requête préparée pour remplacer une valeur ?

## 4. Mise en situation

Jusqu'à présent, vos données étaient volatiles. Si vous redémarrez le serveur, tout est perdu. Nous allons transformer votre blog pour qu'il puisse :

1. **Afficher** les articles stockés en base (Lecture).
2. **Ajouter** de nouveaux articles via un formulaire (Écriture).
3. **Résister** aux pirates qui tenteraient de détruire votre base de données via les champs de saisie.

## 5. Exploration guidée (40 min)

### La Connexion PDO (db.php)

Créez un fichier `db.php` pour centraliser la connexion. Pour les habitués du C, voyez cela comme l'ouverture d'un "socket" ou d'un descripteur de fichier persistant.

### L'Injection SQL : Le danger

Si vous faites : `"SELECT * FROM users WHERE name = '" . $_POST['user'] . "';"`, un utilisateur peut saisir `' OR 1=1 --`. Cela modifierait votre logique de code.

### La solution : La Requête Préparée

En C, vous vérifiez la taille de vos buffers. En PHP/SQL, vous "préparez" le moule de la requête, puis vous envoyez les données séparément. Le serveur SQL ne pourra jamais confondre une donnée avec une commande.

## 6. Production principale (1h30)

### Étape 1 : Lecture (Read)

Dans `index.php`, connectez-vous à la base et affichez tous les articles :

```php
$stmt = $pdo->query("SELECT * FROM articles ORDER BY date_creation DESC");
$articles = $stmt->fetchAll(PDO::FETCH_ASSOC);

```

### Étape 2 : Création (Create)

Créez `ajouter.php` avec un formulaire (titre et contenu). Dans le script de traitement :

1. Vérifiez que les champs ne sont pas vides.
2. Utilisez une **requête préparée** :

```php
$sql = "INSERT INTO articles (titre, contenu) VALUES (:titre, :contenu)";
$stmt = $pdo->prepare($sql);
$stmt->execute([
    'titre' => $_POST['titre'],
    'contenu' => $_POST['contenu']
]);

```

## 7. Exercices progressifs (45 min)

### Niveau 1 : Le compteur d'articles

Affichez dynamiquement en haut de votre page : "Nombre d'articles publiés : X". (Indice : utilisez `COUNT(*)`)

### Niveau 2 : Page de détail sécurisée

Modifiez `article.php?id=X`. Utilisez une requête préparée avec `fetch()` (au lieu de `fetchAll`) pour afficher le contenu complet d'un seul article en fonction de son ID.

### Niveau 3 : Suppression (Delete)

Ajoutez un bouton "Supprimer" sur chaque article dans `index.php`. Créez un fichier `supprimer.php` qui reçoit l'ID et exécute la commande SQL `DELETE`. **Attention : la requête doit être préparée !**

## 8. Difficulté encadrée

**L'erreur "Fatal error: Uncaught PDOException" :**
Cela arrive souvent si le nom de la table ou d'une colonne est mal orthographié dans votre code PHP.
**Sous Ubuntu :** Activez toujours le mode d'exception PDO dans votre `db.php` pour que PHP vous donne le détail de l'erreur SQL :
`$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);`

## 9. Correction détaillée (Extrait de l'insertion)

```php
// Traitement de l'ajout
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $titre = trim($_POST['titre']);
    $contenu = trim($_POST['contenu']);

    if (!empty($titre) && !empty($contenu)) {
        $ins = $pdo->prepare("INSERT INTO articles (titre, contenu) VALUES (?, ?)");
        $ins->execute([$titre, $contenu]);
        header("Location: index.php"); // Redirection après succès
        exit;
    }
}

```

## 10. Challenge autonome (20 min)

**"Le moteur de recherche sécurisé"**

1. Reprenez votre barre de recherche de l'Atelier 4.
2. Implémentez la recherche en SQL utilisant `LIKE`.
3. **Contrainte :** Vous devez utiliser une requête préparée pour le mot-clé recherché afin d'éviter toute injection.

## 11. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*  Configurer une connexion PDO robuste avec gestion d'erreurs.
*  Différencier et utiliser `query()` (données statiques) et `prepare()` (données utilisateurs).
*  Récupérer des données (`SELECT`) et en envoyer (`INSERT`).
*  Expliquer pourquoi l'injection SQL est le risque majeur d'un site web.
