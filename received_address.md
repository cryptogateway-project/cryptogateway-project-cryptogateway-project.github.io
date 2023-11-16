---
layout: home
title: Génération d’addresse de reception
weight: 4
---

# Génération d’addresse de reception
Une adresse est une chaîne de caractères alphanumériques servant de référence pour recevoir et envoyer des fonds en toute sécurité. La génération d'adresses, que nous allons explorer ensemble, vous permettra de créer des addresses sur lesquelles vos utilisateurs pourront envoyer des fonds.

### Recuperation d'addresse
Pour récupérer une adresse de paiement, vous devez effectuer une requête HTTP POST à l'endpoint spécifié `/api/payements/address`. Assurez-vous d'inclure les en-têtes nécessaires tels que `l'x-api-key` pour l'authentification API, et `l'x-signature` pour garantir l'intégrité des données.

***Note:***
Lors de la construction de la donnée à signer pour la génération de la signature HMAC, le processus implique la concaténation du nom du paramètre avec sa valeur associée provenant de la requête. Cela crée une structure spécifique où chaque paramètre est représenté sous la forme "nom=valeur".

**Exemple de requête**
```http
    POST /api/payements/address HTTP/1.1
    Host: <base_url>
    Content-Type: application/json
    x-api-key: <votre_cle_api>
    Accept: application/json
    x-signature: <la_signature_de_la_requete>
    Content-Length: 20
    {
        "coin":"btc"
    }
```

``` bash
    curl --location '<base_url>/api/payements/address' \
    --header 'Content-Type: application/json' \
    --header 'x-api-key: <votre_cle_api>' \
    --header 'Accept: application/json' \
    --header 'x-signature: <la_signature_de_la_requete>' \
    --data '{
        "coin":"btc"
    }'
```


### Verification de la signature
L'intégrité de nos réponses API est garantie par une signature qui associe la clé de l'adresse générée avec l'adresse elle-même. Voici la structure typique de notre réponse signée :

**Exemple de reponse**
```
    {
        "status": true,
        "message": "created",
        "data": {
            "address": "tb1qpvmj4zagnn97amznzgsvp95mfkpyrw2amqz4aq"
        },
        "signature": "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
    }

```
**Exemple de code pour verifier la réponse**

```php
    $receviedData = [
        "status" => true,
        "message" => "created",
        "data" => [
            "address" => "tb1qpvmj4zagnn97amznzgsvp95mfkpyrw2amqz4aq"
        ],
        "signature" => "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
    ];

    $data=$receviedData['data'];

    $receviedDataTostring ="coin=".$data['coin'];

    $secretKey="votre_secret_defini_a_la_generation_de_la_cle";
    $expectedSignature = hash_hmac('sha256',$receviedDataTostring, $secretKey, FALSE);

    if(hash_equals($expectedSignature, $receviedData['signature'])){
        echo "signature valide";
    }else{
        echo "signature invalide";
    }

```