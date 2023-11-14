---
layout: home
title: Procédure de Sécurité 
---

# Procédure de sécurité

Notre API utilise un processus d'authentification sécurisé basé sur la génération de clés API et la vérification de l'intégrité des données à l'aide de HMAC avec l'algorithme SHA-256.

### Authentification des Requêtes

Les requêtes entrantes vers l'API nécessitent l'utilisation de clés API. Chaque client doit générer une clé api unique qui sera incluses dans toutes les requêtes vers notre serveur.

**Génération de la Clé API** 

Pour accéder à notre API, vous devez générer une clé API depuis le dashboard de votre compte utilisateur,chaque clé API générée dispose de deux éléments clés pour garantir un accès sécurisé et personnalisé à nos services. Le premier élément est le secret, une chaîne confidentielle utilisée pour signer les requêtes émises par cette clé API. Cette signature, basée sur un algorithme HMAC, assure l'intégrité des données lors de leur transmission. Le deuxième élément est constitué des scopes associés à la clé, définissant les services spécifiques auxquels cette clé a accès. Les scopes agissent comme des permissions, limitant l'étendue des actions que la clé API peut entreprendre, vous avez aussi la possiblité de définir une liste d'addresse ip à partir duquel seront les requetês qui seront transitées par cette clé api devront transiter voici comment faire :

1. Connectez-vous à votre [dashboard](https://pay.izichange.com/login).
2. Accédez à la section "Paramètres API" puis sur "Information générale".
3. Cliquez sur ajouter au niveau de la section Clé API".
![image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/key1.png?width=200&height=100)
4. Remplissez les champs conformement à vos besoins
![image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/key2.png?width=200&height=100) 
[5] Copiez la clé générée en lieu sûr.
![image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/key4.png?width=200&height=100)

**Utilisation de la Clé API** 

Vous devez inclure la clé générer dans l'entête `x-api-key` de chacune de vos requêtes vers l'API  de toutes les requêtes vers notre API. Assurez-vous que la clé API est générée et gérée correctement depuis la section dédiée de votre compte.

``` http
POST base_url/monendpoint
x-api-key: VotreCleAPI
```

### Vérifications de l'intégrité des requête

Toutes les requêtes vers l'API doivent être signer afin de garantir l'intégrité des données. voici comment faire :

**Génération de la signature**
La génération de la signature pour chaque requête API implique l'utilisation de l'algorithme HMAC avec la fonction de hachage SHA-256 et l'utilisation d'un secret qui n'est rien d'autres celui défini lors de la génération de la clé d'API

**Exemple en pseudocode :**


```php

$dataArray = [
    "amount" => 0.001,
    "coin" => "trx",
    "address" => "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
    "acceptToSupportFees" => true
];


$dataToString ="coin=".$dataArray['coin']."amount=".$dataArray['amount']."address=".$dataArray['address']."acceptToSupportFees="$dataArray['acceptToSupportFees'];

$secretKey="votre_secret_defini_a_la_generation_de_la_cle";
$signature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);


```

**Ajoute la signature à l'en-tête de la requête**
Pour authentifier chaque requête, assurez-vous d'inclure la clé API générée dans l'en-tête `x-signature` de la requête HTTP.

**Exemple en pseudocode :**

```http
    POST /api/exemple
    x-api-key: VotreCleAPI
    x-signature: VotreSIgnature
```

```php
$signature = "votre_signature"; // Remplacez par votre valeur réelle de signature
$apikey="votre clé api";
$options = [
    'http' => [
        'header' => [
            "x-api-key: $apikey",
            "x-signature: $signature",
        ],
    ],
];
```
