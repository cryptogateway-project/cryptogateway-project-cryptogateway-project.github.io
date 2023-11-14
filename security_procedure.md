---
layout: home
title: Procédure de Sécurité 
---

# Procédure de sécurité

Notre API utilise un processus d'authentification sécurisé basé sur la génération de clés API et la vérification de l'intégrité des données à l'aide de HMAC avec l'algorithme SHA-256.

### Authentification des Requêtes

Les requêtes entrantes vers l'API nécessitent l'utilisation de clés API. Chaque client doit générer une clé api unique qui sera incluses dans toutes les requêtes vers notre serveur.

**Génération de la Clé API** 

Pour accéder à notre API, vous devez générer une clé API depuis le dashboard de votre compte utilisateur,chaque clé API générée dispose de deux éléments clés pour garantir un accès sécurisé et personnalisé à nos services. Le premier élément est le secret, une chaîne confidentielle utilisée pour signer les requêtes émises par cette clé API. Cette signature, basée sur un algorithme HMAC, assure l'intégrité des données lors de leur transmission. Le deuxième élément est constitué des scopes associés à la clé, définissant les services spécifiques auxquels cette clé a accès. Les scopes agissent comme des permissions, limitant l'étendue des actions que la clé API peut entreprendre.voici comment faire :

1. Connectez-vous à votre compte sur [notre tableau de bord](https://pay.izichange.com/login).
2. Accédez à la section "Paramètres API".
3. Cliquez sur "Information générales puis sur Ajouter au niveau de la section Clé API".
4. Copiez la clé générée en lieu sûr.

![Image alt image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/_img_gen_api_key.png)

![Image alt image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/_img_gen_api_key_form1.png)

![Image alt image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/_img_gen_api_key_form2.png)


**Utilisation de la Clé API** 

Vous devez inclure la clé générer dans l'entête `x-api-key` de chacune de vos requêtes vers l'API  de toutes les requêtes vers notre API. Assurez-vous que la clé API est générée et gérée correctement depuis la section dédiée de votre compte.

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

    $signature = "votre_signature"; 
    $options = [
        'http' => [
            'header' => "x-signature: $signature",
        ],
    ];
```
### Exemple de Requête finale

```http
    POST /api/exemple
    Authorization: Bearer VotreCleApiKey
    x-signature: VotreSIgnature
```

```php

    $signature = "votre_signature"; 
    $apiKey= "votre_cle_api";
    $options = [
        'http' => [
            'header' => [
                "Authorization:Bearer $apiKey",
                "x-signature: $signature",
            ],
        ],
    ];
```