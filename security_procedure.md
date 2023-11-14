---
layout: home
title: Procédure de Sécurité 
---

# Procédure d'authentification

Notre API utilise un processus d'authentification sécurisé basé sur la génération de clés API et la vérification de l'intégrité des données à l'aide de HMAC avec l'algorithme SHA-256.

### Authentification de l'API

Les requêtes entrantes vers notre API nécessitent l'utilisation de clés API. Chaque client autorisé reçoit une clé API unique qui doit être incluse dans le header `Authorization` des requêtes HTTP. Cela garantit une authentification sécurisée et contrôlée.

**Génération de la Clé API** 

Pour accéder à notre API, vous devez générer une clé API depuis le tableau de bord de notre plateforme web. Suivez ces étapes :

1. Connectez-vous à votre compte sur [notre tableau de bord](https://pay.izichange.com/login).
2. Accédez à la section "Paramètres API".
3. Cliquez sur "Information générales puis sur Ajouter au niveau de lasection Clé API".
4. Copiez la clé générée en lieu sûr. Cette clé sera utilisée pour authentifier vos requêtes API.

**Utilisation de la Clé API** 

Incluez votre clé API dans le header `Authorization` de toutes les requêtes vers notre API. Assurez-vous que la clé API est générée et gérée correctement depuis la section dédiée de votre compte.

### Authentification des Requêtes

Chaque requête API doit être authentifiée pour garantir l'intégrité des données. Suivez ces étapes pour inclure l'authentification dans vos requêtes

**Génération de la signature**
La génération de la signature pour chaque requête API implique l'utilisation de l'algorithme HMAC avec la fonction de hachage SHA-256 et l'utilisation d'un secret qui n'est rien d'autres celui défini lors de la génération de la clé d'API

**Exemple en pseudocode :**

```
    {
    "amount": 0.001,
    "coin": "trx",
    "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
    "acceptToSupportFees": true
    }

    dataToString = "coin=" + data.coin + "amount=" + data.amount + "address=" + data.address + "acceptToSupportFees=" + data.acceptToSupportFees;
    secret = "VotreSecretDeSignature";
    $signature = SHA256(dataToString, secret);

```

**Ajoute la signature à l'en-tête de la requête**
Pour authentifier chaque requête, assurez-vous d'inclure la clé API générée dans l'en-tête `x-signature` de la requête HTTP.

**Exemple en pseudocode :**
```php

    self::$HTTP_WITH_HEADER = Http::withHeaders([
        'x-signature'=>$signature
    ]);
```
### Exemple de Requête finale

```http
    POST /api/exemple
    Authorization: Bearer VotreCleApiKey
    x-signature: VotreSIgnature
```

```php

    self::$HTTP_WITH_HEADER = Http::withHeaders([
        'Authorization' => env('WALLET_MANAGER_PUBLIC_KEY'),
        'x-signature'=>$signature
    ]);
```