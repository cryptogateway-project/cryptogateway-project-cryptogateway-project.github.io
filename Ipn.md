---
layout: home
title: Ipn
nav_order: 7
---

# Ipn

Notre API envoie des Notifications Instantanées de Paiement (IPN) au serveur du client pour l'avertir instantanément lorqu'une nouvelle transaction a lieu ou lorqu'elle change d'état. Ces alertes en temps réel assurent une réactivité immédiate face à chaque modification de statut ou d'étape de paiement. Veuillez vous assurer d'avoir bien configurer votre IPN sur le [dashboard](https://pay.izichange.com/login) ou [cliquez sur ce lien](./ipn_config) pour procéder à la configuration de votre IPN. 

**Format des réponses**
L'intégrité de nos réponses API est garantie par une signature. Voici la structure typique de notre réponse signée :

```
   {
    "detail": {
        "data": {
            "txid": "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
            "amount": "0.01",
            "status": "SUCCESS",
            "coin": "btc",
            "type": "payout"
        },
        "message": ""
    },
    "signature": "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
}

```

```php
    <?php
    $response = [
        "detail" => [
            "data" => [
                "txid" => "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
                "amount" => "0.01",
                "status" => "SUCCESS",
                "coin" => "btc",
                "type" => "payout"
            ],
            "message" => ""
        ],
        "signature" => "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
    ];

```

### Comment valider une transaction de payout:
Lors de la construction de la donnée à signer pour la génération de la signature HMAC, le processus implique la concaténation du nom des paramètres avec leur valeur associée provenant de la requête. Cela crée une structure spécifique où chaque paramètre est représenté sous la forme "nom=valeur".

```php
    <?php
    $response = [
        "detail" => [
            "data" => [
                "txid" => "9a8e033a-9e2e-494c-a2c9-8641404fd3c1",
                "amount" => "0.01",
                "status" => "SUCCESS",
                "coin" => "btc",
                "type" => "payout"
            ],
            "message" => ""
        ],
        "signature" => "7d637a4f666f48be2cd9c118d07508314c42aa59e3354e583994e6a5aa49a773"
    ];

    $data=$response['detail']['data'];
    
    $dataToString="type=".trim($data['type'])."coin=".trim($data['coin'])."amount=".trim($data['amount'])."status".trim($data['status']);


    $secretKey="votre_secret_defini_a_la_configuration_de_l'ipn";
    $expectedSignature = hash_hmac('sha256',$dataToString, $secretKey, FALSE);

    if(hash_equals($expectedSignature, $response['signature'])){
        echo "signature valide";
    }else{
        echo "signature invalide";
    }

```