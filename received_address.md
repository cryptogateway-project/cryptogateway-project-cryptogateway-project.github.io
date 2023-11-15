---
layout: home
title: Génération d’addresse de reception
weight: 4
---

# Génération d’addresse de reception
Une adresse dans le contexte des actifs numériques est une chaîne de caractères unique qui identifie de manière spécifique un emplacement sur la blockchain. Elle sert de destination pour recevoir des fonds et est essentielle pour toutes les transactions. La génération d'adresses, que nous allons explorer ensemble, vous permettra de créer ces points d'interaction sécurisés, facilitant ainsi la réception et l'envoi sécurisés de vos actifs numériques sur notre plateforme.

**Recuperation d'addresse**
Pour récupérer une adresse de paiement, vous devez effectuer une requête HTTP POST à l'endpoint spécifié (/api/payements/address). Assurez-vous d'inclure les en-têtes nécessaires tels que `l'x-api-key` pour l'authentification API, l'Accept pour spécifier le format de réponse attendu, et `l'x-signature` pour garantir l'intégrité des données.

***Note:***
la donnée a signé  est construite en concaténant le nom du paramètre (dans cet exemple, "coin") avec sa valeur associée provenant de la requête.
Exemple: "coin=btc",

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
