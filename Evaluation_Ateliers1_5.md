# 🎓 Évaluation à Mi-parcours (Ateliers 1 à 5) : Le "Livre d'Or"

## 📋 Modalités
* **Objet :** Vérification des acquis sur les bases PHP, les tableaux, la modularité, les formulaires et l'interaction avec la base de données (PDO).
* **Durée :** 1h30 (90 minutes).
* **Outils autorisés :** Cours, notes, documentation officielle (php.net), IDE (VS Code).
* **Livrable :** Un dossier compressé (`Nom_Prenom.zip`) de votre projet et un export de votre base de données (`db.sql`).

---

## 📝 Présentation du sujet
Vous devez développer un **Livre d'Or** basique. C'est un grand classique du développement web.
Les visiteurs de votre site doivent pouvoir lire les messages laissés par d'autres utilisateurs, et formuler eux-mêmes un nouveau message.

Le projet mettra en œuvre :
1. Une base de données pour stocker les messages.
2. Une architecture modulaire pour ne pas répéter de code HTML.
3. L'affichage de données via une boucle.
4. Un formulaire sécurisé pour recueillir des données.

---

## 🕰️ Déroulement de l'épreuve

### Phase 1 : Base de données (15 min)
1. Accédez à `phpMyAdmin` et créez une base de données nommée `livre_or_db`.
2. Créez une table `messages` contenant les champs suivants :
   * `id` : Entier (INT), Clé primaire (Primary Key), Auto-incrémenté (A_I).
   * `auteur` : Chaîne de caractères (VARCHAR(50)).
   * `contenu` : Texte long (TEXT).
   * `date_publication` : Date et Heure (DATETIME), avec pour valeur par défaut `CURRENT_TIMESTAMP`.
3. Insérez manuellement (via l'interface de phpMyAdmin) 2 ou 3 messages de test.

### Phase 2 : Architecture et Modularité (Ateliers 1 & 3) (20 min)
1. Créez votre dossier de projet (ex: `mon_livre_or`) et initiez sa structure :
   * `db.php` : gérera uniquement la connexion PDO à la base de données. N'oubliez pas d'activer l'affichage des erreurs avec `PDO::ERRMODE_EXCEPTION`.
   * `header.php` : contiendra l'entête HTML (le `<!DOCTYPE html>`, le `<head>`, et le titre principal `<h1>` de votre site).
   * `footer.php` : contiendra la fin de la page, avec l'année courante générée dynamiquement en PHP grâce à la fonction `date('Y')`.
   * `index.php` : sera votre page principale.

### Phase 3 : Lecture et Affichage (Ateliers 2 & 5) (25 min)
1. Dans `index.php`, incluez vos fichiers `db.php` et `header.php` en utilisant l'instruction appropriée (`require_once` ou `include`).
2. Rédigez la requête SQL (`SELECT`) qui récupère tous les messages de la table `messages`, triés du plus récent au plus ancien.
3. Exécutez la requête via PDO (`query` et `fetchAll`).
4. À l'aide de la boucle `foreach`, affichez chaque message dans une structure HTML (ex: une `div` ou un `article` contenant le nom de l'auteur en gras, la date, et le texte du message).

> **Bonus (+1 pt) :** Créez un fichier `functions.php`, et codez une fonction `nettoyerTexte($chaine)` qui retourne les 50 premiers caractères d'un texte suivi de "..." si celui-ci est trop long (Atelier 3 - `mb_substr`). Appliquez cette fonction au contenu des messages.

### Phase 4 : Le Formulaire (Atelier 4) (10 min)
1. À la fin de votre page `index.php`, ajoutez un formulaire HTML avec la méthode HTTP adéquate pour l'envoi de données.
2. Le formulaire doit pointer vers un fichier `traitement.php`.
3. Il comportera :
   * Un champ texte pour `l'auteur` (attribut `name` obligatoire).
   * Un champ multi-lignes (`textarea`) pour `le contenu`.
   * Un bouton de soumission.

### Phase 5 : L'Envoi et la Sécurité (Ateliers 4 & 5) (20 min)
1. Créez le fichier `traitement.php`.
2. Vérifiez que les données ont bien été postées et qu'elles ne sont pas vides.
3. Sécurisez les saisies en utilisant `strip_tags()` pour éviter toute faille XSS (les utilisateurs ne doivent pas pouvoir injecter de balises `<script>` ou HTML dans le texte).
4. Insérez le nouveau message dans la base de données à l'aide d'une **requête préparée** (`prepare()` et `execute()`) pour bloquer toute tentative d'injection SQL.
5. Une fois l'insertion réussie, redirigez silencieusement l'utilisateur vers `index.php` à l'aide de l'instruction `header("Location: ...")`.

