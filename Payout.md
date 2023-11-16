---
layout: home
title: Payout 
weight: 5
---

# Payout

Un payout correspond à l'action de transférer ou retirer de la crypto-monnaie depuis votre portefeuille numérique. Cela revient à effectuer un virement ou un retrait de fonds en ligne, vous permettant de déplacer vos avoirs pour effectuer des paiements, retirer des fonds ou réaliser d'autres transactions.

### Initiation d'un paiement
Pour faire un paiement via l'Api, vous devez effectuer une requête HTTP POST à l'endpoint spécifié `/api/payements/payout`. Assurez-vous d'inclure les en-têtes nécessaires tels que `l'x-api-key` pour l'authentification API, et `l'x-signature` pour garantir l'intégrité des données.

**Description des paramètres**

| variables | type | description |
| --- | --- | --- |
| coin | string | le code de la cryptomonnaie |
| address | string | addresse de reception |
| amount | decimal | montant de la transaction |
| acceptToSupportFees | boolean | accepter supporter les frais de la transaction |

**Méthode HTTP :** POST

**Exemple de requête :**

``` http
POST /api/payements/payout HTTP/1.1
Host: <base_url>
Content-Type: application/json
x-api-key: <votre_cle_api>
Accept: application/json
x-signature: <la_signature_de_la_requete>
Content-Length: 300
{
  "amount": 100,
  "coin": "trx",
  "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
  "acceptToSupportFees": true
}
```

``` bash
    curl --location '<base_url>/api/payements/payout' \
--header 'x-api-key: <votre_cle_api>' \
--header 'x-signature: <la_signature_de_la_requete>' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
  "amount": 100,
  "coin": "trx",
  "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
  "acceptToSupportFees": true
}'

```
**Comment signer les requetes**
Lors de la construction de la donnée à signer pour la génération de la signature HMAC, le processus implique la concaténation du nom des paramètres avec leur valeur associée provenant de la requête. Cela crée une structure spécifique où chaque paramètre est représenté sous la forme "nom=valeur".

```
    dataToString = "coin=" + data.coin + "amount=" + data.amount + "address=" + data.address + "acceptToSupportFees=" + data.acceptToSupportFees;

    secret = "VotreSecretDeSignature";

    $signature = SHA256(dataToString, secret);
```

```php

   <?php
   
    $data = [
        "address" => "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
        "amount" => 100,
        "coin" => "trx"
        "acceptToSupportFees": true
    ];

    $dataToString ="coin=".$data['coin']."amount=".$data['amount']."address=".$data['address']."acceptToSupportFees="$data['acceptToSupportFees'];

    $secretKey="votre_secret_defini_a_la_generation_de_la_cle";
    $signature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);

```

**Format des réponses**

```
    {
    "status": true,
    "message": "",
    "data": {
        "id": "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
        "address": "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
        "amount": 100,
        "status": "PENDING",
        "coin": "trx"
    },

    "signature": "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
}

```

**Verification de la signature**
L'intégrité de nos réponses API est garantie par une signature qui associe la clé de l'adresse générée avec l'adresse elle-même. Voici la structure typique de notre réponse signée :


```php
    $receviedData = [
        "status" => true,
        "message" => "",
        "data" => [
            "id" => "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
            "address" => "TPEWaf6ZGJDrMbgKYoiM2Ze6BZydeRvDRQ",
            "amount" => 100,
            "status" => "PENDING",
            "coin" => "trx"
        ]
    ];

    $data=$receviedData['data'];

    $dataToString ="id=".$data['id']."address=".$data['address']."amount=".$data['amount']."status="$data['status']."coin=".$data['coin'];

    $secretKey="votre_secret_defini_a_la_generation_de_la_cle";
    $expectedSignature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);

    if(hash_equals($expectedSignature, $receviedData['signature'])){
        echo "signature valide";
    }else{
        echo "signature invalide";
    }

```