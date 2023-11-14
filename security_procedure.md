---
layout: home
title: Procédure de Sécurité 
---

# Procédure de sécurité

Notre API utilise un processus d'authentification sécurisé basé sur la génération de clés API et la vérification de l'intégrité des données à l'aide de HMAC avec l'algorithme SHA-256.

### Authentification des Requêtes

Les requêtes entrantes vers l'API nécessitent l'utilisation de clés API. Chaque client doit générer une clé api unique qui sera incluses dans toutes les requêtes vers notre serveur.

**Génération de la Clé API** 

Pour accéder à l'API, vous devez générer une clé depuis le dashboard de votre compte utilisateur. Chaque clé générée dispose de deux éléments principaux pour garantir un accès sécurisé et personnalisé à nos services. Le premier élément est le secret, une chaîne confidentielle utilisée pour signer les requêtes émises par la clé. La signature, basée sur un algorithme HMAC, assure l'intégrité des données lors de leur transmission. Le deuxième élément est constitué des accès associés à la clé, définissant les services spécifiques auxquels cette clé a accès. Les accès agissent comme des permissions, limitant les actions que la clé peut effectuées; Vous avez aussi la possiblité de définir une liste d'addresse ip à partir de laquelle seront envoyées les requetês vers l'API. Voici comment faire :

1. Connectez-vous à votre [dashboard](https://pay.izichange.com/login).
2. Accédez à la section "Paramètres API" puis sur "Information générale".
3. Cliquez sur ajouter au niveau de la section Clé API".
![image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/key1.png?width=200&height=100)
4. Remplissez les champs conformement à vos besoins
![image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/key2.png?width=200&height=100) 
5. Copiez la clé générée en lieu sûr.
![image du dashboard generation pour la génération de clé api izichangepay](/cryptogateway-project/assets/images/key4.png?width=200&height=100)

**Utilisation de la Clé API** 

Vous devez inclure la clé générer dans l'entête `x-api-key` de chacune de vos requêtes vers l'API. Assurez vous de garder la clé générée en un lieu sûr auquel vous les seuls à avoir accès. s'il s'avère qu'un utilisateur malveillant accède à cette clé, vous pourriez perdre tous vos fonds

``` http
POST <base_url>/monendpoint
x-api-key: "<VotreCleAPI>"
```

### Vérifications de l'intégrité des requête

Toutes les requêtes vers l'API doivent être signées afin de garantir l'intégrité des données transmis. Voici comment signer vos requetes :

**Génération de la signature**
La génération de la signature pour chaque requête API implique l'utilisation de l'algorithme HMAC avec la fonction de hachage SHA-256 et l'utilisation d'un secret qui n'est rien d'autres celui défini lors de la génération de la clé d'API

**Exemple en PHP :**


```php
<?php

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
Pour authentifier chaque requête, assurez-vous d'inclure la clé générée dans l'en-tête `x-signature` de la requête HTTP.

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

D'un autre coté vous devez vérifier toutes les requetes provenant de l'api vers vos services (IPN) afin de vous assurer qu'elles proviennent véritablement de nous. Le processus de génération présenté plus haut est le même utilisé pour générer la signature des données que nous vous transmettons. Cette signature est ajoutée aux données afin que vous vérifiez sa conformité.Il faudra donc reproduire ce même processus une fois les données reçues et comparez la signature que vous allez générer avec celle présente dans nos requêtes, Voici un exemple: 

```php
    $receviedData = [
        'detail' => [
            'txid' => '9a907970-27cd-41de-8e13-62569740e535',
            'address' => 'tb1qqtlwcd0accgaggfsug84ngzwr5leq4dfvpggql',
            'amount' => '1000',
            'status' => 'PENDING',
            'coin' => 'btc',
            'type' => 'payin',
        ],
        'signature' => '300e0876809980406cc2e8c485de34a4f486472db4edc3d2a99c39874b782f75',
    ];

    $data=$receviedData['detail'];

    $receviedDataTostring ="coin=".$data['coin']."amount=".$data['amount']."address=".$data['address']."acceptToSupportFees="$data['acceptToSupportFees'];

    $secretKey="votre_secret_defini_a_la_generation_de_la_cle";
    $expectedSignature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);

    if(hash_equals($expectedSignature, $receviedData['signature'])){
        echo "signature valide";
    }else{
        echo "signature invalide";
    }

```