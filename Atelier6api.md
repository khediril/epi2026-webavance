#  Atelier 9 : PHP en mode API et consommation avec JS (Fetch)

Ici vous aller utiliser PHP comme un **Backend** (fournisseur de données) et JavaScript comme un **Frontend** (consommateur). 

## 1. Fiche d'identité

* **Thème :** Séparation du Backend (PHP) et du Frontend (JS/JSON).
* **Durée :** 3 heures.
* **Compétences visées :** * Transformer un script PHP en point d'entrée API (Endpoint).
* Manipuler le format **JSON** (l'équivalent des `struct` en C, mais textuel).
* Utiliser l'API `fetch()` de JavaScript pour récupérer des données sans recharger la page.



## 2. Préparation (10 min)

1. **Dossier :** Créez un dossier `api/` dans votre projet.
2. **Fichiers :** Créez `api/get_news.php` et un fichier HTML simple `dashboard.html`.
3. **Référence :** [Pierre Giraud - JSON en PHP](https://www.google.com/search?q=https://www.pierre-giraud.com/php-mysql-apprendre-coder-cours/json-php/).

## 3. Quiz : Du Binaire au JSON (10 min)

1. En C, pour envoyer une structure sur le réseau, on envoie souvent des octets bruts. Quel est l'avantage d'utiliser le format **JSON** sur le Web ?
2. Que signifie l'acronyme JSON ?
3. Quel "Header" HTTP doit-on envoyer en PHP pour dire au navigateur que la réponse n'est pas du HTML, mais du JSON ?
4. Quelle fonction PHP transforme un tableau associatif en chaîne JSON ?

## 4. Mise en situation

Aujourd'hui, la plupart des sites (Facebook, Gmail) ne rechargent pas la page entière. Le JavaScript demande des données au serveur PHP en arrière-plan, reçoit du JSON, et met à jour le DOM. Vous allez créer votre première "API News".

## 5. Exploration guidée (30 min)

### Le format JSON

Le JSON est une chaîne de caractères qui représente des objets ou des tableaux.

* **PHP :** `$data = ["titre" => "Actu"];`
* **JSON :** `{"titre": "Actu"}`

### L'API Fetch (Côté Client)

En JS, on utilise `fetch()` qui renvoie une **Promesse**. C'est un concept asynchrone : "Je lance la requête et je te préviens quand j'ai la réponse".

## 6. Production principale (1h15)

### Étape 1 : L'API (api/get_news.php)

Ce fichier ne contient **aucun** HTML, uniquement du PHP.

```php
<?php
require_once '../db.php';
header('Content-Type: application/json'); // On définit le format de sortie

$stmt = $pdo->query("SELECT id, titre FROM articles LIMIT 5");
$articles = $stmt->fetchAll(PDO::FETCH_ASSOC);

echo json_encode($articles); // On transforme le tableau en JSON

```

### Étape 2 : Le Client (dashboard.html)

Créez une page HTML vide avec une `ul id="liste-news"`. Ajoutez ce script JS :

```javascript
fetch('api/get_news.php')
    .then(response => response.json()) // On décode le JSON reçu
    .then(data => {
        const list = document.getElementById('liste-news');
        data.forEach(article => {
            list.innerHTML += `<li>${article.titre}</li>`;
        });
    });

```

## 7. Exercices progressifs (45 min)

### Niveau 1 : Paramètres d'API

Modifiez l'API pour qu'elle accepte un paramètre `?limit=X` en GET. Utilisez ce paramètre dans votre requête SQL (avec une requête préparée !) pour limiter le nombre de news renvoyées.

### Niveau 2 : Gestion d'erreur API

Si la base de données est vide, l'API doit renvoyer un code d'erreur HTTP 404.
*Indice :* Utilisez `http_response_code(404);` et renvoyez un message JSON : `{"error": "Aucun article"}`.

### Niveau 3 : Rafraîchissement automatique

Ajoutez un bouton "Actualiser" en JS qui relance le `fetch()` pour mettre à jour la liste sans recharger la page.

## 8. Difficulté encadrée

**Le problème des CORS (Cross-Origin Resource Sharing) :**
Si vous essayez d'appeler votre API depuis un autre domaine (ou parfois même entre `localhost` et `127.0.0.1`), le navigateur bloquera la requête par sécurité.
**Sous Ubuntu :** Puisque vous utilisez le serveur intégré sur le même port, vous ne devriez pas avoir ce souci, mais gardez en tête que c'est le "Segmentation Fault" du développeur d'API.

## 9. Correction détaillée (Le point d'entrée API)

```php
// api/get_news.php
require_once '../db.php';
header('Content-Type: application/json');

try {
    $stmt = $pdo->query("SELECT * FROM articles");
    $data = $stmt->fetchAll(PDO::FETCH_ASSOC);
    echo json_encode($data);
} catch (Exception $e) {
    http_response_code(500);
    echo json_encode(["message" => "Erreur serveur"]);
}

```

## 10. Challenge autonome (20 min)

**"L'API de recherche"**

1. Créez `api/search.php?q=motcle`.
2. Elle doit renvoyer en JSON tous les articles contenant le mot-clé dans le titre.
3. Testez votre API simplement en tapant l'URL dans votre navigateur. Si vous voyez du texte brut structuré, c'est gagné !

## 11. Bilan de compétences

À la fin de cet atelier, l'étudiant doit savoir :

*   Configurer les headers HTTP pour envoyer du JSON avec PHP.
*   Utiliser `json_encode()` pour transformer des données complexes.
*   Consommer une source de données asynchrone avec JavaScript (`fetch`).
*   Comprendre le concept de séparation Backend (Données) / Frontend (Interface).
